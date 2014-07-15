---
layout: defaults
title: Boluses V1
published: true
---
# Boluses

Bolus events represent a one-time dose of fast-acting insulin.  Even though a bolus is one-time, it is not immediate.  Pumps generally deliver a bolus over the course of minutes/tens of seconds.  It is possible that it is interrupted and it is important to know what was actually delivered.  For that reason, a bolus is actually a multi-event process.  One to indicate the start and subsequent events to indicate completion.

There are multiple types of boluses that the tidepool platform can handle.

* injected
* normal
* square
* dual/square

Note, some of the field descriptions refer to common fields, those are available on the [base v1 page](../v1.html)

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
  "source": see_common_fields
}
~~~


## Normal

A "normal" bolus is a one-time dose of insulin that is all delivered as quickly as the pump is willing/able.  It is the simplest type of bolus, composed of two events:

~~~json 
{
  "type": "bolus",
  "subType": "normal",
  "normal": number_of_units,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
~~~

It is followed up with a "completion" event that looks the exact same as the above event, but has a `previous` field in it:

~~~json 
{
  "type": "bolus",
  "subType": "normal",
  "normal": number_of_units,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": bolus_event_that_is_now_completed
}
~~~

The `previous` field should be the exact same (including timestamp) as the original bolus event received.  If `normal` of the "completion" event is different from `normal` of the "start" event, we will store the `normal` from the "start" event in the `expectedNormal` field and update the `normal` field to be the value from the "completion" event.

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

It is followed up with a "completion" event that looks the exact same as the completion for a "normal" bolus:

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

Which is then followed up with a "completion" event that looks the exact same as the completion for a "square" bolus:

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
