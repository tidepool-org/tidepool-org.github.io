---
layout: defaults
title: Grab Bag V1
published: true
---
# Grab Bag

Grab Bag represents an event with absolutely no semantic verification done on any of the fields. It exists to allow developers and users to define their own data types. A `grabbag` event looks like

~~~json
{
  "type": "grabbag",
  "subType": your_name_here,
  "time": see_common_fields,
  "uploadId": see_common_fields,
  ... your fields here ...
}
~~~

* `subType` - an identifier to indicate the semantics of this event. We recommend that any individual leveraging the `grabbag` event type uses subTypes in a unique namespace. One of the easiest ways to generate a unique namespace is to use a DNS name that you own and intend to continue owning. For example, if you wanted to have a `subType` = "saliva" and you control the website "allthingssaliva.com", consider creating the subType as "com.allthingssaliva.saliva" instead.

## Storage/Output Format

The storage and output format for this datum is exactly what was initially ingested. There are no modifications performed.