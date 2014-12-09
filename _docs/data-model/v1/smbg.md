---
layout: defaults
title: SMBG V1
published: true
---
# SMBG

SMBG represents blood glucose from a finger prick or other "self-monitoring" method.  These events are point-in-time and look like


~~~json
{
  "type": "smbg",
  "value": bg_value_from_self_monitoring_device,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "uploadId": see_common_fields,
  "source": see_common_fields
}
~~~


## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed