Fishing Events is a GFW data product which groups AIS fishing positions together into events. The Fishing Events table has a set of minimally restrictive rules applied to remove fishing positions considered to be likely noise. See data description for more details. 

The fishing events table was updated to its current logic during the 20201001 pipe update and is currently visualized in the GFW public map. 

Previously (pipeline versions prior to 20201001), fishing events were based on a simpler logic and were utilized in a limited capacity. In the old version fishing events were defined by if there were consecutive AIS points considered ‘fishing’ by the neural net, or they were only separated in time by a short duration (e.g., 10 minutes) they were grouped into a single fishing event by a vessel.

## Key Tables

Current version
+ `world-fishing-827.pipe_production_v20201001.proto_events_fishing` - new logic


Previous version 
+ `world-fishing-827.pipe_production_v20190502.published_events_fishing` - old logic
+ `world-fishing-827.pipe_production_v20200203.published_events_fishing` - old logic
+ `world-fishing-827.pipe_production_v20200805.published_events_fishing` - old logic
+ `world-fishing-827.pipe_production_v20201001.published_events_fishing` - old logic

## Source Tables

The current fishing events are calculated from the `gfw_reasearch.pipe_vYYYYMMDD_fishing` table and using the `gfw_research.vi_ssvid_vYYYMMDD` table. _You can also reference the ‘Details’ tab between ‘Schema’ and ‘Preview’ in BigQuery for list of GFW data tables used to build this database_

## Data Description

Fishing events are constructed through the following steps:

1. **After applying standard noise filter** e.g. `good_segment AS (SELECT seg_id FROM world-fishing-827.gfw_research.pipe_v20201001_segs WHERE good_seg AND positions > 10 AND NOT overlapping_and_short)`
2. **Groups consecutive fishing positions in the same seg_id.** Night_loitering score is used in place of neural net score for squid jigger vessels.
3. **Separates long events** defined as consecutive fishing positions separated by more than 10 km OR 2 hours apart in the same seg_id. 
4. **Joins close events** defined as fishing events separated by non-fishing positions but within 1 hour AND 2 km of another fishing event
5. **Filters out short and fast moving vessel events** defined as events shorter than 20 minutes _OR_ with 5 or fewer ais positions _OR_ with event distance of less than 0.5 km (50 meters for squid)
6. **Filters out fast moving vessel events** defined as events with average vessel speed of 10 knots per hour or greater


## Caveats & Known Issues

Fishing events and fishing effort (see slide deck [HERE](https://docs.google.com/presentation/d/17brGIUs1gsRMKMmaFEqi_dd_TPMapVoE9_9PQH8esrM/edit?usp=sharing********)) do not follow the same restrictions. See Fishing Event Tech Call slide deck in supplementary materials for a comparison of fishing events and pipefishing (eg fishing effort). In future pipeline versions and updates to the public map effort may be made to make fishing events and fishing effort data align.

+ Specifics: If you reference the Data Description section on fishing effort page you’ll see that to get fishing effort grids, the `hours` attribute of fishing positions (`nnet_score > 0.5`) are summed together. This approach does not use any of the fishing events logic to remove likely false fishing. Thus the fishing effort grid which underlies the vessel tracks on the GFW public map where fishing events are highlighted can include fishing position data that has been removed from fishing events. Subsequently it is possible the fishing effort cells in the public map do not completely align with fishing events. 

## Supplementary Materials & Practice Queries 

+ Fishing Event Tech Call Slide Deck link [HERE](https://docs.google.com/presentation/d/1ndJ4aau2Ci0dqmA2xyEp7vPrwlpt8gkVNNi0aFb7csY/edit?usp=sharing) 
