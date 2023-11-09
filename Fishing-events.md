> **Update** *As a result of the All Vessels v2 table, all AIS published event tables now have a new v2 version which should be used moving forward, `pipe_production_v20201001.published_events_fishing_v2`, until we switch to using pipeline 3.0, where all v2 published event tables will become the only maintained versions of the tables. The logic for the events themselves haven’t changed, but due to the change in how shiptype is determined in products, the published events tables have been re-run and ingested into products with the updated shtipetype designation. All example queries referencing published events tables should use the v2 versions of the tables.* 

Fishing Events is a GFW data product which groups AIS fishing positions together into events. In addition to grouping fishing positions together, the Fishing Events table also has a set of minimally restrictive rules applied to separate large fishing events and to remove fishing events considered to be likely noise. 

Fishing Events are displayed on vessel tracks in the Map. 

Note, Fishing Events and the apparent fishing effort layer in the Map are not consistent. See data caveats for more information.  

## Key Tables

Current version
+ `world-fishing-827.pipe_production_v20201001.published_events_fishing`


> **Note** 
> Always use the `published_events_x` or `proto_events_x` (if still considered a prototype) view version of a table rather than `published_events_x_v` (for example, if you want to use `pipe_production_v20201001.published_events_fishing` then use that view table rather than `pipe_production_v20201001.published_events_fishing_v`. The _v form of a published events table only exists for internal engineering purposes. When the tables are updated daily we must calculate if any events have changed from yesterday to today and to add those into the `published_events`. The _v form of a table is created for this calculation, but is not to be used by anyone. 


Previous versions 
+ `world-fishing-827.pipe_production_v20190502.published_events_fishing` - old logic

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

+ [published_fishing_events_and_pipe_comparison.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_fishing_events_and_pipe_comparison.sql) 
+ [published_fishing_events_in_polygon_w_vessel_info.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_fishing_events_in_polygon_w_vessel_info.sql) 
+ [published_fishing_events_by_position_in_polygon.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_fishing_events_by_position_in_polygon_v20230525.sql)
+ [published_events_unnest_auth_info.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_events_unnest_auth_info.sql) - This query is to help reduce barriers with pulling information out of the published event table format (which can be tricky!). While I focus on pulling information on authorization, this example query is a great reference for any time you want to extract information nested in the vessel_info or event_info fields.

## Links

+ Fishing Effort and Events training slide deck link [HERE](https://docs.google.com/presentation/d/17brGIUs1gsRMKMmaFEqi_dd_TPMapVoE9_9PQH8esrM/edit#slide=id.g101ceedbfaa_0_66_) 
+ Fishing Event Tech Call slide deck link [HERE](https://docs.google.com/presentation/d/1ndJ4aau2Ci0dqmA2xyEp7vPrwlpt8gkVNNi0aFb7csY/edit?usp=sharing) 

## Updates
Last updated on **May 25th, 2023**

+ **[May 2023]** The `proto_events_fishing` table which was used in Products was updated to the `published_events_fishing` table, and the `proto_events_fishing` table was removed in June 2023. The published events fishing table now includes information on vessel authorization status, consistent with how authorization status is determined in vessel encounters. 
+ **[Spring 2023]** Authorization information is now included for fishing events, following the same logic as encounters for determining authorization status. Note, authorization information is limited to 7 RFMOs (5 tuna RFMOs, NPFC, and SPRFMO).
+ **[Aug 24 2021]** In the previous version for fishing events (prior to `world-fishing-827.pipe_production_v20201001.proto_events_fishing`) fishing events were more simply defined. Events were created if consecutive AIS points were considered ‘fishing’ by the neural net, and fishing events were grouped if they were only separated in time by a short duration (e.g., 10 minutes). There were no restrictions to remove likely noise nor to separate events made of AIS positions in different seg_id's.
