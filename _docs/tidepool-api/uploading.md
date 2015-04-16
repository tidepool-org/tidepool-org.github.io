---
layout: defaults
title: Tidepool API - Uploading
published: true
---

# Uploading

For other parts of the API, including logging in and downloading data, navigate back or to the [API walkthrough](/tidepool-api/index). 

## Model

Applications can upload user health data to the Tidepool platform by logging in as the user whose health data is being accessed, or by a user with upload permissions (see [Sharing](/tidepool-api/sharing)).

## Post data

To POST data to a given user based on group permissions, name the groupID.  One data point or an array of data points can be handled in the same POST request. 

Note to developers: this request is currently handled by the [Jellyfish backend](https://github.com/tidepool-org/jellyfish/blob/master/lib/jellyfishService.js).

```
POST /data/:groupId
x-tidepool-session-token: <token>
Content-Type: application/json

{ [
  { "type": "basal",
    ... rest of datum 
  }
]  }
```

One can also send data to the current logged-in user by substituting '/data/' for the URL in the above request. 

**Success Response**

```
200 OK 

<number of duplicates found in upload>
```

**Error Responses**

**400 Client Error**: The request data has an unknown data type or other data format error

**403 Forbidden**:  The logged-in user does not have permissions to upload data to the named location.  

**500 Server Error**: A server-side error occurred.


## Carelink Data

```
POST /v1/device/upload/cl?group
x-tidepool-session-token: <token>
```

## Overwriting

As of April 13 2015, the uploading API does not allow clients to overwrite or delete data points.  Overwriting (e.g. to fix an error in adjusting timezones) is in the works. 