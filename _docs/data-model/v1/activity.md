---
layout: defaults
title: Activity V1
published: true
---
# Activity [DRAFT]

Activity is intended to track any exercise acitivity

``` json
{
  "type": "activity",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "subType": "see_below",
  "duration": "milliseconds_of_activity",
  "intensityMet": "optional_MET_intensity_indicator",
  "intensityBorg": "optional_Borg_intensity_indicator",
  "intensityHr": "optional_heartrate_intensity_indicator",
  "intensityWatts": "optional_watts_intensity_indicator",
  "location": "optional_location"
}
```

* `subType` - the type of activity the user did - e.g. cycling, walking, gym...
* `intensity...` - The group of optional intensity indicators are to allow personal tracking of excercise data and also to allow researchers to access potential group data based on these intensities 
* `intensityMet` - an optional intensity indicator using the [MET scale](http://en.wikipedia.org/wiki/Metabolic_equivalent)
* `intensityBorg` - an optional intensity indicator using the [Borg scale](http://en.wikipedia.org/wiki/Borg_scale)
* `intensityHr` -  an optional intensity indicator using heartrate data
* `intensityWatts` - an optional intensity indicator using wattage data
* `location` - an optional location placeholder that will be expanded inline with other types that track location
