# GFW Pipelines {#pipelines}

A GFW "pipeline" refers to an automated process by which GFW converts raw AIS or other data into the datasets and tables used for research and analysis. Pipelines are a multi-step process that includes ingesting and cleaning raw data, applying GFW algorithms, and saving data to BigQuery tables and/or other locations. 
 
## AIS pipeline

The AIS pipeline is GFW's primary pipeline and includes the following processes:

1. Normalize and combine raw AIS data from Orbcomm and Spire
2. Remove noise and group positions into segments (e.g. the "segmenter") 
3. Predict which positions are likely fishing positions
4. Identify various event types (e.g. transshipment, port visits, fishing, AIS gaps, etc.) 

[AIS data tables](#AIS-data-tables) and [AIS event tables](#AIS-event-tables) produced by a given version of the AIS pipeline are stored within several BigQuery datasets. The `pipe_ais_[version]_published` contains final output tables (e.g. positions, events, vessel identity) of the pipeline and are the primary tables used in analyses, research, and products. Additionally, intermediate tables that are primarily used to generate the tables in `pipe_ais_[version]_published` are stored in a separate dataset named `pipe_ais_[version]_internal`.   


### `pipe_ais_[version]_published` tables

+ `messages`
+ `segment_info`
+ `segs_activity_daily`
+ `segs_activity`
+ `ssvids_identities_daily`
+ `ssvids_identities`
+ `vessel_info`

### `pipe_ais_[version]_internal` tables

AIS data tables are tables produced by the AIS pipeline that contain a version or summary of the *entire* AIS dataset. The first type of AIS data tables are those that contain a complete copy of the AIS data output during various steps in the pipeline: 

+ `position_messages_YYYYMMDD`
+ `messages_segmented_YYYYMMDD`
+ `spatial_measures_YYYYMMDD`
+ `features_YYYYMMDD`
+ `fishing_score_YYYYMMDD`
+ `messages_scored_YYYYMMDD`
+ `messages_regions`

For example, the `position_messages_YYYYMMDD` table is output following data normalization, and the `messages_segmented_YYYYMMDD` table is the output of the segmenter stage of the AIS pipeline. The final output is the `messages_scored_` table, which contains *all* AIS position messages and their fishing score.

The other group of AIS data tables include tables that summarize the complete AIS data by segment or vessel. These tables may be further organized into daily date sharded tables and overall tables. Daily summary tables are considered "static", meaning their data is fixed, while overall summary tables are considered *dynamic* because their data is subject to change over time as new data are incorporated.   

+ `segment_vessel_daily_YYYYMMDD` and `segment_vessel`
+ `segment_info`
+ `segment_identity_daily_YYYYMMDD`
+ `vessel_info`

#### `vessel_id`

A key feature of the AIS data tables is `vessel_id`, which is intended as a unique ID for an identity in the AIS data and is used to link AIS positions to an identity and AIS events...[EXPAND].

### AIS research tables

Because raw AIS data tables contain all AIS positions, they are *extremely large tables* and expensive to query. However, AIS messages are often broadcast every few seconds, which is much finer temporal resolution than needed for most research and analysis work. 

Key details of the research tables include the following:
+ AIS positions in the main research table (`messages`) are thinned to one position per minute per segment
+ Every AIS position is assigned an amount of time (`hours`), which is equal to the time since the previous position in the segment
+ There are two different fishing score variables - `nnet_score` and `night_loitering`. Use `night_loitering` for  `squid_jiggers` and `nnet_score` for all other fishing classes.

### AIS event tables

Lastly, the AIS pipeline includes a series of algorithms that identify certain vessel behaviors, referred to as events:

[update links to relative links within the repo]

+ Encounters
+ Loitering
+ Port visits and voyages
+ Fishing events

## VMS pipeline

GFW also creates individual VMS pipelines to process national VMS data. The output tables of the VMS pipelines mirror the data and event tables from the AIS pipeline and are stored in datasets named `pipe_[country]_production_vYYYYMMDD`.
