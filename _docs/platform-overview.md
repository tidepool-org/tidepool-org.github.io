---
layout: defaults
title: Tidepool Platform Architecture
published: true
---

# The Big Picture

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

