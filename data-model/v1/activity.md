# Activity

Activity is intended to track any excercise acitivity

``` json
{
  "type": "activity",
  "subType": your_type_here
  "duration": milliseconds_you_where_active
  "intensityMet": optional_MET_intensity_indicator
  "intensityBorg": optional_Borg_intensity_indicator
  "intensityHr": optional_heartrate_intensity_indicator
  "intensityWatts": optional_watts_intensity_indicator
  "location" : optional_location
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
```

* `subType` - they type of activity the user did. e.g. cycling, walking, gym ...
* `intensity...` - the group of optional intensity indicators are to allow personal tracking of excercise data and also to allow researchers, when authorised, to potential group data based on these intensities 
* `intensityMet` - an optional intensity indicator using the [MET scale](http://en.wikipedia.org/wiki/Metabolic_equivalent)
* `intensityBorg` - an optional intensity indicator using the [Borg scale](http://en.wikipedia.org/wiki/Borg_scale)
* `intensityHr` -  an optional intensity indicator using heartrate data
* `intensityWatts` - an optional intensity indicator using wattage data
* `location` - an optional location placeholder that will be expanded inline with other types that track location
