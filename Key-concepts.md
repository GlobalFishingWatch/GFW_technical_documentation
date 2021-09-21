This page provides an overview of some key concepts to understand when working with GFW data in BigQuery. 

Information is organized in the following topics:
+ Naming conventions
+ Table formats and data types
+ Query syntax and best practices
+ User defined functions (UDFs)

## Naming conventions

### General guidelines
+ Underscores not dashes
+ Lowercase
+ Be clear, concise, and consistent
+ Set a “time to live” (TTL) on all scratch_ datasets. TTL’s should be used with other datasets and tables when appropriate (see BigQuery docs).

### Dataset names
Datasets are the top level containers of our data in BigQuery. They should have standardized names that are easy to understand and navigate. Before creating a new dataset, consider whether the tables could be placed in an existing dataset.

#### Prefixes
The following prefixes should be used to organize datasets into groups:
+ `pipe`: These datasets contain automated tables that are consumed or produced by the pipeline. Do not manually add tables to these datasets
+ `VMS`: These datasets contain raw national VMS data. 
  + When processed by the GFW VMS pipeline, data is saved in a `pipe_[country]` dataset 
+ `vessel`: These datasets contain vessel registries and other inputs and outputs to the vessel database 
+ `machine_learning`: These datasets contain inputs/outputs of our machine learning models
+ `proj`: These datasets are for specific projects. Each project should receive its own dataset clearly named with the project topic and/or client (e.g. `proj_[topic]_[client]`)
+ `scratch`: These datasets are for preliminary tables generated during the early development and should be user specific (e.g. `scratch_tyler`)

##### Flags
The following flags may be included after the dataset prefix to symbolize additional information about the dataset when appropriate

`_[staging/production/published/archived]`
  + `_dev` (intermediate location outside of scratch_ for tables not yet ready for automation, e.g. `machine_learning_dev`)
  + `_staging` (automated precursor tables used only for automatic generation of tables in the corresponding `_production` dataset)
  + `_production` (tables that are automated by the pipeline)
  + `_published` (tables that contain data published on GFW portals and/or download portal)
  + `_archived` (old versions)

**NOTE: the naming for `_staging` and `_published` may change with the new dataset staging process**

`_v[YYYYMMDD]`: version of the dataset as indicated by the creation date
`_ttl_[days]`: default TTL for dataset tables (e.g. ttl_100)

### Table names
It is beyond the scope of this document and unnecessary to prescribe naming conventions for every table. However, tables within a dataset should use clear and self-explanatory names. Try to avoid redundancy by not using a table prefix that matches the dataset prefix (e.g. research_dev.pipe_production not research.research_pipe_production).

## Table formats and data types

### Date sharded and date partitioned tables

Many GFW tables are *date sharded*. This means they are actually a collection of tables, one for each date of data, and the data for a given date is stored in its own table (e.g. `messages_scored_20180101`). When looking in BigQuery, you will see the table name followed by a number in parentheses (e.g. `messages_scored_(3248)`), which indicates that there are 3,248 days of data in the `messages_scored_` table. Similarly, many AIS research tables are *date partitioned*, which are single tables where data is instead stored in date-specific partitions. 

Date sharded tables must end in `_YYYYMMDD`. If there is more than one date of data, the date will not appear in the resulting table name, rather the table name will end in an underscore followed by brackets containing the number of dates included in the sharded table. If the date refers to the creation date of the table and not the date of the data within the file, add a `v` to the beginning of the date to signify version (`_vYYYYMMDD`)

Date sharded and date partitioned tables behave in similar ways but, crucially, require different filtering syntax to limit query size.

### Data types

#### Working with structs and arrays

#### Casting

### Spatial BigQuery 


## User Difined Functions

We have a collection of UDFs in a dataset named `udf`.

## Query syntax and best practices

### Using subqueries

### Commenting 

### Query development and troubleshooting strategies

#### The BigQuery validator

#### Using a test subset
