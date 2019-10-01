Fishing effort is the primary GFW data product and is a measure of the number of hours during which a vessel is fishing as determined by the GFW fishing detection algorithm. Fishing effort is calculated directly from the AIS pipeline tables (e.g. `gfw_research.pipe_vYYYYMMDD`) and is subsequently aggregated into the public gridded fishing effort datasets. 

## Key Tables

+ `pipe_production_vYYYYMMDD.messages_scored_` - this table contains the raw pipeline output with `nnet_score` but does not include hours
+ `gfw_research.pipe_vYYYYMMDD` - this table is a copy of `pipe_production_vYYYYMMDD.messages_scored_` but includes fields important for analysis, including `hours`. Fishing hours are calculated from this table by summing `hours` for positions where `nnet_score > 0.5`.
+ `gfw_research.pipe_vYYYYMMDD_fishing` - this table is identical to `gfw_research.pipe_vYYYYMMDD.` but filtered to only include likely fishing vessels. To lower cost of queries, use this table when calculating fishing effort.
+ `gfw_draft_data.fishing_effort_vYYYYMMDD` and `gfw_draft_data.fishing_effort_byvessel_vYYYYMMDD` - these tables contain aggregated fishing effort data from `gfw_research.pipe_vYYYYMMDD._fishing` that is gridded at 100th degree resolution by flag state and geartype, and at 10th degree resolution by MMSI, respectively. These tables are inexpensive ways to quickly map fishing effort.

## Data Description

When vessels are active, their AIS positions receive a fishing score from the neural net (indicated by the `nnet_score` field) and positions where `nnet_score > 0.5` are considered fishing positions. Each position is also associated with a certain number of `hours` and fishing effort (in units of fishing hours) is calculated by summing the `hours` for all fishing positions.

## Caveats & Known Issues