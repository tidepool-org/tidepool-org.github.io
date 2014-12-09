---
layout: defaults
title: Basals V1
published: true
---
# Basals

Basal events represent a basal dosing over time.  At a high level they are a timestamp for when the dosing started, an amount of insulin given in units/h, and a duration in milliseconds.  The stream of basals represents all of the changes in basal rate over a given timespan.  Each basal rate change event that is received indicates that the previous state has completed and the new state represented by the event received is now active.

To this end, each basal event should completely encapsulate the full story of basal delivery for the pump.  You will notice that a temp overriding a scheduled basal actually contains the scheduled basal as a "suppressed" field on the temp basal event.  Similarly, the "previous" basal rate can optionally (but should always) be included.  Tidepool is able to use this previous field to verify the current state of the basal data before effecting the change.

There are three general types of basal events that tidepool understands:

* injected
* scheduled
* temp
* suspended

Note, some of the field descriptions refer to common fields, those are available on the [base v1 page](..)

### Injected

This represents an injection of a long-acting insulin.

~~~json
{
  "type": "basal",
  "deliveryType": "injected",
  "value": total_number_of_units_injected,
  "duration": number_of_milliseconds_this_injection_is_expected_to_last,
  "insulin": name_of_insulin_used,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
~~~

The fields generally follow the same semantics as other basal events, except

1. ` duration` represents an indication of how long the insulin should last.  With insulin delivered by a pump, this can be overridden by a change in the rate.  With an injection, a new injection will be completely additive to what is currently in the body.  So, handling of injections should be additive with other injections and other forms of delivering insulin.

2. `value` it is worth it to note that the value is the total number of units injected, rather than the amount per hour as is the case with the other basal types

3. `insulin` is a field that exists to provide an indication of what type of insulin was injected.  For example, levemir, lantus, etc.  There is a registry of possible values for this field, submitting a value that is not in this registry will cause the event to be rejected.  It is our hope that this can be used to generate an appoximation of the rate at which the insulin should affect the PwD.

4. `previous` is not specified.  This is because an injection is always additive to whatever other basal activity is going on.

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed

### Scheduled

This is a "scheduled" basal, it is a basal dosing that is operating according to the schedule built in to the pump.

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": name_of_schedule_from_settings,
  "rate": number_of_units_per_hour,
  "duration": number_of_milliseconds_this_basal_rate_will_be_in_effect,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": the_basal_event_that_would_have_been_previously_received,
  "suppressed": basal_events_not_being_delivered_because_this_one_is_active
}
~~~

There are 3 fields worth further explanation here:

1. `duration`: This represents the amount of time that the basal should be in effect.  This event should be emitted when the basal rate takes effect.  It is possible that another basal event will be received before this duration has been fully met (a temp, a change in schedule, etc.).  If we receive another basal that specifies this one as the previous and happens before the end of the duration, the Tidepool platform will update the `duration` field to represent the actual duration and will store a new `expectedDuration` field to represent the original duration that was given.

2. `previous`: This is the previous basal rate that was in effect by the pump before it changed.  It should be the complete event as it was emitted minus the `previous` field.  This field is used by our systems to verify that we have a continuous stream of basal rate changes.  If this does not line up with what we believe to be the previously active basal, we will annotate the previously active basal with such information before instantiating the received event as the current basal rate.

3. `suppressed`: If this scheduled basal is causing some other basal rate to not be delivered, that basal rate should be included in the array of suppressed basals.  This should be another basal event minus the `previous` field.  We do not expect scheduled basals to override other basals, so in the vast majority of cases, this will probably be empty or non-existant, but we include it here because the system can accept it if it makes sense for the pump to generate it.  Note that a suppressed basal also has a suppressed field, so if there is a sequence of suppressions happening, that would be represented as a set of chained suppressed events.

#### Example: Submitting a series of scheduled basals

We start with a basal with no previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 0.7,
  "duration": 10800000,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~

Then, we submit another basal with a previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 1.0,
  "duration": 3600000,
  "time": "2014-01-01T03:00:00.000Z",
  "deviceId": "1234",
  "source": "example",
  "previous": {
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 0.7,
    "duration": 10800000,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
  }
}
~~~

This will result in the Tidepool platform storing

~~~json
[{
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 0.7,
    "duration": 10800000,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
},{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 1.0,
  "duration": 3600000,
  "time": "2014-01-01T03:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}]
~~~

#### Example: Submitting scheduled basals that skip an event

We start with a basal with no previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 0.7,
  "duration": 10800000,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~

Then, we submit another basal with a previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 2.0,
  "duration": 7200000,
  "time": "2014-01-01T04:00:00.000Z",
  "deviceId": "1234",
  "source": "example",
  "previous": {
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 1.0,
    "duration": 3600000,
    "time": "2014-01-01T03:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
  }
}
~~~

This will result in the Tidepool platform storing

~~~json
[{
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 0.7,
    "duration": 10800000,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example",
    "annotations": [{ "code": "basal/mismatched-series" }]
},{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 2.0,
  "duration": 7200000,
  "time": "2014-01-01T04:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}]
~~~

Note that this does *NOT* store the event from the previous, it instead simply marks the previous as "mismatched" and moves on.


#### Example: Submitting scheduled basals that overlap with a previous event

We start with a basal with no previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 1",
  "rate": 0.7,
  "duration": 10800000,
  "time": "2014-01-01T00:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}
~~~

Then, we submit another basal with a previous

~~~json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 2",
  "rate": 0.8,
  "duration":7 14400000,
  "time": "2014-01-01T02:00:00.000Z",
  "deviceId": "1234",
  "source": "example",
  "previous": {
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 0.7,
    "duration": 10800000,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
  }
}
~~~

This will result in the Tidepool platform storing

~~~json
[{
    "type": "basal",
    "deliveryType": "scheduled",
    "scheduleName": "Program 1",
    "rate": 0.7,
    "duration": 7200000,
    "expectedDuration": 14400000,
    "time": "2014-01-01T00:00:00.000Z",
    "deviceId": "1234",
    "source": "example"
},{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": "Program 2",
  "rate": 0.8,
  "duration": 14400000,
  "time": "2014-01-01T02:00:00.000Z",
  "deviceId": "1234",
  "source": "example"
}]
~~~

Note that the duration on the initial basal event was updated to reflect the actual duration before the basal rate was changed.  Also, note that there is no annotation on the event, because the previous event lined up.


### Suspend

Suspended basals are much the same as scheduled and temp basals:

~~~json
    {
      "type": "basal",
      "deliveryType": "suspend",
      "rate": 0.0,
      "duration": number_of_milliseconds_the_suspend_will_be_in_effect_if_known,
      "time": see_common_fields,
      "deviceId": see_common_fields,
      "source": see_common_fields,
      "previous": the_basal_event_that_was_have_been_previously_received,
      "suppressed": basal_events_not_being_delivered_because_this_one_is_active
    }
~~~

These fields are a subset of those from a scheduled basal.  The `rate` is always 0.0.  The `duration` is optional, but *should* be provided if there is some mechanism that will turn the pump back on automatically after some elapsed period of time.

The `suppressed` field represents any basals that this suspend event is suppressing.  The pump should make an effort to try to emit events whenever there is a change in the actual basal that would have been delivered if the suspension weren't in operation.  This may not always be possible, however, and in cases where it is not possible, it is acceptable to send a suspended event without a `suppressed` field.

### Temp

Temp basals are much the same as scheduled basals:

~~~json
    {
      "type": "basal",
      "deliveryType": "temp",
      "rate": number_of_units_per_hour,
      "percent": floating_point_percentage_of_suppressed_basal_that_should_be_delivered
      "duration": number_of_milliseconds_the_temporary_basal_will_be_in_effect,
      "time": see_common_fields,
      "deviceId": see_common_fields,
      "source": see_common_fields,
      "previous": the_basal_event_that_would_have_been_previously_received,
      "suppressed": basal_events_not_being_delivered_because_this_one_is_active
    }
~~~

These fields are basically the same as those from a scheduled basal, except for the introduction of the `percent` field.  If `percent` is provided and `value` is null, our systems will compute the value to be the given percentage of the the value of the first supressed event.

With temps, it is relatively common that the previous field is the exact same as the first element of the suppressed field.


#### Storage/Output Format

The storage and output format of temp basals is essentially the fields defined above with the same adjustments as described for scheduled events.  That is, if a subsequent scheduled event, S, comes in before the stated duration of a temp basal, T, is completed, and S has a `previous` field equivalent to T, the duration of the temp T will be updated to reflect the time at which S entered the platform.
