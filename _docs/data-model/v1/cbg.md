---
layout: defaults
title: CBG V1
published: true
---
# CBG

CBG represents blood glucose from a continuous glucose monitor. These events are point-in-time and look like


~~~json
{
  "type": "cbg",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "inputUnits": "see_common_fields",
  "value": "bg_value_from_cgm",
  "isig": "optional_interstitial_fluid_value_from_cgm"
}
~~~


## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested, minus the `inputUnits`, which are *never* stored. There are no modifications performed.