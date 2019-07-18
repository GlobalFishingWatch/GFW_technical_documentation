
Fishing effort is the primary GFW data product. Every AIS position receives a fishing score from the neural net (`nnet_score`) and positions where `nnet_score > 0.5` are considered fishing positions. 

Fishing effort data are available in the following locations:
+ `pipe_production_b.messages_scored_` - this table contains the raw pipeline output with `nnet_score` but does not include hours
+ `gfw_research.pipe_production_b` - this table is a copy of `pipe_production_b.messages_scored_` but includes fields important for analysis, including `hours`. Fishing hours are calculated from this table by summing `hours` for positions where `nnet_score > 0.5`.
+ `gfw_research.pipe_production_b_fishing` - this table is identical to `gfw_research.pipe_production_b` but filtered to only include likely fishing vessels. To lower cost of queries, use this table when calculating fishing effort.
+ `gfw_draft_data.fishing_effort_vYYYYMMDD` and `gfw_draft_data.fishing_effort_byvessel_vYYYYMMDD` - these tables contain aggregated fishing effort data from `gfw_research.pipe_production_b_fishing` that is gridded at 100th degree resolution by flag state and geartype, and at 10th degree resolution by MMSI, respectively. These tables are inexpensive ways to quickly map fishing effort.