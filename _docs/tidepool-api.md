---
layout: defaults
title: Tidepool API Guidebook
published: true
---

# Tidepool API Guide (work in progress)

All requests URLs in this documentation are relative to `https://api.tidepool.io` (ex: `GET /auth/user` means `GET https://api.tidepool.io/auth/user`).

## Auth

### Login

Login with a user's username and password using [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication):

```
POST /auth/login
Authorization: Basic bmljb2xhc0B0aWRlcG9vbC5vcmc6dGVzdA==
```
Response:

```
200 OK
x-tidepool-session-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9[...]
```

```javascript
{
  "userid": "88144f5ea3",
  "username": "mary@example.com",
  "emails": ["mary@example.com"]
}
```

Store the `x-tidepool-session-token` from the response headers. You will need it to make all subsequent requests for that user.

If the login request fails because the username doesn't exist, or the password provided is wrong, you will get:

```
401 Unauthorized
```

```javascript
"login failed"
```

### Refresh token

A session token is **valid for 1 hour**, so you need to make sure to refresh it regularly by calling:

```
GET /auth/login
x-tidepool-session-token: <current token>
```

Response:

```
200 OK
x-tidepool-session-token: <current token>
```

```javascript
{
  "userid":"88144f5ea3"
}
```

If you try to refresh with an invalid or expired token, you will get:

```
401 Unauthorized
```

```javascript
"Session token required"
```

### Logout

Expire the current session token with:

```
POST /auth/logout
x-tidepool-session-token: <token>
```

Response:

```
200 OK
```

Note: Logging out with a token that has already expired will yield the same result.

## User

### Get user

Defaults to current user

There are two parts to the "user object", **account** and **profile** information.

First, fetch the logged-in user's **account** information with:

```
GET /auth/user/:userid
x-tidepool-session-token: <token>
```

Response:

```
200 OK
```

```javascript
{
  "userid": "88144f5ea3",
  "username": "mary@example.com",
  "emails": [
    "mary@example.com"
  ]
}
```

Then fetch the **profile** information using the logged-in `userid` and session token:

```
GET /metadata/<userid>/profile
x-tidepool-session-token: <token>

```

Response:

```
200 OK
```

```javascript
{
  "fullName": "Mary Smith",
  "patient": {
    "birthday": "1990-01-31",
    "diagnosisDate": "1999-01-31",
    "aboutMe": "I like oranges"
  }
}
```

### Sign up

[Create account, update profile]

### Update user

Defaults to current user

[Update account, update profile]

### Change password

### Forgot password

[Request password change, confirm new password]

## Health data

[Link to format documentation]

### Get data

[For Care Team, for date range]

### Post data

[One datum, an array of data]

## Notes

### Get notes

Top level notes (i.e. no comments) within an optional date range

```
GET /message/notes/:groupid?starttime&endtime
x-tidepool-session-token: <token>
```

Response:

```
200 OK
```

```javascript
messages: [
 {
    id: '123',
    parentmessage : null,
    userid: '777',
    user: { "fullName": "Mary Smith" }, //users public profile
    groupid: '777',
    timestamp: '2013-11-28T23:07:40+00:00',
    createdtime: '2013-11-28T23:07:40+00:00',
    messagetext: 'In three words I can sum up everything I have learned about life: it goes on.'
  },
  {
    id: '456',
    parentmessage:null,
    user: { "fullName": "Bob Smith" }, //users public profile
    userid: '112',
    groupid: '777',
    timestamp: '2013-11-29T23:05:40+00:00',
    createdtime: '2013-11-28T23:07:40+00:00',
    messagetext: 'Second message.'
  }
]
```

### Get messages

All messages within an optional date range

```
GET /message/all/:groupid?starttime&endtime
x-tidepool-session-token: <token>
```

Response:

```
200 OK
```

```javascript
messages: [
 {
    id: '123',
    parentmessage : null,
    userid: '777',
    user: { "fullName": "Mary Smith" }, //users public profile
    groupid: '777',
    timestamp: '2013-11-28T23:07:40+00:00',
    createdtime: '2013-11-28T23:07:40+00:00',
    messagetext: 'In three words I can sum up everything I have learned about life: it goes on.'
  },
  {
    id: '456',
    parentmessage:'123',
    user: { "fullName": "Bob Smith" },
    userid: '112',
    groupid: '777',
    timestamp: '2013-11-29T23:05:40+00:00',
    createdtime: '2013-11-28T23:07:40+00:00',
    messagetext: 'A comment.'
  },
  {
    id: '789',
    parentmessage: null,
    user: { "fullName": "Bob Smith" },
    userid: '112',
    groupid: '777',
    timestamp: '2013-11-29T23:05:40+00:00',
    createdtime: '2013-11-28T23:07:40+00:00',
    messagetext: 'A new message.'
  }
]
```


### Add note

### Edit note

### Get thread

## Groups

A better name for Groups would be "Permissions", but for historical reasons they're called groups. 

### Overview

The "gatekeeper" project provides the mechanism by which Tidepool manages permissions between users.

A user's "groups" are the set of accounts for which the user has some permission.

The permissions currently available are:

* **view** -- If B has view permission on account A, B can see all of A's data and metadata but does not have the ability to change it.
* **upload** -- If B has upload permission, B can add new data to A's account, which implies that B can also read enough information to determine the date and time of A's last upload.
* **note** -- If B has note permission, B can read the notes records in A's account and can create new notes in that account, as well as edit the notes that B has created.
* **edit** -- If B has edit permission, B is allowed to change A's data and metadata, but does not have the ability to delete it.
* **administer** -- If B has administer permission, B can delete the account, change metadata, and change invite permissions. 
* **root** -- Root can do anything; account A is presumed to have all permissions on itself. No one except A can have root on account A.

### Example
Let's make this more real with an example:

Alice is 10. She has diabetes. She has an account on Tidepool.

Bob is Alice's dad. Carol is Alice's doctor. Dave is Alice's teacher, and Ellen is Alice's aunt, where Alice goes after school every day.


It's Alice's account, so she (and only she) has root.
Dad has total control over the account (has every possible permission).
The doctor can view, upload, and make notes.
The teacher can make notes but not see any data.
Alice's aunt provides care but not management.

Here's the permissions table:

 permission| Alice | Bob | Carol | Dave | Ellen
- | - | - | - | - | -
view | |X|X| |
upload | |X|X| |X
note | |X|X|X|X
edit | |X| | | 
administer | |X| | | 
root | X | | | | 

If Alice asks "who can access my data?", she gets back the equivalent of:

  Alice: root
  Bob: view, upload, note, edit, administer
  Carol: view, upload, note
  Dave: note
  Ellen: upload, note

If Bob asks "which groups am I in?", he gets back:

  Alice: view, upload, note, edit, administer

If Carol asks "which groups am I in?", she might get back:

  Alice: view, upload, note
  Susie: view, upload, note
  Michael: view

The gatekeeper API is at the /access endpoint on Tidepool servers. Here's the list of things you can do with gatekeeper:

### Which groups am I in?


This really means "whose user data can I access, and what permissions do I have for each of them?"

```
GET /access/groups/:userId
x-tidepool-session-token: <token>
```

The userId must be your own, or someone for whom you have "administrative" permission.

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

### Who can access my data?

This generates the list of userids that have access to your data, and which permissions they have.

```
GET /access/:groupId
x-tidepool-session-token: <token>
```

The groupId here is your userId, or someone for whom you have administrative permission. The response to this request would look something like this:

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

### What permissions does user X have on user Y?

This call checks a specific pair of users. The request is only valid if  the userID making the request has administrative permission on one of the accounts in the pair.

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

### Set a user's permissions

  This gives a user a specific set of permissions to access your data.

  ```
  POST /access/:groupId/:userId
  x-tidepool-session-token: <token>
  ```

  The groupId is the user being modified, and the userId is the user getting the permission. If permissions are being added, the user making the request must have administrative permission on the user being modified. If permissions are being dropped, the user with the permission is allowed to drop it (it's OK to say you don't want to be able to see someone's data). 

  Body is a block of permissions that looks like this:

 ```javascript
 {
     "note": {}
     "view": {}
 }
 ```

  The permissions block *replaces* the set of permissions for that user. There is no separate call to add or remove a permission -- you must first read the existing permissions and then change them.

  Note that each permission has an empty object to define its existence; this is to support the idea of more granular permissions later (for example, the "view" permission could support the concept of specific subsets of data to be viewed).

  If a permission is not listed, the permission is not granted. 


## Member Invitations

### Send member invitation

  Send invitation to add a new member to a care team you are an admin of.

  ```
  POST /groups/:userid/invite

  x-tidepool-session-token: <token>
  body: {
    "email": "personToInvite@email.com",
    "permission": "admin" //or view_only
  }
  ```

### Get Sent invitations

  Get sent invitation for a group you own or are an admin of.

  ```
  GET /groups/:userid/invited

  x-tidepool-session-token: <token>
  ```

### Cancel Sent invitation

  Removes a pending invitation (one that has not yet been accepted by the recipient). This can happen before or after it being dismissed but not after it being approved. Requires admin privilieges.

  ```
  DELETE /groups/:userid/invited/:inviteId

  x-tidepool-session-token: <token>
  ```
### Get received invitations

  Get list of received invitations for logged in user.

  ```
  GET /user/invites

  x-tidepool-session-token: <token>
  ```
### Dismiss invitation

  Sets a dismissed flag in the invitation so that the users that received the invitation does not see it again. The user that sent it still sees it.

  ```
  POST /user/invites/:invitationId/dismiss

  x-tidepool-session-token: <token>
  ```

### Accept invitation

  ```
  POST /user/invites/:invitationId/accept
  ```
  An invitation gets accepted and a care team is created, then invitation gets deleted.


# Ideally we (api consumers) would deal with member invitation and confirmations is the way invitations are handeled by the backend.

## Confirmations

This section is meant to describe what confirmations are and how to use them.

A transction id for example in links generated by the system. A transation id is passed to the client application which can then query the transaction api to get the state and metadata for the transaction.

Transcation types: careteam_invitation, password_reset and email_confirmation.

Sample data format:

```javascript
{
  // Confirmation id
  "id": "abcd098",

  // Destination of confirmation can be:
  // an existing user...
  "userid": "123",
  // OR an email address
  "userEmail": "msmith@example.com",

  // Origin of confirmation
  // (optional, only if different from destination)
  "createdBy": "456",

  // Confirmation type
  // snake_case naming convention?
  "type": "careteam_invitation",

  // Confirmation status
  // Possible values: pending, confirmed, failed, expired
  // Note that this is the only field that can change
  // (along with "modifiedOn" of course)
  "status": "pending",

  // Confirmation key
  // The only thing public, sent via email?
  "key": "slj39jng921nquirtey=",

  // Confirmation lifecyle (used to expire or for analytics)
  "createdOn": "2014-01-31T12:23:32Z",
  "modifiedOn": "2014-02-14T09:00:25Z",

  // Error object when status is "failed"?
  "error": {
    "name": "EmailDeliveryFailure",
    "message": "Could not deliver email to provided address"
  }
}
```

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

Need create new "dummy account" (user `456`, no password).

First create dummy user and profile. We need to figure out how user `123` becomes an owner for dummy account `456`. Ideally would happend in one call, to avoid risks of a ghosts account.

Would having Root access to the acconut mean that i can delte the acount. If have root and account was login.


```javascript
var account = {
	"userid": "456",
    "username": "generated6534"
};
var profile = {
	"fullName": "Elisabeth Smith",
    "isDataAccount": true
};
```

(User `123`, me, has `admin` permissions (maybe root permisons too?) to user `456`, my daughter.)
Removal of last admin would also require account deletion.
