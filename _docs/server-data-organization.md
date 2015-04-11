---
layout: defaults
title: Tidepool Server Data Model
published: true
---
# Tidepool's Server Data Model

The core concept behind Tidepool's data model is that there are different classes of security needed for different types of data. User data needed for logins needs to be in one database, searchable. User metadata is encrypted on a per-user basis, and protected so that even having both the user data and the metadata, you still wouldn't be able to decrypt it without also having our deploy keys. It is also the case that you can't go backward from metadata to user data, you can only go forward from user data to metadata. The same is true for metadata -> patientdata. If you only have patient data, you can't find the metadata associated with it.

## Glossary

* **Tidepool-generated** -- This is a field that was generated automatically by Tidepool. It is effectively an unpredictable random string, and there is no derivable relationship between this field and the user it is associated with, nor is there any relationship between this field and others like it.

* **User-supplied** -- The end user can create this field.

* **Unique** -- This field must be unique within the database.

* **Array** -- This field can have multiple values. The first value in the array is used whenever only one is required.

* **Permanent** -- This field is a permanent identifier that can never be changed.

* **Semi-permanent** -- This field is used as an identifier, but it could theoretically be changed (eventually every field marked this way should have a way to change it, but we may not support that initially).

* **Server-only** -- This field can only be fetched and used by Tidepool servers; it is not available through the external API.

* **Editable** -- The end user can change this field arbitrarily.

* **Hash** -- This field is a hash code that can and should be used as an encryption key.

* **Optional** -- The field may be empty.

* **Required** -- The field may not be empty.

## User / Login information

This is information that lives in the user API (tidepool-org/user-api). This consists of the minimum amount of information about individual users to permit them to log in, and explicitly excludes any information about whether they are patients.

**NOTE** -- *PATIENT data should never touch the user repository.* The reason is that the user repository uses a single key for all data, while other repositories can encrypt information on a per-user basis.

Fields in this data structure include:

* **userid** -- Tidepool-generated, unique, permanent -- The userid created when the user first creates an account. It never changes.
* **username** -- User-supplied, required, unique, editable -- A unique username provided by the user (could be an email name or a user name, whatever). Used to provide a unique login name.
* **emails** -- User-supplied, required, array, editable -- Each email in the list must be unique in the database. The first item in the list is used for sending notifications and messages. Password recovery can take place with any of them. Any of them can be used instead of username to log in.
* **pwhash** -- Required, user-supplied but obfuscated, editable -- The user's password is hashed and stored; only the hashes are ever compared. Actual password is never stored and not recoverable (we'll have a password generation system). This field never leaves the user API for any reason.
* **private** -- A JavaScript object containing id and hash information for other private data fields. Each element in the private object is itself an object with two fields. There are API methods for generating these objects, for saving them in the **private** field, and for regenerating them if they need to change. This field is never returned from any query outside the firewall -- Only Tidepool servers can access it. It is generally expected that each separate Tidepool API will create a unique element in the private object (one for metadata, one for message data, etc).
  * **id** -- A short, Tidepool-generated, unique, semi-permanent ID that is intended for use in naming things.
  * **hash** -- Tidepool-generated, unique, hash -- used for encrypting things

## User Metadata

This is further information about the user. The main field (called value) is the user-api private.meta.id value. The value of the field is a block of json, encrypted with private.meta.hash plus a deploy-specific salt value.

The field is divided into named compartments called "collections". The intent is that someday collections will each be able to have a schema that can control what members they can have.

Metadata fields include:

* **metaid** -- Unique, semipermanent, server-only -- the key field in the database. Stored in user-api as private.meta.id.
* **value** -- An object, consisting of a block of JSON that has been stringized and encrypted with userapi/private.meta.hash (plus salt). Top-level fields in this object are collections; each collection is itself an object with a specified format:
  * **profile** -- Tidepool-defined, required -- Contains the profile collection.
    * **fullname** -- User-supplied, required, editable -- The full name of the user, as the user wishes to be known to others. Would be used in things like "Full Name wants to invite you to join..."
    * **shortname** -- User-supplied, required, editable -- the short form of the name, used in things like UIs. The short name would be displayed on a summary of a chat message. (Length limited?)
    * **publicbio** -- User-supplied, optional, editable -- A block of text (limited to 500 characters) that is displayed in the "about you" section of your user profile.
  * **groups** -- server-only; all groups are optional, but the groups object itself must exist even if empty -- An object containing members, each of which is a group ID. Specific members are:
    * **team** -- This is the group of users who participate in the message traffic about you. Messages posted to this group are attached to your user data.
    * **uploaders** -- This is a group of users who also have the right to upload data to this account. The account holder always has this right so this field is only needed if multiple people have upload rights.
    * **viewers** -- This is a group of users who can see patient data associated with this account. Basically, this is the inbound list -- the people who can see your data.
    * **patients** -- This is the group of users whose patient data you can see (you're a member of one or more groups for them -- team, uploaders, viewers). Users always have the right to see their own data, so this field only applies if you need to see data for someone other than yourself.
    * **invited** -- This is a list of people you've invited to be on your team but who have not yet accepted.
    * **invitedby** -- This is a list of users who have sent you invitations to be on their team. You must accept invitations to be on a team. (We may skip this step for pilot.)
  * **private** -- Similiar to the `private` field in the user API, but for information that is related to patient data. Has a list of named objects, each with id and hash elements. If the user is not a patient, this field will contain no members.
      * **patient** -- The id and hash of patient data
      * **sandcastle** -- The id and hash used for the raw data API ([sandcastle](https://github.com/tidepool-org/sandcastle))

## Group Data

Groups are just lists of users. Nothing more.

They are currently used for two things, primarily -- permissions management and messaging. They might someday do more than that.

Group information has use cases not only where users want access to their own groups, but where users like doctors want a list of groups they belong to. Consequently, there's no easy way to leave the group data encrypted or have each group tied to a single patient. The current plan is that group membership is stored only with disk-level encryption. The viewers and patients lists must both be updated frequently.

Since groups are just lists of people, there are no real semantics to the group information other than association. While there is meaning that can be guessed by knowledge of association between users, it's not a huge hole.

Fields in a group include:

* **groupid** -- Tidepool-generated, unique, permanent -- The ID for the group.
* **members** -- Array -- The userids of the people in the list.

## Patient Data

Patient data contains health information about a patient as well as demographic information that is meaningful but not explicitly identifiable. It is information that could conceivably be released to a researcher with patient approval but under normal circumstances is protected.

Note that top-level fields are separately encrypted so that we could theoretically release one and not others. (This needs further thought.)

Patient data includes:

* **patientid** -- The value from metadata/private.patient.id
* **health** -- Required, encrypted -- An object that contains health-related data
  * **diagnosisdate** -- User-supplied, editable, semi-optional -- The date the patient was diagnosed. An object with year, month, day fields, but absence of day, day and month, or all three fields is permitted (user may only know or care about part of this information)
  * **other?** -- What other health fields do we need?
* **demographic** -- Required, encrypted -- an object containing demographic data
  * **birthdate** -- Optional
  * **zip code** -- Optional
  * **gender** -- Optional

# Still to do...
## Messages
