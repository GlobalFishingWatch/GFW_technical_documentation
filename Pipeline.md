# GFW Pipelines

A GFW "pipeline" refers to an automated process by which GFW converts raw AIS or other data into the datasets and tables used for research and analysis. Pipelines are a multi-step process that includes ingesting and cleaning raw data, applying GFW algorithms, and saving data to BigQuery tables and/or other locations. 
 
## AIS pipeline

The AIS pipeline is GFW's primary pipeline and includes the following processes:

1. Normalize and combine raw AIS data from Orbcomm and Spire
2. Remove noise and group positions into segments (e.g. the "segmenter") 
3. Predict which positions are likely fishing positions
4. Identify various event types (e.g. transshipment, port visits, fishing, AIS gaps, etc.) 

[AIS data tables](#AIS-data-tables) and [AIS event tables](#AIS-event-tables) produced by a given version of the AIS pipeline are stored within a BigQuery dataset named `pipe_production_vYYYYMMDD`, where the `YYYYMMDD` refers to the date the pipeline was created. Additionally, [AIS research tables](#AIS-research-tables), which are designed for research and analysis, are stored in `gfw_research`, with each table including the AIS pipeline version (`YYYYMMDD`) in its name (e.g. `pipe_vYYYYMMDD`).   

### AIS data tables

AIS data tables are tables produced by the AIS pipeline that contain a version or summary of the *entire* AIS dataset. The first type of AIS data tables are those that contain a complete copy of the AIS data output during various steps in the pipeline: 

+ `position_messages_YYYYMMDD`
+ `messages_segmented_YYYYMMDD`
+ `spatial_measures_YYYYMMDD`
+ `features_YYYYMMDD`
+ `fishing_score_YYYYMMDD`
+ `messages_scored_YYYYMMDD`

For example, the `position_messages_YYYYMMDD` table is output following data normalization, and the `messages_segmented_YYYYMMDD` table is the output of the segmenter stage of the AIS pipeline. The final output is the `messages_scored_` table, which contains *all* AIS position messages and their fishing score.

The other group of AIS data tables include tables that summarize the complete AIS data by segment or vessel. These tables may be further organized into daily date sharded tables and overall tables. Daily summary tables are considered "static", meaning their data is fixed, while overall summary tables are considered *dynamic* because their data is subject to change over time as new data are incorporated.   

+ `segment_vessel_daily_YYYYMMDD` and `segment_vessel`
+ `segment_info`
+ `segment_identity_daily_YYYYMMDD`
+ `vessel_info`

#### `vessel_id`

A key feature of the AIS data tables is `vessel_id`, which is intended as a unique ID for an identity in the AIS data and is used to link AIS positions to an identity and AIS events...[EXPAND].

### AIS research tables

Because AIS data tables, specifically `messages_scored_`, contain all AIS positions, they are *extremely large tables* and expensive to query. However, AIS messages are often broadcast every few seconds, which is much finer temporal resolution than needed for most research and analysis work. **Therefore, the GFW AIS pipeline produces a series of "research tables" designed specifically to be more efficient for research/analysis**. At present, these tables are all stored in the `gfw_research` dataset and start with the prefix `pipe_vYYYYMMDD`, with `YYYYMMDD` referring to the corresponding version of the `pipe_production_vYYYYMMDD` dataset. 

Key details of the research tables include the following:
+ Data are thinned to one position per minute per segment
+ Every AIS position is assigned an amount of time (`hours`), which is equal to the time since the previous position in the segment
+ There are two research tables of AIS positions - one that includes data for all vessels (`pipe_vYYYYMMDD`) and one that only includes likely fishing vessels (`pipe_vYYYYMMDD_fishing`)

### AIS event tables

Lastly, the AIS pipeline includes a series of algorithms that identify certain vessel behaviors, referred to as events:
+ [Encounters](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters-(Soon-to-be-Updated))
+ [Loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Loitering)
+ [Port visits and voyages](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages)
+ [Fishing events](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-events)  

## VMS pipeline

GFW also creates individual VMS pipelines to process national VMS data. The output tables of the VMS pipelines mirror the data and event tables from the AIS pipeline and are stored in datasets named `pipe_[country]_production_vYYYYMMDD`. The VMS pipeline does not (yet) output VMS research tables.