---
layout: defaults
title: CGM Settings V1
published: true
---
# CGM Settings

A cgmSettings object represents the settings of a continuous glucose monitor. A cbgSettings object sent into our API looks like

~~~
{
  "type": "cgmSettings",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "displayUnits": "see_common_fields",
  "inputUnits": "see_common_fields",
  "transmitterId": "transmitter_ID_as_a_string",
  "lowAlerts": {
    "enabled": "boolean",
    "level": "blood_glucose_in_mmol/L",
    "snooze": "see_below"
  },
  "highAlerts": {
    "enabled": "boolean",
    "level": "blood_glucose_in_mmol/L",
    "snooze": "see_below"
  },
  "rateOfChangeAlerts": {
    "fallRate": {
      "enabled": "boolean",
      "rate": "see_below"
    },
    "riseRate": {
      "enabled": "boolean",
      "rate": "see_below"
    }
  },
  "outOfRangeAlerts": {
    "enabled": "boolean",
    "snooze": "see_below"
  },
  "predictiveAlerts": {
    "lowPrediction": {
      "enabled": "boolean",
      "timeSensitivity": "see_below"
    },
    "highPrediction": {
      "enabled": "boolean",
      "timeSensitivity": "see_below"
    }
  },
  "calibrationAlerts": {
    "preReminder": "see_below",
    "overdueAlert": "see_below"
  },
  "payload": "see_common_fields"
}
~~~

* `snooze` is the amount of time scheduled to pass between repetitions of the alert or alarm, provided in milliseconds.
* `rate` for `fallRate` and `riseRate` is expressed in mmol/L/min. The `rate` inside the `fallRate` is required to be negative.
* `timeSensitivity` for predicate alerts refers to the amount of time before blood glucose is expected to reach the low or high threshold that the CGM should produce an alert; this setting is provided in milliseconds.

### Calibration

The options for sensor calibration alerts are represented as an object that looks like

~~~
{
  "preReminder": "see_below",
  "overdueAlert": "see_below"
}
~~~

Where:

* `preReminder` is the amount of time, in milliseconds, before the next calibration that an alert should be produced.
* `overdueAlert` is the amount of time, in milliseconds, after a calibration was due that an additional alert should be produced.

### Optional Fields

Some attributes in cgmSettings are optional because they only apply to the current devices available from some, but not all, device vendors. These are:

* `calibration`
* `outOfRangeAlerts`
* `predictiveAlerts`

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested, minus the `inputUnits`, which are *never* stored. There are no modifications performed.