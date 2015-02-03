---
layout: defaults
title: DeviceMeta V1
published: true
---
# DeviceMeta

DeviceMeta events are a catch-all for metadata about whatever device is currently being used.  We expect the domain of deviceMeta events to expand regularly as we want to track more and more things about the device proper.

The "type" of a deviceMeta event is defined by the `subType` field.

Current DeviceMeta events are

* alarm
* prime
* deliveryReset
* status
* calibration

## Alarm

An alarm event is used to represent a user-facing alarm from an insulin pump. An alarm event looks like

~~~json
{
  "type": "deviceMeta",
  "subType": "alarm",
  "alarmType": "type_of_alarm_or_other",
  "payload": "see_common_fields",
  "status": "optional_status_object",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields"
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
  "type": "deviceMeta",
  "subType": "prime",
  "primeTarget": "tubing_or_cannula",
  "volume": "optional_units_delivered",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields"
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
  "type": "deviceMeta",
  "subType": "reservoirChange",
  "payload": "see_common_fields",
  "status": "optional_status_object",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields"
}
~~~

The `payload` object may be included in order to expose the specifics of the event type that is being interpreted more generally as a `reset`, along with any other relevant device-specific information.

### Storage/Output Format

If the `status` field of a reset event is present as an object, it is reduced to its ID hash upon upload. There are no other modifications performed.

## Status

A status event is used to represent the status of a pump.  Specifically, this is used to indicate when the pump is suspended/resumed for any reason (user suspend, low glucose suspend, etc).  A status event looks like


~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "suspended_or_resumed",
  "reason": "indication_of_why",
  "duration": "max_duration_of_status_if_known",
  "payload": "see_common_fields",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "previous": "previously_active_status_event"
}
~~~

`status` is the new status, it can be "suspended" or "resumed".

`reason` is an indication of why the status was changed; it is an object with fields for either or both of `suspended` and `resumed` because the causes for each of these status changes may differ. For both `suspended` and `resumed` statuses, `reason` is a strictly binary distinction between `manual` (caused by direct action of the user) and `automatic` (all cases not involving direct user intervention), as a more nuanced taxonomy allows too much room for interpretation around the edge cases. However, a third option `unknown` is available when the device data does not include enough information to determine whether a suspension or resumption was caused by direct user intervention or not. (This is the case, for example, with Insulet OmniPod pod deactivations, which can be manual or automatic, but the deactivation records themselves do not include the cause of the deactivation.)

The `payload` object is expected to contain more information about the cause of the suspension or resumption where it is available. For example, for MiniMed 530G insulin pumps with low-glucose suspend, there is a distinction between circumstances in which the pump resumes from a low-glucose suspend automatically after two hours depending on whether the user interacted with any of the LGS alarms during the duration of the suspend. We represent this distinction in the payload as follows:

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "suspended",
  "reason": {
    "suspended": "automatic",
    "resumed": "automatic"
  },
  "time": "see_common_fields",
  "duration": 7200000,
  "payload": {
    "suspended": {
      "cause": "low_glucose",
      "threshold": 80,
    },
    "resumed": {
      "cause": "timed_out",
      "user_intervention": "ignored"
    }
   }
   ...
}
~~~

Status events come in tuples delivered over time.  The first event in the tuple must be a non "resumed" status, subsequent statuses can be any other status change until a "resumed" status is received.  The "resumed" event closes the tuple.  As each event in the tuple is received the system will update the duration of the previous event in the tuple according to the differences in the timestamps.

Also, the final status event in an incomplete tuple will be annotated as such.  When the tuple is complete, the server will remove the annotation.  Said another way, if everything is in order, "resumed" events will not be stored, but instead it will be used to adjust the `duration` of the status event that came before them.

In the unlikely event that the event specified by the `previous` field does not exist in the Tidepool platform, the submitted event will be stored, but it will be annotated with "status/unknown-previous" to indicate the condition.  If there is no `previous` field specified on a "resumed" event, that will also result in this annotation being added.

### Storage/Output Format

The storage and output format for status events differs from what was initially ingested only with respect to the `reason` field for `status`-type events. (Also, the `previous` field will not actually be stored. Instead, it is used simply as a verfication mechanism.) The storage and output format combines the `reason` codes for each of the events making up a `status` tuple. In all cases, the `reason` field is an object with keys that corresponded to the `status` (i.e., `suspended` or `resumed`) and values chosen from the sets of appropriate reason codes for each `status`.

The storage and output format for status events is essentially a mirror except the `previous` field will not actually be stored.  Instead, it is used simply as a verification mechanism.

#### Example: Event submitted with previous that exists

If you were to first emit the event

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "suspended",
  "reason": {"suspended": "manual"},
  "time": "2014-01-01T01:00:00.000Z",
  "deviceId": "123",
  "source": "example"
}
~~~

At this point, the tidepool platform will store and allow you to retrieve

~~~json
[
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": {"suspended": "manual"},
    "time": "2014-01-01T01:00:00.000Z",
    "deviceId": "123",
    "source": "example"
    "annotations": [{ "code": "status/incomplete-tuple" }]
  }
]
~~~

If you then emit

~~~json
{
  "type": "deviceMeta",
  "subType": "status",
  "status": "resumed",
  "reason": {"resumed": "manual"},
  "time": "2014-01-01T01:30:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": {"suspended": "manual"},
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
    "status": "suspended",
    "reason": {"suspended": "manual", "resumed": "manual"},
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
  "type": "deviceMeta",
  "subType": "status",
  "status": "suspended",
  "reason": {"resumed": "manual"},
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
  "reason": {"resumed": "manual"},
  "time": "2014-01-01T02:00:00.000Z",
  "deviceId": "123",
  "source": "example",
  "previous": {
    "type": "deviceMeta",
    "subType": "status",
    "status": "suspended",
    "reason": {"suspended": "manual"},
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
    "status": "suspended",
    "reason": {"suspended": "manual"},
    "time": "2014-01-00T00:00:00.000Z",
    "deviceId": "123",
    "source": "example",
    "annotations": [{ "code": "status/incomplete-tuple" }]
  },
  {
    "type": "deviceMeta",
    "subType": "status",
    "status": "resumed",
    "reason": {"resumed": "manual"},
    "time": "2014-01-01T02:00:00.000Z",
    "deviceId": "123",
    "source": "example",
    "annotations": [{ "code": "status/unknown-previous", "id": "1234-expected-id-of-previous-abcd" }]
  }
]
~~~

### Storage/Output Format

See examples above for the storage and output format for this data type and note that `previous` is never stored.

## Calibration

A calibration event represents a calibration of a CGM.  It looks like

~~~json
{
  "type": "deviceMeta",
  "subType": "calibration",
  "value": "bg_value_for_calibration",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields"
}
~~~

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.