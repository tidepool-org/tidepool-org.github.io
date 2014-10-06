---
layout: defaults
title: Tidepool Project Contributions
published: true
---
# _NOTE_

_As of 5 October 2014, this document describes how things will work when we actually start creating API tokens. We don't currently have them, so we don't currently have a means for third party apps to use Tidepool data. We expect to resolve this within the next few months. If you have an urgent need, please talk to us._

# How to build things that talk to Tidepool

To successfully exchange data with Tidepool's servers, you need two things:

* An API key from Tidepool. This will be used to identify the app that is talking to Tidepool, so you should get a new key for every app. 

* A user's permission to access the API. This permission is given to each app individually.

## Getting an API key

To get an API key for Tidepool, log in as a user (create an account if you don't already have one). Then, using your user token, request an API key from the user API. (Details to follow, but you'll do two curl requests on the command line and the response from the second one will contain your API key. Eventually we'll build a web page for this.)

There are two types of app keys available: user app keys and research app keys. User app keys are available to anyone. Research app keys allow access to a broad range of data and will only be available to qualified researchers upon application. Research app keys will have individual restrictions regarding which users and data types can be queried.

Users of your app must then authenticate to Tidepool using OAuth. Briefly, the user will log in to their account at Tidepool using your API token and their account.

## Permissions
The OAuth request will include the set of permissions your app is asking for. The user must agree to give your app those permissions or the request will fail. 

Possible permissions a user can grant to an app include:

* Read/manage public data
* Read/manage private data
* Read/manage messages
* Upload/Manage/Query health records

All of these permissions are layered on top of Tidepool's user permissions model. In other words, if the app is granted the ability to read messages, the app can read any messages that the user has permission to see (both their own and messages in other people's accounts). 

## Tokens
Once the permissions are granted, the app gets a token. This token is then used in all other requests to the Tidepool platform. 

The token has a lifetime, but within its original lifetime it can be renewed and a new token will be created. Once the token expires, the user must re-authenticate to get a new token. It is likely that our token lifetime will be about 30 days; thus, if a user fails to run the app for a month, the token will expire and the user will need to re-authenticate. 

Web apps should offer users a "remember me" checkbox that will store the token, and if the user deselects this checkbox, the token should not be stored.

## Storing data

Many apps will offer some mechanism to store data (if not medical data, perhaps notes); Tidepool's storage API is quite simple -- an app will POST one or more data points conforming to Tidepool specs to the standard upload endpoint.

## Querying data

Apps that need to display data will execute queries in TQL, the Tidepool Query Language. Similar to the operation of SQL, an app will compose a query, POST the query to an endpoint, and get back a block of JSON data with the query results. 

User tokens have a rate limit. App keys also have a rate limit. These limits control the amount of data that can be retrieved in a given amount of time. Our goal is to be reasonable with rate limits but to still be able to keep badly-behaving applications from damaging our system. If you find that the rate limits are too low, please talk to us and we'll see what we can do. 

Researchers will have additional capabilities in TQL that allow queries across multiple users -- but they will also have greater restrictions. The research key may disallow access to certain fields.

