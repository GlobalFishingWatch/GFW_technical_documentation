---
date-modified: last-modified
---


# BigQuery Key Concepts

This page provides an overview and key concepts for working with GFW data in BigQuery, Google's serverless, highly scalable enterprise data warehouse. 

Information is organized in the following topics:

+ BigQuery access and billing
+ Projects and datasets
+ Naming conventions
+ Versioning
+ Table formats
+ Key concepts
+ Best practices

## BigQuery access and billing

<!-- Mitgth want to think how to describe access without mentioning our projects--> 

<br>

### Billing{.unnumbered}

BigQuery costs are incurred in two ways - storing and querying data. 
    + Queries cost $6.25 per terabyte of data scanned
    + GFW staff can use the `gfw-google` billing project. Queries using this project are "free" for GFW staff, but we need to be careful.

### The BigQuery validator{.unnumbered}

The BigQuery validator is a built-in query debugger that also estimates query cost. The validator will turn green when a query is valid and be red otherwise. *Always use the validator to check the cost of a query before running it!*

## GFW datasets

Within a BigQuery project, data is stored in tables and organized in datasets. There are many datasets in the `world-fishing-827` and `global-fishing-watch` projects. See the [BigQuery Table Reference page](bigquery-pipe3-table-reference.md) for the list of key datasets.

## Naming conventions

Many datasets and tables follow several key naming conventions. Abiding by these conventions will help make it easier for everyone to navigate the large amount of GFW data in BigQuery. It is beyond the scope of this document and unnecessary to prescribe naming conventions for every table. However, tables within a dataset should use clear and self-explanatory names. 

### General guidelines{.unnumbered}

+ Underscores not dashes
+ Lowercase
+ Be clear, concise, and consistent
+ Set a “time to live” (TTL) on all scratch_ datasets. TTL’s should be used with other datasets and tables when appropriate (see BigQuery docs).

### Dataset names{.unnumbered}

Datasets are the top level containers of our data in BigQuery. They should have standardized names that are easy to understand and navigate. Before creating a new dataset, consider whether the tables could be placed in an existing dataset.

#### Prefixes{.unnumbered}

The following prefixes are used to organize datasets into groups:

+ `pipe_[source]`: These datasets contain automated tables that are consumed or produced by a GFW pipeline. The `source` indicates the type of data included in the pipeline.
+ `gfw_`: Datasets related to GFW's research work and public data 
+ `proj_`/`prj`: Project-specific datasets. Each project should receive its own dataset clearly named with the project topic (e.g. `proj_[topic]`)
+ `scratch_`: These datasets are for preliminary tables generated during early development and should be user specific (e.g. `scratch_tyler`).  
+ `VMS`: Datasets containing raw national VMS data. 
  + When processed by the GFW VMS pipeline, data is saved in a `pipe_[country]` dataset 
+ `machine_learning_`: These datasets contain inputs/outputs of our machine learning models

**Note**: Some datasets may end with the suffix `_ttl_[days]`. This indicates that the dataset has a "time to live" (TTL) set and tables in the dataset will be automatically deleted after the set period.

## Versioning

### Pipeline versioning{.unnumbered}

GFW strives to use a [semantic versioning](https://semver.org/) approach to version our data pipelines. These version numbers are indicated in the dataset name using the `_v` flag following the `pipe_[source]` prefix. For example, the `pipe_ais_v3_published` dataset name indicates that the it contains published tables for the 3rd version of the AIS pipeline. 

### Date versioning{.unnumbered}

GFW is constantly receiving and processing new data. As a result, certain tables may be updated periodically to incorporate new data. To differentiate versions of the same table created at different points in time, GFW uses the `_v[YYYYMMDD]` naming convention for the suffix of a table, where the `YYYYMMDD` indicates the date the table was created. When multiple tables have the same name but end in different date strings, BigQuery will "stack" them together with the table name followed by a number in parentheses (e.g. `vi_ssvid_v(13)`). 

**When a dataset or table ends in `_v` followed by a date string (`_v20201001`), the date refers to the version of the table. However, when there is no `_v` preceeding a date string, the date indicates that that table includes data for that specific date, as explained in the next section on table formats**. 

> **Note: Using the latest view versus versioned tables**
> For `product_events_{EVENT_TYPE}` tables we only preserve the most recent version and it is recommended to use the view version of the table rather than a specific dated version. In a future pipeline release we will remove the specific dated versions of these tables. It is generally recommended to use the latest view instead of versioned tables and the way tables are versioned may change in the future. Furthermore, access to versioned tables may be restricted to certain users in future pipeline releases.

### Compatability tables{.unnumbered}
Occasionally, we need to apply breaking changes within a pipeline version. E.g. during the pipe3 lifetime we had to make a breaking change to the definition of `product_events_port_visits`. When this happens, we add a versioning suffix, in this case `product_events_port_visits_v2`. This "compatability table" will be maintained for the lifetime of the pipeline version and will replace the original table in the next pipeline version. In this case, `product_events_port_visits_v2` will become `product_events_port_visits` in pipe4 and the original `product_events_port_visits` table will be removed.

## Table formats

BigQuery tables can have several important formats that are used to organize data and minimize query cost and execution time. In particular, GFW creates tables that may be *date sharded* or *date partitioned*. Additionally, tables may be *clustered* on certain fields. Date sharded and date partitioned tables behave in similar ways but, crucially, require different filtering syntax to limit query size. Therefore, understanding how to recognize if a table is date sharded, partitioned, and/or clustered, and how to query them properly, is critical.

### Date partitioned tables{.unnumbered}

Many GFW tables are date partitioned. These are single tables where data is stored in invisible partitions. If a table is partitioned, you will see `This is a partitioned table` when viewing the table's info in BigQuery. Tables are generally partitioned on an actual field in the table (e.g. `date` or `event_start`). You can check the partitioning specification by viewing the table's details.

Queries against date partitioned tables can filter on the partitioned column using a `WHERE` statement, limiting the amount of data scanned and thus the cost. 

```
SELECT *
FROM pipe_ais_v3_published.messages
WHERE DATE(timestamp) = "2019-01-01"
```

### Date sharded tables{.unnumbered}

Unlike date partitioned tables, date sharded tables are actually a collection of tables, one for each date, where the data for a given date is stored in its own table where the table name ends in a date string (e.g. `messages_scored_20180101`). When there is data for more than one date, the table name will be followed by a number in parentheses, with the number indicating how many days of data are in the sharded table (e.g. `messages_scored_(3248)`). 

There are two main ways to restrict queries of date sharded tables depending on whether you’re querying one or multiple dates:

For a single date, you can specify the table directly using the full table name:

```
SELECT *
FROM pipe_ais_v3_internal.messages_scored_20180101
```

When querying multiple dates, you use a wildcard character (`*`) in the table name and then use the `_TABLE_SUFFIX` argument in the `WHERE` statement to specify the date range:

```
SELECT *
FROM `world-fishing-827.pipe_ais_v3_internal.messages_scored_*` 
WHERE _TABLE_SUFFIX BETWEEN '20180101' AND '20180102'
```

*Note*: Unlike date partitioned tables, use the `YYYYMMDD` format when filtering date sharded tables. Additionally, when using the `*` wildcard you must surround the table name in backticks.

### Clustered tables{.unnumbered}

BigQuery tables can be [clustered](https://cloud.google.com/bigquery/docs/clustered-tables) on certain fields. Similar to partitioning, clustering allows table data to be grouped together based on one or more table fields and improve query performance and cost when filtering on the clustered fields. You can check the clustering specification by viewing the table's details.

*Note*: Queries that filter on a clustered field may be cheaper than what is estimated by the BigQuery validator. 

## Key concepts

BigQuery is powerful but complex tool and understanding certain key concepts can help get the most out of BigQuery.

### User Defined Functions{.unnumbered}

User-defined functions (UDF) are functions created using another SQL expression or JavaScript. These functions accept columns of input, perform actions, and return a value. UDFs can be defined directly at the top of a query or stored as functions in a special type of dataset. GFW maintains a collection of helpful UDFs in the `udfs` dataset in `world-fishing-827`. These queries can be called in any query using the `udfs.function()` syntax.

### `STRUCTS` and `ARRAYS` {.unnumbered}

Two key BigQuery data types used by GFW are `ARRAYS` and `STRUCTS`, which allow for nesting and/or repeating data.

An `ARRAY` is an ordered list of zero or more elements of non-ARRAY values (e.g. `STRING`, `INT64`). An `ARRAY` is listed as having a `REPEATED` mode. GFW uses arrays to store multiple types of information, such as the list of identities (e.g. callsigns) broadcast by each MMSI over AIS.

A `STRUCT` is a container of ordered fields each with a type (required) and field name (optional). A `STRUCT` is listed as having a `RECORD` type. Fields are nested within a STRUCT - e.g. `struct_field.nested_field`. In general, GFW uses `STRUCT`s to group together information from different sources. For example, the vessel info tables use organize vessel identities from AIS and registries in separate `STRUCT`s.

### Spatial BigQuery{.unnumbered}

BigQuery now supports a suite of [powerful spatial analysis tools](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions). These functions use the `GEOGRAPHY` data type, which represents a pointset (points, lines, polygons) on WGS84. Single points can be described by a (longitude, latitude) pair. For describing more complex geographies (lines, polygons), BigQuery supports GeoJSON, Well-known text (WKT), and well-known binary (WKB) data formats in the `GEOGRAPHY` column. GFW uses `GEOGRAPHY` data types to represent several data types, including the location of “events” (e.g. encounters, port visits, fishing).

### Data definition language{.unnumbered}

BigQuery supports the use of [data definition language (DDL)](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language), which allow you to create, alter, and delete tables directly in your query statement. This is a particularly helpful way to create partitioned and clustered tables.

## Best Practices

+ Always use standard SQL - first line of your query should be `#standardSQL`
+ Write subqueries using `WITH` statement -- avoid overly nested queries
+ Comment between queries to give space visually. 
+ Put all tables referenced at the top of the query.
+ When using UDFs, try to use SQL based instead of JavaScript
+ For numerical parameters that will be used in multiple places, you can use a UDF at the top of the query that returns the value
+ To limit costs when developing or debugging queries:
  + Restrict the date range or partitioned or sharded tables
  + Use a `LIMIT 1000` clause at the end of a query. Comment this out when running for real
+ For complex queries, consider including a (commented out)  test query that does a sanity check
