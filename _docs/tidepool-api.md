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
  ],
  "userhash": "5b11bb9da396133d527ee6db",
  "private": {
    "meta": {
      "id": "3619e1628a",
      "hash": "f6f40eeda7ce22ad5fa2032a"
    }
  }
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

### Add note

### Edit note

### Get thread

## Groups

### Get Groups

  Get list of care teams i own or am a member of including my own care team.

  ```
  GET /groups/:userId(optional)
  x-tidepool-session-token: <token>
  ```

  Maybe see [Get User/s]

### Create group

  Creates a group. In the background this is done by adding data attribute to your profile or creating a dummy account. Userid defaults to user for token. When creating a dummy account you want to the user creating the account to be added as a admin or owner of the group.

  ```
  POST /groups/:userId

  x-tidepool-session-token: <token>
  ```

### Edit group

  See [Update user](#update-user).


### Delete group

  See [Delete User] for deleteing own user id.

  See how to handle deletion of dummy accounts that:
    Havent been claimed. i.e. are only data account
    Have only 1 admin that is you.
    Have multiple admins.


  Set a flag on care team object and its data that marks the object for deletion and removes it from care team view. (Who would be alowed to do this?).

## Group Members

### Get members
  Get members for a group.

  ```
  GET /groups/:userid/members/:memberUserId(optional)

  x-tidepool-session-token: <token>
  ```

### Change member permissions

  Change a members permissions (we need to see if only the careteam crator can do this or also an admin)

  ```
  POST /groups/:userid/members/:memberUserId

  x-tidepool-session-token: <token>
  body: {
    "permision": "admin" //or view_only
  }
  ```

### Remove member

  Remove a member from a careteam. (we need to see if only the careteam crator can do this or also an admin)

  A member can always remove himself from a care team.

  ```
  DELETE /groups/:userid/members/:memberUserId

  x-tidepool-session-token: <token>
  ```

## Member Invitations

### Send member invitation

  Send invitation to add a new member to a care team you are an admin of.

  ```
  POST /groups/:userid/invite

  x-tidepool-session-token: <token>
  body: {
    "email": "personToInvite@email.com",
    "permision": "admin" //or view_only
  }
  ```

### Get Sent invitations

  Get sent invitation for a group you own or are and amin of.

  ```
  GET /groups/:userid/invited

  x-tidepool-session-token: <token>
  ```

### Cancel Sent invitation

  Removes an invitation. This can happend before or after it being dismissed but not after it being approved. Requires admin privilieges.

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
