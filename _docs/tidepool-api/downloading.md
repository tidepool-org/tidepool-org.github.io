---
layout: defaults
title: Tidepool API - Downloading
published: true
---

# Tidepool API: Downloading Health Data

For other parts of the API, including logging in and uploading data, navigate back or to the [API walkthrough](/tidepool-api/index). 


## Health data

### Get data

Health data can be downloaded through Tidepool's /data/ API. This API takes a user ID, and optionally start and end timestamps, data type, and data subtype. The userId indicates the user for which data is to be downloaded. If no optional parameters are included, all data is returned. If startdate and/or enddate are included, then only objects within the date range are returned. If type or subtype are included, then only objects matching those types or subtypes are returned. Matching data is returned as an array of Tidepool objects. The format for the requests is:

```
GET /data/:userId?startdate=&enddate=&type=&subtype=
x-tidepool-session-token: <token>

```
Parameter details:
* userId: the ID of the user you want to retrieve data for
* type (optional) : The Tidepool data type to search for. Only objects with a type field matching the specified type param will be returned. Can be /userid?type=smbg or a comma seperated list e.g /userid?type=smgb,cbg . If is a comma seperated 
list, then objects matching any of the types will be returned
* subtype (optional) : The Tidepool data subtype to search for. Only objects with a subtype field matching the specified subtype param will be returned. Can be /userid?subtype=physicalactivity or a comma seperated list e.g /userid?subtypetype=physicalactivity,steps . If it is a comma seperated  list, then objects matching any of the subtypes will be returned
* startdate (optional) : Only objects with 'time' field equal to or greater than start date will be returned. Must be in ISO date/time format e.g. 2015-10-10T15:00:00.000Z
* enddate (optional) : Only objects with 'time' field less than to or equal to start date will be returned. Must be in ISO date/time format e.g. 2015-10-10T15:00:00.000Z 

Example:
```
GET /data/450f582c78?startdate=2015-10-08T15:00:00.000Z&enddate=2015-10-11T15:00:00.000Z&type=smbg
```
Response:
```
[
{
    "conversionOffset": 0,
    "deviceId": "postman1",
    "deviceTime": "2015-10-10T11:00:00",
    "id": "p3uac1etornvvsgitlvddaaks6t18k1a",
    "time": "2015-10-10T15:00:00.000Z",
    "timezoneOffset": -240,
    "type": "smbg",
    "units": "mmol/L",
    "uploadId": "0001",
    "value": 6.66089758925464
  },
  {...},
  {...}
]
```


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
