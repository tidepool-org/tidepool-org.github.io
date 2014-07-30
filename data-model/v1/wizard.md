---
layout: defaults
title: Wizard V1
published: true
---
# Wizard

Wizard events represent interactions with a "bolus wizard" or "bolus calculator" on a pump.  The event is intended to contain the values that were input into the wizard, as well as any recommendations that the wizard might have made.

Wizard events are point-in-time and look like

~~~json
{
  "type": "wizard",
  "recommended": number_of_units_recommended,
  "bgInput": bg_as_input_into_wizard,
  "carbInput": carbohydrates_as_input_into_wizard_in_mg,
  "payload": see_below,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "bolus": bolus_event_resulting_from_wizard_if_exists
}
~~~

* `recommended` is the number of units of insulin that the wizard recommended that the PwD inject.
* `bgInput` the blood glucose value input into the wizard.
* `carbInput` the carbohydrate value (mg) input into the wizard, note that this is not necessarily an indication of carbohydrates that were actually consumed.  It is, instead, an indication of carbohydrates that the PwD entered into their pump.
* `payload` is an object of arbitrary fields that will be stored alongside the wizard event.  An example of things that might be stored is:
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
* `bolus` is the bolus event that resulted from the wizard.  This field is used in much the same way as the `previous` field in other events: it can be either the full object or the id of an object that has already been submitted.  It is expected that the bolus event is generated and submitted separately from the wizard because bolus events have their own lifecycle.  The order of when the events are submitted does not matter as the bolus value simply gets converted into an id and stored.

