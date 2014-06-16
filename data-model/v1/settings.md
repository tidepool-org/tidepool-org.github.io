---
layout: defaults
title: Settings V1
published: true
---
# Settings

## Object
A settings object represents the settings of a pump.  A settings object sent into our API looks like

~~~
{
    "type": "settings",
    "time": see_common_fields,
    "deviceId": see_common_fields,
    "source": see_common_fields
    "activeBasalSchedule": currently_active_schedule_name,
    "units": see_below,
    "basalSchedules": see_below,
    "carbRatio" : see_below,
    "insulinSensitivity" : see_below,
    "bgTarget": see_below
}
~~~

### units

`units`: an object representing the units that the pump is operating with.  Looks like

~~~json
    {
      "carb": "grams",
      "bg": "mg/dL"
    }
~~~

### basalSchedules

`basalSchedules`: an object with entries for each schedule programmed into the pump.  Looks like

~~~json
    {
        "standard": [ 
          { "rate": 0.8, "start": 0 },
          { "rate": 0.75, "start": 3600000 },
          ...
        ],
        "pattern a": [
          { "rate": 0.95, "start": 0 },
          { "rate": 0.9, "start": 3600000 },
          ...
        ]
    }
~~~

Where:

* `rate` is the basal rate 
* `start` is the millisecond offset from midnight that day, representing when the rate should take effect

### carbRatio

`carbRatio`: The carb ratios in effect on the pump.  These look like

~~~json
    [
      { "amount": 12, "start": 0 },
      { "amount": 10, "start": 21600000 },
      ...
    ]
~~~
 
Where 
* `amount` is the units of carbs per unit of insulin 
* `start` is the millisecond offset from midnight that day, representing when the ratio should take effect

### insulinSensitivity

`insulinSensitivity`: The insulin sensitivity rates in effect on the pump.  These look like

~~~json
    [
      { "amount": 65, "start": 0 },
      { "amount": 45, "start": 18000000 },
      ...
    ]
~~~

Where:

* `amount` is the expected decrease in bg per unit of insulin 
* `start` is the millisecond offset from midnight that day, representing when the sensitivity value should take effect

### bgTarget

`bgTarget`: The target blood glucose values represented as the high and low threshold.  Note that some pumps internally represent this as a target with a +/-, we are choosing to standarding on the high/low representation so settings should be converted to the high and low values of the target range before emission to the Tidepool platform.

~~~json
    [
      { "low": 100, "high": 120, "start": 0 },
      { "low": 90, "high": 110, "start": 18000000 },
      ...
    ]
~~~

Where:

* `low` is the target low blood glucose
* `high` is the target high blood glucose
* `start` is the millisecond offset from midnight that day, representing when the setting should take effect
