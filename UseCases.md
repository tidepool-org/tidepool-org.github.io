---
layout: defaults
title: Tidepool User Stories
published: true
---
# User stories for Tidepool applications
This document is attempting to aggregate a set of relatively high-level "user stories" or "use cases" for Tidepool. They don't necessarily follow the modern traditional form often adopted by software development teams. We're OK with that.

The information in *italics* is the rough algorithm or description of the technical solution.

## Patient with T1D

* I can define a group to permit access to my data. *Look up my metadata, edit the members in the team access group. Visit all the users who were removed from the access group and remove me from their patients group. Visit all the users who were added, add them to my invited group, and add me to their invitedby group. If they're not users, send them an email invitation to join.*
* I can send a message to my care team. *Look up metadata, look up team, use message API to send message to that team.*
* I can include a link in a message to a member of my care team. *We'll have the ability to generate a URL that will record the data view you're currently looking at, so you can paste that URL into a message to a member of your team and they can click on it and see the same view.*


## User but not a patient

* I can receive an email inviting me to be on a care team. *Patient enters email, we create a database entry noting the invitation, we send link to a special signup page.*
* I can create an account by following a link in an invitation email. *We use database entry for this email address to preload the user's account with the relationship to the patient.*
* I can read messages in any patient team I'm a member of. *Read my patients group, read their team groups, fetch messages to those groups.*

## Parent of a patient
* I can set up an account for my child to be a patient. *We should probably have a user flow that creates a child's account AND a parent account at the same time and with the appropriate group relationships, so that we can minimize the problem of parents pretending to be their children. We really want a user account to be the patient account.*
* I can upload data to my child's account. *Read my patients group, read those patients uploaders groups; if I'm in the uploaders group for a patient then I can upload to that account.*

## Doctor with multiple patients
* I can upload data for a patient when they're in my office. *Read my patients group, read those patients uploaders groups; if I'm in the uploaders group for a patient then I can upload to that account.*
* I can choose a patient from the list of people I have access to, and view the data of a particular patient. *Look at my patients group and present a list of patients to choose from.*

