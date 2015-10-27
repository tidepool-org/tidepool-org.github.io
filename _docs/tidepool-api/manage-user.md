---
layout: defaults
title: Tidepool API - Managing User
published: true
---

# Tidepool API: Managing User

For authentication, maintaining session token and logging out, see the introductory [API walkthrough](/tidepool-api/index) . 

## User

### Signup user

There are two parts creating a "user object", the **account** and then the **profile**.

First, create the **account**:

```
POST /auth/user
```

Example Request:
```
curl -X POST -H "Content-Type: application/json" -d '{"username":"mary@example.com", "password":"myn3wpa55ord", "emails":["mary@example.com"]}'' '<api-endpoint>/auth/user'
```

Example Response:
```
201 Created
```

```json
{
  "userid": "b816f97ec5",
  "username": "mary@example.com",
  "emails": [
    "mary@example.com"
  ],
  "termsAccepted": ""
}
```

```
-H "x-tidepool-session-token" : "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkdXIiOjIuNTkyZSswNiwiZXhwIjoxNDQ4NTY3NDMwLCJzdnIiOiJubyIsInVzciI6IjMzZGYwNTJhNGIifQ.9-5Tf-vMXEtSl9EhWwHq_yAR9k2hCaEv0E3Akd0DX3w"
```

Then, create the **profile**:

```
POST /metadata/:userid/profile
x-tidepool-session-token: <token>
```

Example Request:
```
curl -X POST -H "Content-Type: application/json" -H "x-tidepool-session-token: <token>" -d '{"fullName": "Mary Smith", "shortName":"Mary", "patient": { "birthday": "1990-01-31", "diagnosisDate": "1999-01-31", "aboutMe": "I like oranges"}}' '<api-endpoint>/metadata/<userid>/profile'
```

Example Response:
```
200 OK
```

```json
{
  "fullName": "Mary Smith",
  "shortName":"Mary",
  "patient": {
    "birthday": "1990-01-31",
    "diagnosisDate": "1999-01-31",
    "aboutMe": "I like oranges"
  }
}
```


### Get user

Defaults to current user

There are two parts to the "user object", **account** and **profile** information.

First, fetch the logged-in user's **account** information with:

```
GET /auth/user/:userid
x-tidepool-session-token: <token>
```

Example Request:

```
curl -X GET -H "Content-Type: application/json" -H "x-tidepool-session-token: <token>" '<api-endpoint>/auth/user/<userid>'
```

Example Response:

```
200 OK
```

```json
{
  "userid": "b816f97ec5",
  "username": "mary@example.com",
  "emails": [
    "mary@example.com"
  ],
  "termsAccepted": "2015-10-26T22:53:39.199Z"
}
```

Then fetch the **profile** information using the logged-in `userid` and session token:

```
GET /metadata/<userid>/profile
x-tidepool-session-token: <token>
```

Example Request:

```
curl -X GET -H "Content-Type: application/json" -H "x-tidepool-session-token: <token>" '<api-endpoint>/metadata/<userid>/profile'
```

Example Response:

```
200 OK
```

```json
{
  "fullName": "Mary Smith",
  "shortName": "Mary",
  "patient": {
    "birthday": "1990-01-31",
    "diagnosisDate": "1999-01-31",
    "aboutMe": "I like oranges"
  }
}
```


### Update user

Apply updates to an existing users account for any or all of the feilds listed below

```json
{
  "username":"mary_other@example.com",
  "emails":["mary_other@example.com"],
  "termsAccepted":"2015-10-26T22:53:39.199Z",
  "password":"n3wpa55wOrd"
}
```

```
PUT /auth/user/<userid>
x-tidepool-session-token: <token>
```

Example Request:

```
curl -X PUT -H "Content-Type: application/json" -H "x-tidepool-session-token: <token>" -d '{"updates": {"termsAccepted":"2015-10-26T22:53:39.199Z"}}' '<api-endpoint>/auth/user/<userid>'
```

Example Response:

```
200 OK
```


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

### Creating a lost password request:
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


### Sign up

[Create account, update profile]



### Change password

### Forgot password

[Request password change, confirm new password]

