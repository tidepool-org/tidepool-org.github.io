# Tidepool API Guide

All requests URLs in this documentation are relative to `https://api.tidepool.io` (ex: `GET /auth/user` means `GET https://api.tidepool.io/auth/user`).

Table of contents:

- [Auth](#auth)
    - [Login](#login)
    - [Refresh token](#refresh-token)
    - [Logout](#logout)
- [User](#user)
    - [Get current user](#get-current-user)
    - [Sign up](#sign-up)
    - [Update current user](#update-current-user)
    - [Change password](#change-password)
    - [Forgot password](#forgot-password)
- [Health data](#health-data)
    - [Get data](#get-data)
    - [Post data](#post-data)
- [Notes](#notes)
    - [Get notes](#get-notes)
    - [Add note](#add-note)
    - [Edit note](#edit-note)
    - [Get thread](#get-thread)
- [Care Team](#care-team)
    - [Create Care Team](#create-care-team)
    - [Get Care Teams](#get-care-teams)
    - [Get members](#get-members)
    - [Send member invitation](#send-member-invitation)
    - [Get sent invitations](#get-pending-invitations)
    - [Cancel invitation](#cancel-invitation)
    - [Get received invitations](#get-received-invitations)
    - [Accept invitation](#accept-invitation)
    - [Change member permissions](#change-member-permissions)
    - [Remove member](#remove-member)

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

### Get current user

There are two parts to the "user object", **account** and **profile** information.

First, fetch the logged-in user's **account** information with:

```
GET /auth/user
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

### Update current user

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

## Care Team

### Create Care Team

  Create a new care team (we need to see if only the careteam crator can do this or also an admin)

  ```
  Set permissions for either admin or view only.
  ```


### Get Care Teams

  Get list of care teams i own or am a member of including my own care team.

### Get members
  Get members for a care team,

### Send member invitation

  Dont think we need this, how is this different from create care team. Was this meant for re-sending invitations?

### Get Sent invitations

  Get a list of sent pending invitations

### Cancel invitation

  Removes an invitation. This can happend before of after it being dismissed by not after it being approved.

### Dismiss invitation

  Sets a dismissed flag in invitation so that the users that recieved then invitation does not see it again.

### Get received invitations

  Get list of received invitations for a given user.

### Accept invitation

  An invitation gets accepted and a care team is created, then invitation gets deleted.

### Change member permissions

  Change a members permissions (we need to see if only the careteam crator can do this or also an admin)

### Remove member

  Remove a member from a careteam. (we need to see if only the careteam crator can do this or also an admin)

  A member can always remove himself from a care team.
