---
layout: defaults
title: Tidepool Query Implementation specification
published: true
---

This document was created by Kent Quirk and last edited in late August, 2014. It is a work in progress.

# Query Implementation Spec

# Introduction

This document attempts to spec out the framework on which we will implement TQL, the [Tidepool Query language](../queries-and-notifications). 

It is a simple endpoint that implements a basic JSON-based query system intended to be able to express TQL queries.

## Structure

The query object is an object. 

Top-level fields can be:

### types

The `types` object contains an array of objects. Each object in that array contains
two fields: `type`, and `columns`. The `type` is a string containing the name of a
Tidepool type. `columns` contains a list of column definitions.

Each column definition contains a key (called `k`) and value (called `v`). The key is a string; the value is a value object.

A value object can be either: 
  * a numeric value ('{"n": 123.45}')
  * a string ('{"s": "hello"}`
  * a named column alias in the current result set (`"column": "timestamp"`)
  * a named field in the current row (`"field": "deviceTime"`)
  * the result of a function call (`{"fn": "functionname", "a": []}`), where a is an array of arguments, all of which can be value objects themselves.

The key (`k`) is the alias by which the column is known.

### where

The `where` clause defines the set of records to be returned. Where takes a single
value object that should return a nonzero value if the row is to be included in the result.

### sortby and groupby

Sortby takes an array of objects defining decreasing importance in a sort (the first item is the most important, etc). 

Each object has `column` which is the name of the column, and may also provide
`comparator` as the name of the comparator and `reversed` to invert the sense of
the comparator.

Groupby names a list of columns to group by after sorting.

### limit and offset
Limit and offset take numeric values for the limit of rows to be returned and the offset of the starting row. Offset defaults to 0. If not specified, limit may be limited by the constraints of the authentication token being used.

````text
    QUERY
        TYPE IN cbg
        COLUMNS time, value*18.01559 AS bg
        WHERE bg > 200 and Month(time) = Month(Now()) And Year(time) == Year(Now())
````


````json
{
    "types": [{
        "type": "cbg",
        "columns": [
            { "k": "time", "v": { "field": "time"}},
            { 
                "k": "bg", 
                "v": {
                    "fn": "mult", "a": [
                        { "field": "time"}, 
                        { "n": 18.01559 }
                    ]
                }
            }
        ]
    }],
    "where": {
        "fn": "and", "a": [
            { "fn": "gt", "a": [{"column": "bg"}, { "n": 200 }] },
            { "fn": "eq": "a": [
                {"fn": "Month", "a": [{"column": "time"}]}, 
                {"fn": "Month", "a": [{"fn": "Now", "a": []}]}
            ]},
            { "fn": "eq": "a": [
                {"fn": "Year", "a": [{"column": "time"}]}, 
                {"fn": "Year", "a": [{"fn": "Now", "a": []}]}
            ]},
        ]
    },
    "sortby": [
        { "column": "time", "comparator", "Numeric", "reversed": true },
    ],
    "groupby": ["time"],
    "limit": 100,
    "offset": 0
}
````
