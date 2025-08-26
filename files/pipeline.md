# GFW Pipelines {#pipelines}

A GFW "pipeline" refers to an automated process by which GFW converts raw AIS or other data into the datasets and tables used for research and analysis. Pipelines are a multi-step process that includes ingesting and cleaning raw data, applying GFW algorithms, and saving data to BigQuery tables and/or other locations. 

GFW maintains multiple pipelines to continually process and update our datasets and products. These include:

+ Vessel tracking data pipelines
  + AIS
  + VMS
+ Vessel identity pipeline
+ Object detection pipelines
  + S1
  + S2
  + Planet
  + VIIRs
 
## Vessel tracking pipelines

### AIS pipeline

The AIS pipeline is GFW's primary data pipeline and includes the following processes:

1. Normalize and combine raw AIS data from multiple providers
2. Remove noise and group positions into segments (e.g. the "segmenter") 
3. Predict which positions are likely fishing positions (e.g. the GFW fishing model)
4. Detect vessel events (e.g. transshipment, port visits, fishing, AIS gaps, etc.)
5. Summarize vessel identity from AIS data and link to vessel identity information from vessel registries

[AIS data tables](#AIS-data-tables) and [AIS event tables](#AIS-event-tables) produced by a given version of the AIS pipeline are stored within several BigQuery dataset - `pipe_ais_v3_published` and `pipe_ais_v3_internal`. 

AIS data tables produced by the AIS pipeline contain a version or summary of the *entire* AIS dataset. The first type of AIS data tables are those that contain individual AIS position data output during various steps in the pipeline. Because raw AIS data tables contain all AIS positions, they are *extremely large tables* and expensive to query. However, AIS messages are often broadcast every few seconds, which is much finer temporal resolution than needed for most research and analysis work. For this reason, intermediate AIS positions tables are stored in the `pipe_ais_v3_internal` dataset and only the final AIS position table (`messages`), which has been thinned to one position per minute per segment, is stored within `pipe_ais_v3_published` dataset. **This `pipe_ais_v3_internal.messages` table is the primary table for research, analysis, and products.**

The other group of AIS data tables include tables that summarize the complete AIS data by segment or vessel identity. These tables may be further organized into daily date sharded tables and overall tables. Daily summary tables are considered "static", meaning their data is fixed, while overall summary tables are considered *dynamic* because their data is subject to change over time as new data are incorporated.   

#### AIS event tables

The AIS pipeline includes a series of algorithms that identify certain vessel behaviors, referred to as events:

[update links to relative links within the repo]

+ Encounters
+ Loitering
+ Port visits and voyages
+ Fishing events
+ AIS gaps and disabling

### VMS pipeline

GFW also creates individual VMS pipelines to process national VMS data. The VMS pipelines are very similar to the AIS pipeline and output many of the same position and event tables as the AIS pipeline into separate datasets named `pipe_[country]_production_vYYYYMMDD`.

## Vessel identity pipeline

## Object detection pipelines
