---
layout: defaults
title: Tidepool Platform Architecture
published: true
---

#The Big Picture

Tidepool's platform is based around a few key principles:

## Small pieces, loosely joined

The phrase, stolen from [David Weinberger](http://www.smallpieces.com/index.php), was talking about the architecture of the web in general, but it applies equally well to specific implementations like Tidepool. By building our platform on a collection of small, individual components, we give ourselves a few benefits:

* Easy to design, build, and test each piece
* Easy to understand what each piece does and convince yourself that it's correct
* Each piece is isolated and does a limited set of tasks well
* Easy for others to contribute to in an open source environment

These benefits all interlock and reinforce each other. 

## Open Source

All of our code will be open source and licensed with the [permissive BSD-2 license](http://opensource.org/licenses/BSD-2-Clause). This means that everything we do is open for study, experimentation, and reuse by other people. James Wilson has written [a good article](http://oss-watch.ac.uk/resources/whoneedssource) on the topic.


## Robust data security

The HIPAA act requires a certain minimum level of effort and care around patient data security, but we want to go beyond that and establish Tidepool as the gold standard for data security and integrity -- while doing so in the open. We want to show people not only that it can be done, but how it should be done.

Consequently, we're establishing a set of practices around our internal data design that mean that even if our data is exposed, it won't present the kinds of risks that people have been reading about recently. Only someone who has complete access to our server systems (and the number of those people can be counted on one hand) could put all the pieces together.

## RESTful API design

We're going to take a stab at a definition here, and say that RESTful APIs are ones where resources provided by the API are mapped to URLs, and where common database actions (typically called [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete)) are mapped to HTTP verbs like POST, GET, PUT, and DELETE.

The value of this is that it makes it easy to write services that communicate using standard web protocols in whatever language is convenient. It also makes it easy to make those services available over the web.

## JavaScript and Node

The kind of client applications that Tidepool wants to build -- beautiful, elegant presentation of sophisticated data -- require JavaScript. There is an explosion of engineering effort being expended to build better and better tools and techniques. Furthermore, nearly all the work being done in JavaScript is open source.

Because of this, and because nearly every good client application needs support on the server side as well, JavaScript is rapidly becoming a strong competitor in the server space. [Node](http://nodejs.org/) is a credible server platform with a strong community. By standardizing on Node, Tidepool will benefit both from the tremendous rate of growth in this space as well as allowing our engineers and contributors to work in JavaScript for both client and server. Furthermore, the tooling support for such environments is also growing leaps and bounds. It's an exciting place to be.

## Heavily tested and testable
Having isolated, small components makes it a lot easier to write test systems that help to verify that the pieces work as designed. We are writing test scripts to validate our APIs at the unit test level as well as with integration tests. When we find bugs, we have a general policy to try to write a (failing) test that will demonstrate the bug, then fix the bug, and then prove that the test now succeeds. We expect to run tests automatically on our repositories (we're currently ramping up with Travis-CI) and measure test coverage (experimenting with Coveralls). 

Please note that testing is something that improves over time. The developer who wrote the code is both the best person to write some tests (because presumably the dev knows the code) and the worst (because any blind spots in the code are likely to be duplicated in the tests). 

# High-level Structure

## Process structure, routing, and discovery

Each individual component of the Tidepool Platform is a nodejs application that manages some list of URLs and HTTP verbs. For example, the message API handles a URL called "send", among others. These node applications are deployed on physical servers. Each server has one or more "slots" for applications. The servers and slots are managed by a system we call shio. We can deploy a given node application on multiple slots if necessary to support scaling.

A system called hakken operates as a discovery system. Hakken maintains a list of services and their locations in the system. It supports multiple instances of a given service, and hakken itself can be deployed multiple times. This level of redundancy allows us to build a system that can survive the loss of any individual component.

External requests from applications on the internet come in to the system through the router, which is known as styx. The router is the only application that has an externally-accessible IP and internet address (api.tidepool.io). The router maintains a mapping of URL paths to particular process names.

When a request arrives, the router looks up its path and attempts to match it to a service name. When a match is found, it looks up the service name using hakken and routes the request to an instance of the appropriate node server, rewriting the url along the way.

Example:
* Client requests ```POST https://api.tidepool.io/message/send```
* Router receives request, looks up "message", and finds the service called "message-api"
* A request to hakken with "message-api" returns an internal IP address a.b.c.d and port number p.
* The request is rewritten and forwarded to that IP and port as ```https://a.b.c.d:p/send```

# Tidepool Components

## Server management
### Discovery

* *Name* hakken (Hakken means "discovery" in Japanese.)
* *Purpose* Service Discovery
* *GitHub repository and documentation* [https://github.com/tidepool-org/hakken](https://github.com/tidepool-org/hakken)
* *Description* Hakken provides a mechanism for services to find one another without having to store configuration information in each service. It also allows multiple copies of each service (including hakken itself). Hakken has one or more coordinators that communicate with one another to maintain a common list of other services. The services register themselves with hakken as they wake up. When and if they disappear, hakken will eventually notice the fact and remove them from the registry.

### Routing
* *Name* styx
* *Purpose* Routing requests from the open internet to the various endpoints within Tidepool's backend.
* *GitHub repository and documentation* [https://github.com/tidepool-org/styx](https://github.com/tidepool-org/styx)
* *Description* Styx is a service that uses hakken to discover services and route requests. It is essentially a software load balancer, but it integrates with hakken (and can be extended to integrate with other service discovery mechanisms) to find services. It also allows for integration with an authentication layer and intelligent forwarding, like consistent hashing. It is intended as a front-tier layer for a backend API.

### Server process management
* *Name* shio (The name comes from the Japanese word for the tide.)
* *Purpose* Managing a list of servers and processes to assign to those servers.
* *GitHub repository and documentation* [https://github.com/tidepool-org/shio](https://github.com/tidepool-org/shio)
* *Description* Shio operates by combining "deployable tarballs" with "config tarballs" and running the application. Shio has two main concepts: servers and slots. Servers are the physical machines that you deploy to. Slots are deployment slots that exist on servers for deployment. You can have multiple different slots on the same server and deploy different things to that one server.

## User data storage
### User Login and ID
* *Name* user-api
* *Purpose* User login and session management
* *GitHub repository* [https://github.com/tidepool-org/user-api](https://github.com/tidepool-org/user-api)
* *API documentation* [http://docs.tidepooluserapi.apiary.io/](http://docs.tidepooluserapi.apiary.io/)
* *Description* The Tidepool User API is used to manage the user login information and pretty much nothing else. (Temporarily, it also manages sessions, but that will change.) It manages authentication and the most basic user information necessary for logging in.

### User Metadata
* *Name* seagull
* *Purpose* Tracking user metadata with per-user encryption.
* *GitHub repository* [https://github.com/tidepool-org/seagull](https://github.com/tidepool-org/seagull)
* *API documentation* [http://docs.tidepoolseagull.apiary.io/](http://docs.tidepoolseagull.apiary.io/)
* *Description* Seagull manages user metadata in a storage vault that encrypts each user's private data with a different key. This helps to keep things secure against unauthorized access to the data store. It also provides a framework for generating and managing IDs and keys that can be used for keeping user data secure in other storage systems.

### Groups
* *Name* armada
* *Purpose* Manage groups of users for messaging and authorization
* *GitHub repository* [https://github.com/tidepool-org/armada](https://github.com/tidepool-org/armada)
* *API documentation* [http://docs.tidepoolarmada.apiary.io/](http://docs.tidepoolarmada.apiary.io/)
* *Description* Armada has a simple task -- managing groups of users. It just tracks a set of user IDs under an anonymous key. The keys and their meanings are stored in seagull. 

### Messages
* *Name* message-api
* *Purpose* Recording time-stamped message threads sent to a group (as defined by the armada)
* *GitHub repository* [https://github.com/tidepool-org/message-api](https://github.com/tidepool-org/message-api)
* *API documentation* [http://docs.tidepoolmessages.apiary.io/](http://docs.tidepoolmessages.apiary.io/)
* *Description* Tracking health data will also often involve tracking commentary on that data by the patient and possibly the patient's care group. The message API provides an infrastructure for tracking a secure message store with timestamps to allow data to be associated with other health events. Eventually this message system will probably be integrated into the event stream.

## Medical Data Storage
### Raw data tracking and provenance
* *Name* sandcastle
* *Purpose* Tracking blocks of raw data, including the source of that data and all modifications to it.
* *GitHub repository and documentation* [https://github.com/tidepool-org/sandcastle](https://github.com/tidepool-org/sandcastle)
* *Description* Sandcastle uses the version control system git as a storage base. Blocks of data can be sent to sandcastle and they are then stored in git in a special repository unique to each user. Future changes to that data are stored as revisions in git, meaning that the state of the data and all changes to it (the provenance) are tracked. A future benefit is that an interested user could clone and manage their personal git repository.

### Data Upload
* *Name* jellyfish
* *Purpose* Manages uploads of health data through sandcastle
* *GitHub repository* [https://github.com/tidepool-org/jellyfish](https://github.com/tidepool-org/jellyfish)
* *Description* Under active development

## Application Server
### Blip Data API
* *Name* blip-data-api
* *Purpose* To provide the server-side support for the blip application
* *GitHub repository* [https://github.com/tidepool-org/blip-data-api](https://github.com/tidepool-org/blip-data-api)
* *Description* Under active development

## Client Applications and Libraries
### Visualization of diabetes data
* *Name* tideline
* *Purpose* A visualization module for diabetes data
* *GitHub repository and documentation* [https://github.com/tidepool-org/tideline](https://github.com/tidepool-org/tideline)
* *Description* Under active development

### Message app
* *Name* clamshell
* *Purpose* A browser-based, mobile-friendly message application
* *GitHub repository and documentation* [https://github.com/tidepool-org/clamshell](https://github.com/tidepool-org/clamshell)
* *Description* Under active development. This is an application that can be used to get and read messages relating to health data. It uses user-api, seagull, and message-api from the client side to read and send messages. It allows patients and their care team to communicate with each other securely and to add context to the data being tracked by Tidepool. 


## Useful libraries
### Time conversion
* *Name* sundial
* *Purpose* Centralize time conversion and manipulation
* *GitHub repository* [https://github.com/tidepool-org/sundial](https://github.com/tidepool-org/sundial)
* *Description* Intended to create a central place for managing time formats, timezone management, and the like. Under active development.

### Testing
* *Name* salinity
* *Purpose* A library to help in creating test fixtures and mocks
* *GitHub repository* [https://github.com/tidepool-org/salinity](https://github.com/tidepool-org/salinity)
* *Description* Under active development

### Wrapper for easy access to user API
* *Name* user-api-client
* *Purpose* Server-side npm package providing functions and middleware for easily using the user-api.
* *GitHub repository* [https://github.com/tidepool-org/user-api-client](https://github.com/tidepool-org/user-api-client)
* *Description* Under active development

### Wrapper for easy access to message API
* *Name* message-api-client
* *Purpose* Server-side npm package providing functions and middleware for easily using the message-api.
* *GitHub repository* [https://github.com/tidepool-org/message-api-client](https://github.com/tidepool-org/message-api-client)
* *Description* Under active development




