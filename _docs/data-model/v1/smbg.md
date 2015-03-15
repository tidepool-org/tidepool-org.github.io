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
  "subType": "optional_entry_type_info",
  "value": "bg_value_from_self_monitoring_device",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields"
}
~~~

## subType

The Tidepool platform ingests `smbg` records from many more sources than just blood glucose meters - e.g., insulin pumps (with or without linked meters), and manual logging. The `subType` field records more information about the type of `smbg` record given its source. The possible values, at present, are the following:

- `manual`
- `linked`

Values that come directly from a blood glucose meter can either contain an empty string for the `subType` or omit the field entirely.

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested.  There are no modifications performed.