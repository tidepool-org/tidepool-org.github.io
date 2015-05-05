---
layout: defaults
title: Ketones V1
published: true
---
# Ketones

Ketones represent blood or urine ketone concentration. Because blood and urine ketone testing differ on several dimensions, we define a separate type for each.

## Blood Ketones

Blood ketones represent ketone concentration values (specifically beta-ketones, primarily beta-hydroxy butyric acid) obtained from a fingerstick meter capable of reading specialized blood ketone testing strips. These records are point-in-time and look like

~~~json
{
  "type": "bloodKetone",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "value": "ketone_concentration_measurement"
}
~~~

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested. There are no modifications performed.

## Urine Ketones

Urine ketones represent ketone concentration values obtained by home testing with ketone reagant strips, usually reacting positively to quantities of acetoacetic acid (rather than beta-ketones) in the urine. These records differ significantly from blood ketone records because the values obtained are qualitative, not quantitative. They look like

~~~json
{
  "type": "urineKetone",
  "time": "see_common_fields",
  "uploadId": "see_common_fields",
  "value": "see_below"
}
~~~

The possible qualitative `value`s for urine ketone concentration are limited to the following:

- `negative`
- `trace`
- `small`
- `moderate`
- `large`

### Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested. There are no modifications performed
