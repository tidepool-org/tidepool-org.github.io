---
layout: defaults
title: Tidepool Query Language and Notifications system
published: true
---

This document was created by Kent Quirk and last edited in mid-July, 2014.

#Background

This is an attempt to sketch out a framework for how Tidepool will provide a system for handling queries against the data we track.

A simple query environment might supply a few basic tools for handling the sorts of queries needed by our applications. For example, there might be an API call to return blood glucose readings from a CGM over a given period of time, and another to return pump settings records.

The trouble with such a design is that it requires us to reasonably anticipate the kinds of queries that would be useful, and if we get it wrong, it requires us to rev the API every time something new comes up.

Our data design somewhat minimizes the problem in that we have the concept of a database full of common record types, so that we can have a somewhat uniform structure to return the results of queries. But as we hope eventually to permit researchers to query de-identified collections of data across multiple users, it's unlikely that we'll be able to reasonably anticipate all of their needs.

Another approach to queries is the query language. The idea of this sort of structure is that you provide a single query API that accepts a string; the string is a textual description of a query; the system decodes the request, performs the query, and returns the results in a standard form. Only a single API endpoint is required.

SQL is a model for this sort of concept -- but the language isn't quite appropriate for our needs. First of all, we aren't using a SQL database; furthermore, the SQL language is famously complex. 

If we spec out a flexible design for a query language, we can implement it over time without ever changing the API or breaking existing code.

#Design rules

* It's a query language, not a data manipulation language -- it will not be possible to modify data with this language, only to retrieve it. 

* It should scale to not only retrieve data for a single user, but permit the concept of longitudinal retrieval across multiple patients.

* It should learn from SQL without being slavishly imitative -- SQL has lots of dark corners and we don't need or want to replicate those.

* It should be relatively easy to write and to read -- there shouldn't be a lot of hangup on formatting for the query language, and it should be easy to embed in a string within another programming language. This probably means that line endings shouldn't be any different than other whitespace, and we should minimize the amount of syntactic sugar required (terminators, etc). 

* The query language should be "easy" to parse, which in the language of compiler writers means that it's LR1-parsable -- only one token of lookahead is required. This restriction means that it's simple to create such a parser with industry-standard compiler tools like YACC and its descendants.

# First pass

I started by sketching out some use cases and attempting to write down sensible queries for them. Over time, a proposal has congealed for what the language should look like. The examples below use that language.

## Notes

A few notes when reading the examples:

* TYPE controls the row types returned by the query; FIELDS returns the fields within the rows. WHERE applies other constraints. 

* By having a TYPE clause rather than specifying the record type as part of the WHERE clause, it becomes easier to check for correctness and to specify the type-specific fields as part of the fields clause. This is an example of not mimicking SQL.

* It's useful to look at examples of plausible queries first as that can inform the definition of the JSON-based query language.

* ALL CAPS are keywords, Initial Caps are functions provided by the language, lowercase are variables or fields.


#Example use cases and the queries for them

## Simple use cases

* Fetch all fields in all records for CBG, SMBG, basal, bolus, and notes for the last two weeks to present it all on one graph.

~~~
    QUERY
      TYPE IN cbg, smbg, basal, bolus, notes
      // Omitting a FIELDS spec implies all fields
      WHERE Age(time) < Weeks(2)
~~~

* Fetch all CGM blood glucose readings over the last 90 days in order to calculate a profile of the "average" day's BG.

~~~
    QUERY
      TYPE IN cbg
      FIELDS time, value
      WHERE Age(time) < Days(90)
~~~

* Fetch BG readings as above, but return only the date portion of the time (this is likely the first step in an aggregation)

~~~
    QUERY
      TYPE IN cbg
      FIELDS Date(time), value
~~~

* Show the single most recent SMBG value and the time it was taken.

~~~
    QUERY
        TYPE=smbg
        FIELDS time, value, trend
        SORT BY time REVERSED
        LIMIT 1
~~~

* Find all the times in the current month when BG was above 200 mg/dL and report the results in mg/dL

~~~
    QUERY
        TYPE IN smbg, cbg
        FIELDS time, value*18.01559 AS bg
        WHERE bg > 200 and Month(time) = Month(Now()) And Year(time) == Year(Now())
~~~

* Find all boluses of at least 10 units within the last quarter, whether they're normal or extended. Because fields default to Null if not present, we need to compare to Null to get valid results.

~~~
    QUERY
        TYPE IN bolus
        FIELDS time, If(Null(normal), 0, normal) AS n,
            If(Null(extended), 0, extended) AS e, n + e AS totalbolus
        WHERE totalbolus > 10 AND Age() < Days(90)
~~~



## More difficult but plausible

* Find the min and max values of BG today

~~~
    QUERY
        TYPE IN cbg, smbg
        FIELDS Min(bg), Max(bg)
        WHERE Date(time) == Today()
~~~

* Find the estimated A1C based on BG readings for the past 90 days.

~~~
    QUERY
        TYPE IN cbg, smbg
        FIELDS (Avg(bg) + 46.7) / 28.7
        WHERE Age() < Days(90)
~~~

* Calculate the number of cgm readings that were below range, in range, and above range.

~~~
    QUERY
        TYPE IN cbg
        FIELDS 
            IF(bg < 60, "Low", IF(bg > 180, "High", "In Range")) AS reading, 
            Count() AS count
        SORT BY reading
        GROUP BY reading
~~~

* Fetch pump settings change records and look for all bolus records within 1 unit of the max value for the pump setting in effect as of the given bolus record.

~~~
    LET q1 = QUERY
        TYPE IN settings
        FIELDS time AS starttime, maxbolus, time + Duration() AS endtime
        WHERE Age(starttime) < Days(90)

    QUERY IN q1
        TYPE IN bolus
        FIELDS *
        WHERE time BETWEEN q.starttime, q.endtime
        AND q.maxbolus - units < 1.0
~~~



* Find all the dates in the last 30 days when all BG readings were lower than 50 for more than 30 minutes. 

~~~
    LET q1 = QUERY
        // this query finds the transitions
        TYPE IS cbg
        SORT BY time
        FIELDS time, bg, Previous(bg) AS lastbg
        WHERE lastbg >= 50 AND bg < 50 OR lastbg < 50 and bg >= 50

    QUERY IN q1
        // and this query finds the durations by looking at the up transitions
        FIELDS time, bg, time - Previous(time) AS duration
        WHERE bg > 50 AND duration > Minutes(30)

    FILTER IN q2
        FIELDS time, bg
~~~


* Find all the boluses that were not preceded by a carb record within the previous 15 minutes.

I can't even think of a plausible solution to this that doesn't require self-join syntax and some complex built-in functions. I think this is one where you do a query for all the data and iterate it manually. If you have ideas, please share!

## For research

* Find all the girls 12 and under who had a bg reading over 300 on their birthday (presumably from too much cake and ice cream).

~~~
    LET subject = METAQUERY
        WHERE Age(birthdate) < YEARS(12)

    QUERY
        TYPE IN smbg, cbg
        FIELDS subject.birthdate AS bday, time AS ts, bg
        WHERE bg > 300 AND Month(ts) == Month(bday) AND Day(ts) == Day(bday)
~~~


* Fetch all of the hypoglycemic episodes lasting longer than 60 minutes for 17-year-old boys and correlate them to time of exercise.

~~~
    LET subject = METAQUERY
        WHERE Age(birthdate) BETWEEN YEARS(17), YEARS(18)

    QUERY IN subject

    (not finished)
~~~

# Query Language Description

## General

* Whitespace is used as a separator; tab, space, newline, carriage return all qualify as whitespace and no distinction is made between them (except that // comments end at the next newline). 

* Comments are anything beginning with // up to the next newline. Comments are treated as whitespace.

* Case is ignored; nothing is case-sensitive. By convention, keywords are in CAPS and Functions() are in titlecase, and field names are lowercase. But all of that is completely optional.

* Strings can be contained within single quotes ('), double quotes ("), or triple-quotes (''' or """); strings do not require quotes unless they contain whitespace, start with a digit, or contain a non-alphanumeric character. Strings may not contain a newline and there is no escape character. All of these are equivalent:
  * ```hello```
  * ```"hello"```
  * ```'hello'```
  * ```'''hello'''```
  * ```"""hello"""```

* There are a small number of keywords. These cannot be used as field names and will not be used as function names.

* Functions are names followed by an argument list in parentheses. Function names must start with a letter and can contain alphanumerics, plus underscore.

* Numbers are accepted as integer and float in decimal format, in scientific notation (-9.9e99), or in hex as integers only (0xbaadf00d).

* True and False are accepted as boolean values.

* A timestamp expressed in ISO 8601 format (YYYY-MM-DDTHH:MM:SS with optional timezone -- if no timezone is specified it's assumed to be UTC) is parsed as a datetime object and converted to the internal format. If you want a timestamp to be a string, use quotes.

* Operators are traditional; some operators have multiple forms (& or AND, for example). Both = and == are accepted for conditional equality.

## Metaquery

The metaquery identifies the set of accounts to whom the query applies. 

For user tokens, the only accounts that the query can apply to are those accounts where the user has either root privilege or view privilege. If the metaquery is not specified, then the query is applied only to the account data for whom the token has root privilege.

If the metaquery is specified, it must omit the TYPE qualifier and use field names found in the metadata (to be more completely specified at a future time).

Examples:

    METAQUERY
        WHERE emails CONTAINS "user@tidepool.org"  // empty if you don't have access

    METAQUERY
        WHERE userid IS "12d7bc90fa"


For research tokens, only the fields accessible to the researcher can be selected against (for example, user name is probably not accessible). Note that some fields may be accessible for some users who have explicitly consented, but not for other users. In these circumstances, a constraint that specifies a restricted field will only ever return users who have authorized that restricted field, even if the result of the constraint would otherwise be true. It is as if every metaquery contains an implied constraint of "authorized(myToken) AND..." before each WHERE clause.

The QUERY following the METAQUERY is executed for each user selected and the results are aggregated. If the METAQUERY is named, the FIELDS clause may include metaquery fields.


## Query

The QUERY clause is made up of a set of subclauses:

### Query name

    [LET name = ] QUERY [IN previousquery]

This begins the query; if the query is to be referred to later, it must be named with the LET prelude.

If a query has no IN clause, it operates on the result of the previous explicit or implied METAQUERY. If it does have an IN clause, it operates on the results of the named query.

### TYPE clause

    TYPE[S] [IN] typelist

The type clause identifies the types of records that will be used in the query; only records of the types mentioned here will be retrieved. The TYPE clause is not permitted in a metaquery. If the TYPE clause is omitted from a query that uses the IN clause, it essentially operates as a filter, acting to further reduce or tune a query.

The typelist is a comma-separated list of types.

### FIELDS clause

    FIELDS fieldlist

    fieldlist: [type.]fieldname (AS alias), ...
    fieldlist: fieldexpression AS alias, ...

The FIELDS clause identifies the set of fields that will be returned by the query. The fieldnames must be fields within the set of types specified by the typelist. If the fieldname is ambiguous then it may be preceded by the type name and a dot (.). The fieldname will be used as the key in the output JSON; an alias may be specified to change the key value.

Instead of fieldname, an expression can be used that can specify operations on fieldnames; for example, ```Month(time) as month```.

### SORT clause

    SORT BY field [REVERSED][, ...]

The SORT clause controls how the output of the query is ordered. 

The SORT clause is run before any LIMIT clause or OFFSET clause is calculated, and before any windowed operations are run (Previous, Next).

If not specified, the sort order is not guaranteed, and the same query may return different results from run to run.

### GROUP BY clause

    GROUP BY field[, ...]

GROUP BY generates a single row for each distinct value of the GROUP BY fields. Aggregation operations (Min, Max, Average, Sum, Count) apply only to the collection of rows with this value. If the data says:

    x   y
    -   -
    a   2
    c   5
    b   3
    b   4
    c   6
    c   7

And the query contains:

    FIELDS x, Count() AS count, Sum() AS sum SORT BY x GROUP BY x

The result will be:

    x   count  sum
    -   -----  ---
    a       1    2
    b       2    7
    c       3   18


### LIMIT clause

    LIMIT number

The LIMIT clause controls the maximum number of records that can be returned from a query. The number must be positive.

The LIMIT is always applied after SORT, OFFSET, and GROUP.

If not specified, no specific limit is applied, but the token restrictions for a given token may cause the results of a query to be limited.


### OFFSET clause

    OFFSET number

The OFFSET clause specifies an offset within the results at which to start the results; it is intended to be combined with the LIMIT clause for use in paginating the results. OFFSET is applied after the SORT and GROUP clauses but before the LIMIT clause.


### WHERE clause

    WHERE constraintexpr

The WHERE clause specifies constraints on the sets of records that will be returned. A constraint expression must evaluate to a boolean; when true for a given record, the record is selected for inclusion in the output of the query.

## Field names

While Tidepool uses well-defined schema for the types, we don't want to embed the schema into the query language. Instead, use of field names that are not defined for a given type will return a null result. Comparisons with a null result return false. 


## Keywords

We'll have a list here someday.

## Operators

We'll support the standard set of operators, probably using AND, NOT, IS rather than
the c-like operators. Parentheses and evaluation order will be standard.

    * / % 
    + -
    AND OR NOT XOR 
    BETWEEN


## Functions

### Conditional

* If(cond, a[, b]) -- returns a if cond is true, b if cond is not true

### Time

Timestamps are represented as a long number of seconds. Should timestamp
functions default to Now() if ts is not specified? 

* Month(ts) -- returns the Month portion of a timestamp
* Day(ts) -- returns the Day portion of a timestamp
* Hour(ts) -- returns the Hour portion of a timestamp
* Year(ts) -- returns the Year portion of a timestamp
* Minute(ts) -- returns the Minute portion of a timestamp
* Second(ts) -- returns the Second portion of a timestamp
* Time(ts) -- returns the Time portion of a timestamp
* Date(ts) -- returns the Date portion of a timestamp
* Now() -- returns a timestamp of the current date and time
* Today() -- returns a date of today's date; same as Date(Now())
* Age(ts) -- returns the result of (Now() - ts) in seconds

### Aggregation and Window functions

* Previous(field[, N]) -- returns the named value from the Nth previous row (N defaults to 1 if not specified), or null if it selects a nonexistent row.
* Min(field) -- returns the minimum value of the field over the whole query, before any LIMIT or OFFSET clauses are applied
* Max(field) -- like Min, but maximum value is returned
* Average(field) -- returns the average value of a numeric field
* Count() -- returns the count of the rows

## Implementation order

First, design a JSON-based query structure that will be the output of the query language. It needs data structures to accommodate the full set of query language capabilities. 

Next, implement the evaluation engine for a subset of the JSON structure; it should be able to do the following:

* Single data account only; metaqueries are not supported.
* TYPE clause
* FIELDS clause for named fields but not expressions, aliases
* SORT BY single named field
* LIMIT and OFFSET
* WHERE for simple expressions based only on field values in the current row 
* Date/time functions and basic expression evaluation
* No GROUP, no window or aggregation functions

Implement a JSON query endpoint that can accept this data structure, validate it, and execute the query. It should also include appropriate error handling and throttling of queries. 

The above will give us a functional query system that can be used for our existing apps; it won't be ready for research use, however.

After this, it should be possible to write a lexical scanner and a parser for the language; recognition of all keywords and operators, and recognition of use of functions. Valid queries are recognized and invalid queries are reported. The Abstract Syntax Tree for the language will be generated. 

Then, a first-pass implementation of the query engine. This does not attempt to reconfigure queries to optimize them -- it just reads the input data structure, does a query against Mongo, and returns the results as a block of JSON. The only parts of the language that it needs to properly handle are the ones that are implemented in the query backend. 

Before the query system can be rolled out to third parties, we need:

* Query costing -- to calculate the cost of a query and allow throttling requests based on the cost.
* It would be nice to have EXPLAIN -- to estimate the cost of a query before 
running it

Next features should be based on actual need, but likely ordering is:

* SORT BY multiple fields
* Calculated columns -- functions and expressions in column definitions
* Functions and expressions in the WHERE clause based on calculated columns
* Metaqueries across multiple users
* Aggregation functions and GROUP BY

## Notifications

### NOTIFY clause

For a named query, it is possible to specify a NOTIFY clause. 

    NOTIFY query EVERY nSeconds NotifyFunction(args...)

Example:

    NOTIFY q1 EVERY Minutes(4) SendSMS("999-999-9999", "The query changed.")

Notifications work like this:

* The query is scheduled to run every nSeconds seconds (not guaranteed -- under heavy load, the query time may lengthen).
* If the query returns no data, nothing happens. If it returns data, the first record of the result is evaluated. If it differs from the previous first record, the NotifyFunction is called with the arguments specified. (The Notify functions will support templates that can fill in values from the query results.)
* Supported NotifyFunctions are likely to be SendSMS, PostURL, GetURL, and SendEmail. The query system doesn't yet specify the exact behavior of these functions, but they can return an error code if the attempt fails. For non-fatal errors, the query system will attempt to retry the NotifyFunction call using exponential backoff. If the same query is retriggered before the previous one succeeds, further attempts will be abandoned (there can be only one active instance of a given query). 
* Notify queries will run against the token they were submitted with, and will consume query resources accordingly. If query resources expire (if the query is using resources at a faster rate than they regenerate) the query will not be run until the resources are available again. This will have the effect of slowing down the query repeat rate.

