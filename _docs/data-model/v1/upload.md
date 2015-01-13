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
  "version": "version_of_uploader",
  "uploadId": "unique_id",
  "byUser": "userId_of_the_uploading user"
  "createdTime": "see_common_fields",
  "modifiedTime": "see_common_fields",
  "source": "see_common_fields",
  "deviceId": "see_common_fields",
}
~~~

### timezone

The timezone is the *name* of the timezone selected by the user for the upload. The timezoneOffset as used in individual records is not sufficient information to present to someone who wants to edit the data, because at different times of the year, the offset can change -- it may also be the case that the offset is different between different records in a single upload.

### version

This is the version of the uploader that was used to parse the data for the upload session, e.g. `"version":"chrome uploader v0.9.0"`. The intention is that then if we have a bug with a particular version of the uploader we can trace all the data that was processed with that version.

### uploadId

The uploadId is generated as a hash from several values:

* The time of the upload
* The deviceId for the upload data
* The current session token

The point is that the uploadId should change for each device, for each upload session. 

For situations where we are uploading data extracted from a non-device source, such as when we scrape data from third party websites that do not support a data API, the uploadId should likely be consistent for an entire session, even if that session contains data from multiple physical devices.

At some point, we may support real-time monitoring of devices, in which case the upload session may be quite long -- so by using the session token, the uploadId will change periodically as the token is refreshed.

### byUser

The Tidepool platform supports the idea that a user other than the data owner may have upload permissions (for example, a clinic or a parent). The byUser field should be the actual userId of the user doing the upload. This field will be stored in encrypted form in the actual data stream as it constitutes PII.

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.