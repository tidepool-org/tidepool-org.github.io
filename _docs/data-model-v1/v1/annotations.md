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
    { "code": "carelink/basal/fabricated-from-schedule" }
~~~

is valid.  Also

~~~json
    { "code": "bg/out-of-range", "threshold": 40, "value": "low" }
~~~

Could also be valid.  That is, the idea is that the annotation can optionally provide some information that helps when explaining what went wrong.  The fabricated example above would be for a CBG reading that ends up below 40 but the CGM can only read down to 40.  If there were another CGM that could read down to 25, then that CGM would set the threshold value to 25 instead of 40.

We document our known annotations below, but we do not limit the set of annotations to what is documented below.  If you are integrating with the Tidepool platform and run into something that is not currently accounted for by the annotations listed below, please make something up and provide a pull request against this documentation.

## Known Annotations

### Generic

* `time-change` happens when we detected a change in a device's date & time settings that results in either gaps or overlaps in the data. This annotation is *required* to appear on data that we know overlaps with other data coming from the same device.

* `basal/mismatched-series` happens when the API receives a basal event with a `previous` field that does not line up with the basal event immediately before it in the stream of basal events.


* `basal/unknown-duration` seems to happen around initial set-up of a new or replacement pump, when we don't yet have a complete set of basal schedules to match basal rate changes against, but more generally speaking this annotation code should appear on all basal rate segments for which we cannot determine a duration. (Since a basal rate segment represents delivery of insulin at a certain rate over an interval of time, any basal rate event lacking a duration is uninterpretable.)


* `basal/intersects-incomplete-suspend` happens when the interval of a basal rate segment contains a `deviceMeta` event annotated with `status/incomplete-tuple`. (See next item.) This basal segment will *not* reflect the incomplete suspend event and thus may not reflect actual delivery (which may have been suspended). The annotation should also include a `deviceMetaId` field:
    * `deviceMetaId` the `id` of the `deviceMeta` annotated with `status/incomplete-tuple` that is contained within the time interval of the basal segment.
* `status/incomplete-tuple` happens when a `deviceMeta` `status` event is sent in and never completed.  See the [Device Metadata](device-meta) page for more details.


* `bg/out-of-range` happens when a blood-glucose sensing device reads LOW or HIGH instead of a numerical value. We store a value of +/-1 from the max or min value (this will vary depending on device) and apply this annotation to expose the change. The non-numerical display value and threshold, if available, should also be included in the annotation object:

~~~json
{
    "code": "bg/out-of-range",
    "threshold": 20,
    "value": "low"
}
~~~

* `status/unknown-previous` happens when a `deviceMeta` `status` event is sent in with a `previous` field that doesn't reference an object in the Tidepool platform.  Accompanied by an `id` field:
    * `id` the expected id of the previous event as specified in the `previous` field on the event submitted to the Tidepool platform.  This might be null or just not exist if there wasn't a `previous` field provided on the submitted event.

### CareLink

* `carelink/basal/off-schedule-rate` happens when basal rates do not match the current settings, where this could mean either that the currently active schedule in the settings matches the basal rate segment's declared schedule (but the rate given does not match the rate for that time of day in the schedule stored in the settings), *or* that the basal rate segment's declared schedule does not match the currently active schedule according to the settings.
    * This is likely indicative of an issue either with the data coming out of CareLink or the interpretation of said data.  If seen, these should be investigated.
* `carelink/basal/fabricated-from-schedule` happens in two situations:
    * First, when a percentage temp basal is active, Medtronic pumps ignore and do not provide any further "basal rate change" events.  Meaning that we must look at the current settings in order to determine when those changes occurred and what the original (scheduled but suppressed) rate was.  Not having these events actually means that it is impossible to audit the activities of the pump during a temp basal.  We generate the events that the pump *should* be generating as part of its audit log and annotate them with this annotation to indicate that they are events we have generated.
    * Second, whenever there is some kind of gap in the basal segment history, either due to discarding an uninterpretable event or because the pump is an older model that does not keep a full running log of basal rate changes. In these cases, we fill in the expected basal rate changes from the currently active basal schedule in the pump's settings and annotate each to make it clear that these data points are events we have generated.
* `carelink/basal/fabricated-from-suppressed` can happen upon resuming from a suspend when Medtronic does not provide (in the proper sequence, or at all) the basal rate event that the pump returned to upon exiting suspension. In cases such as this, we fabricate a basal rate based on the most recent rate that we believe was suppressed by the suspend and annotate this event to make it clear that we have generated it.

    
* `carelink/bolus/missing-square-component` happens when we process a normal bolus with `IS_DUAL_COMPONENT=true` but don't manage to find the corresponding square-wave bolus. (Combining component bolus events and/or bolus wizard events is particularly tricky with CareLink data.)


* `carelink/wizard/long-search` happens when we can't be 100% certain that we've *correctly* combined a wizard event with an actual bolus delivery event. We rely on the upload sequence numbers of events in the CareLink data to combine these events, but sometimes other events (e.g., pump alarms) disturb the sequence. We make a reasonable effort to allow for this by searching beyond exactly where we expect the corresponding event to be in the sequence, but when we have done this, we apply this annotation to express our lack of absolute certainty in the result.

### Insulet

* `insulet/basal/fabricated-from-schedule` appears on the last basal of each upload. Since Insulet does not provide a duration on basal events, we compare the final basal segment's rate and schedule name against the basal schedules in the OmniPod user's current (as of upload) settings and use the duration from the schedule if the rate and schedule name match.

* `insulet/bolus/split-extended` may occur when a bolus with an extended component crosses midnight and the bolus was not entered using the suggested bolus calculator. The Insulet data always splits extended bolus records that cross midnight into two records, but without a suggested bolus calculator record to tie potentially split records together, it is impossible to know which records may have been split and which are actually two separate bolus events. Hence, whenever there is an extended bolus occurring very close to midnight that does not have an associated suggested bolus calculator record, we add this annotation to indicate that this may be the second part of a split record.

### Bayer

* `bayer/smbg/unreported-hi-lo-threshold` may appear with `bg/out-of-range` if the thresholds are not reported in the header. This is only known to happen on the Bayer Contour Next Link so far, possibly due to a bug in the device firmware. With this annotation, 20 is used as the low threshold and 600 as the high threshold, as these are the most common thresholds for these devices.

### Tandem

* `tandem/basal/fabricated-from-schedule` appears on the last basal of each upload. Since Tandem does not provide a duration on basal events, we compare the final basal segment's rate and schedule name against the basal schedules in the user's current (as of upload) settings and use the duration from the schedule if the rate and schedule name match.
* `tandem/basal/fabricated-from-occlusion-alarm` appears on the suspended basal after an occlusion alarm is triggered. A suspended basal is created manually, as an occlusion alarm does not trigger a basal rate change event (used to create all other basals).
* `tandem/basal/fabricated-from-new-day` appears on a basal that crosses midnight, and is used to break long-running or flat basals into smaller segments. When the pump is suspended, new-day events are still generated with the commanded (non-zero) basal rate, so they have to be ignored. As such, we only generate new-day events when we think they're actually appropriate, and annotate them as such.

  
