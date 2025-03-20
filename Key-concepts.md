# Big Query Key concepts

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

### Access 

Working in BigQuery requires two forms of access:

1. [Google Cloud billing project](https://cloud.google.com/billing/docs/concepts#projects): Using BigQuery incurs costs and BigQuery requires users be connected to a GCP billing project before running queries.
   + The enabled billing project will be listed in the top menu bar of the BigQuery console
   + The billing project for GFW staff is `gfw-google` 
   + Research partners cannot be added to `gfw-google` and must create their own GCP billing project. GFW can help with this process and partners can generally receive ~$5,000 in free Google Credits through the Google for Good program. Contact Tyler (tyler@globalfishingwatch.org) for details

2. **[BigQuery project](https://cloud.google.com/resource-manager/docs/creating-managing-projects)**: Data in BigQuery is organized in projects, which in turn contain datasets and tables. To access GFW data, users must be granted access to the GFW BigQuery project.
    + GFW data lives in two primary BigQuery projects:
      + `world-fishing-827`: Primary GFW project containing all internal GFW datasets  
      + `global-fishing-watch`: Contains GFW public fishing effort tables
    + The project will appear in the left menu bar of the BigQuery console and clicking on the project name will expand to show the list of datasets the user can access. 
      + If you don't see a BigQuery project in your console: Click on `ADD DATA` -> `Pin a project` -> `Enter project name` -> type `world-fishing-827` -> select `PIN`    

### Billing

BigQuery costs are incurred in two ways - storing and querying data. 
    + Queries cost $5 per terabyte of data scanned
    + Queries are "free" for GFW staff, but we need to be careful

#### The BigQuery validator

The BigQuery validator is a built-in query debugger that also estimates query cost. The validator will turn green when a query is valid and be red otherwise. *Always use the validator to check the cost of a query before running it!*

## GFW datasets

Within a BigQuery project, data is stored in tables and organized in datasets. There are many datasets in the `world-fishing-827` project, but the following are the primary datasets for research and analysis:  

   + `pipe_production_vYYYYMMDD`: AIS pipeline dataset  
     + `pipe_[country]_production_vYYYYMMDD`: Country-specific VMS pipeline datasets
       > + VMS data can only be used by partners with written permission from the relevant national authorities (see [VMS](#VMS) for more info).
   + `gfw_research`: Contains a variety of static tables useful for analysis and early versions of new research concepts
   + `vessel_database`: Contains tables related to vessel registry information 
   + `anchorages`: Contains the GFW anchorages dataset 

If you don't see at least the `pipe_production_vYYYYMMDD`, `gfw_research`, `vessel_database`, and `anchorages` dataset let Tyler or Enrique know.
 
## Naming conventions

Many datasets and tables in the `world-fishing-827` project follow several key naming conventions. Abiding by these conventions will help make it easier for everyone to navigate the large amount of data in `world-fishing-827`. It is beyond the scope of this document and unnecessary to prescribe naming conventions for every table. However, tables within a dataset should use clear and self-explanatory names. 

### General guidelines

+ Underscores not dashes
+ Lowercase
+ Be clear, concise, and consistent
+ Set a “time to live” (TTL) on all scratch_ datasets. TTL’s should be used with other datasets and tables when appropriate (see BigQuery docs).

### Dataset names

Datasets are the top level containers of our data in BigQuery. They should have standardized names that are easy to understand and navigate. Before creating a new dataset, consider whether the tables could be placed in an existing dataset.

#### Prefixes

The following prefixes are used to organize datasets into groups:

+ `pipe_`: These datasets contain automated tables that are consumed or produced by a GFW pipeline.
+ `gfw_`: Datasets related to GFW's research work and public data 
+ `proj_`: Project-specific datasets. Each project should receive its own dataset clearly named with the project topic and/or client (e.g. `proj_[topic]_[client]`)
+ `scratch_`: These datasets are for preliminary tables generated during early development and should be user specific (e.g. `scratch_tyler`)
+ `VMS`: Datasets containing raw national VMS data. 
  + When processed by the GFW VMS pipeline, data is saved in a `pipe_[country]` dataset 
+ `machine_learning_`: These datasets contain inputs/outputs of our machine learning models

**Note**: Some datasets may end with the suffix `_ttl_[days]`. This indicates that the dataset has a "time to live" (TTL) set and tables in the dataset will be automatically deleted after the set period.

## Versioning

GFW receives new data every day and is constantly working to improve our algorithms and models. As a result, there can be multiple versions of a data product in BigQuery at the same time (e.g. the vessel database). To differentiate versions, GFW uses the `_v[YYYYMMDD]` naming convention for the suffix of a dataset or table, where the `YYYYMMDD` indicates the date the table was created. When multiple tables have the same name but end in different date strings, BigQuery will "stack" them together with the table name followed by a number in parentheses (e.g. `vi_ssvid_v(13)`). 

**When a dataset or table ends in `_v` followed by a date string (`_v20201001`), the date refers to the version of the table**. 

## Table formats

BigQuery tables can have several important formats that are used to organize data and minimize query cost and execution time. In particular, GFW creates tables that may be *date sharded* or *date partitioned*. Additionally, tables may be *clustered* on certain fields. Date sharded and date partitioned tables behave in similar ways but, crucially, require different filtering syntax to limit query size. Therefore, understanding how to recognize if a table is date sharded, partitioned, and/or clustered, and how to query them properly, is critical.

### Date sharded tables

Date sharded tables are actually a collection of tables, one for each date, where the data for a given date is stored in its own table where the table name ends in a date string (e.g. `messages_scored_20180101`). When there is data for more than one date, the table name will be followed by a number in parentheses, with the number indicating how many days of data are in the sharded table (e.g. `messages_scored_(3248)`). 

There are two main ways to restrict queries of date sharded tables depending on whether you’re querying one or multiple dates:

For a single date, you can specify the table directly using the full table name:

```
SELECT *
FROM pipe_production_v20201001.messages_scored_20180101
```

When querying multiple dates, you use a wildcard character (`*`) in the table name and then use the `_TABLE_SUFFIX` argument in the `WHERE` statement to specify the date range:

```
SELECT * 
FROM `world-fishing-827.pipe_production_v20201001.messages_scored_*` 
WHERE _TABLE_SUFFIX BETWEEN '20180101' AND '20180102'
```

*Note*: When using the `*` wildcard you must surround the table name in backticks.

### Date partitioned tables

Similar to date sharded tables, many GFW tables are date partitioned. These are single tables where data is stored in invisible partitions. If a table is partitioned, you will see `This is a partitioned table` when viewing the table's info in BigQuery. Tables can be partitioned on an actual field in the table (e.g. `date`) or on the `_partitiontime` field, which is a "psuedo colum" that is not actually in the table. You can check the partitioning specification by viewing the table's details.

Queries against date partitioned tables can filter on the partitioned column using a `WHERE` statement, limiting the amount of data scanned and thus the cost. 

```
SELECT *
FROM gfw_research.pipe_v20201001
WHERE _partitiontime = "2019-01-01"
```

*Note*: Unlike date sharded tables, use the `YYYY-MM-DD` format when filtering date partitioned tables.

### Clustered tables

BigQuery tables can be [clustered](https://cloud.google.com/bigquery/docs/clustered-tables) on certain fields. Similar to partitioning, clustering allows table data to be grouped together based on one or more table fields and improve query performance and cost when filtering on the clustered fields. You can check the clustering specification by viewing the table's details.

*Note*: Queries that filter on a clustered field may be cheaper than what is estimated by the BigQuery validator. 

## Key concepts

BigQuery is powerful but complex tool and understanding certain key concepts can help get the most out of BigQuery.

### User Defined Functions

User-defined functions (UDF) are functions created using another SQL expression or JavaScript. These functions accept columns of input, perform actions, and return a value. UDFs can be defined directly at the top of a query or stored as functions in a special type of dataset. GFW maintains a collection of helpful UDFs in the `udfs` dataset in `world-fishing-827`. These queries can be called in any query using the `udfs.function()` syntax.

### `STRUCTS` and `ARRAYS`

Two key BigQuery data types used by GFW are `ARRAYS` and `STRUCTS`, which allow for nesting and/or repeating data.

An `ARRAY` is an ordered list of zero or more elements of non-ARRAY values (e.g. `STRING`, `INT64`). An `ARRAY` is listed as having a `REPEATED` mode. GFW uses arrays to store multiple types of information, such as the list of identities (e.g. callsigns) broadcast by each MMSI over AIS.

A `STRUCT` is a container of ordered fields each with a type (required) and field name (optional). A `STRUCT` is listed as having a `RECORD` type. Fields are nested within a STRUCT - e.g. `struct_field.nested_field`. In general, GFW uses `STRUCT`s to group together information from different sources. For example, the vessel info tables use organize vessel identities from AIS and registries in separate `STRUCT`s.

### Spatial BigQuery 

BigQuery now supports a suite of [powerful spatial analysis tools](https://cloud.google.com/bigquery/docs/reference/standard-sql/geography_functions). These functions use the `GEOGRAPHY` data type, which represents a pointset (points, lines, polygons) on WGS84. Single points can be described by a (longitude, latitude) pair. For describing more complex geographies (lines, polygons), BigQuery supports GeoJSON, Well-known text (WKT), and well-known binary (WKB) data formats in the `GEOGRAPHY` column. GFW uses `GEOGRAPHY` data types to represent several data types, including the location of “events” (e.g. encounters, port visits, fishing).

### Data definition language

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
