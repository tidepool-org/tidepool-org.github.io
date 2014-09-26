---
layout: defaults
title: Annotations V1
published: true
---
# Annotations

Any event can have an `annotations` field.  Annotations are attached to indicate that there might be something odd about the event, usually something that affects the allowable interpretations of the data in the event.

The `annotations` field is physically an array of annotation objects, but semantically it is a set.  There should never be two elements in the array with the same `code`.

An annotation object has one required field, `code`, and can have any number of other fields.  For example

~~~json
    { "code": "carelink/basal/temp-percent-create-scheduled" }
~~~

is valid.  Also

~~~json
    { "code": "cbg/below-testable-threshold", "threshold": 40 }
~~~

Could also be valid.  That is, the idea is that the annotation can optionally provide some information that helps when explaining what went wrong.  The fabricated example above would be for a CBG reading that ends up below 40 but the CGM can only read down to 40.  If there were another CGM that could read down to 25, then that CGM would set the threshold value to 25 instead of 40.

We document our known annotations below, but we do not limit the set of annotations to what is documented below.  If you are integrating with the Tidepool platform and run into something that is not currently accounted for by the annotations listed below, please make something up and provide a pull request against this documentation.

## Known Annotations

### Generic

* `basal/mismatched-series` happens when the API receives an basal event with a `previous` field that does not line up with the basal event immediately before it in the stream of basal events.
* `basal/intersects-incomplete-suspend` happens when the interval of a basal segment contains a `deviceMeta` event annotated with `status/incomplete-tuple`. (See next item.) This basal segment will *not* reflect the incomplete suspend event and thus may not reflect actual delivery (which may have been suspended).
* `status/incomplete-tuple` happens when a `deviceMeta` `status` event is sent in and never completed.  See the [Device Metadata](device-meta) page for more details.
* `status/unknown-previous` happens when a `deviceMeta` `status` event is sent in with a `previous` field that doesn't reference an object in the Tidepool platform.  Accompanied by an `id` field:
    * `id` the expected id of the previous event as specified in the `previous` field on the event submitted to the Tidepool platform.  This might be null or just not exist if there wasn't a `previous` field provided on the submitted event.

### Carelink

* `carelink/basal/off-schedule-rate` happens when basal rates do not match the current settings
    * This is likely indicative of an issue either with the data coming out of Carelink or the interpretation of said data.  If seen, these should be investigated.
* `carelink/basal/temp-percent-create-scheduled` happens whenever a percentage temp basal crosses over a scheduled basal boundary on CareLink
    * When a percentage temp basal is active, Medtronic pumps ignore and do not provide any further "basal rate change" events.  Meaning that we must look at the current settings in order to determine when those changes occurred and what the original rate was.  Not having these events actually means that it is impossible to audit the activities of the pump during a temp basal.  We generate the events that the pump *should* be generating as part of its audit log and annotate them with this annotation to indicate that it is an event we have generated.
* `carelink/settings/basal-mismatch` happens when the basal portions of the pump settings do not line up with the data that is provided by CareLink.
* `carelink/settings/wizard-mismatch` same as "basal" but with other wizard settings: insulin sensitivity, insulin:carb ratio, etc
* `carelink/settings/activeSchedule-mismatch` same as "basal" but with the currently active schedule
    * We believe all three of the `mismatch` issues largely arise from the following situation: You've uploaded data from a pump to CareLink, but that pump has been reset between the last upload and the current upload.
* `carelink/impending-device-overlap` occurs when two Medtronic insulin pumps of the same model were uploaded to one account with data overlaping in time. Because Carelink does not identify the serial number of the pump whence each datum originated, we cannot separate such overlapping data, and in the short term will remove it instead. This annotation should appear on the last basal segment *prior* to the removed overlapping data (in order that the annotation will be exposed clearly to the user).

  
