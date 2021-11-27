This page provides an overview and key concepts for working with GFW data in BigQuery, Google's serverless, highly scalable enterprise data warehouse. 

Information is organized in the following topics:

+ BigQuery access and billing
+ Projects and datasets
+ Naming conventions
+ Versioning
+ Table formats and data types
+ Key concepts
  + Subqueries
  + Partitioned vs sharded tables
  + `STRUCTS` and `ARRAYS`
  + Spatial BigQuery
  + User defined functions
  + Data definition language (DDL)
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

`_v[YYYYMMDD]`: version of the dataset as indicated by the creation date

## Table formats and data types

### Date partitioned and date sharded tables

Many GFW tables are *date sharded*. This means they are actually a collection of tables, one for each date of data, and the data for a given date is stored in its own table (e.g. `messages_scored_20180101`). When looking in BigQuery, you will see the table name followed by a number in parentheses (e.g. `messages_scored_(3248)`), which indicates that there are 3,248 days of data in the `messages_scored_` table. Similarly, many AIS research tables are *date partitioned*, which are single tables where data is instead stored in date-specific partitions. 

Date sharded tables must end in `_YYYYMMDD`. If there is more than one date of data, the date will not appear in the resulting table name, rather the table name will end in an underscore followed by brackets containing the number of dates included in the sharded table. If the date refers to the creation date of the table and not the date of the data within the file, add a `v` to the beginning of the date to signify version (`_vYYYYMMDD`)

Date sharded and date partitioned tables behave in similar ways but, crucially, require different filtering syntax to limit query size.

### Data types

#### Working with structs and arrays

### Spatial BigQuery 

## User Defined Functions

We have a collection of UDFs in a dataset named `udf`.

## Query syntax and best practices

### Using subqueries

### Commenting 

### Query development and troubleshooting strategies

#### The BigQuery validator

