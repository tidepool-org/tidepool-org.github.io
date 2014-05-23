# Activity

Activity is intended to track any excercise acitivity

``` json
{
  "type": "activity",
  "subType": your_type_here
  "duration": minutes_you_where_active
  "intensity": optional_freeform_intensity_indicator
  "location" : optinal_location
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
```

* `subType` - they type of activity the user did. e.g. cycling, walking, gym ...
* `intensity` - an optional intensity indicator that is relevent to the user. Could be a percived intensity, heart rate, watts ...
* `location` - an optional location placeholder that will be expanded inline with other types that track location