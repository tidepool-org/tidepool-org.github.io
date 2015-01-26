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
  "byUser": "userId_of_the_uploading user",
  "deviceTags": ["tag_relating_to_device_features", "..."],
  "deviceManufacturers": ["name_of_manufacturer", "..."],
  "deviceModel": "mfr_device_model",
  "deviceSerialNumber": "serial_number",
  "deviceId": "see_common_fields",
  "createdTime": "see_common_fields",
  "modifiedTime": "see_common_fields"
}
~~~

### timezone

The timezone is the *name* of the timezone selected by the user for the upload. The timezoneOffset as used in individual records is not sufficient information to present to someone who wants to edit the data, because at different times of the year, the offset can change -- it may also be the case that the offset is different between different records in a single upload.

### version

This is the version of the uploader software (whether Tidepool's or someone else's) that was used to parse the data for the upload session, e.g. `"version":"chrome uploader v0.9.0"`. The intention is that then if there is a bug with a particular version of software it will be possible to identify all the data that was processed with that version.

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

## device information

For all of the fields under device information, we are trying to provide a basic taxonomy and standards so that it will be easy to query for specific device types or classes.

Thus, if you are storing information in the Tidepool platform and are unsure of what information should go here, please contact us so we can help you and maintain a high standard of data quality.

### deviceTags
This is a list of broad attributes that can be applied to the device. It is an array of individual string values. Please use exactly the attributes below if they apply, in addition to any others that are appropriate:

* `"insulin-pump"` (for insulin pumps)
* `"cgm"` (for continuous glucose monitors)
* `"bgm"` (for blood glucose meters that are not continuous meters)
* `"fgm"` (for 'flash' glucose meters that are not continuous meters but able to read on demand)
* `"pen"` (for insulin injection devices that record the doses)
* `"manual"` (for manually entered data)

Integrated devices or multifunction devices should use multiple tags as appropriate. For example, the Animas Vibe combo insulin delivery and CGM system would have tags `insulin-pump` and `cgm`. For devices that do not fit in these categories, please contact Tidepool so we can standardize on a good tag.

### deviceManufacturers

This is the name or names of the device manufacturer(s), as an array. It may be more than one for devices that are joint projects of multiple organizations; again, the Animas Vibe would have `["Animas", "Dexcom"]` here. We are trying to standardize this field, so please use exactly the following names:

    Abbott
    Animas
    Asante
    Dexcom
    Insulet
    Medtronic
    LifeScan

Again, for new devices please contact Tidepool so we can standardize on a name.

### deviceModel

This is the name the manufacturer uses to identify the device. Examples might be:

    MiniMed 530G
    G4 Platinum
    FreeStyle Precision Xtra

### deviceSerialNumber

If the serial number of the device is available, put it here as a string, even if it's entirely numeric. If the serial number is not available, store it as a blank string.


### deviceId

The deviceId should be a string that is sufficient to uniquely identify the device and which will be reused every time data is uploaded from this device. Since the deviceId shows up in every data element, it probably wants to be on the shorter side, so we suggest something like the first few characters of the name of the device, followed by the serial number. For example: `DexG4-1123449542`.



## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.