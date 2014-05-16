# Wizard

Wizard events represent interactions with a bolus wizard on a pump.  The event is intended to contain the values that were input into the Wizard as well as the values that the wizard might have recommended for a bolus.

Wizard events are point-in-time and look like

``` json
{
  "type": "wizard",
  "recommended": number_of_units_recommended
  "payload": see_below,
  "time": see_common_fields,
  "deviceId": see_common_fields,
  "source": see_common_fields,
  "bolus": bolus_event_resulting_from_wizard_if_exists
}
```

* `recommended` is the number of units of insulin that the wizard recommended that the PwD inject.
* `payload` is an object of arbitrary fields that will be stored alongside the wizard event.  An example of things that might be stored is:

    ``` json
    {
      "targetLow": ...,
      "targetHigh": ...,
      "carbRatio": ...,
      "insulinSensitivity": ...,
      "carbInput": ...,
      "carbUnits": ...,
      "bgInput": ...,
      "bgUnits": ...,
      "correctionEstimate": ...,
      "foodEstimate": ...,
      "activeInsulin": ...
    }
    ```

