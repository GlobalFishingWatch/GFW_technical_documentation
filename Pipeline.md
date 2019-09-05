## Overview

The GFW "pipeline" refers to the automated processes by which GFW converts raw AIS and other data into the datasets and tables used for research and analysis. Pipelines are a multi-step process that includes ingesting and cleaning raw data, applying GFW algorithms, and saving data to BigQuery tables. Each primary raw dataset (AIS, VMS, VIIRS, etc.) receives its own pipeline and because tables output by a pipeline are automated, only tables that are highly stable (modified infrequently) are included. 

## Pipeline stages

[INSERT HIGH LEVEL DESCRIPTION OF THE PIPELINE PROCESS]

The GFW pipeline has the following process: `raw orbcomm/spire/vms` --segmenter--> `messages_segmented` --models--> `messages_scored`

## AIS pipeline

The primary GFW pipeline is the AIS pipeline, which processes raw AIS data from ORBCOMM and Spire and produces the output tables in the `pipe_production_vYYYYMMDD` dataset. The `vYYYYMMDD` indicates the version of the pipeline and is named for the creation date. 

### AIS pipeline tables

+ `position_messages_YYYYMMDD`
+ `messages_segmented_YYYYMMDD`
+ `features_YYYYMMDD`
+ `fishing_score_YYYYMMDD`
+ `messages_scored_YYYYMMDD`
+ `segment_vessel`
+ `segment_vessel_daily_YYYYMMDD`
+ `segment_info`
+ `segment_identity_daily_YYYYMMDD`
+ `port_events_YYYYMMDD`
+ `port_visits_YYYYMMDD`
+ `published_events_ports`
+ `published_events_fishing`
+ `published_events_gaps`
+ `published_events_encounters`
+ `raw_encounters_YYYYMMDD`
+ `raw_encounters_neighbors_YYYYMMDD`
+ `spatial_measures_YYYYMMDD`
+ `vessel_info`
+ `voyages`

## VMS pipeline

GFW also creates individual VMS pipelines to process national VMS data. The output tables of these pipelines are very similar to the AIS pipeline and are stored in datasets named `pipe_[country]_production_vYYYYMMDD`. 