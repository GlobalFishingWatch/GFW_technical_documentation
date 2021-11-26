The flagship datasets of Global Fishing Watch are fishing vessel presences and apparent fishing effort based on transmissions from vessel tracking datasets. These datasets represent the amount of time vessels are present in an area and how much of that time is spent fishing. Using cloud computing, machine learning, and public vessel registry information, GFW analyzes tens of millions of AIS positions each day to map global apparent fishing effort. 

Producing such a dataset involves two key steps: 

1. Identification of fishing vessels in the AIS data

2. Detection of fishing activity

We combine our comprehensive database of known fishing vessels from vessel registries with two convolutional neural networks (CNN)—a cutting edge form of machine learning model—to help us classify fishing vessels and predict when they are fishing. The resulting output are a list of fishing vessels by gear type and a dataset of AIS positions, where each position is labeled as a fishing/non-fishing position. These datasets can then be combined to evaluate the fishing activity of individual vessels or make rasters of aggregate fishing activity.
   
## Key Tables
 
+ `gfw_research.pipe_v20201001` - AIS positions, thinned to one minute per segment, for all vessels. 
+ `pipe_v20201001_fishing` - Identical to `pipe_v20201001` but filtered to only include likely fishing vessels (any `ssvid` on one of the [fishing lists in the vessel info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields)). **This is the default table to use when calculating fishing effort.**
+ `pipe_v20201001_segs_daily` - Contains daily summaries of vessel activity and fishing by segment (`seg_id`), including by EEZ. This table can be used for non-spatial analyses that summarize vessel activity over specific time periods and/or by EEZ. 

### Public Fishing Effort Tables 

GFW's public fishing effort and vessel presence datasets (2012-2020) are gridded at 100th degree resolution by flag state and geartype and at 10th degree resolution by MMSI, respectively. These tables are available in a public BigQuery project (`global-fishing-watch`) and are inexpensive ways to quickly map fishing effort and vessel presence.

+ `global-fishing-watch.gfw_public_data.fishing_effort_v2`: Public daily fishing effort by flag state and geartype (2012-2020) at 0.01 degree resolution.
+ `global-fishing-watch.gfw_public_data.fishing_effort_byvessel_v2`: Public daily fishing effort by MMSI (2012-2020) at 0.1 degree resolution.

## Data Description

### `hours` and `fishing_hours`

Each AIS position is assigned an amount of time (`hours`), which is calculated as the time since the vessel's previous AIS position. Vessel presence is calculated by simply summing the `hours` of all AIS positions from good segments in the analysis. 

AIS positions also receive two types of fishing score (0-1) - one from the GFW fishing detection algorithm (`nnet_score`), and one from a separate algorithm that detects likely fishing by squid jiggers (`night_loitering`). AIS positions where the relevant fishing score is equal to 1 are considered fishing positions and all `hours` assigned to that position are considered fishing effort (`fishing_hours`).   

### Fishing score (`nnet_score` vs. `night_loitering`)

The GFW fishing detection algorithm performs poorly for squid jiggers, which fish at night using lights while remaining largely stationary. To account for this, a second fishing algorithm identifies when vessels exhibit this night loitering behavior. Because the vessel class associated with an MMSI can change - either over time or as a result of improved GFW vessel classification - all AIS positions receive both fishing scores so that they are available. As a result, when calculating fishing effort, it is important to first assign a vessel class to each MMSI in order to use `night_loitering` for squid jiggers and `nnet_score` for all other fishing vessels.     

### `pipe_vYYYYMMDD_segs_daily`

### GFW Public Data

## Caveats & Known Issues

### `pipe_vYYYYMMDD_fishing` outdated over time

The `pipe_vYYYYMMDD_fishing` table subsets the AIS data to only potential fishing vessels in order to make fishing-related queries considerably cheaper. In order to do so, however, the table is created from the list of all `ssvid` on one of the [fishing lists](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables#on_fishing_list_-fields) using the current vessel info table *at the time the `pipe_vYYYYMMDD_fishing` table is created*. As a result, over time as new MMSI appear in the AIS data, or our vessel classifications change for existing MMSI, not all fishing vessels may be included in the `pipe_vYYYYMMDD_fishing` table. We are currently working on improving this logic/organization in order to minimize this issue.  

## Example Queries

## Links

## Updates  