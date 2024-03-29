---
layout: homepage
title: Tidepool Project
published: true
---

# Tidepool Developer Portal

This is Tidepool's developer microsite. If you are looking for our company web site or want to learn more about our products, please visit [tidepool.org](https://tidepool.org).

## About Tidepool

Tidepool is an open source, not-for-profit company focused on liberating data from diabetes devices, supporting researchers, and providing great, free software to people with diabetes and their care teams. We are building a cloud-based platform and applications that help reduce the burden of managing diabetes.

## About this Developer Microsite

The intended audience for this microsite is software developers, but others may also find it valuable. Here are some useful links:

* All of our [source code on GitHub](https://github.com/tidepool-org). Some of the more commonly requested repos are:
  * [Tidepool Uploader](https://github.com/tidepool-org/uploader)
    * [Dexcom device driver](https://github.com/tidepool-org/uploader/tree/master/lib/drivers/dexcom)
    * [Medtronic device driver](https://github.com/tidepool-org/uploader/tree/master/lib/drivers/medtronic)
    * [OmniPod device driver](https://github.com/tidepool-org/uploader/tree/master/lib/drivers/insulet)
    * [Tandem device driver](https://github.com/tidepool-org/uploader/tree/master/lib/drivers/tandem)
  * [Visualization Library](https://github.com/tidepool-org/viz)
  * [Tidepool Mobile for iOS](https://github.com/tidepool-org/mobile-ios)


* All of our user interface designs:
  * [Google Drive](https://drive.google.com/drive/folders/0B1kZGwPgzp9iV2E0dk5UNGRmTTA)
  * [UI designs and prototype for the Bionic Pancreas on GitHub](https://github.com/tidepool-org/bionicpancreas)
* All of our technical documentation (a work in progress!):
  * [General docs](http://developer.tidepool.org/docs/) that don't belong to a specific app/repository, plus:
    * Tidepool for web (codenamed 'Blip') [docs](http://developer.tidepool.org/blip/) and [developer guide](http://developer.tidepool.org/blip/docs/StartHere.html)
    * Tidepool Uploader (Electron application) [docs](http://developer.tidepool.org/uploader/) and [developer guide](http://developer.tidepool.org/uploader/docs/StartHere.html)
    * The @tidepool/viz data visualization library [docs](http://developer.tidepool.org/viz/) and [developer guide](http://developer.tidepool.org/viz/docs/StartHere.html)
  * [Tidepool data model documentation](http://developer.tidepool.org/data-model/)
* Project Management references, including:
  * What we're working in in our [Current Sprint](https://tidepool.atlassian.net/issues/?filter=10097).
  * What we'll be taking on in [Upcoming Sprints](https://tidepool.atlassian.net/issues/?filter=10098).
  * Our past three months of completed tasks:
    * [August 2021](https://tidepool.atlassian.net/browse/UPLOAD-637?filter=10473)
    * [September 2021](https://tidepool.atlassian.net/issues/?filter=10474)
    * [October 2021](https://tidepool.atlassian.net/browse/UPLOAD-666?filter=10475)
* Our [Terms of Use](terms-of-use) and [Privacy Policy](privacy-policy), maintained here on GitHub.
* Our [Release Notes](https://release.tidepool.org).

## A Note about Contributing to Tidepool's Source Code

Since our founding in 2013, Tidepool has had dozens of volunteer contributors to our efforts. Contributions have come in many forms, including source code, user interface designs, testing, legal and product management.

Because we are an FDA registered entity, any contributions we take from you will be tested thoroughly as part of our Quality  System. Depending on our current development priorities, it could be some time before your contribution is tested and made available publicly as part of Tidepool.

We want to be transparent about this, because while we would love to have you as a contributor, we also don't want you to get frustrated or feel like you've wasted valuable time and effort.

## Getting Started as a Contributor

If you think you think you'd like to contribute designs, code, or or anything else back to the project, please read our Volunteer/Contributor License Agreement (VCLA) on our [Contributors](contributors) page. This agreement protects you and Tidepool and does not take away any rights that you have to your work.

Depending on what kind of work you are trying to do, you may wish to engage with our source code in different ways:

* **Tidepool Uploader**: If you'd like to help add device support to the Tidepool Uploader, check out the [Tidepool Uploader](https://github.com/tidepool-org/uploader) repository. (You can also install the production [Tidepool Uploader from our GitHub Releases](https://github.com/tidepool-org/uploader/releases)).
* **Tidepool for web**: You may wish to build Tidepool for web (codename 'Blip'), locally but still run against our hosted back-end services. If so, check out the README in the [blip](https://github.com/tidepool-org/blip) repository. (Of course you can also just run the production Tidepool for web at [app.tidepool.org](https://app.tidepool.org)).
* **Tidepool Platform**: If you want to run the entire platform locally, check out [the instructions in our `development` repository](https://github.com/tidepool-org/development). This will get you running our environment locally using Docker.
* **Your Applications**: You may wish create your own application that accesses our hosted back-end services (the Tidepool Platform). If it's a web app, you may wish to look at Tidepool for web. If it's a mobile app, check out Tidepool Mobile in the [Nutshell](https://github.com/tidepool-org/nutshell-ios) or [Urchin for Android](https://github.com/tidepool-org/urchin-android) or [Urchin for iOS](https://github.com/tidepool-org/urchin). Note that Blip Notes and Nutshell are being retired. Both will be replaced by [Tidepool Mobile](https://github.com/tidepool-org/mobile).
  * Please don't develop your app against our production environment without talking to us. Instead, please use our integration environment, found at int-app.tidepool.org and int-api.tidepool.org.
  * Read our [data model documentation](http://developer.tidepool.org/data-model/) to understand what medical data we ingest and how it’s organized.
  * [These comments](https://github.com/tidepool-org/tide-whisperer/blob/master/tide-whisperer.go#L193) show some documentation for our data APIs.

## Known Issues
We are currently working on adding OAuth support to our API.  
We are aware that until this is done that there are token session handling issues.

## Communication

We are currently using Slack for communication with the open source community. Join us at [tidepoolorg.slack.com](https://public-chat.tidepool.org). We are very grateful to community members who have been so generous with their time supporting other members.

Feel free to drop a note to [info@tidepool.org](mailto:info@tidepool.org), and it will find the right recipient quickly.

*Thank you for your support.*
