---
layout: defaults
title: Tidepool Data Provenance
published: true
---

In the last 14 years, I've moved 14 times.  Something about change excites me, but you're not here to read about me.  No, this is a story to help describe what data provenance means.  'Cause, let's admit it, that conglomeration of words probably means absolutely nothing to everybody reading this.  But, I digress.

Having moved as much as I have, I sometimes have a problem remembering my old addresses.  Probably once every 6 months, I come across some government form or other thing that I have to do in which the last time I did it was ages ago and I have no clue which address I would've been at when I originally did it.  I spend the next few minutes racking my brain, trying to figure out what it could be, inevitably the person I'm doing the paperwork for just gives up and is like, "well, whatever, it'll be fine."

At these points in time, I often wish that everytime I updated my address on the USPS website, for example, it would keep track of when I did the update and allow me access to all of the address changes that I've done.  Then, whenever this happens, I could just pull that up and bam, I'd know what address I was at when.  I'd have a complete "audit trail" for all of my address changes.  Or, said in the big fancy words of the title of this page, I would understand the "provenance" of my address.

At Tidepool, we care about the "provenance" of your data.  Changes to data in the system can happen and when they happen, you should be empowered to know 

1. When a change was made
2. What your data was before the change
3. Why a change was made

And this is for *all* changes made inside of the platform.

## Data

Wow, that sounds great.  But, really, what does that conretely mean for you, the user of the Tidepool platform.  To answer that question, we'll start with an example of what kind of data we are talking about.  Inside of the Tidepool platform, we generally think in terms of JSON objects, if you don't know what those are, just look at the example SMBG (Self-Monitored Blood Glucose) event below.

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T17:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7"
}
~~~

Explaining each of the parts of this event, we essentially have data that says:

* `"type": "sbmg"` - The event is an "SMBG" event
* `"time": "2014-08-01T17:00:00.000Z"` - It was generated at 5pm UTC on August 1st
* `"value": 5.1` - The blood glucose reading was 5.1 mmol/L
* `"deviceId": "aviva-53543975796"` - It was taken from an Aviva BG meter with SN#53543975796
* `"source": "uu-2.3.1_accucheck-1.0.7"` - It was provided by Tidepool's "Universal Uploader" version 2.3.1 running with the accucheck driver version 1.0.7

So, that's what data looks like.  Except, when we store the data, we actually attach two extra fields to it:

* `"createdTime": "2014-08-04T18:02:27.283Z"` - It turns out that the event was delivered to the Tidepool platform at 6pm UTC on August 4th.  That is good to know
* `"id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6"` - This is an identifier for the data point, the letters and numbers have no meaning except that when combined they refernce this one specific data point.  If ever there is a need to change this data point, this id can be used to specify that this is the data point you want to change.  This is similar to a Social Security Number being used to reference one specific human being in the US, except it is safer to have this id on the internet than it is your Social.

So, in actuality, we store

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T17:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6"
}
~~~


And that's actually what data looks like.  Well, for now at least.

## Modifications

Now that we know what data looks like, what happens when we want to change something in the data?  Say, the timestamp was wrong (by now, all diabetics have probably experienced a device that believes it is a different time or even date than it actually is...).  You'd like to be able to modify that.  Well, in a simple world, we would just allow you to change the date and store it.

However, if we do that naively, it takes us back to my problem with my old addresses.  Let's take an example.  Let's say wanted to change 5pm on 08/01 to 5pm on 08/02, but instead you accidentally change it to 2pm on 08/01.  Let's say you do this and you just trust that you did it right, you don't double check.  

A week goes by, you come back and you notice that you don't have any data for 5pm on 08/02, which you coulda swore you set 'cause it should line up with right before that massive 200g of Chicago style pizza you had. And, why did you check your blood sugar twice at 2pm on 08/01 and get 5.1 at one point and 8.9 at another.  That seems like a really wide spread.  Wouldn't it be great if you could look at those data points and verify that they hadn't been changed?  Even better, that you could verify where they came from?

That's exactly what Tidepool does for you. And this is how.

Taking our SMBG data from above again

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T17:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6"
}
~~~

Let's adjust the timestamp erroneously to 2pm:

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T14:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6"
}
~~~

If we were to store this as is, we would clobber the old event and not be able to get at the old copy of the data anymore.  So, instead of clobbering things, we add one more field: a "_version" field that tells us the version of the event.  So, our first event now looks like this

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T17:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6",
  "_version": 0
}
~~~

And our updated event looks like

~~~json
{
  "type": "smbg",
  "time": "2014-08-01T14:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6",
  "_version": 1,
  "modifiedTime": "2014-08-05T16:11:00.991Z"
}
~~~

Did you notice the field I snuck in there in the second event?  Well, if you didn't I'll tell you what it is:

* `"modifiedTime": "2014-08-05T16:11:00.991Z"` - the time that the modification happened.  This would mean that we modified our event around 4pm UTC on August 5th.

So, we now have two events stored in the Tidepool Platform.  When we show you the data, we only show you the one with the newest "_version".  But, if you ever run into the issue described above where you don't know what's up with your data, you can rest assured that the Tidepool Platform has all copies of your data safely stored away.  So, in this new utopian world, you can deal with the above situation by

1. Ask the platform for the history of the odd data points
2. Notice that one of them was modified
3. Look at when it was modified and realize that it was a week ago, that's right, you modified something a week ago, but that should've been to line up with that pizza
4. Realize that you set it to 2pm instead of setting the date to the 2nd
5. Quickly close the gap from wherever your palm currently is to your forehead
6. Edit the event again, this time setting it back to 5pm, but on August 2nd

And, now we have a third event in the tidepool platform:

~~~json
{
  "type": "smbg",
  "time": "2014-08-02T17:00:00.000Z",
  "value": 5.1,
  "deviceId": "aviva-53543973927",
  "source": "uu-2.3.1_accucheck-1.0.7",
  "createdTime": "2014-08-04T18:02:27.283Z",
  "id": "vc23t2tpotlq6nkdcvk12r7rodmiv8p6",
  "_version": 2,
  "modifiedTime": "2014-08-12T23:47:11.741Z"
}
~~~

For those of you who are technically inclined, you are probably already aware of this "versioning" strategy.  It has been employed in database technology for many, many years.  This is just one more useful application of it.

# Summary

In summary,

1. Data provenance is the maintenance of the history of each individual event.  It is essentially an audit trail of all changes done on all events.
2. Tidepool maintains data provenance by "versioning" all copies of events that enter the system.
3. Tidepool will provide a UI that allows you to see the changes made to your data as well as make them yourself