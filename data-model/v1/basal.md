# Basals

Basal events represent a basal dosing over time.  At a high level they are a timestamp for when the dosing started, an amount of insulin given in units/h, and a duration in milliseconds.  The stream of basals represents all of the changes in basal rate over a given timespan.  Each basal rate change event that is received indicates that the previous state has completed and the new state represented by the event received is now active.

To this end, each basal event should completely encapsulate the full story of basal delivery for the pump.  You will notice that a temp overriding a scheduled basal actually contains the scheduled basal as a "suppressed" field on the temp basal event.  Similarly, the "previous" basal rate can optionally (but should always) be included.  Tidepool is able to use this previous field to verify the current state of the basal data before effecting the change.

There are two general types of basal events that tidepool understands:

* scheduled
* temp

Note, some of the field descriptions refer to common fields, those are available on the [base v1 page](../v1.md)

### Scheduled

This is a "scheduled" basal, it is a basal dosing that is operating according to the schedule built in to the pump.  This event indicates that the given basal rate should have happened if the schedule were to be performed and should always be emitted *even if a temp basal is in effect*.

``` json
{
  "type": "basal",
  "deliveryType": "scheduled",
  "scheduleName": name_of_schedule_from_settings,
  "value": number_of_units_per_hour,
  "duration": number_of_milliseconds_this_basal_rate_will_be_in_effect,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": the_basal_event_that_would_have_been_previously_received,
  "suppressed": [ basal_events_not_being_delivered_because_this_one_is_active, ... ]
}
```

There are 3 fields worth further explanation here:

1. `duration`: This is advisory, meaning that it is used in order to provide a "TTL" (or Time To Live) on the basal rate.  We need a TTL on basal rates in order to limit the basal in the case that the pump were to be immediately destroyed and never be able to deliver another message.  Without the TTL, we wouldn't actually be able to tell the difference between the case that (a) someone is on a fixed basal rate and never generates events from their pump from (b) a pump that is destroyed and will never give us information again.  
    If we receive another which terminates this basal early, Tidepool will update the `duration` field to represent the actual duration and will store a new `expectedDuration` field to represent the original duration that was given.

2. `previous`: This is the previous basal rate that was in effect by the pump before it changed.  This field is used by our systems to verify that we have a continuous stream of basal rate changes.  If this does not line up with what we believe to be the previously active basal, we will annotate the previously active basal with such information before instantiating the received event as the current basal rate.

3. `suppressed`: If this scheduled basal is causing some other basal rate to not be delivered, that basal rate (or rates) should be included in the array of suppressed basals.  We do not expect scheduled basals to override other basals, so in the vast majority of cases, this will probably be empty or non-existant, but we include it here because the system can accept it if it makes sense for the pump to generate it.

### Temp

Temp basals are much the same as scheduled basals:

``` json
{
  "type": "basal",
  "deliveryType": "temp",
  "value": number_of_units_per_hour,
  "percent": floating_point_percentage_of_suppressed_basal_that_should_be_delivered
  "duration": number_of_milliseconds_the_temporary_basal_will_be_in_effect,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "previous": the_basal_event_that_would_have_been_previously_received,
  "suppressed": [ basal_events_not_being_delivered_because_this_one_is_active, ... ]
}
```

These fields are basically the same as those from a scheduled basal, except for the introduction of the `percent` field.  If `percent` is provided and `value` is null, our systems will compute the value to be the given percentage of the the value of the first supressed event.

With temps, it is relatively common that the previous field is the exact same as the first element of the suppressed field.