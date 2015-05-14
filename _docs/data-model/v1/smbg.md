---
layout: defaults
title: SMBG V1
published: true
---
# SMBG

SMBG represents blood glucose from a finger prick or other "self-monitoring" method. These events are point-in-time and look like


~~~json
{
  "type": "smbg",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "inputUnits": "see_common_fields",
  "mealTag": "option_meal_tag",
  "payload": "see_common_fields",
  "subType": "optional_entry_type_info",
  "value": "bg_value_from_self_monitoring_device"
}
~~~

## mealTag

Some meters allow tagging of blood glucose readings as pre- or post-prandial. We have created the `mealTag` attribute to store this information. The possible values for this field are:

- `pre-meal`
- `post-meal`
- `pre-exercise`
- `post-exercise`
- `other`

If `other` is provided as the meal tag but more detail is available, further details should be provided in the `payload`.

## subType

The Tidepool platform ingests `smbg` records from many more sources than just blood glucose meters - e.g., insulin pumps (with or without linked meters), and manual logging. The `subType` field records more information about the type of `smbg` record given its source. The possible values, at present, are the following:

- `manual`
- `linked`

Values that come directly from a blood glucose meter can either contain an empty string for the `subType` or omit the field entirely.

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.