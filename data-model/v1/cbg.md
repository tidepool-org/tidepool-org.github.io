# CBG

CBG represents blood glucose from a continuous glucose monitor.  These events are point-in-time and look like


``` json
{
  "type": "cbg",
  "value": bg_value_from_cgm,
  "isig": optional_interstitial_fluid_value_from_cgm,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
```