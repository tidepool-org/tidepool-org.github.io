---
layout: defaults
title: Data Model Migrations
published: true
---

## _schemaVersion=0

This migration applies to changes introduced as part of the "UTC Bootstrap" ADD_REFERENCE_HERE changes and introduces a generic [`_schemaVersion`](v1#versioning-and-updates) to our data model.

**NOTE:**

If you are running locally your device data is most likley in a mongo db called ``streams``. The examples below are using a db called ``data`` as we use in our deployed environments. This inconsistency will be fixed shortly.

### Check

In the mongo console check to see if you need to apply the migration by finding how many records don't have the `_schemaVersion`

```
> use data
> db.deviceData.find({"_schemaVersion": {"$exists":false}}).count()
```

you should see a count of the records

```
49903
```

### Backup

In mongo backup your existing data collection by performing a ``mongoexport``

```
$ mongoexport --db data --collection deviceData --out deviceData_PreSchemaVersion.json
```

Once successful you will see a count of the records that have been copied to the backed up collection

```
$ exported 49903 records
```

### Apply

On the collection you want to update run the following

```
> use data
> db.deviceData.update({_schemaVersion: {"$exists":false}},{$set:{_schemaVersion:0}},{multi:true})
```

You should receive feedback that the update has succeeded similar to the following.

**NOTE:** 

you don't always see this feedback, in our deployed environments we run the query listed below to ensure all records are now upgraded.

```
WriteResult({ "nMatched" : 49903, "nUpserted" : 0, "nModified" : 49903 })
```

or run this query to check all records are now upgraded. Post upgrade it should return 0

```
> use data
> db.deviceData.find({"_schemaVersion": {"$exists”:false}}).count()
```

### Rollback 
**(if required)**

If you want run the process again or rollback for any reason then do the following

```
$ mongoimport --db data --collection deviceData --type json --file ./deviceData_PreSchemaVersion.json --jsonArray --upsert
```

Once successful you will see a count of the records that have been copied back to your original collection.

```
...

$ 2015-07-09T10:39:20.054+1200        Progress: 27120488/30577254 88%
$ 2015-07-09T10:39:20.054+1200            44200   245/second
$ 2015-07-09T10:39:23.004+1200        Progress: 29776906/30577254 97%
$ 2015-07-09T10:39:23.004+1200            48500   265/second
$ 2015-07-09T10:39:23.341+1200 check 9 49903
$ 2015-07-09T10:39:23.439+1200 imported 49903 objects
```
