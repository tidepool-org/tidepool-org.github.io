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
    "uploadId": see_common_fields,
    "activeSchedule": currently_active_basal_schedule_name,
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

On some pumps, there can be many schedules of `carbRatio` settings, parallel to how a user can program several different basal schedules. In these cases `carbRatio` will be an object keyed by the name of each schedule, for example

~~~json
	{
		"standard": [
			{ "amount": 12, "start": 0 },
			{ "amount": 10, "start": 21600000 }
		],
		"pattern a": [
			...
		]
	}
~~~

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

On some pumps, there can be many schedules of `insulinSensitivity` settings, parallel to how a user can program several different basal schedules. In these cases `insulinSensitivity` will be an object keyed by the name of each schedule, for example

~~~json
	{
		"standard": [
			{ "amount": 65, "start": 0 },
			{ "amount": 45, "start": 18000000 }
		],
		"pattern a": [
			...
		]
	}
~~~

### bgTarget

`bgTarget`: The target blood glucose values represented as one of three ways.  Note that some pumps internally represent this as a target with a +/-, we are choosing to standarding on the high/low representation so settings should be converted to the high and low values of the target range before emission to the Tidepool platform.

#### High/Low
~~~json
    [
      { "low": 100, "high": 120, "start": 0 },
      { "low": 90, "high": 110, "start": 18000000 },
      ...
    ]
~~~

#### Target + High/Low
~~~json
    [
      { "target": 100, "low": 80, "high": 120, "start": 0 },
      { "target": 110, "low": 80, "high": 140, "start": 1800000 },
      ...
    ]
~~~

#### Target + Range
~~~json
    [
      { "target": 100, "range": 20, "start": 0 },
      { "target": 110, "range": 30, "start": 1800000 },
      ...
    ]
~~~

#### Target + High
~~~json
    [
      { "target": 100, "high": 120, "start": 0 },
      { "target": 110, "high": 140, "start": 1800000 },
      ...
    ]
~~~

Where:

* `low` is the defined low blood glucose
* `high` is the defined high blood glucose, often used as a threshold by the pump to determine if a correction dose of insulin should be delivered
* `target` is the target glucose that the wizard/calculator should attempt to achieve
* `range` is the range of "acceptable" values around the target
* `start` is the millisecond offset from midnight that day, representing when the setting should take effect

On some pumps, there can be many schedules of `bgTarget` settings, parallel to how a user can program several different basal schedules. In these cases `bgTarget` will be an object keyed by the name of each schedule, for example

~~~json
	{
		"standard": [
		  { "low": 100, "high": 120, "start": 0 },
		  { "low": 90, "high": 110, "start": 18000000 }
		],
		"pattern a": [
			...
		]
	}
~~~

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.