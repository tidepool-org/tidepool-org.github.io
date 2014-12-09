---
layout: defaults
title: Upload V1
published: true
---
# Upload

Upload represents the metadata associated with an upload of information to the Tidepool platform. Every device that adds data to the platform should expect to create an upload record.

An upload record includes information on the time and provenance of the upload and includes a unique identifier that is used to decorate every data item associated with a given upload.


## Object
These events are point-in-time and look like


~~~json
{
  "type": "upload",
  "time": "ISO8601_timestamp",
  "timezone": "name_of_timezone",
  "timezoneOffset": "offset_in_minutes",
  "uploadId": "unique_id",
  "byUser": "userId_of_the_uploading user"
  "deviceTime": "see_common_fields",
  "createdTime": "see_common_fields",
  "modifiedTime": "see_common_fields",
  "source": "see_common_fields",
  "deviceId": "see_common_fields",
}
~~~

### timezone

The timezone is the *name* of the timezone selected by the user for the upload. The offset alone is not sufficient information to present to someone who wants to edit the data, because at different times of the year, the offset can change.

### uploadId

The uploadId is generated as a hash from several values:

* The time of the upload
* The deviceId for the upload data
* The current session token

The point is that the uploadId should change for each device, for each upload session. At some point, we may support real-time monitoring of devices, in which case the upload session may be quite long -- so by using the session token, the uploadId will change periodically as the token is refreshed.

### byUser

The Tidepool platform supports the idea that a user other than the data owner may have upload permissions (for example, a clinic or a parent). The byUser field should be the actual userId of the user doing the upload. This field will be stored in encrypted form in the actual data stream as it constitutes PII.

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.