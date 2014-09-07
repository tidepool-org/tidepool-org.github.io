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

(Where Susie and Michael are other clients of hers.) The gatekeeper API is at the ```/access``` endpoint on Tidepool servers. Here's the list of things you can do with gatekeeper:

### Which groups is a user in?

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

### Who can access a given user's data?

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

### What permissions does userid have for groupid?

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

### Set a user's permissions

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


## Confirmations

There are three types of confirmations that can be generated:

* Initial signup ("signup")
* Forgot password ("forgot")
* Invitations to another user to "join your care team" (a way of saying "give a user permission to access your data). ("invite")

The three systems share the fact that they all deal with email notifications, but they differ in their details. The forgot password and signup systems are special because several of their operations need to be done without being authenticated (if you could log in, you wouldn't need to send a forgot password notification). The invitation system requires two users, and one of them may not yet have an account.

The signup system (where we get a confirmation that new users actually have a valid email address) is the most generic, so we'll deal with it first:

## Initial signup

### Send a signup confirmation

Send a signup confirmation email to a userid. 

```
POST /confirm/send/signup/:userid
x-tidepool-session-token: <token>
```

This post is sent by the signup logic. In this state, the user account has been created but has a flag that forces the user to the confirmation-required page until the signup has been confirmed.

(We need some rules about how often you can attempt a signup with a given email address, to keep this from being used to spam people either deliberately or accidentally. This call should also be throttled at the system level to prevent distributed attacks.)

It sends an email that contains a random confirmation link. 

### Resend confirmation 

If a user didn't receive the confirmation email and logs in, they're directed to the confirmation-required page which can offer to resend the confirmation email. 

```
POST /confirm/resend/signup/:userid
x-tidepool-session-token: <token>
```


### Accept/confirm a signup

This would be PUT by the web page at the link in the signup email. No authentication is required.

```
PUT /confirm/accept/signup/:userid/:confirmationID
```

When this call is made, the flag that prevents login on an account is removed, and the user is directed to the login page. If the user has an active cookie for signup (created with a short lifetime) we can accept the presence of that cookie to allow the actual login to be skipped.

### Dismiss a signup

In the event that someone uses the wrong email address, the receiver could explicitly dismiss a signup attempt with this link (useful for metrics and to identify phishing attempts). This link would be some sort of parenthetical comment in the signup confirmation email, like "(I didn't try to sign up for Tidepool.)" No authentication required.

```
PUT /confirm/dismiss/signup/:userid
```

Making this request deletes the account record that was used to create the signup.

### List signup confirmations

This call is provided for completeness -- we don't expect to need it in the actual user flow.

Fetch any existing confirmation requests. Will always return either 404 (not found) or a 200 with a single result in an array. 

```
GET /confirm/signup/:userid
x-tidepool-session-token: <token>
```


### Delete an existing signup confirmation

This call is provided for completeness -- we don't expect to need it in the actual user flow.

Delete any existing confirmation request. If this is called, it deletes the request but does not delete the account.

```
DELETE /confirm/signup/:userid
x-tidepool-session-token: <token>
```


## Lost password

Lost passwords are special because they have to be created and accepted without the usual authentication.

###Creating a lost password request:
```
POST /confirm/send/forgot/:useremail
```

If the request is correctly formed, always returns a 200, even if the email address was not found (this way it can't be used to validate email addresses).

The call is throttled at the system level to limit distributed attacks.

If the email address is found in the Tidepool system, this will:

* Create a confirm record and a random key
* Send an email with a link containing the key

Visiting the URL in the email will fetch a page that offers the user the chance to accept or reject the lost password request. If accepted, the user must then create a new password that will replace the old one.


### Accepting a lost password request

```
PUT /confirm/accept/forgot/

body {
  "key": "confirmkey"
  "email": "address_on_the_account"
  "password": "new_password"
}
```

This call will be invoked by the lost password screen with the key that was included in the URL of the lost password screen. For additional safety, the user will be required to manually enter the email address on the account as part of the UI, and also to enter a new password which will replace the password on the account. 

If this call is completed without error, the lost password request is marked as accepted. Otherwise, the lost password request remains active until it expires.

### Dismiss a lost password request

This is not necessary -- upon any successful login, the lost password request will be automatically deleted. (If the user successfully logs in after creating the lost password request, it is presumed that they didn't actually need to change it.)


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
