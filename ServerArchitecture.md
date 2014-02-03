---
layout: defaults
title: Tidepool Platform Architecture
published: true
---
# Server Architecture

## Process structure, routing, and discovery

Each individual component of the Tidepool Platform is a nodejs application that manages some list of URLs and HTTP verbs. For example, the message API handles a URL called "send", among others. These node applications are deployed on physical servers. Each server has one or more "slots" for applications. The servers and slots are managed by a system we call shio. We can deploy a given node application on multiple slots if necessary to support scaling.

A system called hakken operates as a discovery system. Hakken maintains a list of services and their locations in the system. It supports multiple instances of a given service, and hakken itself can be deployed multiple times. This level of redundancy allows us to build a system that can survive the loss of any individual component.

External requests from applications on the internet come in to the system through the router, which is known as styx. The router is the only application that has an externally-accessible IP and internet address (api.tidepool.io). The router maintains a mapping of URL paths to particular process names.

When a request arrives, the router looks up its path and attempts to match it to a service name. When a match is found, it looks up the service name using hakken and routes the request to an instance of the appropriate node server, rewriting the url along the way.

![Server operations]({{ site.url }}/images/architecture/RoutingDiagram.png)

###Example:
* The 'message-api' server starts up at internal IP address 20.10.10.10:5050 and registers itself with hakken.
* The router is set up to know that requests for the path ```/message``` go to "message-api".
* The client requests ```POST https://api.tidepool.io/message/send```
* Router receives request, looks up ```/message```, and finds the service called "message-api"
* A request to hakken with "message-api" returns the IP address and port number ```20.10.10.10:5050```.
* The original IP and the ```/message``` part are stripped from the message, and the request is forwarded (behind the firewall) as ```https://20.10.10.10:5050/send```


## Users and Patients
Every person using Tidepool's platform needs to have a user account. Some users are patients. Patients have patient data. In other words, all users have accounts -- only some people have medical data.

We are very careful to keep the two separate. 

We are also very careful not to leak which of our users might be patients. Patient data is stored under an encrypted name, and that name is only stored in an account's privately encrypted metadata. It is not possible to tell by looking at our user database which of our users are patients. Conversely, given access to a database of medical information, even a Tidepool employee can't tell which patient it belongs to.

For a detailed explanation of data organization, please see the [Server Data Organization](ServerDataOrganization.html) page. This document just sketches out the big picture.

## Accounts and login
That account has a unique number in the Tidepool system that never changes. The user can associate that with an arbitrary list of email addresses. We also have a slot for a unique user name, which may simply be an email address. User name and email addresses are completely under the user's control.

When a user logs in, they can use any of these unique identifiers plus their password to identify themselves.

The user database is deliberately very spare, for two reasons:

* We intend that the userdata database design and implementation will almost never change once we get it stable. As the userdata is central to access to the entire Tidepool platform, changes to it will have a tendency to destabilize multiple systems.
* The user database cannot be encrypted on a per-user basis (because we need to be able to find users and do things like password reset). Consequently, it is more secure to move most userdata into a separate metadata system.

Consequently, the user database only has the necessary fields for logging in, plus a private ID and hash that are used to store and encrypt the user's private metadata.

## User Metadata and Groups

All other user information is either stored by or stored through the user metadata system. This system stores information under an opaque ID, and is encrypted with a different hash for every user. This ID and hash never leave Tidepool's private network. Metadata, in turn, also maintains different per-user ID/hash pairs so that each portion of user data that is encrypted has a different key.

## Patient Data
Identifiable medical information (messages, device IDs, etc) will be similarly encoded.

Medical data that has been stripped of identifiable information, on the other hand, is encrypted only in the aggregate (not per-patient). For example, readings from a blood glucose monitor are not identifiable, so they are stored so that anonymized information can be aggregated and used in future research.

## Encryption

When we use encryption, we use industry-standard algorithms that have been tested, with key lengths that are considered adequate even under modern understanding of the potential weaknesses of industry-standard encryption. 

Every encryption system in Tidepool uses a long "deploy salt" string that ensures that even someone who steals the keys in the user database would not be able to decrypt the patient data without this additional deploy key (which is not stored in the database -- it's an environment variable set up during the deployment).

Furthermore, Tidepool's hard disks are set up using a RAID array, which means that we have multiple redundant copies of all of the information on the disks. Any disk that fails can be replaced without any loss of information.

Each individual disk is encrypted at the volume with a random key that is setup during installation -- and not even Tidepool knows the key. We essentially treat individual disks as ephemeral. If one were to be physically stolen or recycled to someone who tried to read it, no one would be capable of decrypting the information on it.

