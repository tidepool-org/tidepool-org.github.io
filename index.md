---
layout: homepage
title: Tidepool Project
published: true
---

# Tidepool Developer Portal

This is Tidepool's developer microsite. If you are looking for our company web site or want to learn more about our products, please visit [tidepool.org](https://tidepool.org).

## About Tidepool
Tidepool is building a cloud-based platform and applications that help reduce the burden of managing Type 1 diabetes. We are a nonprofit organization doing everything in the open.

## About this Developer Microsite
The intended audience for this microsite is software developers, but others may also find it valuable. Here are some useful links:

* All of our [source code on GitHub](https://github.com/tidepool-org)

* All of our user interface designs:
  * [UI designs for all Tidepool apps on Pixelapse](https://www.pixelapse.com/tidepool)
  * [UI designs and prototype for the Bionic Pancreas on GitHub](https://github.com/tidepool-org/bionicpancreas)
* A list of Technical References, including:
  * A Tidepool Platform architectural overview. (TODO)
  * [Tidepool data model documentation](data-model)
* Project Management reference, including:
  * Current and recent sprint boards (on Trello)
  * [Sprint planning priority documents](https://drive.google.com/a/lookfamily.org/folderview?id=0Bzqw_G5XWp9KY0pZTnFUSVFoUU0&usp=drive_web) (in Google Docs).
  * Product Backlogs  (on Trello)
  *  * [Active, force-ranked product backlog](https://trello.com/b/pQtao5gN/active-force-ranked-product-backlog)
  *  * "Maybe someday" backlogs for [Blip](https://trello.com/b/iKydvoiJ/backlog-blip), [Uploader](https://trello.com/b/oHy9VXYY/backlog-uploader), [Blip Notes](https://trello.com/b/jjciNmRJ/backlog-blip-notes), [Backend](https://trello.com/b/RUDDN3yq/backlog-backend)
* Our [Terms of Use](terms-of-use) and [Privacy Policy](privacy-policy), maintained here on GitHub.

## A Note about Contributing to Tidepool's Source Code
Since our founding in 2013, Tidepool has had dozens of volunteer contributors to our efforts. Contributions have come in many forms, including source code, user interface designs, testing, legal and product management.

To cut to the chase: Although we are an open source project, we are not currently set up in a way that makes it easy to contribute code back to Tidepool and have it merged in with our production software. There are lots of reasons for this:

* Our platform is pretty complicated to get set up and running locally, and we don't have a good Docker or Vagrant container setup.
* We tend to give ourselves aggressive sprint goals, which does not leave enough slack to support outside contributors.
* We don't have anyone whose job is to ensure the success of the open source community (though we'd like to add this… please [let us know](mailto:jobs@tidepool.org) if you are interested!)

Said another way: **We love being an open source project, and we intend to stay that way.** Being open source reinforces our commitment to transparency, and it helps to catalyze innovation in the pursuit of a diabetes app ecosystem. **But we still have work to do before we will feel *good* about being a *good* open source project.** We want to be easier to setup locally and easier to contribute back to. We want to be transparent about this, because while we would love to have you as a contributor, we also don't want you to get frustrated or feel like you've wasted valuable time and effort.

## Getting Started as a Contributor

With that out of the way, here are some ways to get started:
If you think you think you'd like to contribute designs, code, or or anything else back to the project, please read our Volunteer/Contributor License Agreement (VCLA) on our [Contributors](contributors) page. This agreement protects you and Tidepool and does not take away any rights that you have to your work.

Depending on what kind of work you are trying to do, you may wish to engage with our source code in different ways:

* **Tidepool Uploader**: If you'd like to help add device support to the Tidepool Uploader, check out the [chrome-uploader](https://github.com/tidepool-org/chrome-uploader) repository. (You can also install the production [Tidepool Uploader from the Chrome Web Store](https://chrome.google.com/webstore/detail/tidepool-uploader/cabklgajffclbljkhmjphejemhpbghfb).)
* **Blip**: You may wish to build Blip, our web application, locally but still run against our hosted back-end services. If so, check out the README in the [blip](https://github.com/tidepool-org/blip) repository. (Of course you can also just run the prodiction Blip at [blip.tidepool.org](https://blip.tidepool.org).
* **Your Applications**: You may wish create your own application that accesses our hosted back-end services (the Tidepool Platform). If it's a web app, you may wish to look at Blip. If it's a mobile app, check out the [Nutshell](https://github.com/tidepool-org/nutshell-ios) or Blip Notes repos (a.k.a. [Urchin for Android](https://github.com/tidepool-org/urchin-android) or [Urchin for iOS](https://github.com/tidepool-org/urchin)).
  * Please don't develop your app against our production environment without talking to us. Instead, please use our integration environment, found at int-blip.tidepool.org and int-api.tidepool.org.
  * Read our [data model documentation](/data-model/) to understand what medical data we ingest and how it’s organized.
  * [These comments](https://github.com/tidepool-org/tide-whisperer/blob/master/tide-whisperer.go#L193) show documentation for our data APIs or you can help us improve our [draft API documentation](http://developer.tidepool.io/tidepool-api/index/).

## Communication
We are currently using [this Gitter channel](https://gitter.im/tidepool-org/public) for communication with the open-source community, although we plan to move to a public Slack channel as soon as that option is available. **We will be the first to admit that we aren't as responsive as we could or should be in Gitter.** We are very grateful to community members who have been so generous with their time supporting other members.

Feel free to drop a note to [info@tidepool.org](mailto:info@tidepool.org), and it will find the right recipient quickly.

*Thank you for your support.*
