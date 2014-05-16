# SMBG

SMBG represents blood glucose from a finger prick or other "self-monitoring" mehod.  These events are point-in-time and look like


``` json
{
  "type": "smbg",
  "value": bg_value_from_self_monitoring_device,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields
}
```