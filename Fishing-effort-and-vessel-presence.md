AIS-based vessel presence and apparent fishing effort are core GFW data products and represent the amount of time vessels are present in an area and how much of that time is spent fishing. Values for time (hours) and probability of fishing are assigned to individual AIS positions, and analyses of vessel presence and/or apparent fishing effort aggregate these values in various ways, such as by combining them with [vessel identity information](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields) to summarize activity by flag state and/or geartype. Finally, rasters of vessel activity, such as [Global Fishing Watch's public fishing effort data](https://globalfishingwatch.org/data-download/datasets/public-fishing-effort), can be created by aggregating values by lat/lon bins.     

## Key Tables

+ `pipe_production_vYYYYMMDD.messages_scored_` - This table contains the complete AIS pipeline output, including neural net predictions (`nnet_score`) for whether each AIS position is a fishing position. It's primary purpose is to provide vessel tracks to the GFW map and is generally not used for analysis.  
+ `gfw_research.pipe_vYYYYMMDD` - This table is a streamlined version of `pipe_production_vYYYYMMDD.messages_scored_` designed for research and analysis. 
+ `gfw_research.pipe_vYYYYMMDD_fishing` - Identical to `gfw_research.pipe_vYYYYMMDD` but filtered to only include likely fishing vessels (any `ssvid` on one of the [fishing lists in the vessel info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields)). This is the default table to use when calculating fishing effort.
+ `global-fishing-watch.gfw_public_data.fishing_effort_v2` and `global-fishing-watch.gfw_public_data.fishing_effort_byvessel_v2` - These tables contain the most current GFW public fishing effort and vessel presence data, which is gridded at 100th degree resolution by flag state and geartype and at 10th degree resolution by MMSI, respectively. These tables are inexpensive ways to quickly map fishing effort and vessel presence.

## Data Description

### AIS position tables

#### `messages_scored_`

The GFW AIS pipeline is a sequence of algorithms that normalize and combine raw AIS data from Orbcomm and Spire, remove noise and group positions into segments, and predict which positions are likely fishing positions. All tables that are produced as part of a given version of the AIS pipeline are stored within a BigQuery dataset named `pipe_production_vYYYYMMDD`, where the `YYYYMMDD` refers to the date the pipeline was created. The final output of the pipeline is the `messages_scored_` table, which contains *all* AIS position messages. 

The `messages_scored_` table is *date sharded*, which means that it is actually a collection of tables, one for each date of data. The data for a given date is stored in its own table (e.g. `messages_scored_20180101`). When looking in BigQuery, you will see the table name followed by a number in parentheses (e.g. `messages_scored_(3248)`), which indicates that there are 3,248 days of data in the `messages_scored_` table.

#### Research tables (`pipe_vYYYYMMDD` and `pipe_vYYYYMMDD_fishing`)

Because `messages_scored_` contains all AIS positions, it is an *extremely large table* and expensive to query. However, many vessels broadcast AIS messages every few seconds, which is much finer temporal resolution than what's needed for nearly all research and analysis work. Therefore, the GFW AIS pipeline has a final step that produces a series of "research tables" designed specifically to be more efficient for research/analysis. At present, these tables are all stored in the `gfw_research` dataset and start with the prefix `pipe_vYYYYMMDD`, with `YYYYMMDD` referring to the corresponding version of the `pipe_production_vYYYYMMDD` dataset. 

Key details of the `pipe_vYYYYMMDD` and `pipe_vYYYYMMDD_fishing` tables include the following:
+ Data are thinned to one position per minute per segment
+ Every AIS position is assigned an amount of time (`hours`), which is equal to the time since the previous position in the segment
+ A secondary fishing algorithm to detect fishing for squid jiggers (`night_loitering`) is applied to all positions in `pipe_vYYYYMMDD_fishing`

### Calculating `hours` and `fishing_hours`

Each AIS position is assigned an amount of time (`hours`), which is calculated as the time since the vessel's previous AIS position. Vessel presence can then calculated by simply summarizing the `hours` of all AIS positions in the analysis. AIS positions also receive two types of fishing score (0-1) - one from the GFW fishing detection algorithm (`nnet_score`), and one from a separate algorithm that detects likely fishing by squid jiggers (`night_loitering`). AIS positions where the relevant fishing score is equal to 1 are considered fishing positions and all `hours` assigned to that position are considered fishing effort (`fishing_hours`).   

#### When to use `nnet_score` vs. `night_loitering`

The GFW fishing detection algorithm performs poorly for squid jiggers, which fish at night using lights while remaining largely stationary. To account for this, a second fishing algorithm identifies when vessels exhibit this night loitering behavior. Because the vessel class associated with an MMSI can change - either over time or as a result of improved GFW vessel classification - all AIS positions receive both fishing scores so that they are available. As a result, when calculating fishing effort, it is important to first assign a vessel class to each MMSI in order to use `night_loitering` for squid jiggers and `nnet_score` for all other fishing vessels.     

## Caveats & Known Issues

### `pipe_vYYYYMMDD_fishing` outdated over time

The `pipe_vYYYYMMDD_fishing` table subsets the AIS data to only potential fishing vessels in order to make fishing-related queries considerably cheaper. In order to do so, however, the table is created from the list of all `ssvid` on one of the [fishing lists](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields) using the current vessel info table *at the time the `pipe_vYYYYMMDD_fishing` table is created*. As a result, over time as new MMSI appear in the AIS data, or our vessel classifications change for existing MMSI, not all fishing vessels may be included in the `pipe_vYYYYMMDD_fishing` table. We are currently working on improving this logic/organization in order to minimize this issue.    