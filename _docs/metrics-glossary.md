---
layout: defaults
title: Metrics Glossary
published: true
---
#Metrics Glossary

This is a glossary for our metrics tracking, by module.

##Blip

Clicked Back To Care Team List
: User clicked the Back To Care Team List button

Clicked Back To Data
: User clicked the Back To Data button

Clicked Back To Profile
: User clicked the Back To Profile button

Clicked Chart Refresh { fromChart }
: User clicked the Chart Refresh button (fromChart property indicates which chart was the source of the click)

Clicked Create Profile
: User clicked the Create Profile button

Clicked Edit Profile
: User clicked the Edit Profile button

Clicked Go To Most Recent { fromChart }
: User clicked the Go To Most Recent button (fromChart property indicates which chart was the source of the click)

Clicked Hide Two Week Values
: User clicked the Hide Two Week Values button

Clicked Message Icon
: User clicked the Message Icon button

Clicked Message Pool Background
: User clicked the Message Pool Background button

Clicked Navbar Logged In User
: User clicked the Navbar Logged In User button

Clicked Navbar Logo
: User clicked the Navbar Logo button

Clicked Navbar Upload Data
: User clicked the Navbar Upload Data button

Clicked Navbar View Profile
: User clicked the Navbar View Profile button

Clicked No Data Refresh
: User clicked the No Data Refresh button

Clicked No Data Upload
: User clicked the No Data Upload button

Clicked Other Care Team
: User clicked the Other Care Team button

Clicked Own Care Team
: User clicked the Own Care Team button

Clicked Show Two Week Values
: User clicked the Show Two Week Values button

Clicked Switch To One Day
: User clicked the Switch To One Day button

Clicked Switch To Settings { fromChart }
: User clicked the Switch To Settings button (fromChart property indicates which chart was the source of the click)

Clicked Switch To Two Week
: User clicked the Switch To Two Week button

Closed Message Thread Modal
: User clicked the Message Thread Modal button

Closed New Message Modal
: User clicked the New Message Modal button

Created New Message
: User clicked the New Message button

Double Clicked Two Week Value
: User doubleclicked the Two Week Value button

Logged In
: User logged in

Logged Out
: User logged out

Panned Chart { fromChart, direction (forward/back) }
: User panned the Chart (fromChart property indicates which chart was the source, direction is forward or back)

Replied To Message
: User replied to a message

Signed Up
: User signed up

Updated Account
: User updated their account

Updated Profile
: User updated their profile

Viewed Account Edit
: User viewed the Account Edit page

Viewed Care Team List
: User viewed the Care Team List page

Viewed Data
: User viewed the Data page

Viewed Profile
: User viewed the Profile page

Viewed Profile Create
: User viewed the Profile Create page

Viewed Profile Edit
: User viewed the Profile Edit page

##Clamshell

##Platform

###Metrics that are tied to a user action

collection update {coll: collection}
: Posted by Seagull when a collection is updated for the given user.

collectionRead {coll: collection}
: Posted by Seagull when a collection is read for the given user.

deleteuser {server: boolean}
: Posted by Seagull when a user is deleted; the "server" property tells whether the deletion came from a server (and therefore included a user ID) or whether it came from a user session (and the user ID was implied). 

getuserinfo {server: boolean}
: Posted by user-api when information about a user is requested; the "server" property tells whether the request came from a server (and therefore included a user ID) or whether it came from a user session (and the user ID was implied). 

usercreated
: Posted by user-api when a user is created. 

userlogin
: Posted by user-api when a user logs in.

userupdated {server: boolean}
: Posted by user-api when information about a user is updated; the "server" property tells whether the update came from a server (and therefore included a user ID) or whether it came from a user session (and the user ID was implied). 

manageprivatepair {verb: req.method}
: Posted by user-api when a request is made to manage a "private pair". The request method controls which operation is executed.



###Metrics without a user associated with them

addThread
: Posted by the message-api when a new message thread is created.

findAllById
: Posted by the message-api when the findAllById call is used.

findById
: Posted by the message-api when the findById call is used

getNotes
: Posted by the message-api when the getNotes call is used.

getThread
: Posted by the message-api when the getThread call is used.

private read {pair: name}
: Posted by the user-api when a server reads a private pair; property is the name of the pair.

replyToThread
: Posted by the message-api when the replyToThread call is used.

serverlogin
: Posted by the user-api when a server logs in to the system.

