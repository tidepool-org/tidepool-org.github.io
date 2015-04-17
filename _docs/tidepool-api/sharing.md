---
layout: defaults
title: Tidepool API - Sharing
published: true
---

# Tidepool API: Sharing and Permissions

Tidepool user data can be shared flexibly with groups of other Tidepool users.  This document explains how to interact with the backend API to access those sharing functions. 

For other API endpoints, see the introductory [API walkthrough](/tidepool-api/index).  This API documentation is open-source -- please help keep it up-to-date.


## Overview

A user's "groups" are the set of accounts for which the user has some permission.  A better name for Groups would be "Permissions", but for historical reasons they're called groups. 


The permissions currently available are:

* **view** -- If B has view permission on account A, B can see all of A's data and metadata but does not have the ability to change it.
* **upload** -- If B has upload permission, B can add new data to A's account. This implies that B can also read enough information to determine the date and time of A's last upload.
* **note** -- If B has note permission, B can read the notes records in A's account and can create new notes and comments in that account, as well as edit the notes that B has created.
* **edit** -- If B has edit permission, B is allowed to change A's data and metadata, but does not have the ability to delete it.
* **admin** -- If B has admin permission, B can delete the account, change metadata, and change all permissions. It also includes the ability to send and accept invitations. 
* **root** -- Root can do anything; account A always has root on itself. No one except A can have root on account A. (Root doesn't actually exist as a permission bit in the database -- it's just returned from the API as if it does). 

### Example
Let's make this more real with an example:

Alice is 10. She has diabetes. She has an account on Tidepool.

Bob is Alice's dad. Carol is Alice's doctor. Dave is Alice's teacher, and Ellen is Alice's aunt, where Alice goes after school every day.


It's Alice's account, so she (and only she) has root.
Bob, the dad, has total control over the account (has every possible permission).
Carol, the doctor, can view, upload, and make notes.
Dave, the teacher, can make notes but not see any data.
Ellen, Alice's aunt, provides care but not management.

Here's the permissions table:

 permission| Alice | Bob | Carol | Dave | Ellen
--- | --- | --- | --- | --- | ---
view | |X|X| |
upload | |X|X| |X
note | |X|X|X|X
edit | |X| | | 
admin | |X| | | 
root | X | | | | 

If Alice asks "who can access my data?", she gets back the equivalent of:

  Alice: root
  Bob: view, upload, note, edit, admin
  Carol: view, upload, note
  Dave: note
  Ellen: upload, note

If Bob asks "which groups am I in?", he gets back:

  Alice: view, upload, note, edit, admin

If Carol asks "which groups am I in?", she might get back:

  Alice: view, upload, note
  Susie: view, upload, note
  Michael: view

(Where Susie and Michael are other clients of hers.) 

Note to Tidepool developers: in the backend this is implemented in (gatekeeper)[https://github.com/tidepool-org/gatekeeper/blob/master/lib/server.js].  The gatekeeper API is at the ```/access``` endpoint on Tidepool servers.  

## Which groups is a user in?

This really means "whose data can a user access, and what permissions exist for each of them?"


```
GET /access/groups/:userId
x-tidepool-session-token: <token>
```

The userId must be user belonging to the session token, or someone for whom that user has admin permission. If not, a 403 will be returned.

The response to this request would look something like this:

```javascript
{
  "123": {
    "root": {}
  },
  "456": {
    "note": {}
  }
  "789": {
    "view": {}
    "note": {}
    "upload": {}
  }
}
```

### Error responses

  * **401 Unauthorized**: The currently authenticated user does not have permissions to look at groups for the requested user.

## Who can access a given user's data?

This generates the list of userids that have access to the specified user's data, and which permissions they have.

```
GET /access/:groupId
x-tidepool-session-token: <token>
```

The groupId here is the userId belonging to the session token, or someone for whom that user has admin permission. The response to this request would look something like this:

```javascript
{
  "123": {
    "root": {}
  },
  "456": {
    "view": {}
  }
}
```

### Error responses

  * **401 Unauthorized**: The currently authenticated user does not have permissions to look at this group.

## What permissions does userid have for groupid?

This call checks a specific pair of users. The request is only valid if the session token making the request has admin permission on groupid, or if it is userid.

```
GET /access/:groupId/:userid
x-tidepool-session-token: <token>
```

The "groupId" belongs to the user whose data is being accessed, and the userId belongs to the user who may have permissions. The response to this request would look something like this:

```javascript
{
    "note": {}
    "view": {}
}
```

### Error responses

  * **401 Unauthorized**: The currently authenticated user does not have permissions to look at this information.
  * **404 Not Found**: The group does not grant any permissions to the user identified in the URL.
  * **500 ServerError**: Another error occurred in the backend. 

## Set a user's permissions

  This gives a user a specific set of permissions to access data.

  ```
  POST /access/:groupId/:userId
  x-tidepool-session-token: <token>
  ```

  The groupId must be the user being modified, and the userId must be the user getting the permission. If permissions are being added, the user making the request (the user in the session token) must have admin permission on the group being modified. If permissions are being dropped, the user with the permission is allowed to drop it (in other words, it's OK to say you don't want to be able to see someone's data). 

  Body is a block of permissions that looks like this:

 ```javascript
 {
     "note": {}
     "view": {}
 }
 ```

  If the call succeeds, the permissions block *replaces* the set of permissions for that user. There is no separate call to add or remove a permission -- you must first read the existing permissions and then change them.

  Note that each permission has an empty object to define its existence; this is to support the idea of more granular permissions later (for example, the "view" permission could support the concept of specific subsets of data to be viewed).

  If a permission is not listed, the permission is not granted. 


## Member Invitations

### Send member invitation

Send invitation to add a new member to a care team you are an admin of.

The invitation must include the permissions being granted (edit, notes, view, upload,  admin) in the same format as granting permissions, so that the invitation can properly express the request.

```
POST /confirm/send/invite/:userid

x-tidepool-session-token: <token>

body: {
  "email": "personToInvite@email.com",
  "permissions": [
    "view": {},
    "note": {}
  ]
}
```

The userid is the user sending the invitation. When sending an invitation, you don't necessarily know if the user is already on the system or not, which is why there's no recipient ID field. 

### Get Sent invitations

Get the still-pending invitations for a group you own or are an admin of. These are the invitations you have sent that have not been accepted. There is no way to tell if an invitation has been ignored. Requires admin privileges.

```
GET /confirm/invite/:userid

x-tidepool-session-token: <token>

body: [{
  "email": "invitation1@email.com",
  "permissions": {
    "view": {},
    "note": {}
  }
}]
```

### Cancel Sent invitation

Removes a pending invitation (one that has not yet been accepted by the recipient). This can happen before or after it being dismissed but not after it has been accepted. Requires admin privileges.

```
DELETE /confirm/:userid/invited/:invited_address

x-tidepool-session-token: <token>
```

The userid field is the user who sent the invitation, and the invited_address field is the email address that was invited.

### Get received invitations

Get list of received invitations for logged in user. These are invitations that have been sent to this user but not yet acted upon. (This call is unique to invite; there is no equivalent for signup or forgot.)

```
GET /confirm/invitations/:userid

x-tidepool-session-token: <token>

body: [{
  "invitedBy": "123abc",
  "permissions": {
    "view": {},
    "note": {}
  }
}]
```

The permissions object is showing the permissions that have been offered by the user who sent the invitation.

### Dismiss invitation

Sets a dismissed flag in the invitation so that the users that received the invitation does not see it again. The user that sent it still sees it.

```
PUT /confirm/dismiss/invite/:userid/:invited_by

x-tidepool-session-token: <token>
```

No body is required, but both :userid and :invitedby are required.

### Accept invitation

Performs the required permission actions to set the permissions as spec'd in the invitation, then deletes the invitation record. 

After performing this action, the client should refresh its information about the user's careteams.

```
PUT /confirm/accept/invite/:userid/:invited_by

x-tidepool-session-token: <token>
```

No body is required, but both :userid and :invitedby are required.




## Data accounts

Create data account for myself (user `123`):

Update my profile (or account) object, from this:

```javascript
var account = {
  "userid": "123",
  "username": "msmith@example.com",
  "password": "secret"
};
var profile = {
  "fullName": "Mary Smith"
};
```

to this:

```javascript
var account = {
  "userid": "123",
  "username": "msmith@example.com",
  "password": "secret"
};
var profile = {
  "fullName": "Mary Smith",
  "isDataAccount": true
};
```

Create a data account for my daughter:

```
POST /user/createChild/:userid

x-tidepool-session-token: <token>
```

Creates a child data account that is managed by the account :userid. For client-side uses, :userid must be the account owned by the token.

Body of post is:

```javascript
{
  "username": "a_username",
  "password": "a_password",
  "emails": [list_of_emails]
  "fullName": "Jane Smith"
}
```

The fullName field and username are both required. The username can be any valid username, or it can be omitted or blank. If present, the call will be rejected if the username is not unique. If no username is specified then a random one will be generated and assigned (it can be changed later, if desired). The password may be omitted or blank. If so, it will not be possible to log into the account directly. Emails is a list of email addresses and it may be omitted or blank. 

If validation fails, a 400 error is returned with an explanation of the error in the body.

If all validation passes, the new account is created and the caller's account is given all access rights to it (view, edit, note, admin, upload). The return body contains the user data for the new account:

```javascript
{
  "username": "username",
  "emails": [list_of_emails]
  "fullName": "Jane Smith"
  "userid": "abc123"
}
```



It is not possible to remove last admin if account doesn't have login or recovery capabilities (i.e, if account doesn't have emails or password).

