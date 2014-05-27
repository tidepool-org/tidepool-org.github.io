---
layout: defaults
title: Tidepool Platform Roadmap
published: true
---

This document was last edited at the end of May, 2014.

#Platform Roadmap

This is an attempt to document the general direction Tidepool is moving with respect to the development of our platform. 

##Overview

Tidepool's team is small. We are constrained by our available resources, and we are also constrained by various external factors. Our primary goal is to have the maximum possible positive impact on people who have Type 1 Diabetes. We believe that for the duration of 2014, that means that we should concentrate on getting our first bit of software into the hands of real users. That, in turn, means making it useful AND making it so that we can apply for FDA approval as soon as possible. 

Blip, in turn, depends on having a robust, HIPAA-compliant backend built, tested, deployed, and capable of being FDA-approved. 

It is these ends that drive the prioritization and sequencing of the items in this document.

##Our technical goals

These are the high level technical goals that are informed by our company goals. The order in which they're listed is definitely not the order in which they will be implemented. Note that these are the high level goals; details are later in the document.

* *Complete the Blip feature set.*  Our initial implementation of Blip, currently undergoing its first medical trial, is lacking a few important features that will have to exist before we make it available to a larger population. For a general outline, see below. For specific details on exactly what we're working on, please visit [our Blip Trello board](https://trello.com/b/GPadCYvP/blip).
* *Build a "Universal Uploader".* We believe that extracting data from third party sites that have already processed and filtered it is the wrong way to collect data. We want to build interfaces that will allow Tidepool's platform to extract data directly from devices like pumps and CGMs. We intend to do this by working directly with the manufacturers of these devices.
* *Continue to extend the Tidepool platform for third party uses.* Tidepool intends to be a hosting provider for data related to diabetes -- not just device data, but all the data that has an effect on blood sugar. Exercise, etc. We also believe that we promote the best possible outcomes by being open about the entire platform, and by hosting data for anyone who chooses to work with us. 
* *Build a world-class quality system.* We recognize that the work we're doing directly affects people's health. We must be very, very confident in the code we write and how we deploy it. But the traditional approach to life-safety software development uses a "design-first" approach that attempts to specify every requirement in advance. It's been shown time and again that that approach leads to massive cost overruns and explodes the size of development teams and the time required to build a solution. At Tidepool, we believe that we can show the world how to build great, high-quality software with a small team in an agile way. To prove it, we will need a concerted approach to how we manage all elements of quality -- from documentation to testing to verification to validation to requirements traceability. 
* *Get an FDA submission ready by end of year.* In order to put anything Tidepool builds into the hands of end users, we must get FDA approval for it. All of the above is necessary to get to this step. 

##Steps to get there:

Below are the major areas we are working towards to achieve the goals set out above. There is some sequence to this list of steps, but we will certainly not follow this sequence exactly. There are too many variables and people to impose significant control over sequencing.

###Provide a full spectrum of standard platform capabilities

Our platform needs to be a world-class, documented, secure, tested framework of capabilities for storing and retrieving diabetes data. Certain things are required from anything that wants to call itself a platform.

Descriptions here will usually be terse -- fuller explanations of what they mean can usually be found in github or our trello boards; if you want to know more about something and can't find it here, ask us in our IRC channel and someone will help you find the right place.

####For our own needs:

These are API features that we need in order to support Blip in the next few months.

  * Allow creation of multiple data sets per account, and transfer of ownership between accounts so that we can support test data, alternate data sets, and transfer of data from one owner to another (for example, when a child grows up).
  * Send emails so we can do signup confirmation, lost password transactions, and simple notifications.
  * Implement features for:
    * Invitations
    * Password recovery
    * User confirmation

  * Develop an "undergoing maintenance" page that we can quickly failover to if there are technical problems.
  * Develop a framework that allows partial deploys and A/B testing. Our current implementation of hakken and styx allow for a fair amount of flexibility here, but addition of a few capabilities would give us more control.

####To support third parties:

These features are aimed at allowing us to support third parties -- people who want to use Tidepool's platform as a backend, or open source people who want to stand up their own versions of Tidepool's APIs. 

  * Create API token system so we can let other apps use the platform.
  * Support OAuth2 and provide a platform login page so we can let users control which apps use the platform.
  * Support these things on a sandboxed version of our API so that app developers can build against a dummy API without real customer data. Provide some test accounts or test account generators so that people can experiment without creating lots of bogus data in our production database.
  * Version our API in the Accepts: header (makes the platform more predictable for third parties and makes it so that we can improve it without immediately obsoleting existing systems that depend on it).
  * Further refactoring; extract commonalities for our standard API. Make it easier to create things that fit into the platform.
  * Further work on automation -- what we do, how we do it (tools, etc). This has to do with builds and testing.

####For privacy / security / data stewardship:

We need to follow security best practices and defend our systems against attacks and poor programming. We also believe strongly that people own their own data and need to finish the work that will allow people to control their data.

  * Throttle API calls -- especially to the user-api -- to limit ability of badly-behaved clients to damage us.
  * Add password quality rules so people can't create bad passwords. 
  * Allow downloading all of someone's data in one action (result should be something that is machine-interpretable, but that can be understood with a little effort by someone who is not a software developer -- for example, a zip of JSON files).
  * Allow upload of one of these data blobs to recreate an individual's data. (This is lower priority than making sure people can download their data.)
  * Complete the features involved in allowing someone to delete their own account and all associated data.
  * Log database transactions so we can play them back for recovery between daily backups. This permits backup recovery without losing any data.
    * Requires ability to play back a log and feed it into the db.



###Update data storage system

The data storage system currently in use in Blip is a custom framework designed to support Blip, but it's not a general purpose system that allows multiple types of data to be stored together and used in different ways by different apps. Our goal is a unified storage model that integrates a wide range of data types. There is a separate document that is being developed on the data formats. There will be another on query languages.

####First:

  * Design and document our unified data storage model:
    * What is the data storage model? Define data schemas, data formats, and hierarchy of data.
    * How do we handle queries? We probably need a small query language. Design something that can start small and be extended.
    * How do we do notifications based on those queries?
    * How is data provenance tracked? How are modifications to data expressed?

####For Blip:

  * Create HTTP API for ingestion of data to the unified form. This will support upload of as little as a single point at a time (to support cloud-connected devices) as well as block uploads.
  * Create simple query system for first pass data extraction for Blip.
  * Implement new access API that can use either format
  * Adjust blip to operate using the new access API
  * Convert our uploaders to use the new format.
  * Migrate existing accounts from old to new.
  * Remove conversion logic from the access API.

####Further development:

  * Build tool for adjusting timestamps and time zones on blocks of data (possibly extend query system to support it). Make sure it's tracked in the provenance system.
    * Requires UI design and lots of testing, plus frontend and backend coding and probably several iterations.
  * Develop ability to query data provenance information for auditing.
  * Design and build a notification system that allows apps to subscribe to changes based on stored queries. It should support both connected and disconnected notifications (so, for example, someone could set an alert to send a message if the most recent BG reading goes below 50). 


###Universal Uploader

  * Build a general-purpose technology for downloading data from a USB serial device.
    * Probably browser-based.

  * Build a simulation device that can act like a pump or CGM and use that to build and test the integration.
  * Acquire or build a serial USB protocol analyzer for monitoring serial comms between a device and the PC.
  * Implement protocol plugin for each of the devices for which we have specifications:
    * Asante
    * ...

  * Implement app that can use a usb/bluetooth converter to upload data from a Dexcom.

###Step up test coverage, documentation and process

As discussed above, we need to be very serious about testing and measurement of that testing. We should resist pointless testing but should be confident in our test strategy and effectiveness of our coverage. We should also have documented policies of situations where new tests *must* be written.

  * Get amoeba covered by tests.
  * Drastically increase the number of tests in our deployed backend APIs.
  * Create better test suites for the whatever-client modules.
  * Get all active backend repositories covered by Travis-CI.
  * Start using code coverage tools to measure code coverage -- how do we decide when we have "enough"? Hook that up to everything after Travis.
  * Start analyzing our dependencies and their code coverage; decide how to manage them in a way we can track.
  * Re-engage on our test strategies for the frontend. Automated end-to-end testing is probably not the most effective use of our resources. How can we do this better?
  * Create model for how we do user requirements and how we tie them to Trello and GitHub.
  * Get further documentation set up and figure out how to make sure it's current and correct (probably auto-generate from our code -- but look into the recent Heroku schema-based approach). 

###Additional support for third parties like OSS devs and NightScout users

  * Extract commonalities for standard API modules. Where do we repeat ourselves and how can we do less of it?
  * Decide on development platform support (how much do we support Windows, for example?) and automation strategy (something other than shell scripts?)

###EHR Integration

For proper acceptance of Blip in clinics, we will have to provide a safe and effective means for the clinic's EHR systems to talk to Blip and Tidepool. It will not be feasible for Tidepool to do the integration work here -- what we can do is offer a set of features to make it as easy as possible for EHR systems to work with us. We are working on a set of user requirements for this. Roughly, though:

  * Allow patients to issue tokens to their providers for EHR integration. Tokens allow EHR systems to link to a specific patient's data without individual login
  * Require data package to identify user for messages and logging
  * Allow EHR deep links to individual messages that go straight to a particular time in a data stream
  * Define mechanism for supporting backlinks into EHR for posting notifications






