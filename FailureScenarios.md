---
layout: defaults
title: Tidepool Platform Failure Scenarios
published: true
---


This page discusses the various failure scenarios that could happen to the Tidepool Platform and how we deal with them.

## Data Loss Scenarios

Any time data is stored somewhere, it can be lost.  It is the job of a data storage platform to mitigate the risks of such a loss, but it is impossible to ensure that data will be available 100% of the time.  Consider the pathological case of an alien race coming and destroying all computers everywhere on Earth before we are able to upload a virus into their mothership.  If this were to happen, all data stored on computers would be lost.

So, if it is impossible to eliminate the risk of losing data, why even bother?  Well, it turns out that we can take measures to minimize the risk of data loss.  It's true that it could still happen, but that same alien race could also destroy all of humanity as well, in which case, your diabetes data might not be the top priority on your mind anymore.  The following are the measures that we take at Tidepool to ensure that your data is safe.

### Replicated database

Our database is 3-way replicated in a master-slave fashion.  This means that when something is added or changed in our database it is rapidly replicated to 2 other servers that house the exact same data.  If the master node goes down, your data is still available from one of the other replicas.  If one of the slaves goes down, we can bring up a new slave and it will re-replicate in a matter of hours.

### Nightly backups

We maintain nightly backups of data stored in our databases.  In the unlikely event that all replicas of our database are completely destroyed, we can stand up new servers and load them up with the most recent backup.  Given that these backups are only taken nightly, it does mean that we would lose any changes to the data that happened between when we took the backup and when the servers met their doom.

### Known Risk Factors

Even given the systems described, there are some failure scenarios that could cause prolonged downtime or lack of access to data.  These are

#### Data Center Loss

Our servers operate out of a single data center in Oregon.  If a catastrophe of some sort were to happen such that this data center were no longer available, our services would go down.  When the data center returns to service, we should be able to come back from our backups.

#### Amazon S3 Outage

At the time of this writing, our backups are stored in encrypted form on [Amazon S3](http://aws.amazon.com/s3/details/) (full redundancy) in the US West (Oregon) region.  If something were to happen such that Amazon lost the data in this region, our backups would be affected.

Note, if S3 were to have an issue, but our servers were still operational, the Tidepool Platform would continue to operate as intended and we would be able to take measures such that our backups are stored somewhere else.