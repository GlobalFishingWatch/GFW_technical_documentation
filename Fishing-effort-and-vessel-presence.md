AIS-based vessel presence and apparent fishing effort are core GFW data products and represent the amount of time vessels are present in an area and how much of that time is spent fishing. They are produced using the [AIS research tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline#ais-research-tables), which are stored in the `gfw_research` dataset and include values for time (hours) and probability of fishing assigned to individual AIS positions. Analyses of vessel presence and apparent fishing effort aggregate these values in various ways, such as by combining them with [vessel identity information](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields) to summarize activity by flag state and/or geartype. Finally, rasters of vessel activity, such as [Global Fishing Watch's public fishing effort data](https://globalfishingwatch.org/data-download/datasets/public-fishing-effort), can be created by aggregating values by lat/lon bins.     

## Key Tables
 
+ `pipe_vYYYYMMDD` - AIS positions, thinned to one minute per segment, for all vessels. 
+ `pipe_vYYYYMMDD_fishing` - Identical to `pipe_vYYYYMMDD` but filtered to only include likely fishing vessels (any `ssvid` on one of the [fishing lists in the vessel info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields)). **This is the default table to use when calculating fishing effort.**
+ `pipe_vYYYYMMDD_segs_daily` - Contains daily summaries of vessel activity by segment, including fishing. Activity is also summarized by EEZ. This table can therefore be used for non-spatial analyses that summarize vessel activity over specific time periods.  
+ `global-fishing-watch.gfw_public_data.fishing_effort_v2` and `global-fishing-watch.gfw_public_data.fishing_effort_byvessel_v2` - These tables contain the most current GFW public fishing effort and vessel presence data, which is gridded at 100th degree resolution by flag state and geartype and at 10th degree resolution by MMSI, respectively. These tables are inexpensive ways to quickly map fishing effort and vessel presence.

## Data Description

### `pipe_vYYYYMMDD` and `pipe_vYYYYMMDD_fishing`

#### `hours` and `fishing_hours`

Each AIS position in `pipe_vYYYYMMDD` and `pipe_vYYYYMMDD_fishing` is assigned an amount of time (`hours`), which is calculated as the time since the vessel's previous AIS position. Vessel presence is calculated by simply summing the `hours` of all AIS positions from good segments in the analysis. AIS positions also receive two types of fishing score (0-1) - one from the GFW fishing detection algorithm (`nnet_score`), and one from a separate algorithm that detects likely fishing by squid jiggers (`night_loitering`). AIS positions where the relevant fishing score is equal to 1 are considered fishing positions and all `hours` assigned to that position are considered fishing effort (`fishing_hours`).   

##### When to use `nnet_score` vs. `night_loitering`

The GFW fishing detection algorithm performs poorly for squid jiggers, which fish at night using lights while remaining largely stationary. To account for this, a second fishing algorithm identifies when vessels exhibit this night loitering behavior. Because the vessel class associated with an MMSI can change - either over time or as a result of improved GFW vessel classification - all AIS positions receive both fishing scores so that they are available. As a result, when calculating fishing effort, it is important to first assign a vessel class to each MMSI in order to use `night_loitering` for squid jiggers and `nnet_score` for all other fishing vessels.     

#### Gridded vessel presence and fishing effort 

### `pipe_vYYYYMMDD_segs_daily`

### GFW Public Data

## Caveats & Known Issues

### `pipe_vYYYYMMDD_fishing` outdated over time

The `pipe_vYYYYMMDD_fishing` table subsets the AIS data to only potential fishing vessels in order to make fishing-related queries considerably cheaper. In order to do so, however, the table is created from the list of all `ssvid` on one of the [fishing lists](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields) using the current vessel info table *at the time the `pipe_vYYYYMMDD_fishing` table is created*. As a result, over time as new MMSI appear in the AIS data, or our vessel classifications change for existing MMSI, not all fishing vessels may be included in the `pipe_vYYYYMMDD_fishing` table. We are currently working on improving this logic/organization in order to minimize this issue.    