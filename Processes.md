---
layout: defaults
title: Tidepool Development Processes
published: true
---

# Tidepool Development Processes

## Overview

This document contains the processes we use for developing our software. It's intended 
to be read by Tidepool employees, by open source contributors, and by regulators. This is how we do what we do. 

Table of contents:

* [Migration and Deployment](#migration-and-deployment)
* [Tidepool's Trello Boards](#tidepools-trello-boards)
* [Bug Triage and Management](#bug-triage-and-management)



## Migration and Deployment

Product input will be vetted by product team. New ideas will be captured on a Trello board (see [Tidepool's Trello boards](#tidepools-trello-boards)) as an "idea."
Potential sources: Users, community, employees, or contributors
Types of input: Data quality, feature suggestion, general question, usabilty, broken functionality (for prioritization, see [Bug Triage and Management](#bug-triage-and-management)

* Before moving a Trello card onto the “In Master and on Devel” Trell list, make sure it includes link(s) to the pertinent GitHub pull request(s).

* Before moving a Trello card onto “On Staging; Needs Testing”, make sure it includes instructions on how to test if it’s not self-explanatory.

* Before a pull request moves to production, it must have a Trello Card representing it that includes the link to the pull request. This PR must be tested and the tester must acknowledge test completion and approval on the Trello card.


## Tidepool's Trello Boards

* [Blip](https://trello.com/b/GPadCYvP/blip) - This board is actively used by Tidepool's development team to manage front-end work in progress.
* [Blip Product Backlog](https://trello.com/b/iKydvoiJ/blip-product-backlog) - This board stores all feature suggestions (we call them "ideas") for Blip. The list of "Force-Ranked" ideas have been prioritized for future development.
* [Open Source Tasks](https://trello.com/b/uTNKzwka/open-source-tasks) - This board lists all the ideas that are ripe for the open source pickens. Contributions are welcome! Each card will point to the relevant GitHub repository and include a "developer buddy" - that's a Tidepool employee who will be your primary point of contact as you develop this feature.
* [Platform](https://trello.com/b/xLF1XmeQ/platform) - This board is actively used by Tidepool's development team to manage backend work in progress. 



## Bug Triage and Management

Tidepool follows a developer-specific method of bug triage. In this practice, the issue is escalated to the developer most likely to have developed the code where the bug exists. If there is ambiguity, it starts with the developer who created the most visible front end component affected and they determine whether it needs to be handed off (presumably to a backend developer).


### Prioritization

#### P1. Drop Everything

A P1 issue needs to be addressed immediately, ahead of anything else other than P1 issues. The kinds of things that qualify to be P1 are:

* Primary service down: This is when the app or a part of the app becomes unavailable. The appropriate team members drop everything and address the issue.
* Data quality issue: This is where a user will see health data that is inaccurate or can be misleading.
* Urgent business issue: This is when there are critical business issues related to time-sensitive matters.
The developer who owns the issue will stop what they’re working on to address the issue.

#### P2. Important

A P2 issue is important and it needs to be handled soon. P2 issues are subject to adjustment in priority, especially relative to each other, but for the most part, P2 issues will be handled before P3 issues. Things that qualify as P2 include:

* Expected functionality is not working and it affects existing users or something of importance to the business.
* An issue that is less important to Tidepool but has some time-critical component where its value drops if we don’t do it on time.
* An issue that enables other important work.
* An issue that can be addressed with a patch or workaround for the time being; in this case, the patch might be P2 but the long-term fix could be P3.

#### P3. As Time Permits

P3 issues are items that are due for development but take a back seat to urgent or important issues. Examples are:

* Product ideas that will improve the speed or usability of poorly performing features. 
* Bugs that do not affect critical functionality -- cosmetic or minor functional bugs.
* Features that are required for commercialization but are not critical or enabling.

#### P4. Someday

P4 issues are essentially on the backlog, but available for being worked on if they happen to fall out of other work or fit between other, more urgent tasks. Examples are: 

* Product ideas to improve existing features that already work reasonably well.
* Product ideas that add brand new features. 
* Visual issues not affecting functionality.

Note that issues can change in priority over time.

