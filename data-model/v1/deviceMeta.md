---
layout: defaults
title: DeviceMeta V1
published: true
---
# DeviceMeta

DeviceMeta events are a catch-all for metadata about whatever device is currently being used.  We expect the domain of deviceMeta events to expand regularly as we want to track more and more things about the device-proper.

The "type" of a deviceMeta event is defined by the `subType` field.

Current DeviceMeta events are

* status
* calibration

## Status

A status event is used to represent the status of a pump.  Specifically, this is used to indicate when the pump is suspended/resumed for any reason (user suspend, low glucose suspend, etc).  A status event looks like


~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": suspended_or_resumed,
  "reason": indication_of_why
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": previously_active_status_event
}
~~~

`status` is the new status, it can be "suspended" or "resumed".

`reason` is an indication of why the status was changed.  It can be:

* For suspended status
    * "manual" - the user manually suspended the pump
    * "low_glucose" - the pump automatically suspended due to low blood glucose
    * "alarm" - the pump automatically suspended due to an alarm
* For resumed status
    * "manual" - the user manually resumed the pump
    * "automatic" - the pump resumed on its own

If the status from the `previous` field does not exist in the Tidepool platform, the object provided in the previous field will be annotated and saved along with the current event.

### Storage/Output Format

The storage and output format for status events is essentially a mirror except the `previous` field will not actually be stored.  Instead, it is used simply as a verification mechanism.

#### Example: Event submitted with previous that exists

If you were to first emit the event

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "resumed",
  "reason": "manual",
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "123",
  "source": "example"
}
~~~

And then emit

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "suspended",
  "reason": "manual",
  "time": "2014-01-01T01:00:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceMeta",
    "subType": "status",
    "status": "resumed",
    "reason": "manual",
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
}
~~~

The tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "resumed",
    "reason": "manual",
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  },
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": "manual",
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
]
~~~

#### Example: Event submitted with previous that does *NOT* exist

If you were to first emit the event

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "resumed",
  "reason": "manual",
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "123",
  "source": "example"
}
~~~

And then emit

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "resumed",
  "reason": "manual",
  "time": "2014-01-01T02:00:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": "manual",
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
}
~~~

The tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "resumed",
    "reason": "manual",
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  },
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": "manual",
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "resumed",
    "reason": "manual",
    "time": "2014-01-01T02:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
]
~~~

That is, it will *generate* the previous event as if it had been originall sent in.

## Calibration

A calibration event represents a calibration of a CGM.  It looks like

~~~json
{
  "type": "deviceMeta",
  "subType": "calibration",
  "value": bg_value_for_calibration,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
~~~

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed