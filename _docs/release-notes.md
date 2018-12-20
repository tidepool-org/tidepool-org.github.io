---
layout: homepage
title: Tidepool Release Notes
published: true
---
# Tidepool Release Notes  

We love GitHub, but it can be an intimidating place for newcomers if you don't know where to look, or what you're looking for. To help with that, these release notes will make updates to Tidepool for Web, the Tidepool Uploader, and Tidepool Mobile a bit more readable.  

Because we love transparency, along with a description of what's new, each update includes a link out to the specific GitHub repository release (**Code Update**) and relevant public Trello cards (**Feature Requirements**) so you can get a better sense of who, what, when, where, and why.  

Questions? Contact us at [support@tidepool.org](mailto:support@tidepool.org)  

* [Jump to Tidepool Web Updates](#tidepool-web)  
* [Jump to Tidepool Uploader Updates](#tidepool-uploader)  
* [Jump to Tidepool Mobile Updates](#tidepool-mobile)  

# Last Updated: 2018-12-19

<hr>

## Tidepool Web  
Visit [https://app.tidepool.org](https://app.tidepool.org) to see the results of these updates.  

Don't have a Tidepool account? Visit [tidepool.org/signup](https://tidepool.org/signup) to create your free Tidepool account.  

### 1.13.0 (Released 2018-12-19)
Adding support for Basal-IQ data brought to light the fact that we do not differentiate between basal rates that are manually and automatically suspended. We added a friendly indicator to denote the times basal rates were automatically suspended.  
[Feature Requirements](https://trello.com/c/wZk0cMNU/35-distinguish-between-auto-and-manual-suspend-for-plgs-like-basal-iq)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.13.0)

### 1.12.6 (Released 2018-11-13)
We added a handy plugin called Beacon to Tidepool Web to provide faster access to our support articles, guides, and walkthroughs. With Beacon, you can also send an email to our support team or even chat live with us if you have questions without leaving the page. Our goal is to get you answers to your questions faster so you can get back to exploring your data. Enjoy!  
[Feature Requirements](https://trello.com/c/GILzFQMb/6-add-helpscout-beacon-to-apptidepoolorg)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.6)

### 1.12.5 (Released 2018-10-25)
Blood glucose values imported from Apple Health that were shown as HIGH BG or LOW BG in Tidepool Web generated a message that "Tidepool is stuck and isn't doing what you want it to do". With this fix, we unstuck Tidepool and it should do what you want it to do now.  
[Feature Requirements](https://trello.com/c/pQv4sTKV/22-uncaught-typeerror-on-high-bg-annotations-in-data-imported-through-tidepool-mobile)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.5)

### 1.12.4 (Released 2018-10-03)
While testing an update to Tidepool Mobile, we noticed an error that occurred when attempting to mouse over a time change icon. And this, my friends, is why we have a rigorous testing process.    
[Feature Requirements](https://trello.com/c/5stilew8/2-uncaught-typeerror-in-timechange)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.4)

### 1.12.3 (Released 2018-09-12)
We added timestamps to CGM data in your Daily View. With so many dots to keep track of, this should help keep things organized.  
[Feature Requirements](https://trello.com/c/QWz76ymx/16-s-data-viz-daily-view-cgm-add-timestamp-in-hover)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.3)

### 1.12.2 (Released 2018-09-11)
Our new Content Security Policy caused issues for people trying to create a Tidepool account on Safari. Sorry about that one, that's been fixed. (You still need to use Chrome to view the data.)  
[Feature Requirements](https://trello.com/c/GbuHSNpr/13-csp-error-in-safari)

While we are working on an update to Tidepool Mobile, we noticed a bug that caused Tidepool Web to crash. It's nothing you would have encountered unless you are running a test build of Tidepool Mobile, but we fixed this nonetheless.  
[Feature Requirements](https://trello.com/c/VNKOkCOY/14-uncaught-typeerror-when-navigating-tidepool-web-after-uploading-via-mobile-bld-211-522)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.2)

### 1.12.1 (Released 2018-09-04)
We identified and resolved a bug that showed unnecessary scrollbars on our warning page in browsers that are not named Google Chrome.  
[Feature Requirements](https://trello.com/c/YHTZsl3m/3-scrollbars-in-browser-warning-page-on-safari)

We also identified and resolved a bug that displayed our fun loading animation for accounts with no data.  
[Feature Requirements](https://trello.com/c/9GOEnw99/4-loading-dots-briefly-showing-over-the-upload-data-screen)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.1)

### 1.12.0 (Released 2018-08-28)
Our friends from Diabeloop submitted PRs to translate Tidepool Web into French. Le diabète, après tout, ne connaît pas de frontières.  
[Feature Requirements](https://trello.com/c/wJgM1E2H/48-tidepool-web-internationalization)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.12.0)

### 1.11.9 (Released 2018-08-22)
We found a bug that incorrectly reported basal suspends for Medtronic 600-series pumps. And promptly fixed it.  
[Feature Requirements](https://trello.com/c/uzlypSjy/46-tidepool-web-basics-page-shows-incorrect-suspend-count)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.9)

### 1.11.8 (Released 2018-08-09)
Care team members and clinicians can now set the site change source for accounts they are viewing if no preference has been selected, for the duration of their session.  
[Feature Requirements](https://trello.com/c/DVZk4lg8/41-new-card-allow-care-team-members-to-assign-infusion-site-change-rule)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.8)

### 1.11.7 (Released 2018-07-31)
In preparation for Medtronic 630G, 640G, and 670G support, we resolved a bug that incorrectly rounded delivered dosage data.  
[Feature Requirements](https://trello.com/c/9ET8Bmh5/26-setting-6-series-bolus-increment-to-025-causes-loss-of-precision-due-to-rounding)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.7)

### 1.11.6 (Released 2018-07-31)
As part of our preparation for Medtronic 630G, 640G, and 670G support, we resolved a bug that incorrectly displayed some blood glucose readings as calibrations.  
[Feature Requirements](https://trello.com/c/u8Fid7Hi/23-600-series-bug-remove-calibration-yes-annotation)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.6)

### 1.11.5 (Released 2018-07-30)
A bug that showed too many decimal places in our print view came back, so we fixed it again.  
[Feature Requirements](https://trello.com/c/4vRWbVp3/20-daily-print-view-bg-tick-formatting-regression)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.5)

### 1.11.4 (Released 2018-07-19)
For clinicians logged into Tidepool, we've added an upload link in the header that launches the Tidepool Uploader (if it's installed). Sometimes little things like that make all the difference.  
[Feature Requirements](https://trello.com/c/W6P8MTyq/11-upload-links-on-careteam-accounts)

For our friends in Auto Mode, your bolus tooltips will show "Target: Auto" to make it clear that Auto Mode has the wheel.  
[Feature Requirements](https://trello.com/c/krrF6CmW/12-when-in-auto-mode-boluses-that-include-bg-data-list-target-null-in-the-tooltip)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.4)

### 1.11.2 (Released 2018-07-02)
We received an external contribution [from @rspier](https://github.com/rspier) to fix some formatting when you use Chrome's Print functionality (not our PDF fun).  
[Feature Requirements](https://trello.com/c/OKhvKFnN/2-display-date-range-when-using-browser-print-functionality)

Turns out the Basal:Bolus ratio widget is still important to clinicians when looking at 670G data, so we added that back to the Basics view. Welcome back!  
[Feature Requirements](https://trello.com/c/NFKNjxyT/3-add-basalbolus-ratio-to-basics-view-for-automated-basal-devices)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.2)

### 1.11.1 (Released 2018-06-26)
Once we started putting our 670G visualization through the paces, we noticed a few scenarios that caused the Time in Auto Mode widget to be off in the Weekly view. We're looking to squash as many bugs as we can find before releasing this to the world, including this one.  
[Feature Requirements](https://trello.com/c/Jpi5YurW/24-weekly-view-aggregated-stats-calculations-are-off)

Previously, there was no way to dismiss the prompt to download the Tidepool Uploader. A little X in the corner makes all the difference.  
[Feature Requirements](https://trello.com/c/XstB6XTv/25-add-a-dismiss-option-to-the-tidepool-uploader-launch-popup)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.11.1)

### 1.11.0 (Released 2018-06-19)
If we're going through all the trouble to upload your Medtronic 630G, 640G, and 670G data, it's got to look good, too.  

Auto mode is a great feature, but it needed a fresh design to stand out from Manual mode.  
[Feature Requirements](https://trello.com/c/AV4gpns3/16-change-how-viz-renders-auto-mode-for-670g-support)

When Auto mode is active during an upload, we'll include that schedule in your pump settings.  
[Feature Requirements](https://trello.com/c/Ni2DGNEI/17-show-auto-mode-schedule-on-settings-view-when-is-active-at-time-of-upload)

There are a lot of different annotations and designations for blood glucose readings coming off a 670G. It took some time to sort it all out, but we're good now.  
[Feature Requirements](https://trello.com/c/X5TCJYAZ/18-tideline-handle-annotations-for-600-series-smbgs)

Auto mode also means updating our basal labels on the Daily View.  
[Feature Requirements](https://trello.com/c/h1QqftJ6/19-map-for-auto-mode-and-regular-mode)

And adjusting the data we show in Basics to show Auto mode exits rather than every temp basal issued. We lost count at 1,000 when in Auto mode, so this seemed like a better solution. We will still show temp basal events in Manual mode, though.  
[Feature Requirements](https://trello.com/c/XwEYulG5/20-basal-events-in-basics-view-for-670g)

We added a "Time in Auto mode" widget to replace the Basal:Bolus ratio widget for 670G users. We hope you like it!  
[Feature Requirements](https://trello.com/c/nvJocRRd/21-viz-for-670-add-time-in-and-out-of-auto-mode-agg-stat-and-limit-basalbolus-ratio-agg-stats-to-manual-mode-only)

Here's an interesting bug we discovered (and fixed): While in Auto mode, if you suspend and then resume your pump, that suspend event was displaying as an Auto mode exit. We have to do a little more work to improve how that is visualized, but making sure your data is not misrepresented is the important bit. Polish will surely follow.  
[Feature Requirements](https://trello.com/c/UCwOXfD7/22-fix-auto-mode-suspends-so-that-they-occur-without-an-auto-mode-exit-as-indicated-from-pump-data)

An external contributor updated some of our documentation, and because we're an easy-going open source project, we've merged that pull request. Thanks [Mikael!](https://github.com/mrinnetmaki)  
[Feature Requirements](https://trello.com/c/Ms9tymZg/15-document-email-verification-bypass-key-for-docker-setup-in-blip-repo)

### 1.10.9 (Released 2018-06-07)
We found a bug that prevented a certain Tidepool Community Manager from logging into our demo accounts James and Jill Jellyfish (no relation). That's been fixed.  
[Feature Requirements](https://trello.com/c/4lnTETQg/4-cannot-log-in-to-jill-jellyfish-account)

An International Consensus Panel has updated recommended blood glucose thresholds. They seem pretty legit, so we've updated the default very low, low, in target, high, and very high thresholds for new accounts. Of course, you can change your target range to your liking any time you want.  
[Feature Requirements](https://trello.com/c/OKj6Qyji/5-update-glucose-defaults-to-reflect-standards)

While we were updating default thresholds, we noticed a bug that caused the y-axis labels on the daily view to disappear. Ah, the joys of writing code. We fixed that one too.  
[Feature Requirements](https://trello.com/c/EJQHZnyv/6-bg-ticks-missing-in-daily-view-y-axis-for-mg-dl-display)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.9)

### 1.10.7 (Released 2018-05-14)
We’ve added the AADE Foundation to our Tidepool Big Data Donation Project Nonprofit Partner list. Now you can choose to support an organization providing resources to diabetes educators with your data donation.  
[Feature Requirements](https://trello.com/c/LWBzvGMr/2-add-aade-foundation-to-tidepool-big-data-donation-project-nonprofit-partner-list)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.7)

### 1.10.5 (Released 2018-04-17)
We noticed when some of you created a new account and we displayed a message that says “Please click on the link in the email we sent you at (your email address)” and (your email address) would up on two lines if it was really long. We fixed that bug so now (your email address) shows up on its own line. Little details like that are important.  
[Feature Requirements](https://trello.com/c/67H081EV/8-put-the-email-address-on-its-own-line-so-it-doesnt-get-split)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.5)

### 1.10.4 (Released 2018-04-09)
Sometimes data lines in the Trends view would appear to loop around, like a weird connect-the-dots drawing. We fixed that.  
[Feature Requirements](https://trello.com/c/98DFbS3V/4-overlapping-lines-in-trends-view)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.4)

### 1.10.3 (Released 2018-03-29)
For users signing up for a new account, you can now see which email address you signed up with on our email verification page. If you accidentally typed in the wrong address (it happens to the best of us), you’ll know right away instead of wondering where that account verification email went.  
[Feature Requirements](https://trello.com/c/1qAvkpEH/24-blip-show-email-address-as-part-of-signup-email-verification)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.3)

### 1.10.2 (Released 2018-03-28)
The printed Daily view started at 5:00pm rather than midnight. Sure, time zones are weird, but we fixed that so everyone’s day starts at a logical time.  
[Feature Requirements](https://trello.com/c/dr9EeALW/22-print-view-daily-view-starts-at-5pm-rather-than-midnight)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.2)

### 1.10.1 (Released 2018-03-19)
For anyone whose iPhone CGM upload is 8 weeks (or more) more recent than a pump upload, there was a small chance you may not be able to see any of your data based on how we process things behind the scenes. We cleaned up that order of operations and you can see your data again.  
[Feature Requirements](https://trello.com/c/Inr3LmRc/18-cgm-only-from-dexcom-api-results-in-non-rendered-viz-due-to-data-paging)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.1)

### 1.10.0 (Released 2018-03-19)
Tired of watching those dots bounce while your data loads? Frustrated at seeing “Aw, snap!” if Tidepool gets bogged down by all the data you’ve uploaded? Welcome to Data Paging. We’ve reengineered how we visualize your data, reducing the time spent waiting for your data to load. Scrolling through the days/weeks is also faster, too.  
[Feature Requirements](https://trello.com/c/jmYSnJCw/15-tidepool-web-data-paging)

Along the way, we made some adjustments to how and where your bolus tooltips display.  
[Feature Requirements](https://trello.com/c/MI81nZVi/16-bolus-tooltips-are-displaying-below-the-basal-rates-in-daily-view)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.10.0)

### 1.9.9 (Released 2018-02-27)
Data with no above-target values caused the Y scale on our daily view to be off. That's not the way we should treat no-hitters - we fixed that bug.  
[Feature Requirements](https://trello.com/c/1r7IxaiA/19-data-with-no-above-target-values-causes-y-scale-on-daily-view-to-be-off)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.9)

### 1.9.8 (Released 2018-02-23)
Introducing our Bolus Tooltip. This will help you identify the difference in insulin delivery for a meal or a correction. Hover over any bolus in the daily view to see all your pump settings and a breakdown of insulin delivered.  
[Feature Requirements](https://trello.com/c/vFjtdPcF/17-xl-data-viz-daily-differentiate-bolus-from-carbs-vs-bolus-from-correction)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.8)

### 1.9.7 (Released 2018-02-14)
We've added two new organizations you can support through the Tidepool Big Data Donation Project: DYF, and DiabetesSisters. We've also removed Diabetes Hands Foundation from the list of nonprofit organizations. We will miss their advocacy efforts, but are glad to see Beyond Type 1 pick up the baton and carry on their legacy.  
[Feature Requirements](https://trello.com/c/F056JnEQ/8-update-big-data-nonprofit-partners)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.7)

### 1.9.6 (Released 2018-02-14)
In the sign up process for people with diabetes between 13 and 17 years old, the text on the Privacy Policy and Terms of Use verification page said "you'll need to have a parent or guardian agree to the terms on the next screen"...which wasn't quite right. We fixed that to say "you'll need to have a parent or guardian agree to the terms below."  
[Feature Requirements](https://trello.com/c/8iv5C67V/5-text-on-privacy-policy-tou-verification-page-should-change-from-next-screen-to-below)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.6)

### 1.9.5 (Released 2018-02-13)
In an effort to better respect and reflect the diversity of the Tidepool community, you can now indicate your diabetes diagnosis: type 1, type 2, gestational, pre-diabetes, LADA, parent, or other. It's optional, but it's also important.  
[Feature Requirements](https://trello.com/c/VyoPE9t0/4-xs-demographic-data-inclusion-phase-1)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.5)

### 1.9.4 (Released 2018-01-22)
Because the FreeStyle Libre only samples blood glucose data every 15 minutes instead of every 5 like other CGMs, our blood glucose distribution widget didn't factor in Libre data as CGM values. We've fixed that bug and adjusted the criteria for CGM data required for that widget. Sorry about that one.  
[Feature Requirements](https://trello.com/c/FNlDqd1A/11-libre-data-not-being-calculated-due-to-insufficient-data-points)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.4)

### 1.9.2 (Released 2018-01-04)
Folks trying to verify their Tidepool account that was initially created by their clinician were encountering an error message. For what it's worth, their account was still verified, but no one wants to see unwarranted error messages. We don't want to be the developers who cried wolf. That bug has been fixed.  
[Feature Requirements](https://trello.com/c/HKrQyncg/2-error-when-home-verifying-a-vca-created-account)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.2)

### 1.9.1 (Released 2017-12-18)
As much as we love how your data looks in Tidepool, some people need to hold the data in their hands. So we've expanded our print feature to include Basics, the last six Daily views, and Device Settings.  
[Feature Requirements](https://trello.com/c/zdyHFY0M/8-l-blip-print-view-combined-basics-daily-settings)

Part of our work on the expanded print view included a fix to a bug that caused three-digit carb counts to be truncated. Sorry about that.
[Feature Requirements](https://trello.com/c/t777c7sr/10-daily-view-print-view-cannot-show-bolus-wizard-carb-counts-99g-only-shows-two-digits#)

Hovering on a blood glucose value in development mode would cause our app to crash. We don't want to treat our developers like that, so we fixed this bug.  
[Feature Requirements](https://trello.com/c/6gwIwBIF/9-bugfix-for-redux-state-mutation-violation-crashes-app-in-dev-mode)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.9.1)

### 1.8.3 (Released 2017-11-30)
Our Daily Print View kept spinning for some accounts, like a perpetual merry-go-round. This bug has been fixed.  
[Feature Requirements](https://trello.com/c/G3u35uCo/24-bug-print-option-fails-to-load-with-latest-release)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.8.3)

### 1.8.2 (Released 2017-11-29)
The initial text we used to promote the Dexcom API connection was a bit confusing. We gave that green banner a copy-edit. Hopefully this clears things up. Remember how we debuted this feature the day before? That's why we call this a "hot fix".  
[Feature Requirements](https://trello.com/c/1H3rwUBF/22-update-text-on-dexcom-api-banner)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.8.2)

### 1.8.1 (Released 2017-11-28)
We recently added the ability to connect your Dexcom account to Tidepool, automatically importing the data you upload to Dexcom Clarity. Most of this work happened on the back end, but we needed a way to easily notify you about this beyond emails, tweets, and Facebook posts. So here's a banner at the top of your page to keep you in the loop (you can dismiss it if you want, no worries.)  
[Feature Update](https://trello.com/c/YzpXRsNJ/16-dexcom-api-messaging-banner)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.8.1)

### 1.7.0 (Released 2017-11-06)
We noticed some legacy code that got in the way of our Dexcom API connection. That's been resolved now.  
[Feature Requirements](https://trello.com/c/y3HJ5D4k/4-deployment-details-2017-11-06-dexcom-api)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.7.0)

### 1.6.13 (Released 2017-10-24)
We updated the Bolus data on Basics to let you filter between override and underride boluses. The suggestion for manual boluses to be renamed to "letitride", however, was not approved.  
[Feature Requirements](https://trello.com/c/pQaZdjad/21-data-viz-basics-distinguish-override-up-from-override-down)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.13)

### 1.6.12 (Released 2017-10-18)
We noticed a bug that displayed out of range CGM values as HI/LO and out of range blood glucose meter values as +1/-1 the threshold (for example, if the high threshold for a meter was 600, we would put the value at 601). In fixing this bug, we want these values to behave similarly, so now everything will display HI/LO or High/Low depending on the available space. Keeps things simple.  
[Feature Requirements](https://trello.com/c/p7L3RoNn/19-hi-lo-smbg-shows-actual-value-instead)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.12)

### 1.6.11 (Released 2017-10-17)
We used to only show data on Basics if you uploaded at least once from a supported insulin pump. That didn't seem fair to non-pumpers, so now we'll show relevant portions in Basics based on whatever you've uploaded.  
[Feature Requirements](https://trello.com/c/s3LiAYnA/5-s-blip-allow-basics-use-regardless-of-type-of-data-available)

An outside contributor submitted a quick fix for a link in one of our ReadMe files.  
[Feature Requirements](https://trello.com/c/vmZ1w6iG/7-fix-mocha-link-in-blip-readme)

Did we mention Tidepool is open source? We also added an outside contribution that fixed some of our development tools.  
[Feature Requirements](https://trello.com/c/1tLMULDP/6-restore-dev-tools-by-default-in-blip)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.11)

### 1.6.8 (Released 2017-09-27)
With the release of Tidepool Mobile on iOS, we're updating references across the Tidepool platform, like this one after you've created your account and signed up for data storage.  
[Feature Requirements](https://trello.com/c/dkz5JMMg/39-xs-update-blip-notes-reference-to-tidepool-mobile-in-blip)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.8)

### 1.6.6 (Released 2017-09-18)
Continuing the post-Tidepool Uploader 2.0 launch, when you logged into your account, clicking "Upload" will either launch the Tidepool Uploader if you have it installed on your computer or prompt you to download the latest version of the Uploader if you haven't installed our software yet. Similarly, if you've installed the Tidepool Uploader before creating your account (kudos for jumping ahead in the fun), you'll be able to launch the Uploader from your browser after you've created your Data Storage Account. It only makes sense that the Upload links and buttons actually get you to the Uploader, right?  
[Feature Requirements](https://trello.com/c/CTjNregM/36-490-electron-uploader-blip-integration)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.6)

### 1.6.2 (Released 2017-09-12)
Dear friends of Millie Mol, you can now change your display settings in your profile to display your blood glucose data in mmol/L.  
[Feature Requirements](https://trello.com/c/FJKBYCVt/28-proper-support-for-mmol-l)

When we first implemented the ability to display blood glucose data in mmol/L, it was a feature for internal-use only, and we noted a number of display bugs unique to that unit measurement. Along the way to rolling mmol/L out to all of you, we cleaned up those display bugs.  
[Feature Requirements](https://trello.com/c/iUicHdcW/29-blip-regression-display-bugs-when-in-mmoll-mode)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.6.2)

### 1.5.22 (Released 2017-09-07)
Tidepool is free for people with diabetes and clinicians. That won't change. But the sign up process is a little different for those groups - and the signup forms should reflect that. So, we've updated the signup forms for new accounts to better distinguish between patient and clinician users.  
[Feature Requirements](https://trello.com/c/yY5mTQjf/20-new-account-creation-forms-for-clinic-and-personal)

Because we love our developers (internal and external), we've made a bunch of improvements and fixes for the Docker development experience. Technically this isn't a user-facing update, but it doesn't hurt to remind you that we care about the engineering process and we value your feedback on stuff like this.  
[Feature Requirements](https://trello.com/c/PnIamSBO/21-improvements-and-fixes-for-the-docker-development-experience)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.22)

### 1.5.17 (Released 2017-08-30)
If you go more than 72 hours without a bolus from your insulin pump, we will hide the aggregated stats on our Basics view. Our previous threshold used to be 24 hours, be we decided to extended this out to a full 3-day vacation. Sometimes it's nice to take a break.  
[Feature Requirements](https://trello.com/c/4wrRxoqr/15-extend-24-hours-no-bolus-rule-on-basics-to-72-hours)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.17)

### 1.5.13 (Released 2017-08-16)
Time zone changes during a temp basal on supported Medtronic pumps were not treated with the utmost care. Yes, this is a highly specific scenario, but we can do better. This bug has been fixed.  
[Feature Requirements](https://trello.com/c/bnEjgwgW/13-uploaderbug-medtronic-direct-upload-with-erics-pump-fails-platform-validation)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.13)

### 1.5.12 (Released 2017-08-07)
We added a banner to help existing users share their data with the Tidepool Big Data Donation Project. Sharing, after all, is caring.  
[Feature Requirements](https://trello.com/c/v8ovj43d/9-share-with-big-data-donation-ui-on-share-page)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.12)

### 1.5.10 (Released 2017-08-02)
Ordinarily, it's up to people with diabetes to share their data with their health care providers. But sometimes duplicate accounts are created and clinicians need to have the ability to remove those accounts from their patient list to minimize confusion. So we did that.  
[Feature Requirements](https://trello.com/c/LIrmtf4i/5-remove-clinic-account-from-care-team)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.10)

### 1.5.7 (Released 2017-08-01)
We got reports that our PDF print view was rendering oddly in some applications. It took some sleuthing, but we've cleaned up our code and fixed those bugs. You're clear to print and save those PDFs to your heart and hard drive's content.  
[Feature Requirements](https://trello.com/c/oVpqm14o/3-pdfs-do-not-show-data-visualization-in-some-apps)

Along the way to fixing those PDF print view rendering bugs, we noticed that basal rates were showing up beyond the last upload time in the PDF print out. That was a bit presumptive of us, so now we're only showing basal data in print outs up to the last upload time.  
[Feature Requirements](https://trello.com/c/hLh5RXez/2-daily-print-view-renders-basal-beyond-the-latest-pump-data-upload-time)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.7)

### 1.5.5 (Released 2017-07-24)
When you log into your Tidepool account, we will show you the view (Trends, Basics, Weekly) with your most recent data. This should help you get to the good parts sooner.  
[Feature Requirements](https://trello.com/c/V8EvYZPi/27-m-people-get-confused-with-basics-date-being-pump-upload-date-when-they-have-more-recent-cgm-data)

Previously, clicking "Refresh" would sometimes cause an "infinite refresh" that never finished. Since the phrase "infinite crisis" was already in use elsewhere in pop culture, figured it was best to fix this bug.  
[Feature Requirements](https://trello.com/c/3HKSQKm5/26-refresh-button-data-load-hangs-forever)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.5)

### 1.5.1 (Released 2017-07-06)
We get it, sometimes you want to hold your data in your hands. Now you can click the "Print" icon in the Daily view and generate a PDF of your last 6 days of uploaded data. This can be printed, or saved to an EHR, or used in some other creative way PDFs can be used. This is just the beginning - we have grand plans for this feature in the future.  
[Feature Requirements](https://trello.com/c/tjD655oz/2-l-blip-daily-print-view-first-production-release)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.5.1)

### 1.4.7 (Released 2017-06-21)
The "US/Pacific-New" timezone selection caused some trouble. Even though there isn't a realistic reason to use that timezone, the bug needs to be fixed to make sure data shows up as expected. And here we are.  
[Feature Requirements](https://trello.com/c/w6UgCOQS/13-blipbug-dexcom-data-disappearing-after-carelink-data-uploaded)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.4.7)

### 1.4.4 (Released 2017-06-21)
Creating clinician accounts used to be manual process. Now, it's automated. It's likely you won't appreciate this one nearly as much as we do, but it's still a worthy thing to document. We've also automated our welcome email to new clinician accounts as well. Yay automation!  
[Feature Requirements](https://trello.com/c/iYshJmxh/10-new-sign-up)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.4.4)

### 1.4.1 (Released 2017-06-06)
We're rolling out the ability to upload Medtronic data directly to Tidepool instead of importing from CareLink soon, this means we need to be able to deduplicate overlapping data. Your aggregate statistics are most appreciative of this update.  
[Feature Requirements](https://trello.com/c/3uyQGXo9/6-medtronic-direct-deduplication)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.4.1)

### 1.3.8 (Released 2017-05-23)
We reduced the number of clicks it takes to donate your data through the Tidepool Big Data Donation Project.  
[Feature Requirements](https://trello.com/c/ZnKDZhzv/10-improve-the-experience-sharing-with-big-data-donation-upon-tidepool-signup)

We added Medtronic-specific annotations to basal data to ensure you know your Medtronic data is, in fact, Medtronic data.  
[Feature Requirements](https://trello.com/c/dwFcsbNp/11-add-support-for-medtronic-direct-specific-annotations)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.3.8)

### 1.3.4 (Released 2017-05-16)
A bunch of information on Device Settings disappeared after a recent release. Not sure how that happened, but we waved a wand (and fixed the bug) and brought that info back. Abracadabra!  
[Feature Requirements](https://trello.com/c/Q15cxITp/7-bug-header-info-regression-on-device-settings-print-view)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.3.4)

### 1.3.3 (Released 2017-05-01)
One of our updates froze the Device Settings page for Tandem users. We thawed that one out and fixed that bug. Sorry about that one.  
[Feature Requirements](https://trello.com/c/Tn72jJMW/2-viz-bug-view-settings-for-tandem-device-hangs-on-v0714)

[Code Update](https://github.com/tidepool-org/blip/releases/tag/v1.3.3)
<hr>

## Tidepool Uploader
Visit [https://tidepool.org/uploader](https://tidepool.org/uploader) to download the latest version of the Tidepool Uploader.  

If you already have the Tidepool Uploader installed on your computer, it will automatically update to the latest version.  

### 2.9.0 (Released 2018-12-17
After resolving a very, very specific dependency, Tidepool Uploader now supports Verio and Verio Flex meters on Windows.  
[Feature Requirements](https://trello.com/c/QvV3uNlH/31-uploader-verio-verio-flex-on-windows)

Tidepool Uploader used to get upset if it tried to read the Active Time Insulin setting on a Medtronic pump that didn't have the bolus wizard enabled. This bug has been resolved, and Tidepool Uploader is much happier now.  
[Feature Requirements](https://trello.com/c/oMvCEXJV/32-dont-read-active-insulin-time-pre-bolus-wizard-medtronic-5-7-series)

Tidepool Uploader also used to get upset when it encountered corrupted data on Tandem pumps. We've updated the driver to avoid corrupted data, paving the way for more successful uploads.  
[Feature Requirements](https://trello.com/c/WsbPV97L/33-tandem-driver-gets-stuck-when-packets-are-corrupted)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.9.0)

### 2.8.6 (Released 2018-11-26)
We've added a silent option for the Windows installer allowing Tidepool Uploader to be installed from the command line. IT staff at clinics using Tidepool will really appreciate this one.  
[Feature Requirements](https://trello.com/c/aFhdUDcq/16-windows-installer-for-uploader-should-install-usb-driver-silently)

We changed how Tidepool Uploader handles the "matching bolus wizard for bolus event not found" scenario. Uploads for Medtronic 600-series pumps should be a bit more graceful now.  
[Feature Requirements](https://trello.com/c/bZMPsQYV/17-600-series-upload-error-matching-bolus-wizard-for-bolus-event-not-found)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.8.6)

### 2.8.4 (Released 2018-11-01)
Older Medtronic pumps with a whole lot of CGM data (32 pages, to be precise) would only upload the most recent page of data for a delta upload (reminder: delta uploads are uploads that only consist of new data). This bug has been fixed, and now Tidepool Uploader will capture all CGM data on older Medtronic pumps since the last upload.  
[Feature Requirements](https://trello.com/c/yRKhiyqw/2-5-7-series-only-most-recent-cgm-data-with-delta-upload-if-more-than-32-pages-of-cgm-data-available)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.8.4)

### 2.8.3 (Released 2018-10-25)
We identified a bug that prevented users attempting to upload Verio and Verio Flex meters on macOS Mojave. The fix? On Mojave, there's an extra space in a driver path that we were looking for...seriously. This bug is squashed.  
[Feature Requirements](https://trello.com/c/VsEy6yN2/25-verio-verio-flex-not-uploading-on-macos-mojave)

Tidepool Uploader is now bringing in additional pump settings for supported insulin pumps. Now we can get to work on displaying those additional settings in Tidepool Web. Stay tuned!  
[Feature Requirements](https://trello.com/c/7hyUrl24/24-additional-pump-settings)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.8.3)

### 2.8.0 (Released 2018-10-18)
We found that initial uploads for Medtronic 630G, 640G, and 670G were taking a bit longer than we would like, especially for folks uploading in a clinic. So we added a 4-week filter to for Medtronic 630G, 640G, and 670G uploads. This should move things along nicely.  
[Feature Requirements](https://trello.com/c/DTDsGtkK/11-add-4-weeks-filter-to-tidepool-uploader-for-medtronic-600-series-uploads)

Here's the thing: Sometimes you don't notice those requests to install or approve drivers from a new piece of software. It happens. The problem is, if those drivers for Tidepool Uploader aren't installed some of your devices won't upload. That's no fun. Now Tidepool Uploader on macOS will fall back to user-space drivers we wrote that will support Animas pumps, and Ultra2, Ultra Mini, or VerioIQ meters if we detect the original drivers were not installed. Pretty cool, huh?  
[Feature Requirements](https://trello.com/c/nabqaof0/10-devices-apple-high-sierra-kernel-extension-issues)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.8.0)

### 2.7.7 (Released 2018-10-11)
Attention folks running Tandem's new Basal-IQ feature, Tidepool Uploader now supports your fancy new automatic basal suspend events. We're also able to upload Dexcom G6 data coming off a t:slim X2. Your Friendly Neighborhood Tidepool Community Manager is particularly excited about this one.  
[Feature Requirements](https://trello.com/c/fNigcnTf/7-tandem-predictive-low-glucose-suspend)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.7)

### 2.7.6 (Released 2018-09-13)
We fixed a bug in our blob storage service that tried to send an empty data field to our database. Why send an empty box when you can just save on postage and send nothing at all?  
[Feature Requirements](https://trello.com/c/0IAOAEXl/19-uploader-shouldnt-send-clientprivate-when-theres-no-content)

We linked Rollbar with our support ticketing system, which is great! But it wasn't always linking the email address that generated a Rollbar error, which was not great! But now we fixed that and now we'll be able to see usernames with Rollbar errors, which will help us troubleshoot future problems, which is great!  
[Feature Requirements](https://trello.com/c/NAUHEvHf/18-rollbar-should-always-record-username)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.6)

### 2.7.5 (Released 2018-09-06)
It's another bug bash bonanza!  

HID errors will now fail more gracefully. Admittedly, this is an awkward concept to wrap your head around, but this will help us figure out what is really going on.  
[Feature Requirements](https://trello.com/c/i95g40KF/7-uncaught-error-cannot-write-to-hid-device)

We fixed a bug that caused an error if an upload only included the second part of a dual bolus. Glad that's settled.  
[Feature Requirements](https://trello.com/c/9mr5tbHX/8-5-7-series-error-details-expected-a-square-bolus-but-found-type-4)

And, we fixed a bug that occurred when a 24-hour temporary basal rate was followed by a basal suspend.  
[Feature Requirements](https://trello.com/c/dtJAGcvz/9-5-7-series-if-a-24-hour-temp-basal-is-followed-by-a-suspend-the-duration-of-the-temp-basal-is-incorrect)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.5)

### 2.7.4 (Released 2018-08-10)
The Tidepool Uploader now saves not only processed data, but also the raw binary data retrieved from FreeStyle Libre and all supported Medtronic pumps. The data is stored in a secure location for our engineers to access when it's time to squash some bugs or if re-processing is required. Between this and Rollbar support we added last November, turnaround times from us on bug fixes should improve tremendously.  
[Feature Requirements](https://trello.com/c/DMgzvGOL/39-store-upload-blobs)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.4)

### 2.7.3 (Released 2018-08-06)
A few of our external beta tests identified some new bugs once 2.7.2 went live. This is why we test.  

If the first basal event found in the pump history was a temp basal start event without a proper scheduled basal start event, things went wrong. We fixed this bug.  
[Feature Requirements](https://trello.com/c/7hNQG2qR/34-600-series-bug-dont-create-temp-basals-when-missing-suppressed-basal-info)

If the first basal event found in the pump history was a suspend event, things went wrong. We fixed this bug, too.  
[Feature Requirements](https://trello.com/c/FQ2o9TgH/35-600-series-bug-throw-away-any-isolated-initial-suspends)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.3)

### 2.7.2 (Released 2018-08-01)
At last, Tidepool fully supports Medtronic MiniMed 630G, 640G, and 670G. As you can imagine, a lot of work went into this one. How much work, you ask? (Deep breath)  

The Tidepool Uploader needed to convert 670G micro boluses in auto mode to basal rates.  
[Feature Requirements](https://trello.com/c/SmmJ2c2W/2-update-uploader-to-support-auto-mode-for-670g)

And handles low glucose suspend events properly.  
[Feature Requirements](https://trello.com/c/pPZVg7BE/11-6-series-lgs)

A lot of work went into the messaging and error handling around our ad-hoc pairing functionality.  
[Feature Requirements](https://trello.com/c/6ESCkbTM/5-600-series-bug-throw-an-error-when-attempting-to-use-ad-hoc-pairing-with-a-linked-meter)

Medtronic 630G, 640G, and 670G also benefits from our "delta upload" functionality. This means after the first upload, future uploads go by much quicker since we're only capturing new data.  
[Feature Requirements](https://trello.com/c/K4dHbrSj/9-s-new-card-6-series-delta-upload)

We ensured the raw data coming off 670G is preserved.  
[Feature Requirements](https://trello.com/c/WZbBxkXA/7-670g-add-raw-microbolus-data-to-payload-field-of-basal-data-type)

Including the entire basal history.  
[Feature Requirements](https://trello.com/c/YM1TDo7Q/14-498-medtronic-6-history-message-basals-ux0-dev3-tst5)

Even when the basal schedule is...a lot.  
[Feature Requirements](https://trello.com/c/RWhAGJze/15-large-basal-schedules-should-not-cause-upload-failure)

The manual bolus history? We're getting all of that.  
[Feature Requirements](https://trello.com/c/asHiqHmq/18-502-medtronic-6-history-message-manual-bolus-ux0-dev3-tst3)

Bolus calculator history, too.  
[Feature Requirements](https://trello.com/c/pdOnbBwT/20-501-medtronic-6-history-message-calculator-bolus-ux0-dev5-tst3)

And all those recorded and reported blood glucose readings? Yup.  
[Feature Requirements](https://trello.com/c/BHVmZoHh/19-500-medtronic-6-history-message-smbg-values-ux0-dev3-tst3)

Can't forget about reservoir changes and prime event history, can we?  
[Feature Requirements](https://trello.com/c/YwljcNPd/21-uploader-6-series-rewind-prime-device-events)

CGM data? You're coming too.  
[Feature Requirements](https://trello.com/c/HNi2st5n/29-497-medtronic-6-history-message-cgm-data-ux0-dev3-tst3)

No matter what time zone you live in, our UTC bootstrapping algorithm will ensure your data is in sync.  
[Feature Requirements](https://trello.com/c/vwaKLzuT/17-test-medtronic-6-utc-bootstrapping)

Our time check feature also helps with that.  
[Feature Requirements](https://trello.com/c/0bEUjkmz/25-xs-new-card-6-series-time-check)

New images and UI components were added to the Uploader for a paired and unpaired Contour Next Link 2.4 meter.  
[Feature Requirements](https://trello.com/c/9LBpSPxH/4-new-images-for-6-series-ui)

This includes a proper option to select in the "Choose Devices" section.  
[Feature Requirements](https://trello.com/c/BF1URAvH/31-494-s-uploader-medtronic-6-driver-copy-for-choose-devices-screen)

All of those data points need to be displayed correctly on the Basics view.  
[Feature Requirements](https://trello.com/c/JgH7y7VY/26-6-series-basics-page)

Device settings also are properly displayed.  
[Feature Requirements](https://trello.com/c/JkZGdYaS/28-6-series-device-settings)

Including all the different basal schedule profiles.  
[Feature Requirements](https://trello.com/c/CwJHioPO/32-504-medtronic-6-1b-implement-basal-profiles-ux0-dev3-tst1)

Over the course of internal and beta testing we documented and resolved a handful of bugs. Ready for those?  

We documented and resolved a bug that showed a previously scheduled basal extending into Auto Mode time if Auto Mode started with no insulin delivery.  
[Feature Requirements](https://trello.com/c/sYxXIylr/3-if-auto-mode-starts-with-no-microbolus-delivery-data-is-incorrect)

We also documented and resolved a bug that displayed temp basals during an upload as a scheduled basal.  
[Feature Requirements](https://trello.com/c/fk1vqCBz/6-uploader-600-series-when-a-temp-basal-starts-before-an-upload-data-is-uploaded-incorrectly)

To ensure we don't lose any raw data, we took extra measures to capture all of the microbolus data from 670G pumps.  
[Feature Requirements](https://trello.com/c/WZbBxkXA/7-670g-add-raw-microbolus-data-to-payload-field-of-basal-data-type)

We documented and resolved a bug that noted carb entries for a bolus were not being clearly identified with their respective bolus.  
[Feature Requirements](https://trello.com/c/hZSvQjMZ/10-daily-view-carb-circle-misaligned-with-related-bolus-bar)

We documented and resolved a bug that showed all blood glucose readings as linked even if they were manually entered.  
[Feature Requirements](https://trello.com/c/PTvtwFk1/12-on-the-630g-all-bg-readings-show-up-as-linked-even-when-manually-entered)

That one also impacted data displayed on Basics, but we fixed that, too.  
[Feature Requirements](https://trello.com/c/jnkv0yVy/24-multiple-bg-readings-showing-in-the-basics-view-for-each-actual-bg-reading)

We documented and resolved a bug that incorrectly visualized suspended basal events.  
[Feature Requirements](https://trello.com/c/T5x6YSN7/13-6-series-bug-basal-suspend-gaps-missing-from-daily-view)

We documented and resolved a bug that prevented a successful upload during an extended bolus.  
[Feature Requirements](https://trello.com/c/scIIYbAi/16-600-series-bug-cant-upload-while-an-extended-bolus-is-in-progress)

We documented and resolved a bug that displayed the wrong upload date in device settings.  
[Feature Requirements](https://trello.com/c/3rHnY5nM/23-wrong-upload-date-on-6-series-device-settings)

We documented and resolved a bug that dealt with unconfirmed blood glucose readings. It's tricky, but we worked through the different annotation scenarios to make sense of it all.  
[Feature Requirements](https://trello.com/c/1NJlI2cx/27-600-series-bug-document-that-pump-history-and-carelink-discard-unconfirmed-smbgs-and-that-two-smbgs-within-a-short-window-trunca)

We documented and resolved a bug that mixed up linked and manual blood glucose readings.  
[Feature Requirements](https://trello.com/c/lATExj5x/30-670g-importer-linked-and-manual-bg-readings-are-mixed-up)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.7.2)

### 2.6.5 (Released 2018-07-24)
It's another Bug Fix Bonanza™! This this round is devoted to supported Medtronic 5/7 pumps.  

We fixed a bug for pumpers who use dual/square boluses without extending their bolus (100%/0%).  
[Feature Requirements](https://trello.com/c/3zeww3MJ/14-error-details-expected-a-normal-bolus-but-found-type-4-or-5)

We fixed a bug that inelegantly handled how we display scheduled basal rates after a 24-hour temp basals. Things are much more elegant now.  
[Feature Requirements](https://trello.com/c/KToTSpZx/15-after-a-24-hour-temp-basal-resuming-scheduled-basal-sometimes-missing-in-5-7-series-pump-history)

We used to have problems with temp basals that were set to 0%. That's been fixed.  
[Feature Requirements](https://trello.com/c/RVSAh1aT/16-uploader-medronic-direct-no-rate-for-temp-basal-uploading-data-to-platform-failed)

Turns out CGM time change records can go "missing" if the pump has been switched off for a long time. We now handle this more gracefully.  
[Feature Requirements](https://trello.com/c/ow0JdtX9/17-missing-cgm-time-change-records-for-5-7-series)

And more dependency updates! Trust us, these are important.  
[Feature Requirements](https://trello.com/c/juzkA0SF/18-uploader-another-round-of-dependency-updates)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.6.5)

### 2.6.4 (Released 2018-07-11)
We noticed a bug that prevented all data from OneTouch Ultra 2 meters from being uploaded. Those readings can't hide from us! This has been fixed.  
[Feature Requirements](https://trello.com/c/m1Q6G5UO/5-ultra-2-not-uploading-all-available-data-on-meter)

For third party developers, and our own sanity, we added "cgm" device tags to upload records for pumps that integrate CGM data. (Waves to compatible Animas, Tandem, and Medtronic pumps!)  
[Feature Requirements](https://trello.com/c/W9oys1cy/6-uploader-add-cgm-to-devicetags-for-the-relevant-insulin-pumps-eg-tandem-g4-and-x2)

We also added a hotfix that addresses a frozen Uploader if the backend returns an error.  
[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.6.4)

### 2.6.2 (Released 2018-06-13)
We fixed some of our code that deals with identifying the right time for your data. If you really want to dig into our UTC Bootstrapping algorithm, send us an email and we'll talk your ear off about the challenges of device times.  
[Feature Requirements](https://trello.com/c/rh2oLpjR/8-apply-utc-bootstrapping-fix-for-invalid-timezone-offset-to-new-data)

We're running on node-serialport v6 now. You may not think that's exciting, but it fixes a bunch of issues for devices that connect through a serial port on Windows. And you thought dependency updates were boring? We now also automatically retry a serial port upload if it fails the first time. Boom!  
[Feature Requirements](https://trello.com/c/ucvNt5d0/9-update-node-serialport-to-version-6)

We fixed a bug in the OneTouch Ultra 2 driver which could cause some uploads to fail.  
[Feature Requirements](https://trello.com/c/o8yPVF7v/10-onetouch-ultra-2-typeerror-details-cannot-read-property-length-of-null)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.6.2)

### 2.6.0 (Released 2018-06-04)
Tidepool now supports Trividia Health True Metrix, True Metrix Go, and True Metrix Air blood glucose meters. If you're looking to upload one of those meters, we've combined them all under _Trividia Health True Metrix_ label in our devices list. Keeps things nice and tidy.  
[Feature Requirements](https://trello.com/c/OFkyZUe6/2-uploader-trividia-true-metrix-meters)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.6.0)

### 2.5.13 (Released 2018-05-31)
Currently we import IOB data from Medtronic pumps from the Bolus Wizard Estimate of Active Insulin. However, if a manual bolus or a square wave bolus is done without the Bolus Wizard, we don't get the IOB information. This data is listed at Unabsorbed insulin and is separate from the Bolus Wizard data. Medtronic also does not include active insulin data for any Bolus Wizard boluses without an associated BG value, despite it being listed in Unabsorbed insulin. By importing Unabsorbed insulin instead of Active insulin we will be able to provide our users with more data. All of this to say we fixed that bug and now use "Unabsorbed Insulin" instead of "Active Insulin" to display IOB.  
[Feature Requirements](https://trello.com/c/0jpKslMv/23-change-iob-data-from-medtronic-from-active-insulin-to-unabsorbed-insulin)

We’ve made more improvements to our Uploader error-logging service, Rollbar, this time to log data anomalies for any events with a time significantly in the past or future. Time travel isn't real (yet), this update should help us understand why some uploads have a date that is way, way off.  
Feature Requirements  
[Part 1](https://trello.com/c/HFxhx2Kh/22-log-data-anomalies-to-rollbar-for-any-events-with-time-field-significantly-in-past-or-future), [Part 2](https://trello.com/c/VhJD2moG/24-add-a-rollbar-notification-for-when-a-user-gets-the-timezone-conflict-message)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.13)

### 2.5.11 (Released 2018-05-16)
Tidepool now supports Medtronic Veo (554/754) insulin pumps. Yay!  
[Feature Requirements](https://trello.com/c/RU2SjKA1/7-uploader-support-medtronic-veo-554-754-pump)

Bayer was acquired by Ascencia, which means all those Contour meters needed some new branding. We’ve gone with Ascencia (Bayer) to minimize confusion.  
[Feature Requirements](https://trello.com/c/mt2xb3qV/6-update-bayer-listing-to-ascencia-bayer)

Our friends at Medtronic removed the ability to export your data as a CSV file. We relied on that functionality to import your CareLink data into Tidepool. As a result, we’ve removed the “Upload from CareLink option in the Tidepool Uploader” (Don’t worry, you can still upload directly to Tidepool with your Contour Next Link meter.)  
[Feature Requirements](https://trello.com/c/pz3ltLai/4-remove-upload-from-carelink-option)

Previously, if two consecutive square wave calculator boluses were given on a Medtronic 530G, the Tidepool Uploader interpreted the data as a single Dual Wave bolus with the extended portion of the bolus being the insulin total from the first bolus, which is listed as an override of the total. Also, if the last bolus on the 530G was a square wave bolus, data uploads failed. It’s hip to be square, but not that square - we’ve fixed both those bugs.  
[Feature Requirements](https://trello.com/c/izgIetxG/5-2-consecutive-square-calculator-boluses-are-merged-into-a-single-bolus-when-uploaded-from-medtronic-530g)

Finally, if an account was set up to upload devices on one operating system, you’d see “Unknown Device” on an operating system that does not support those devices. That bug has been squashed. We will remove that device from your upload options so it doesn’t look like you’re trying to upload mystery data.  
[Feature Requirements](https://trello.com/c/6XAazELZ/8-unknown-devices-appearing-in-uploader)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.11)

### 2.5.9 (Released 2018-04-26)
We changed some of the text for our Time Check pop up to make it clear that you should triple-check the time on the device you’re trying to upload. This is somewhere between a bug fix and a new feature. Feature fix?  
[Feature Requirements](https://trello.com/c/ERwBfwRa/19-update-device-time-check-copy)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.9)

### 2.5.8 (Released 2018-04-26)
Dependency updates aren’t always the most exciting thing, but we want to make sure you’re getting the latest and greatest software possible, so here we are with some shiny new code under the hood of the Tidepool Uploader.  
[Feature Requirements](https://trello.com/c/LLQU6QaX/17-new-card-uploader-dependency-updates)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.8)

### 2.5.7 (Released 2018-04-23)
Remember when we said we fixed that bug that prevented all your Libre data from uploading. This time we really, really fixed it. (To be fair, the FreeStyle Libre hasn’t been out that long, so it took a little time for larger datasets to come our way and push the boundaries of our code.)  
[Feature Requirements](https://trello.com/c/UUfpJcP2/10-reading-all-the-available-data-from-a-libre-device)

For our friends with a Medtronic pump using more than 6 or 7 insulin sensitivities or carb ratios, your uploads used to fail. We fixed that bug too.  
[Feature Requirements](https://trello.com/c/MyWefnN2/11-upload-fails-when-a-large-number-of-insulin-sensitivities-or-carb-ratios-are-specified)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.7)

### 2.5.5 (Released 2018-04-10)
Be honest, how diligent are you at keeping your devices on time when you travel? The Tidepool Uploader will now warn you if it detects a difference between the time zone selected and the time indicated on your device.  
[Feature Requirements](https://trello.com/c/1eAw2Fpc/2-uploader-timezones-as-a-user-i-want-the-uploader-to-warn-me-of-any-difference-between-my-device-time-and-computer-time-or-select)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.5)

### 2.5.4 (Released 2018-03-27)
We found a bug that prevented every last data point on your FreeStyle Libre from uploading. That won’t be a problem anymore.  
[Feature Requirements](https://trello.com/c/5KPT5Iim/20-libre-uploading-with-gaps-in-data)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.4)

### 2.5.3 (Released 2018-03-16)
We’ve made improvements to our Uploader error-logging service, Rollbar to catch more hiccups and bummers that you throw our way. This will go a long way to helping us identify areas we can improve our software.  
[Feature Requirements](https://trello.com/c/144Btqt7/13-uploader-improve-rollbar-logging)

Omnipod device settings were hell-bent on displaying as mg/dL even when uploaded from a pump set to mmol/L. We’d like to formally apologize to our friends who prefer blood glucose concentrations measured by molarity instead of weight - this bug has been fixed.  
[Feature Requirements](https://trello.com/c/KVEO8OTR/12-omnipod-showing-mg-dl-in-device-settings-even-after-mmol-l-selected-in-user-profile)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.3)

### 2.5.1 (Released 2018-03-12)
We gave upload times for Medtronic and Tandem pumps a serious speed boost. Prior to this update, we would pull all data off compatible Medtronic and Tandem pumps and sort out duplicate data later. Now, initial uploads of compatible Tandem and Medtronic pumps will show “We've improved how devices upload. This upload will take longer than usual, but your future uploads will be much, much faster.” After that, we will only upload new data. The result? A bolus from the Speed Force and much faster uploads in the future.  
Feature Requirements  
[Part 1](https://trello.com/c/gwB5pykC/3-uploader-delta-upload-ui), [Part 2](https://trello.com/c/5i0sM0wx/4-m-delta-uploads-3-of-4-medtronic-5-series), [Part 3](https://trello.com/c/ieSwFQq5/5-m-delta-uploads-4-of-4-tandem) 

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.1)

### 2.4.0 (Released 2018-02-22)
Tidepool now supports the Bayer Contour Next One blood glucose meter on Windows and Mac, and OneTouch Verio and Verio Flex blood glucose meters on Mac. (We're working on adding support for those two meters for our friends running Windows, stay tuned.)  
Feature Requirements  
[Part 1](https://trello.com/c/XGNCkEmk/14-add-support-for-bayer-contour-next-one), [Part 2](https://trello.com/c/UhxOg1zZ/15-uploader-onetouch-verio-and-verio-flex)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.4.0)

### 2.3.0 (Released 2018-02-15)
Clinicians and clinical study investigators, this one is for you: Tidepool now supports Abbott's FreeStyle Libre Pro.  
[Feature Requirements](https://trello.com/c/2sVdsOOU/10-libre-pro)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.3.0)

### 2.2.5 (Released 2018-01-08)
This is a bug bash bonanza.  

We were notified of a bug that generated a "could not find basal schedule" error that was caused by a basal rate created for pizza. We affectionately called this the "Pizza Basal Bug". This has been fixed.  
[Feature Requirement](https://trello.com/c/0yXu3EPF/9-pizza-basal-bug)

Medtronic 5/7 series pumps can create "other" markers. That was news to us. We've fixed that bug so incoming "other" markers don't mess with your uploads.  
[Feature Requirements](https://trello.com/c/heVgxuHy/8-unknown-medtronic-direct-record-type)

We identified a bug in the UltraMini driver that calls a callback twice after a successful upload. A trivial bug fix, but a bug fix nonetheless.  
[Feature Requirements](https://trello.com/c/DhwgWqsi/7-ultramini-callback-was-already-called)

Animas uploads were getting fussy after a recent dependacy update. We've addressed this bug by adding functionality to resume a dropped connection.  
[Feature Requirements](https://trello.com/c/re4NHbSw/6-animas-uncaught-typeerror-commandpacketparser-is-not-a-function)

We were storing CGM values that were currently displayed on a Tandem t:slim G4 instead of the calibrated CGM value that was entered. That's our bad. We've fixed this bug.  
[Feature Requirements](https://trello.com/c/VIhtCzG5/5-tandem-calibration-values-show-cgm-value-instead-of-calibration-value)

We noticed when a Tandem temporary basal crossed into a new scheduled basal segment we only stored the rate and not the percentage. Best to keep things consistent - we fixed that bug and now temporary basals display as expected.  
[Feature Requirements](https://trello.com/c/zGZ65BOr/4-tandem-temp-basal-display-bug)

Well...that was productive.  
[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.5)

### 2.2.4 (Released 2017-12-21)
Turns out a Precision Xtra meter needs a little more time than we initially thought to upload more than 400 blood glucose readings. We fixed that bug and upped the timeout time from 10 to 20 seconds.  
[Feature Requirements](https://trello.com/c/NRFZHzbo/16-uploader-pxtra-times-out-if-there-are-many-bg-records-on-the-meter)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.4)

### 2.2.3 (Released 2017-12-12)
The Dexcom driver doesn't always handle error communication gracefully. We've addressed this bug by adding a little more context in case an upload cable gets disconnected.  
[Feature Requirements](https://trello.com/c/X5Y3bDP7/3-uncaught-typeerror-cannot-read-property-attrs-of-null)

FreeStyle Lite uploads were hanging up a bit. We poked around and fixed the bug causing this.  
[Feature Requirements](https://trello.com/c/3lCJF3qM/4-freestyle-lite-upload-sometimes-hangs-at-70)

We found a bug that caused a minor hubub when a Clinician account tried to load a patient profile that generated an error. We cleaned that up - things should operate smoothly now.  
[Feature Requirements](https://trello.com/c/LMqqMx1x/5-errored-user-profile-still-being-added-to-available-upload-groups)

Turns out, Low Glucose Suspend events on Medtronic pumps could have three or fewer records to represent the event. We found a bug that included the assumption that LGS events had exactly three records. You know what happens when you assume...this has been fixed.  
[Feature Requirements](https://trello.com/c/vKfxm83B/2-uploader-error-suspend-resume-events-out-of-order)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.3)

### 2.2.2 (Released 2017-11-29)
It's hard to make dependency updates sound interesting, even though they're super important. You're going to have to trust us on this one, this stuff is a big deal.  
[Feature Requirements](https://trello.com/c/kTDfWrzS/18-492-l-uploader-update-dependencies)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.2)

### 2.2.1 (Released 2017-11-17)
While the phrase "brand consolidation" makes us feel queasy, we do need to simplify things a bit. Since the name "Blip" is not a thing anymore, we changed the "Go to Blip" button to read "See data".  
[Feature Requirements](https://trello.com/c/WU728Ih2/8-xs-rebrand-blip-to-tidepool-uploader)

We fixed a single line of code that caused Precision Xtra uploads to fail. Yes, literally a single line of code.  
[Feature Requirements](https://trello.com/c/PZzb3Ch2/7-precision-xtra-upload-fails)

Our time change algorithm didn't like a particular default date that could be set in Medtronic pumps. We've fixed that bug so everyone can play nice again.  
[Feature Requirements](https://trello.com/c/wonZCRNy/9-532-missing-time-change-record-on-530g)

If a percentage temporary basal rate was the first record on a Medtronic pump, the Tidepool Uploader was not happy. With this bug fix, the Uploader has improved its morale.  
[Feature Requirements](https://trello.com/c/RiSWtcG1/10-medtronic-direct-temp-basal-has-missing-rate)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.1)

### 2.2.0 (Released 2017-11-03)
Tidepool now supports Abbott's FreeStyle Libre. Woohoo! You have no idea how long we spent discussing the nuance and semantics between continuous and flash glucose monitoring.  
[Feature Requirements](https://trello.com/c/XMchviCK/2-521-uploader-abbott-freestyle-libre)

We've added a new feature called _Rollbar_ which automatically sends us error reports when something goes wrong. The entire team is quite excited about this one. You can expect a lot more bug fixes in the future.  
[Feature Requirements](https://trello.com/c/cYSNiB1W/3-uploader-send-error-reports-automatically)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.2.0)

### 2.1.1 (Released 2017-10-20)
Tidepool now supports OneTouch Ultra 2 and UltraMini meters. Welcome to the fun!  
[Feature Requirements](https://trello.com/c/drBwiv9K/15-522-onetouch-ultra-2-and-onetouch-mini)

We added a super secret toggle that enables Debug Mode, which helps us troubleshoot issues that are reported to us. It's not actually super secret, but you don't need to worry about it unless something goes wrong and we ask you to do it. Speaking of help, if you ever run into problems, [support@tidepool.org](mailto:support@tidepool.org) is your best way to get in touch.  
[Feature Requirements](https://trello.com/c/jo0EfNg2/14-uploader-unify-dev-prod-build)

Our recent update to Electron means we can do fun things like support multiple cables for a single device. This means 3rd party cables can be used to upload, for example, supported Abbott meters.  
[Feature Requirements](https://trello.com/c/mE0Q860H/16-519-electron-support-multiple-usb-pid-vid-combos-per-driver)

Using Electron also means we can simplify options on the Choose Device screen. So we did that and combined all the different Contour Next meters as well as Abbott FreeStyle Lite and Freedom Lite to single options. As always, neat and tidy is the goal.  
[Feature Requirements](https://trello.com/c/06aRAIAq/17-uploader-collapse-bayer-meters-into-a-single-bayer-contour-selection-prevent-unknown-device)

We addressed two bugs causing unexpected data from Medtronic pumps. One was an assumption there would always be at least one basal record on a pump. The other resolved an issue we noticed when pump basal schedules get reset after a pump error.  
[Feature Requirements](https://trello.com/c/TWzhnHRM/13-pump-errors-cause-unexpected-data-on-pump)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.1.1)

### 2.0.2 (Released 2017-09-15)
In our continued effort to support the entire suite of Tandem products, the Tidepool Uploader will now upload Dexcom G5 data from a t:slim X2 pump.  
[Feature Requirements](https://trello.com/c/aKu0nzdf/31-511-uploader-add-g5-support-for-tandem-pumps)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.0.2)

### 2.0.1 (Released 2017-09-08)
We noticed links in the Uploader were crashing computers running 32-bit versions of Windows. Sorry about that. We've fixed that bug and you can resume clicking away to your heart's content.  
[Feature Requirements](https://trello.com/c/FFKssPse/23-518-uploader-windows-32-crashes-when-clicking-on-most-links-like-the-footer)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.0.1)

### 2.0.0 (Released 2017-09-06)
Introducing Tidepool Uploader 2.0. It's a stand-alone application that sets the stage for some ambitious improvements down the road (scroll up to see what we mean). We'll document the work behind each of the Feature Requirements below, but trust that this is an incredibly big deal and we're so happy to deliver this new and improved experience to the diabetes community.  

The Tidepool Uploader works on Mac and Windows.  
[Feature Requirements](https://trello.com/c/A86J1Lbt/8-485-electron-uploader-develop-installer-for-mac-and-windows)

It still supports all the devices you've come to enjoy connecting to Tidepool.  
[Feature Requirements](https://trello.com/c/AxAexUcA/10-486-electron-uploader-device-support)

The new Tidepool Uploader runs on Electron, which means a lot of work went into moving our code.  
[Feature Requirements](https://trello.com/c/RzFYcXyz/17-503-1ux-5dev-3tst-uploader-electron-uploader-part-2)

We gave the login screen a bit of a makeover.  
[Feature Requirements](https://trello.com/c/HOJrre0z/14-495-s-1ux-3dev-1tst-electron-uploader-update-tidepool-uploader-login-screen-footer)

The Tidepool Uploader now supports direct uploading of supported Medtronic insulin pumps (523, 530G, 723). You'll need a Contour Next Link meter to make that happen.  
Feature Requirements [Part 1](https://trello.com/c/8nJtBvT7/7-482-update-error-message-for-medtronic-direct-5-series), [Part 2](https://trello.com/c/hYZjKBPK/6-505-electron-uploader-encrypt-and-store-locally-the-users-medtronic-serial-number)

The new Tidepool Uploader will automatically check for new updates.  
[Feature Requirements](https://trello.com/c/iwNjYKJn/16-491-electron-uploader-auto-update-for-tidepool-uploader)

We rounded off the Tidepool Uploader icon, because that's what the modern web design textbooks suggested. But we're keeping all the vowels in our name.  
[Feature Requirements](https://trello.com/c/XpgO8bbO/5-new-icon-for-tidepool-uploader)

Menu items link out where you expect them to.  
[Feature Requirements](https://trello.com/c/JfZWcESh/13-uploader-20-help-menu)

Support for importing your Medtronic data from CareLink will go away eventually, so we reordered the devices list a bit to set the stage.  
[Feature Requirements](https://trello.com/c/UkEO5ymQ/3-478-electron-uploader-move-carelink-checkbox-to-bottom-of-list)

We added a fun little chime when your upload completes, and a fun, but more somber tone if it fails.  
[Feature Requirements](https://trello.com/c/faV0ToYq/4-electron-uploader-should-have-notification-sounds-earcons-for-upload-complete-upload-failed)

The Mac Installer needed some love, so we splashed a bucket of Tidepool-inspired paint on it.  
[Feature Requirements](https://trello.com/c/i5r0UVTT/40-uploader-20-mac-installer-branding)

And we updated the metrics the Tidepool Uploader reports so we can better understand all of you fine people and how you're using our software.  
[Feature Requirements](https://trello.com/c/cbseB9qW/12-489-xs-electron-uploader-kissmetrics)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.0.0)

### 0.312.0 (Released 2017-09-01)
We received a bug report including the phrase "Timestamps must be in order". We've applied the proper sorting method and fixed this bug.  
[Feature Requirements](https://trello.com/c/tFZ5mrMN/1-uploaderbug-470-medtronic-direct-bugfixes)

The Tidepool Uploader is no longer in beta. We're in the big time now, folks. Let's remove that "beta" label from here on out, shall we?  
[Feature Requirements](https://trello.com/c/3fRwxot6/2-481-uploader-remove-beta-from-production-uploader)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v0.312.0)

### 0.309.0 (Released 2017-08-04)
We addressed a bug we identified that resulted from "no power" alerts and auto-suspend events from Medtronic pumps.  

We also made some changes to address how the Uploader handles time changes during temporary basal rates.  
[Feature Requirements](https://trello.com/c/bnEjgwgW/13-uploaderbug-medtronic-direct-upload-with-erics-pump-fails-platform-validation)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v0.309.0)

### 0.307.0 (Released 2017-07-11)
Before we tell the world that Tidepool officially supports direct uploading of Medtronic 523, 530G, and 723 pumps using a Contour Next Link meter, we need to solidify our work and finish testing. How did we get here? I'm glad you asked.  

We need to make sure only supported pumps are uploaded through our direct uploading adventures. Sorry 522 and 722 folks.  
[Feature Requirements](https://trello.com/c/CdWv2kUz/23-prevent-un-support-medtronic-pumps-from-being-uploaded-through-medtronic-direct)

Seriously, we tried. The basal data coming off x22 pumps is not reliable enough for us.  
[Feature Requirements](https://trello.com/c/bu8ZX4WZ/22-0ux-3dev-2tst-medtronic-522)

We need to account for time change events accurately, because Daylight Saving is still a thing.  
[Feature Requirements](https://trello.com/c/pGmiXf4s/16-ux0dev0tst3-uploader-test-medtronic-drivers-utc-bootstrapping)

We need to show CGM data from 530G pumps.  
[Feature Requirements](https://trello.com/c/vMtU8xkK/20-medtronic-cgm-data-uploader-dev-5-tst-2-ux-0)

We need to show accurate and detailed basal data.  
[Feature Requirements](https://trello.com/c/zlAnS0iw/12-uploader-medtronic-basals)

We need to store and show device settings.  
[Feature Requirements](https://trello.com/c/qsKqeuFh/18-uploader-dev-3-tst-1-ux-0-medtronic-device-settings)

In case something goes wrong, we need to be able to save the raw binary data from the pump, affectionately called a "binary blob" for debugging and testing purposes.  
[Feature Requirements](https://trello.com/c/1IFzNbHP/13-uploader-button-to-get-raw-binary-data-from-pump-in-debug-mode)

The Tidepool Uploader needs to show specific instructions to make sure our direct uploading works as intended.  
[Feature Requirements](https://trello.com/c/TiTc17UL/15-uploader-ui-enhancements-for-medtronic-direct-upload-dev-5-ux-1-tst-1)

We need to show blood glucose values stored on the pump.  
[Feature Requirements](https://trello.com/c/F4vTGbuQ/11-uploader-medtronic-smbg-values)

We need to make sure the data the Uploader and the back end are on the same page.  
[Feature Requirements](https://trello.com/c/HuNfebgZ/14-technical-investigation-for-medtronic-device-planning-ux0-dev2-tst0-platform-uploader)

We need to make sure you could view bolus wizard calculations.  
[Feature Requirements](https://trello.com/c/4Gn33urX/10-uploader-medtronic-calculator-data)

We need to accurately handle manual boluses.  
[Feature Requirements](https://trello.com/c/OAHqN8Ng/9-0ux-5dev-0tstuploader-medtronic-manual-boluses)

We need to handle device events gracefully.  
[Feature Requirements](https://trello.com/c/6b5oFTZl/8-uploader-medtronic-device-events)

Node needs an update.  
[Feature Requirements](https://trello.com/c/NWi3QK35/7-node-update-uploader)

Our Medtronic driver needs to be accurate for inquiring developers.  
[Feature Requirements](https://trello.com/c/qkp0Mfj6/21-uploader-medtronic-remaining-work-dev5-ux0-tst1)

The data we upload using our Medtronic driver has to match the data generated by the CareLink driver. We can't say we have a better option if the data tells a different story.  
[Feature Requirements](https://trello.com/c/T9jR2zH0/19-tool-to-compare-carelink-driver-output-with-medtronic-driver-output-uploader-ux0-dev5-tst2)

And all of that needs to be verified through an extensive Alpha test.  
[Feature Requirements](https://trello.com/c/oY4TZF73/17-l-medtronic-alpha-test-issue-resolution-card-uploader)

Pretty cool, right?  
[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v0.307.0)

### 0.306.1 (Released 2017-06-10)
Chrome version 59.0.3071.86 started throwing "Invalid Time Value" errors during Dexcom uploads. The reason behind this is incredibly technical, but we figured it out and fixed that bug. Check out the Feature Requirements below for a detailed explanation of what's what.  
[Feature Requirements](https://trello.com/c/NZKjk2An/2-chrome-590307186-invalid-time-value-error-when-uploading-dexcom-receiver)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v0.306.1)

### 0.306.0 (Released 2017-06-06)
OmniPod uploads were running into problems if certain error codes were identified in the .ibf file. We've cleared the path, and fixed this bug so tubeless pumpers can upload in peace.  
[Feature Requirements](https://trello.com/c/r7sDfxHz/3-uploaderbug-omnipod-records-with-checksum-errors-should-be-dropped)

[Code Update](https://github.com/tidepool-org/chrome-uploader/releases/tag/v0.306.0)
<hr>

## Tidepool Mobile  
Visit https://tidepool.org/mobile to learn more about Tidepool Mobile.  

### Android - 3.0 (2018-12-04)
Tidepool Mobile has arrived on Android. See your data, search your notes, and prepare for more meaningful updates now that our Android app is built in React Native.  
Feature Requirements...there were quite a few  
[Part 1](https://trello.com/c/bKIgj0ZR/2-tidepool-mobile-3x-cgm-chart-y-axis-does-not-respect-the-mmol-l-units-setting-in-a-consistent-way), [Part 2](https://trello.com/c/zekkuXNp/1-rn-metrics-m), [Part 3](https://trello.com/c/RoxQnQL4/3-rn-bld-25300-app-crashes-the-first-time-it-is-launched-from-play-store-subsequent-launches-work-properly), [Part 4](https://trello.com/c/y7ry0u76/5-rn-ios-date-picker-does-not-come-up-after-selecting-edit-date), [Part 5](https://trello.com/c/SJ8S4Vm7/4-rn-android-app-data-is-not-deleted-when-deleting-the-app), [Part 6](https://trello.com/c/X10lZVXW/6-rn-3025700-date-and-time-does-not-dynamically-refresh-after-editing-the-time-of-a-note), [Part 7](https://trello.com/c/0tJXhKmi/8-default-first-character-in-password-field-is-capitalized-on-some-devices), [Part 8](https://trello.com/c/0sarb6Th/10-changing-the-date-of-a-note-causes-tidepool-mobile-to-crash), [Part 9](https://trello.com/c/W4VKns4V/9-note-flag-does-not-dynamically-update-after-editing-the-note-time), [Part 10](https://trello.com/c/c32gFG1U/11-rn-moving-notes-forward-in-time-is-causing-the-app-to-crash), [Part 11](https://trello.com/c/G4k9AhyN/7-rn-extended-typing-slows-and-drops-keystrokes-when-adding-notes-on-the-blackberry-keyone-android), [Part 12](https://trello.com/c/qL8gh8rl/13-rn-add-code-to-detect-day-changes-and-refresh-relative-dates-accordingly), [Part 13](https://trello.com/c/MwECDRZm/12-cant-log-out-or-switch-profile-on-mobile-without-adding-a-note), [Part 14](https://trello.com/c/6P4xk9wf/14-rn-when-user-has-no-network-and-is-already-logged-in-attempting-to-add-a-note-should-give-user-a-not-connected-to-network-error)

[Android Release](https://play.google.com/store/apps/details?id=io.tidepool.urchin)

### iOS - 2.0.3 (Released 2018-04-17)  
Dexcom uploading has improved, and we’ve updated some behind the scenes components to improve performance and (if we need it) troubleshooting bugs.  
Feature Requirements...(wait for it)  
[Part 1](https://trello.com/c/6w9gu1Ze/23-ios-tidepool-mobile-uploader-upload-in-batches-of-2000-instead-of-1000), [Part 2](https://trello.com/c/2IubSfH7/37-ios-tidepool-mobile-after-time-change-on-mobile-device-tidepool-notes-display-at-incorrect-place-on-the-timeline), [Part 3](https://trello.com/c/lsafiu38/26-ios-tidepool-mobile-use-7-tap-on-footer-on-login-screen-and-in-side-menu-to-access-debug-settings), [Part 4](https://trello.com/c/6bOOaSUY/32-ios-tidepool-mobile-uploader-when-user-taps-sync-button-on-initial-sync-the-title-incorrectly-changes-to-manual-sync), [Part 5](https://trello.com/c/VbnuPL3N/31-ios-tidepool-mobile-uploader-text-formatting-issue-in-manual-sync-help-text), [Part 6](https://trello.com/c/AI0NPm8w/34-ios-tidepool-mobile-uploader-show-a-stop-syncing-confirmation-alert-when-user-taps-stop-on-initial-manual-sync), [Part 7](https://trello.com/c/nR4PWzcg/36-ios-tidepool-mobile-simplify-debug-settings-ui), [Part 8](https://trello.com/c/HhQTYS10/35-ios-tidepool-mobile-uploader-instead-of-showing-finished-when-upload-is-finished-just-continue-to-show-x-of-y-days-with-100-prog), [Part 9](https://trello.com/c/CYk71cNo/22-ios-tidepool-mobile-integration-server-is-not-an-option-under-the-the-development-menu), [Part 10](https://trello.com/c/GvGni10O/30-ios-tidepool-mobile-uploader-implement-new-ui-spec-for-uploader-current-samples-and-manual-all-and-manual-last-two-weeks), [Part 11](https://trello.com/c/TG0DBp2w/21-ios-tidepool-mobile-uploader-use-separate-foreground-and-background-sessions-dont-invalidate-sessions), [Part 12](https://trello.com/c/fFWmd7oA/28-ios-tidepool-mobile-uploader-we-should-consider-taking-advantage-of-background-task-registration-to-get-more-time-to-continue-up), [Part 13](https://trello.com/c/DikLviaT/25-ios-tidepool-mobile-logging-appears-to-be-enabled-by-default-on-clean-install-we-should-only-enable-it-if-user-toggles-it-on-via), [Part 14](https://trello.com/c/K6JjrzGM/33-ios-tidepool-mobile-update-bugsee), [Part 15](https://trello.com/c/gRyj0mwi/20-ios-tidepool-mobile-uploader-when-observing-new-samples-in-background-if-we-have-persistent-pending-uploads-just-cancel-them-and), [Part 16](https://trello.com/c/g2ktMcYJ/24-ios-tidepool-mobile-uploader-fix-handling-of-deleted-samples-so-that-observing-reading-deleted-samples-doesnt-cause-the-reader-u), [Part 17](https://trello.com/c/OiEnzl7t/29-ios-tidepool-mobile-uploader-remove-the-phases-and-just-have-a-continuous-current-samples-uploader-and-support-for-manual-all-an), [Part 18](https://trello.com/c/AszRl3WW/27-ios-tidepool-mobile-uploader-improve-logging), [Part 19](https://trello.com/c/i9DvtpgK/38-tidepool-mobile-203-6-series-support) 

[iOS Release](https://appsto.re/us/aXyl9.i)

### iOS - App Store Update (Released 2017-06-01)
We've updated the screenshots for the Tidepool Mobile app in the App Store to show data from our demo account, Jill Jellyfish. This helps us add a little context to the experience.  
[Feature Requirements](https://trello.com/c/DyKCRV7W/14-new-ios-app-store-design-assets-for-tidepool-mobile)
