# Release Notes
> Because GitHub can be a scary place if you don't know where to look, or what you're looking for, these release notes will make updates to Tidepool for Web, the Tidepool Uploader, and Tidepool Mobile a bit more readable.  

> Because we love transparency, along with a description of what's new, each update includes a link out to the speicifc GitHub repository release and relevant public Trello cards so you can get a better sense of who, what, when, where, and why.  

> Questions? Contact us at support@tidepool.org

[Jump to Tidepool for Web Updates](#tidepool-for-web)  
[Jump to Tidepool Uploader Updates](#tidepool-uploader)  
[Jump to Tidepool Mobile Updates](#tidepool-mobile)  

# Last Updated: 2018-05-25

<hr>

## Tidepool for Web  
> Visit https://app.tidepool.org to see the results of these updates.  

> Don't have a Tidepool account? Visit https://tidepool.org/signup to create your free Tidepool account.  
### 1.10.7 (Released 2018-05-14)
We’ve added the AADE Foundation to our Tidepool Big Data Donation Project Nonprofit Partner list. Now you can choose to support an organization providing resources to diabetes educators with your data donation.  
[Trello Details](https://trello.com/c/LWBzvGMr/2-add-aade-foundation-to-tidepool-big-data-donation-project-nonprofit-partner-list)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.7)

### 1.10.5 (Released 2018-04-17)
You know how when you create a new account and we display a message that says “Please click on the link in the email we sent you at (your email address)” and (your email address) sometimes showed up on two lines if it was really long? We fixed that so now (your email address) shows up on its own line. Little details like that are important.  
[Trello Details](https://trello.com/c/67H081EV/8-put-the-email-address-on-its-own-line-so-it-doesnt-get-split)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.5)

### 1.10.4 (Released 2018-04-09)
Sometimes data lines in the Trends view would appear to loop around, like a weird connect-the-dots drawing. We fixed that.  
[Trello Details](https://trello.com/c/98DFbS3V/4-overlapping-lines-in-trends-view)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.4)

### 1.10.3 (Released 2018-03-29)
For users signing up for a new account, you can now see which email address you signed up with on our email verification page. If you accidentally typed in the wrong address (it happens to the best of us), you’ll know right away instead of wondering where that account verification email went.  
[Trello Details](https://trello.com/c/1qAvkpEH/24-blip-show-email-address-as-part-of-signup-email-verification)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.3)

### 1.10.2 (Released 2018-03-28)
The printed Daily view started at 5:00pm rather than midnight. Sure, time zones are weird, but we fixed that so everyone’s day starts at a logical time.  
[Trello Details](https://trello.com/c/dr9EeALW/22-print-view-daily-view-starts-at-5pm-rather-than-midnight)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.2)

### 1.10.1 (Released 2018-03-19)
For anyone whose iPhone CGM upload is 8 weeks (or more) more recent than a pump upload, there was a small chance you may not be able to see any of your data based on how we process things behind the scenes. We cleaned up that order of operations and you can see your data again.  
[Trello Details](https://trello.com/c/Inr3LmRc/18-cgm-only-from-dexcom-api-results-in-non-rendered-viz-due-to-data-paging)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.1)

### 1.10.0 (Released 2018-03-19)
Tired of watching those dots bounce while your data loads? Frustrated at seeing “Aw, snap!” if Tidepool gets bogged down by all the data you’ve uploaded? Welcome to Data Paging. We’ve reengineered how we visualize your data, reducing the time spent waiting for your data to load. Scrolling through the days/weeks is also faster, too.  
[Trello Details](https://trello.com/c/jmYSnJCw/15-tidepool-web-data-paging)

Along the way, we made some adjustments to how and where your bolus tooltips display.  
[Trello Details](https://trello.com/c/MI81nZVi/16-bolus-tooltips-are-displaying-below-the-basal-rates-in-daily-view)

[GitHub Details](https://github.com/tidepool-org/blip/releases/tag/v1.10.0)
<hr>

## Tidepool Uploader
> Visit https://tidepool.org/uploader to download the latest version of the Tidepool Uploader.  

> If you already have the Tidepool Uploader installed on your computer, it will automatically update to the latest version.
### 2.5.11 (Released 5/16/2018)
Tidepool now supports Medtronic Veo (554/754) insulin pumps. Yay!  
[Trello Details](https://trello.com/c/RU2SjKA1/7-uploader-support-medtronic-veo-554-754-pump)

Bayer was acquired by Ascencia, which means all those Contour meters needed some new branding. We’ve gone with Ascencia (Bayer) to minimize confusion.  
[Trello Details](https://trello.com/c/mt2xb3qV/6-update-bayer-listing-to-ascencia-bayer)

Our friends at Medtronic removed the ability to export your data as a CSV file. We relied on that functionality to import your CareLink data into Tidepool. As a result, we’ve removed the “Upload from CareLink option in the Tidepool Uploader” (Don’t worry, you can still upload directly to Tidepool with your Contour Next Link meter.)  
[Trello Details](https://trello.com/c/pz3ltLai/4-remove-upload-from-carelink-option)

Previously, if two consecutive square wave calculator boluses were given on a Medtronic 530G, the Tidepool Uploader interpreted the data as a single Dual Wave bolus with the extended portion of the bolus being the insulin total from the first bolus, which is listed as an override of the total. Also, if the last bolus on the 530G was a square wave bolus, data uploads failed. It’s hip to be square, but not that square - we’ve fixed both those issues.  
[Trello Details](https://trello.com/c/izgIetxG/5-2-consecutive-square-calculator-boluses-are-merged-into-a-single-bolus-when-uploaded-from-medtronic-530g)

Finally, if an account was set up to upload devices on one operating system, you’d see “Unknown Device” on an operating system that does not support those devices. Now, we will remove that device from your upload options so it doesn’t look like you’re trying to upload mystery data.  
[Trello Details](https://trello.com/c/6XAazELZ/8-unknown-devices-appearing-in-uploader)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.11)

### 2.5.9 (Released 4/26/2018)
We change some of the text for our Time Check pop up to make it clear that you should triple-check the time on the device you’re trying to upload.  
[Trello Details](https://trello.com/c/ERwBfwRa/19-update-device-time-check-copy)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.9)

### 2.5.8 (Released 4/26/2018)
Dependency updates aren’t always the most exciting thing, but we want to make sure you’re getting the latest and greatest software possible, so here we are.  
[Trello Details](https://trello.com/c/LLQU6QaX/17-new-card-uploader-dependency-updates)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.8)

### 2.5.7 (Released 4/23/2018)
Remember when we said we fixed that bug that prevented all your Libre data from uploading. This time we really, really fixed it. (To be fair, the FreeStyle Libre hasn’t been out that long, so it took a little time for larger datasets to come our way and push the boundaries of our code.)  
[Trello Details](https://trello.com/c/UUfpJcP2/10-reading-all-the-available-data-from-a-libre-device)

For our friends with a Medtronic pump using more than 6 or 7 insulin sensitivities or carb ratios, your uploads used to fail. We fixed that too.  
[Trello Details](https://trello.com/c/MyWefnN2/11-upload-fails-when-a-large-number-of-insulin-sensitivities-or-carb-ratios-are-specified)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.7)

### 2.5.5 (Released 4/10/2018)
Be honest, how diligent are you at keeping your devices on time when you travel? The Tidepool Uploader will now warn you if it detects a difference between the time zone selected and the time indicated on your device.  
[Trello Details](https://trello.com/c/1eAw2Fpc/2-uploader-timezones-as-a-user-i-want-the-uploader-to-warn-me-of-any-difference-between-my-device-time-and-computer-time-or-select)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.5)

### 2.5.4 (Released 3/27/2018)
We found a bug that prevented every last data point on your FreeStyle Libre from uploading. That won’t be a problem anymore.  
[Trello Details](https://trello.com/c/5KPT5Iim/20-libre-uploading-with-gaps-in-data)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.4)

### 2.5.3 (Released 3/16/2018)
We’ve made improvements to our Uploader error-logging service, Rollbar to catch more hiccups and bummers that you throw our way. This will go a long way to helping us identify areas we can improve our software.  
[Trello Details](https://trello.com/c/144Btqt7/13-uploader-improve-rollbar-logging)

Omnipod device settings were hell-bent on displaying as mg/dL even when uploaded from a pump set to mmol/L. We’d like to formally apologize to our friends who prefer blood glucose concentrations measured by molarity instead of weight - this has been fixed.  
[Trello Details](https://trello.com/c/KVEO8OTR/12-omnipod-showing-mg-dl-in-device-settings-even-after-mmol-l-selected-in-user-profile)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.3)

### 2.5.1 (Released 3/12/2018)
We gave upload times for Medtronic and Tandem pumps a serious speed boost. Prior to this update, we would pull all data off compatible Medtronic and Tandem pumps and sort out duplicate data later. Now, initial uploads of compatible Tandem and Medtronic pumps will show “We've improved how devices upload. This upload will take longer than usual, but your future uploads will be much, much faster.” After that, we will only upload new data. The result? A bolus from the Speed Force and much faster uploads in the future.  
Trello Details  
[Part 1](https://trello.com/c/gwB5pykC/3-uploader-delta-upload-ui), [Part 2](https://trello.com/c/5i0sM0wx/4-m-delta-uploads-3-of-4-medtronic-5-series), [Part 3](https://trello.com/c/ieSwFQq5/5-m-delta-uploads-4-of-4-tandem)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.1)
<hr>

## Tidepool Mobile  
> Visit https://tidepool.org/mobile to learn more about Tidepool Mobile.  
### 2.0.3 (Released 4/17/2018)  
Dexcom uploading has improved, and we’ve updated some behind the scenes components to improve performance and (if we need it) troubleshooting bugs.  
Trello Details...(wait for it)  
[Part 1](https://trello.com/c/6w9gu1Ze/23-ios-tidepool-mobile-uploader-upload-in-batches-of-2000-instead-of-1000), [Part 2](https://trello.com/c/2IubSfH7/37-ios-tidepool-mobile-after-time-change-on-mobile-device-tidepool-notes-display-at-incorrect-place-on-the-timeline), [Part 3](https://trello.com/c/lsafiu38/26-ios-tidepool-mobile-use-7-tap-on-footer-on-login-screen-and-in-side-menu-to-access-debug-settings), [Part 4](https://trello.com/c/6bOOaSUY/32-ios-tidepool-mobile-uploader-when-user-taps-sync-button-on-initial-sync-the-title-incorrectly-changes-to-manual-sync), [Part 5](https://trello.com/c/VbnuPL3N/31-ios-tidepool-mobile-uploader-text-formatting-issue-in-manual-sync-help-text), [Part 6](https://trello.com/c/AI0NPm8w/34-ios-tidepool-mobile-uploader-show-a-stop-syncing-confirmation-alert-when-user-taps-stop-on-initial-manual-sync), [Part 7](https://trello.com/c/nR4PWzcg/36-ios-tidepool-mobile-simplify-debug-settings-ui), [Part 8](https://trello.com/c/HhQTYS10/35-ios-tidepool-mobile-uploader-instead-of-showing-finished-when-upload-is-finished-just-continue-to-show-x-of-y-days-with-100-prog), [Part 9](https://trello.com/c/CYk71cNo/22-ios-tidepool-mobile-integration-server-is-not-an-option-under-the-the-development-menu), [Part 10](https://trello.com/c/GvGni10O/30-ios-tidepool-mobile-uploader-implement-new-ui-spec-for-uploader-current-samples-and-manual-all-and-manual-last-two-weeks), [Part 11](https://trello.com/c/TG0DBp2w/21-ios-tidepool-mobile-uploader-use-separate-foreground-and-background-sessions-dont-invalidate-sessions), [Part 12](https://trello.com/c/fFWmd7oA/28-ios-tidepool-mobile-uploader-we-should-consider-taking-advantage-of-background-task-registration-to-get-more-time-to-continue-up), [Part 13](https://trello.com/c/DikLviaT/25-ios-tidepool-mobile-logging-appears-to-be-enabled-by-default-on-clean-install-we-should-only-enable-it-if-user-toggles-it-on-via), [Part 14](https://trello.com/c/K6JjrzGM/33-ios-tidepool-mobile-update-bugsee), [Part 15](https://trello.com/c/gRyj0mwi/20-ios-tidepool-mobile-uploader-when-observing-new-samples-in-background-if-we-have-persistent-pending-uploads-just-cancel-them-and), [Part 16](https://trello.com/c/g2ktMcYJ/24-ios-tidepool-mobile-uploader-fix-handling-of-deleted-samples-so-that-observing-reading-deleted-samples-doesnt-cause-the-reader-u), [Part 17](https://trello.com/c/OiEnzl7t/29-ios-tidepool-mobile-uploader-remove-the-phases-and-just-have-a-continuous-current-samples-uploader-and-support-for-manual-all-an), [Part 18](https://trello.com/c/AszRl3WW/27-ios-tidepool-mobile-uploader-improve-logging), [Part 19](https://trello.com/c/i9DvtpgK/38-tidepool-mobile-203-6-series-support) 

[iOS Release](https://appsto.re/us/aXyl9.i)
