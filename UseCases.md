---
layout: defaults
title: Tidepool User Stories
published: true
---
# Use cases for Tidepool applications
This document is attempting to aggregate a set of relatively high-level "user stories" or "use cases" for Tidepool. They don't necessarily follow the modern traditional form often adopted by software development teams. We're OK with that.

The information under "Detail" is the rough algorithm or description of the technical solution.

*(Note on May 12 2014 -- this document needs serious work.)*

## Patient with T1D

| Story | Detail |
| ---- | ---- |
| I can define a group to permit access to my data. | Look up my metadata, let me edit the members in the ```team``` access group. Visit all the users who were removed from the access group and remove me from their patients group. Visit all the users who were added, add them to my invited group, and add me to their invitedby group. If they're not users, send them an email invitation to join.
| I can send a message to my care team. | Look up metadata, look up team, use message API to send message to that team.|
| I can include a link in a message to a member of my care team. | We'll have the ability to generate a URL that will record the data view you're currently looking at, so you can paste that URL into a message to a member of your team and they can click on it and see the same view.|
| I can invite someone who is already a Tidepool user to join my care team. | I can specify an email address for my friend; we look up that address and find the account associated with that email address; place me in friend's "invitedBy" group, and place friend in my "invited" group. Friend gets an email with a custom link to accept; also a notification at next logon.|
| I can invite someone who is not already a Tidepool user to join my care team. | I can specify an email address for my friend; if we look up that email and do not find the account associated with that email address, we create a temporary account for it (we need to figure out how the temporary accounts are marked); place me in my friend's "invitedBy" group, and place my friend in my "invited" group. My friend gets an email with a custom link to sign up for Tidepool; upon completion of signup, she gets the same notification as if the account had already existed.|


## User but not a patient

| Story | Detail |
| ---- | ---- |
| I can receive an email or notification inviting me to be on a care team. | Patient enters email, we create a database entry noting the invitation, we send link to a special signup page. Clicking the link gives me a page explaining the care team invitation with accept/decline buttons on it. Accepting moves people from the "invited" groups into the "team" groups. |
| I can create an account by following a link in an invitation email. | We use database entry for this email address to preload the user's account with the relationship to the patient. |
| I can read messages in any patient team I'm a member of. | Read my patients group, read their team groups, fetch messages to those groups. |

## Parent of a patient

| Story | Detail |
| ---- | ---- |
| I can set up an account for my child to be a patient. | We should probably have a user flow that creates a child's account AND a parent account at the same time and with the appropriate group relationships, so that we can minimize the problem of parents pretending to be their children. We really want a user account to be the patient account. |
| I can upload data to my child's account. | Read my patients group, read those patients uploaders groups; if I'm in the uploaders group for a patient then I can upload to that account. |

## Doctor with multiple patients

| Story | Detail |
| ---- | ---- |
| I can upload data for a patient when they're in my office. | Read my patients group, read those uploaders groups; if I'm in the uploaders group for a patient then I can upload to that account. |
| I can view data for any patient I have access to. | Look at my patients group and present a list of patients to choose from based on who I have access to. |

