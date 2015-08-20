---
layout: defaults
title: Wizard V1
published: true
---
# Wizard

Wizard events represent interactions with a "bolus wizard" or "bolus calculator" on a pump. The event is intended to contain the values that were input into the wizard, as well as any recommendations that the wizard might have made.

Wizard events are point-in-time and look like

~~~json
{
  "type": "wizard",
  "time": "see_common_fields",
  "deviceId": "see_common_fields",
  "uploadId": "see_common_fields",
  "bgInput": "see_below",
  "bgTarget": "see_below",
  "carbInput": "see_below",
  "inputUnits": "see_common_fields",
  "insulinOnBoard": "see_below",
  "insulinCarbRatio": "see_below",
  "insulinSensitivity": "see_below",
  "recommended": {
    "carb": "see_below",
    "correction": "see_below",
    "net": "see_below"
  },
  "payload": "see_below",
  "bolus": "see_below"
}
~~~
* `bgInput` is the blood glucose value input into the wizard. This is converted to mmol/L based on the `inputUnits` field.
* `bgTarget` is the complex representation of the target blood glucose.  It can be  varying schemas that should align with the schemas in the [settings object](../settings) except there is no `start` field.
* `carbInput` is the carbohydrate value (in grams) input into the wizard, note that this is not necessarily an indication of carbohydrates that were actually consumed. It is, instead, an indication of carbohydrates that the PwD entered into their pump.
* `insulinOnBoard` is the units of insulin currently in suspension inside of the PwD. Metaphorically speaking, this can be thought of as how many little yellow school buses with Mrs. Frizzle and kids are floating around learning about the PwD's anatomy.
* `insulinCarbRatio` is the number of grams of carbohydrates that one unit of insulin is expected to cover.
* `insulinSensitivity` is the amount that one unit of insulin is expected to decrease blood glucose.  This is converted to mmol/L based on the `inputUnits` field.
* `recommended.carb` is the number of units of insulin that the wizard recommended to cover carbohydrate intake.	
* `recommended.correction` is the number of units of insulin that the wizard recommended to attempt to bring the PwD down to their target BG.
* `recommended.net` is the net number of units of insulin that the wizard recommended, taking into account `recommended.carb`, `recommended.correction`, and `insulinOnBoard`. We store the result of this calculation because different devices do this calculation in different ways.
* `payload` is an object of arbitrary fields that will be stored alongside the wizard event. This is just an instance of the common field `payload`. An example of things that might be stored is:

~~~json
{
  "targetLow": ...,
  "targetHigh": ...,
  "carbRatio": ...,
  "insulinSensitivity": ...,
  "correctionEstimate": ...,
  "foodEstimate": ...,
  "activeInsulin": ...
}
~~~

* `bolus` is the bolus event that resulted from the wizard. This field is used in much the same way as the `previous` field in other events: it can be either the full object or the id of an object that has already been submitted. It is expected that the bolus event is generated and submitted separately from the wizard because bolus events have their own lifecycle. The order of when the events are submitted does not matter as the bolus value simply gets converted into an id and stored.

## Storage/Output format

The storage and output format of wizard events is a straight mirror of the input format with two exceptions. First, the `bolus` field will always be the id of the bolus event that this wizard event would have generated. Note that the id is not guaranteed to exist, as the wizard event could be sent before the bolus event. Second, the `inputUnits` units are never stored, only used to trigger conversion to mmol/L where needed.
