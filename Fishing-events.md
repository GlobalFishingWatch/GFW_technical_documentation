Fishing Events is a GFW data product which groups AIS fishing positions together into events. In addition to grouping fishing positions together, the Fishing Events table also has a set of minimally restrictive rules applied to separate large fishing events and to remove fishing events considered to be likely noise. 

The current fishing events table was implemented in the 20201001 pipe as a proto table (`world-fishing-827.pipe_production_v20201001.proto_events_fishing`). The proto_events_fishing is currently visualized in the GFW public map.

## Key Tables

Current version
+ `world-fishing-827.pipe_production_v20201001.proto_events_fishing` - new logic


Previous versions 
+ `world-fishing-827.pipe_production_v20190502.published_events_fishing` - old logic
+ `world-fishing-827.pipe_production_v20201001.published_events_fishing` - old logic

## Source Tables

The current fishing events are calculated from the `gfw_reasearch.pipe_vYYYYMMDD_fishing` table and using the `gfw_research.vi_ssvid_vYYYMMDD` table. _You can also reference the ‘Details’ tab between ‘Schema’ and ‘Preview’ in BigQuery for list of GFW data tables used to build this database_

## Data Description

The current Fishing Events are constructed through the following steps. Step 1 applies the standard noise filter; Step 2 groups fishing positions together; Step 3 breaks events up events if AIS positions are too spatially or temporally far away to be reasonably related; Step 4 joins fishing events separated by non-fishing if the events are spatially and temporally close together; Steps 5 and 6 filter out likely false or noisy fishing events.

1. **Applies standard noise filter** e.g. `good_segment AS (SELECT seg_id FROM world-fishing-827.gfw_research.pipe_v20201001_segs WHERE good_seg AND positions > 10 AND NOT overlapping_and_short)`
2. **Groups consecutive fishing positions in the same seg_id.** Night_loitering score is used in place of neural net score for squid jigger vessels.
3. **Separates long events** defined as consecutive fishing positions separated by more than 10 km OR 2 hours apart in the same seg_id. 
4. **Joins close events** defined as fishing events separated by non-fishing positions but within 1 hour AND 2 km of another fishing event
5. **Filters out short events** defined as events shorter than 20 minutes _OR_ with 5 or fewer ais positions _OR_ with event distance of less than 0.5 km (50 meters for squid)
6. **Filters out fast moving vessel events** defined as events with average vessel speed of 10 knots per hour or greater


## Caveats and Known Issues

Point nature of events: 
+ if there is poor AIS transmission, average location of fishing event can be inconsistent with full location of tracks during time period.
+ currently, average location may be a limiting geographic attribute, as long events may cross various EEZ's or user boundaries. Since there is only one average position per event, there is not an easy way to only keep the portion of a fishing event the user is interested in if not the entire event. In the future a practice query or second data table may be produced to address this caveat. 


On the GFW Map, fishing events and fishing effort (see slide deck [HERE](https://docs.google.com/presentation/d/17brGIUs1gsRMKMmaFEqi_dd_TPMapVoE9_9PQH8esrM/edit?usp=sharing********)) do not follow the same restrictions. Fishing effort is not as restrictive and does not have filters to remove false/noisy fishing positions. Given Fishing Events and Fishing Efforts both appear in the GFW Map, this may lead to discrepancies on the public map on occasion (eg Fishing effort cells appearing on GFW map but no fishing events). See Fishing Event Tech Call slide deck in supplementary materials for a comparison of fishing events and pipefishing (eg fishing effort). In future pipeline versions and updates to the public map effort may be made to make fishing events and fishing effort data align.

## Example Queries

+ [proto_fishing_events_and_pipe_comparison_20211005.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/proto_fishing_events_and_pipe_comparison_20211005.sql) 
+ [proto_fishing_events_in_polygon_w_vessel_info_20211005.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/proto_fishing_events_in_polygon_w_vessel_info_20211005.sql) 

## Links

+ Fishing Effort and Events training slide deck link [HERE](https://docs.google.com/presentation/d/17brGIUs1gsRMKMmaFEqi_dd_TPMapVoE9_9PQH8esrM/edit#slide=id.g101ceedbfaa_0_66_) 
+ Fishing Event Tech Call slide deck link [HERE](https://docs.google.com/presentation/d/1ndJ4aau2Ci0dqmA2xyEp7vPrwlpt8gkVNNi0aFb7csY/edit?usp=sharing) 

## Updates
Last updated on **August 24th, 2021**

+ **[Aug 24 2021]** In the previous version for fishing events (prior to `world-fishing-827.pipe_production_v20201001.proto_events_fishing`) fishing events were more simply defined. Events were created if consecutive AIS points were considered ‘fishing’ by the neural net, and fishing events were grouped if they were only separated in time by a short duration (e.g., 10 minutes). There were no restrictions to remove likely noise nor to separate events made of AIS positions in different seg_id's.
