---
layout: defaults
title: DeviceEvent V1
published: true
---
# DeviceEvent

`deviceEvent` events are a catch-all for device metadata and events that do not fall into the other types. We expect the domain of deviceEvent events to expand regularly as we want to track more and more things about the device proper.

The "type" of a deviceEvent event is defined by the `subType` field.

Current deviceEvent events are

* alarm
* prime
* reservoirChange
* status
* calibration
* timeChange

## Alarm

An alarm event is used to represent a user-facing alarm from an insulin pump. An alarm event looks like

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "alarm",
  "alarmType": "type_of_alarm_or_other",
  "payload": "see_common_fields",
  "status": "optional_status_object"
}
~~~

The `alarmType`s built into the data model are all and only those alarms that are common to most, if not all, insulin pumps. Currently this list is:

- `low_insulin` for low reservoir/cartridge alarms
- `no_insulin` for empty reservoir/cartridge alarms
- `low_power` for low battery alarms
- `no_power` for out of battery alarms
- `occlusion` for occlusion (blockage of infusion set) alarms
- `no_delivery` for no insulin delivery alerts not due to occlusion
- `auto_off` for when the pump stops all delivery due to inactivity for a period longer than the user's programmed threshold (if any)
- `over_limit` for when insulin delivery has surpassed any of a user's programmed maximum bolus, basal, or hourly delivery thresholds

Many if not all `alarm` events will include a `payload` object with more information about the alarm that is device-specific. For example, a `low_insulin` alarm may have a `units_left` field in its `payload` to record the number of units of insulin that were remaining in the insulin pump's reservoir at the time of the alarm.

In addition, a `payload` object is *required* when `alarmType` is `other`; this value is used to capture all alarms that are device-specific. For example, a pod expiration alarm is specific to the Insulet OmniPod insulin delivery system, and a 'pump body disconnected' alarm is specific to an Asante Snap insulin pump. The `payload` object should include all information that could be relevant to anyone wishing to audit the history and performance of the insulin pump in question.

For some devices, an alarm event (e.g., an occlusion alarm) is the only indication of a suspension of insulin delivery. In such a case, a `status` event should also be uploaded to the platform and should be included (in its entirety) in the `status` field of the `alarm` event.

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.

## Prime

A prime event represents the priming of either an infusion line (tubing) or an infusion cannula. It looks like

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "prime",
  "primeTarget": "tubing_or_cannula",
  "volume": "optional_units_delivered"
}
~~~

`primeTarget` can have two values depending on the object of the priming action - either `tubing` for an infusion line prime or `cannula` for a cannula prime.

The `volume` field is optional. It should be included if the data specifies the volume of insulin delivered in the course of the priming.

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.

## Reservoir Change

A reservoir change event represents any event in an insulin delivery system that implies a return to a device state not yet ready to deliver insulin. This varies depending on the type of insulin delivery device. For conventional syringe-type insulin pumps, this will be a rewind event. For an Insulet OmniPod system, it is a pod deactivation event. This event often implies a suspension of insulin delivery; in the case that the device data includes a reset event but does not include a separate indication of insulin delivery suspension, a `status` event should also be uploaded to the platform and should be included (in its entirety) in the `status` field.

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "reservoirChange",
  "payload": "see_common_fields",
  "status": "optional_status_object"
}
~~~

The `payload` object may be included in order to expose the specifics of the event type that is being interpreted more generally as a `reset`, along with any other relevant device-specific information.

### Storage/Output Format

If the `status` field of a reset event is present as an object, it is reduced to its ID hash upon upload. There are no other modifications performed.

## Status

A status event is used to represent the status of a pump.  Specifically, this is used to indicate when the pump is suspended/resumed for any reason (user suspend, low glucose suspend, etc).  A status event looks like


~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "status",
  "status": "suspended_or_resumed",
  "reason": "indication_of_why",
  "duration": "max_duration_of_status_if_known",
  "payload": "see_common_fields",
  "previous": "previously_active_status_event"
}
~~~

`status` is the new status, it can be "suspended" or "resumed".

`reason` is an indication of why the status was changed; it is an object with fields for either or both of `suspended` and `resumed` because the causes for each of these status changes may differ. For both `suspended` and `resumed` statuses, `reason` is a strictly binary distinction between `manual` (caused by direct action of the user) and `automatic` (all cases not involving direct user intervention), as a more nuanced taxonomy allows too much room for interpretation around the edge cases. However, a third option `unknown` is available when the device data does not include enough information to determine whether a suspension or resumption was caused by direct user intervention or not. (This is the case, for example, with Insulet OmniPod pod deactivations, which can be manual or automatic, but the deactivation records themselves do not include the cause of the deactivation.)

The `payload` object is expected to contain more information about the cause of the suspension or resumption where it is available. For example, for MiniMed 530G insulin pumps with low-glucose suspend, there is a distinction between circumstances in which the pump resumes from a low-glucose suspend automatically after two hours depending on whether the user interacted with any of the LGS alarms during the duration of the suspend. We represent this distinction in the payload as follows:

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "status",
  "status": "suspended",
  "reason": {
    "suspended": "automatic",
    "resumed": "automatic"
  },
  "duration": 7200000,
  "payload": {
    "suspended": {
      "cause": "low_glucose",
      "threshold": 80
    },
    "resumed": {
      "cause": "timed_out",
      "user_intervention": "ignored"
    }
  }
  ...
}
~~~

Status events come in tuples delivered over time. The first event in the tuple must be a non "resumed" status, subsequent statuses can be any other status change until a "resumed" status is received.  The "resumed" event closes the tuple. As each event in the tuple is received the system will update the duration of the previous event in the tuple according to the differences in the timestamps.

Also, the final status event in an incomplete tuple will be annotated as such. When the tuple is complete, the server will remove the annotation. Said another way, if everything is in order, "resumed" events will not be stored, but instead it will be used to adjust the `duration` of the status event that came before them.

In the unlikely event that the event specified by the `previous` field does not exist in the Tidepool platform, the submitted event will be stored, but it will be annotated with "status/unknown-previous" to indicate the condition. If there is no `previous` field specified on a "resumed" event, that will also result in this annotation being added.

### Storage/Output Format

The storage and output format for status events differs from what was initially ingested only with respect to the `reason` field for `status`-type events. (Also, the `previous` field will not actually be stored. Instead, it is used simply as a verfication mechanism.) The storage and output format combines the `reason` codes for each of the events making up a `status` tuple. In all cases, the `reason` field is an object with keys that corresponded to the `status` (i.e., `suspended` or `resumed`) and values chosen from the sets of appropriate reason codes for each `status`.

The storage and output format for status events is essentially a mirror except the `previous` field will not actually be stored. Instead, it is used simply as a verification mechanism.

#### Example: Event submitted with previous that exists

If you were to first emit the event

~~~json
{
  "type": "deviceEvent",
  "subType": "status",
  "status": "suspended",
  "reason": {
    "suspended": "manual"
  },
  "time": "2014-01-01T01:00:00.000Z",
  "deviceId": "123",
  "source": "example"
}
~~~

At this point, the Tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceEvent",
    "subType": "status",
    "status": "suspended",
    "reason": {
      "suspended": "manual"
    },
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example",
    "annotations": [
      {
        "code": "status/incomplete-tuple"
      }
    ]
  }
]
~~~

If you then emit

~~~json
{
  "type": "deviceEvent",
  "subType": "status",
  "status": "resumed",
  "reason": {
    "resumed": "manual"
  },
  "time": "2014-01-01T01:30:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceEvent",
    "subType": "status",
    "status": "suspended",
    "reason": {
      "suspended": "manual"
    },
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
}
~~~

The Tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceEvent",
    "subType": "status",
    "status": "suspended",
    "reason": {
      "suspended": "manual",
      "resumed": "manual"
    },
    "time": "2014-01-01T01:00:00.000Z",
    "duration": 1800000,
    "deviceId": "123",
    "source": "example"
  }
]
~~~

#### Example: Event submitted with previous that does *NOT* exist

If you were to first emit the event

~~~json
{
  "type": "deviceEvent",
  "subType": "status",
  "status": "suspended",
  "reason": {
    "resumed": "manual"
  },
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "123",
  "source": "example"
}
~~~

And then emit

~~~json
{
  "type": "deviceEvent",
  "subType": "status",
  "status": "resumed",
  "reason": {
    "resumed": "manual"
  },
  "time": "2014-01-01T02:00:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceEvent",
    "subType": "status",
    "status": "suspended",
    "reason": {
      "suspended": "manual"
    },
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example"
  }
}
~~~

The Tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceEvent",
    "subType": "status",
    "status": "suspended",
    "reason": {
      "suspended": "manual"
    },
    "time": "2014-01-00T00:00:00.000Z",
    "deviceId": "123",
    "source": "example",
    "annotations": [
      {
        "code": "status/incomplete-tuple"
      }
    ]
  },
  {
    "type": "deviceEvent",
    "subType": "status",
    "status": "resumed",
    "reason": {
      "resumed": "manual"
    },
    "time": "2014-01-01T02:00:00.000Z",
    "deviceId": "123",
    "source": "example",
    "annotations": [
      {
        "code": "status/unknown-previous",
        "id": "1234-expected-id-of-previous-abcd"
      }
    ]
  }
]
~~~

### Storage/Output Format

See examples above for the storage and output format for this data type and note that `previous` is never stored.

## Calibration

A calibration event represents a calibration of a CGM.  It looks like

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "inputUnits": "see_common_fields",
  "subType": "calibration",
  "value": "bg_value_for_calibration"
}
~~~

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested, minus the `inputUnits`, which are *never* stored. There are no modifications performed.

## Time Change

A time change event represents a change to the device's date and/or time settings. It looks like

~~~json
{
  "type": "deviceEvent",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "timeChange",
  "change": {
    "from": "local_device_time_before_time_change",
    "to": "local_device_time_after_time_change",
    "agent": "manual_or_automatic",
    "timezone": "optional_(see_below)",
    "reasons": "optional_array_of_reason_codes_(see_below)"
  },
  "payload": "see_common_fields"
}
~~~

A time change concerns the local time displayed on a device. The date and time displayed on the device just before the change is made is stored in the `from` field of the `change` attribute object, and the date and time resulting from the change is stored in the `to` field. Both date and time objects are formatted as ISO8601 dates without any offset from UTC specified - i.e., `YYYY-MM-DDThh:mm:ss`.

The `agent` field within `change` describes the source of the change - either `manual` (the user) or `automatic` (the device). This field is required. At present, we know of no devices that include a timezone in the device settings and automatically change to and from daylight savings time, but we expect that this may happen in the future as Bluetooth-communicating devices enter the market.

The `change` object may also optionally contain the name of the timezone that is to be understood as applying to all times at and after the device time in the `to` field. One or several `reasons` codes may also be applied as an array to explain why a change to the device's date and time settings was necessary. Possible values at present are:

- `from_daylight_savings` (fall back)
- `to_daylight_savings` (spring forward)
- `travel`
- `correction` for corrections to device clock "drift" or larger corrections (such as realizing a device was set to 7 a.m. instead of 7 p.m., etc.)
- `other`

`reasons` is an array because it is quite conceivable that more than one code will apply to a singular date and time change - for example, an adjustment for clock "drift" may often co-occur with a switch to or from daylight savings time.

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.
