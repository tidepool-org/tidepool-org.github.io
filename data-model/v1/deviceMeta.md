# DeviceMeta

DeviceMeta events are a catch-all for metadata about whatever device is currently being used.  We expect the domain of deviceMeta events to expand regularly as we want to track more and more things about the device-proper.

The "type" of a deviceMeta event is defined by the `subType` field.

Current DeviceMeta events are

* status
* calibration

## Status

A status event is used to represent the status of a pump.  Specifically, this is used to indicate when the pump is suspended/resumed for any reason (user suspend, low glucose suspend, etc).  A status event looks like


``` json
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
```

`status` is the new status, it can be "suspended" or "resumed".

`reason` is an indication of why the status was changed.  It can be 
* For suspended status
    * "manual" - the user manually suspended the pump
    * "low_glucose" - the pump automatically suspended due to low blood glucose
    * "alarm" - the pump automatically suspended due to an alarm
* For resumed status
    * "manual" - the user manually resumed the pump
    * "automatic" - the pump resumed on its own

If the status from the `previous` field does not match the currently known status in the Tidepool platform, the previous status object will be annotated as such and the current status will be set according to the received event.

## Calibration

A calibration event represents a calibration of a CGM.  It looks like

``` json
{
  "type": "deviceMeta",
  "subType": "calibration",
  "value": bg_value_for_calibration,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
```

