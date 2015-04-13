---
layout: defaults
title: Tidepool API - Downloading
published: true
---

# Tidepool API: Downloading Health Data

For other parts of the API, including logging in and uploading data, navigate back or to the [API walkthrough](/tidepool-api/index). 


## Health data

[Link to format documentation]

### Get data

[For Care Team, for date range]


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
