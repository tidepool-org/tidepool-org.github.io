---
layout: defaults
title: Data Model Migrations
published: true
---

### _schemaVersion=0

This migration applies to change introducted as part of the "UTC Bootstrap" ADD_REFERENCE_HERE changes and introduces a generic [`_schemaVersion`](v1#versioning-and-updates) to our data model.

**Check**

In the mongo console check to see if you need to apply the migration by finding how many records don't have the `_schemaVersion`

```
> db.deviceData.find({"_schemaVersion": {"$exists”:false}}).count()
```

you should see a count of the records

```
49903
```

**Backup**

In mongo backup your existing data collection by performing a copyTo

```
$ mongoexport --db data --collection deviceData --out deviceData_PreSchemaVersion.json
```

Once successful you will see a count of the records that have been copied to the backed up collection

```
$ exported 49903 records
```

**Apply**

On the collection you want to update run the following

```
> db.deviceData.update({_schemaVersion: {"$exists":false}},{$set:{_schemaVersion:0}},{multi:true})
```

You should revieve feedback that the update has succeeded similar to the following

```
WriteResult({ "nMatched" : 49903, "nUpserted" : 0, "nModified" : 49903 })
```

```
> db.deviceData.find({"_schemaVersion": {"$exists”:false}}).count()
```

**Rollback** (if required)

If you want run the process again or rollback for any reason then do the following

```
$ mongoimport --db data --collection deviceData --type json --file ./deviceData_PreSchemaVersion.json --jsonArray
```

Once successful you will see a count of the records that have been copied back to your original collection.

```
$ imported 49903 records
```