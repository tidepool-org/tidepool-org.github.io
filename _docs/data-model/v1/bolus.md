---
layout: defaults
title: Boluses V1
published: true
---
# Boluses

Bolus events represent a one-time dose of fast-acting insulin.  Even though a bolus is one-time, it is not immediate.  Pumps generally deliver a bolus over the course of minutes/tens of seconds.  It is possible that it is interrupted and it is important to know what was actually delivered.  For that reason, a bolus can be a multi-event process. One event indicates the start and subsequent events indicate completion.

There are multiple types of boluses that the tidepool platform can handle.

* injected
* normal
* square
* dual/square

Note, some of the field descriptions refer to common fields, those are available on the [base v1 page](..)

## Injected

An "injected" bolus is a bolus that was delivered via injection.  It is a single, point-in-time event:

~~~json
{
  "type": "bolus",
  "subType": "injected",
  "value": number_of_units_injected,
  "insulin": name_of_insulin_used,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "uploadId": see_common_fields,
  "source": see_common_fields
}
~~~

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed

## Normal

A "normal" bolus is a one-time dose of insulin that is all delivered as quickly as the pump is willing/able.  It is the simplest type of bolus, composed of two events:

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": number_of_units,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "uploadId": see_common_fields,
  "source": see_common_fields
}
~~~

It may be followed by a "completion" event that looks almost the same as the above event, but has

* A different timestamp
* A `previous` field

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": number_of_units,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "uploadId": see_common_fields,
  "source": see_common_fields,
  "previous": bolus_event_that_is_now_completed
}
~~~

The `previous` field should be an exact copy (including timestamp, but uploadId will be ignored) of the original bolus event received.  If `normal` of the "completion" event is different from the `normal` of the "start" event, we will store the `normal` from the "start" event in the `expectedNormal` field of the original event, and update the `normal` field to be the value from the "completion" event.

### Example: Submitting a bolus that succeeded

If you first submit

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 2.0,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "uploadId": "abc123def567",
  "source": "example"
}
~~~

you *may* also subsequently submit

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 2.0,
  "time": "2014-01-01T00:02:00.000Z",
  "deviceId": "1234",
  "uploadId": "ccc111222333",
  "source": "example",
  "previous": {
    "type": "bolus",
    "subType": "normal",
    "normal": 2.0,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
  }
}
~~~

In this case, whether you submit the follow-up event or not, the Tidepool platform will store and return

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 2.0,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~


### Example: Submitting a bolus that was cut short

If you start by submitting

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 2.0,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~

and subsequently submit

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 1.0,
  "time": "2014-01-01T00:02:00.000Z",
  "deviceId": "1234",
  "source": "example",
  "previous": {
    "type": "bolus",
    "subType": "normal",
    "normal": 2.0,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
  }
}
~~~

the follow-up event will update the original datum, changing the value of `normal` and moving the old value to `expectedNormal`.  This modification follows the versioning and update rules described on the main [v1](../v1.html) page.

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": 1.0,
  "expectedNormal": 2.0,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~



## Square

A "square" bolus is a bolus that is delivered over some time duration, somewhat similar to a large, fixed-value temp basal.  It is composed of two events:

~~~json
{
  "type": "bolus",
  "subType": "square",
  "extended": number_of_units,
  "duration": milliseconds_over_which_the_bolus_should_be_delivered,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
~~~

It is followed up with a "completion" event that looks the exact same as the above event, but has a `previous` field in it:

~~~json
{
  "type": "bolus",
  "subType": "square",
  "extended": number_of_units,
  "duration": milliseconds_over_which_the_bolus_was_delivered,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": bolus_event_that_is_now_completed
}
~~~

The caveat about `normal` from the "normal" bolus above also applies, except it is applied to the `extended` field.  If the `duration` of the "start" and "completion" events differ, we will store the "start" duration as `expectedDuration` and the "completion" duration as `duration`.

### Storage/Output Format

`square` boluses follow the same rules for storage and output format as `normal` boluses (see examples above).  Except that instead of "normal" becoming "expectedNormal", for `square` boluses both the "extended" and the "duration" fields can be updated.  When updated, the old values are stored as "expectedExtended" and "expectedDuration", respectively.

## Dual/Square

A "dual/square" bolus is a bolus that starts out with a normal bolus and then continues into a square bolus.  It is composed of 3 events:

~~~json
{
  "type": "bolus",
  "subType": "dual/square",
  "normal": number_of_units,
  "extended": number_of_units_for_extended_delivery,
  "duration": milliseconds_over_which_the_extended_portion_should_be_delivered,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
~~~

It may be followed up with a "completion" event that looks the exact same as the completion for a "normal" bolus:

~~~json
{
  "type": "bolus",
  "subType": "normal",
  "normal": number_of_units
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": initial_dual_square_bolus
}
~~~

Which may also be followed up with a "completion" event that looks the exact same as the completion for a "square" bolus:

~~~json
{
  "type": "bolus",
  "subType": "square",
  "extended": number_of_units,
  "duration": milliseconds_over_which_the_extended_portion_was_delivered,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": bolus_event_that_is_now_completed
}
~~~

With the exact same caveats about differences in values for the `value`, `extended` and `duration` fields.

Note that the completion events are not required.  It is legal to not send a `normal` completion event, but send a `square` completion event in the case that the `normal` bolus successfully completes and the `square` portion gets canceled.  It is also legal to send the completion event for `square` before the completion event for `normal` if both get canceled.

### Storage/Output Format

`dual/square` boluses follow the same rules for storage and output format as `normal` and `square` boluses (see examples above).  It is correct to think of a `dual/square` bolus as effectively the concatenation of a `normal` and a `square` event.  For example, if the `normal` completes "correctly" and the `square` portion is cut short, only the `square` parameters "duration" and "extended" will be updated.
