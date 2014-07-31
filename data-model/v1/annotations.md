---
layout: defaults
title: Annotations V1
published: true
---
# Annotations

Annotations are attached to any type of event to indicate that there might be something odd about the event.  

An annotation is a free-form object except for one restriction: it must have a "code" field.  For example

~~~json
    { "code": "carelink/basal/temp-percent-create-scheduled" }
~~~

The annotations field is a set of annotation objects.  Where equality for the set is determined by equivalent codes.

We document our known annotations below, but we do not limit the set of annotations to what is documented below.  If you are integrating with the Tidepool platform and run into something that is not currently accounted for by the annotations listed below, please make something up and provide a pull request against this documentation.

## Known Annotations

### Generic

* `basal/mismatched-series` happens when the API receives an basal event with a `previous` field that does not line up with the basal event immediately before it in the stream of basal events.

### Carelink

* `carelink/basal/off-schedule-rate` happens when basal rates do not match the current settings
    * This is likely indicative of an issue either with the data coming out of Carelink or the interpretation of said data.  If seen, these should be investigated.
* `carelink/basal/temp-percent-create-scheduled` happens whenever a percentage temp basal crosses over a scheduled basal boundary on CareLink
    * When a percentage temp basal is active, Medtronic pumps ignore and do not provide any further "basal rate change" events.  Meaning that we must look at the current settings in order to determine when those changes occurred and what the original rate was.  Not having these events actually means that it is impossible to audit the activities of the pump during a temp basal.  We generate the events that the pump *should* be generating as part of its audit log and annotate them with this annotation to indicate that it is an event we have generated.
* `carelink/settings/basal-mismatch` happens when the basal portions of the pump settings do not line up with the data that is provided by CareLink.
* `carelink/settings/wizard-mismatch` same as "basal" but with other wizard settings: insulin sensitivity, insulin:carb ratio, etc
* `carelink/settings/activeSchedule-mismatch` same as "basal" but with the currently active schedule
    * We believe all three of the `mismatch` issues largely arise from the following situation: You've uploaded data from a pump to CareLink, but that pump has been reset between the last upload and the current upload.

  
