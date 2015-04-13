---
layout: defaults
title: Tidepool API Guidebook
published: true
---

# Tidepool API Guide (work in progress)

This API is broken down into the following sections

  * [User management](/tidepool-api/manage-user)
  * [Uploading device data](/tidepool-api/uploading)
  * [Accessing a user's data](/tidepool-api/downloading)
  * [Editing notes](/tidepool-api/notes)
  * [Sharing data](/tidepool-api/sharing)

In addition, this page has logging in and making a data request (download) as a simple API walkthrough or tutorial.

All requests URLs in this documentation are relative to `https://api.tidepool.io` (ex: `GET /auth/user` means `GET https://api.tidepool.io/auth/user`).

## Simple Walkthrough

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

### Download some data

TBD


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






