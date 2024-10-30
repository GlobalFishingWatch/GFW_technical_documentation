Fishing Events is a GFW data product which groups AIS fishing positions together into events. In addition to grouping fishing positions together, the Fishing Events table also has a set of minimally restrictive rules applied to separate large fishing events and to remove fishing events considered to be likely noise. 

Fishing Events are displayed on vessel tracks in the Map. 

Note, Fishing Events and the apparent fishing effort layer in the Map are not consistent. See data caveats for more information.  

## Key Tables

+ `world-fishing-827.pipe_ais_v3_published.fishing_events` - Internal. Fishing events logic, described in `Data Description` section, applied to least restrictive list of fishing vessels (eg when potential_fishing OR on_fishing_list_sr)
+ `world-fishing-827.pipe_ais_v3_published.product_events_fishing` - Fishing events displayed in products. Fishing events logic, described in 'Data Description` applied to vessels when they are indicated as fishing vessels in GFW products (eg when prod_shiptype = "fishing")


> **Note** 
> Always use the `product_events_x` or `proto_events_x` (if still considered a prototype) view version of a table rather than `product_events_x_v` (for example, if you want to use `pipe_ais_v3_published.product_events_fishing` then use that view table rather than `pipe_ais_v3_published.product_events_fishing_v`. The _v form of a product events table only exists for internal engineering purposes. When the tables are updated daily we must calculate if any events have changed from yesterday to today and to add those into the `product_events`. The _v form of a table is created for this calculation, but is not to be used by anyone. 

## Source Tables

See the `Details` tab in BigQuery for this information.

![Fishing events BQ source tables](https://github.com/willabrooksGFW/gfw_photos/blob/main/product_events_fishing_BQ_details.png)

## Data Description

The current Fishing Events are constructed through the following steps. Step 1 applies the standard noise filter; Step 2 groups fishing positions together; Step 3 breaks events up events if AIS positions are too spatially or temporally far away to be reasonably related; Step 4 joins fishing events separated by non-fishing if the events are spatially and temporally close together; Steps 5 and 6 filter out likely false or noisy fishing events.

1. **Applies standard noise filter** e.g. `good_segment AS (SELECT seg_id FROM world-fishing-827.gfw_research.pipe_v20201001_segs WHERE good_seg AND positions > 10 AND NOT overlapping_and_short)`
2. **Groups consecutive fishing positions in the same seg_id.** Night_loitering score is used in place of neural net score for squid jigger vessels.
3. **Separates long events** defined as consecutive fishing positions separated by more than 10 km OR 2 hours apart in the same seg_id. 
4. **Joins close events** defined as fishing events separated by non-fishing positions but within 1 hour AND 2 km of another fishing event
5. **Filters out short events** defined as events shorter than 20 minutes _OR_ with 5 or fewer ais positions _OR_ with event distance of less than 0.5 km (50 meters for squid)
6. **Filters out fast moving vessel events** defined as events with average vessel speed of 10 knots per hour or greater

## Fishing events versus apparent fishing effort 

| **Category**                        | **Description**                                                                                                                                                                                                                                                                                                                      | **Logic**                                                                                                                                                                                                                                                                                                                                                                                                         | **Vessels meeting the criteria**                                                                                                                              |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| **Least Restrictive Fishing Events** | Fishing events query logic applied to vessels in PVIS table during any year where `potential_fishing OR on_fishing_list_sr`.<br><br>Least restrictive                                                                                                                                                                                       | - Applies standard noise filter: filters segments by criteria (`good_seg`, positions > 10, not overlapping or short).<br>- Groups consecutive fishing positions in `seg_id`.<br>- Splits events separated by >10 km or >2 hours.<br>- Joins events within 1 hr & 2 km of another.<br>- Removes short events (<20 min, <5 AIS positions, <0.5 km distance) and fast-moving vessels (speed > 10 knots). | `SELECT DISTINCT vessel_id, year FROM pipe_ais_v3_published.product_vessel_info_summary WHERE potential_fishing OR on_fishing_list_sr`                                                    |
| **Fishing Events Used in Products**  | Fishing events query logic applied to vessels in PVIS table during any year where `prod_shiptype = ‘fishing’`.                                                                                                                                                                                                                                           | - Same logic as above but specific to `prod_shiptype = ‘fishing’`.<br>- Applies noise filter, grouping, splitting, joining, and filtering same as the Least Restrictive events criteria.                                                                                                                                                                        | `SELECT DISTINCT vessel_id, year FROM pipe_ais_v3_published.product_vessel_info_summary WHERE prod_shiptype = 'fishing'`                                                                  |
| **Apparent Fishing Effort**          | Incorporates apparent fishing positions from gridded fishing effort layer in map view for years where vessels meet both the “Fishing Events Used in Products” criteria and noise filter requirements.                                                                                                                             | Includes all criteria and filters as specified in the **Fishing Events Used in Products** logic.                                                                                                                                                                                                                                                                           | `SELECT DISTINCT vessel_id, year FROM pipe_ais_v3_published.product_vessel_info_summary WHERE prod_shiptype = 'fishing' AND NOT offsetting AND (overlap_hours_multinames < 24 OR overlap_hours_multinames IS NULL) AND (shipname_count <= 5 OR shipname_count IS NULL) AND fishing_hours > 24 AND active_hours > 24*5 AND CAST(ssvid AS int64) NOT IN (SELECT ssvid FROM source_research_bad_mmsi, UNNEST(ssvid) as ssvid) AND (best_vessel_class != 'gear' OR best_vessel_class IS NULL)` |

[What is the difference between the apparent fishing effort layer and the fishing events in the vessel track? FAQ](https://globalfishingwatch.org/faqs/difference-between-fishing-effort-and-fishing-events/#:~:text=Therefore%2C%20GFW%20developed%20fishing%20events,fishing%20for%20a%20given%20vessel.) 

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
Last updated on **Oct 30th, 2024**

+ **[August 2024]** Pipeline 3.0 released in Products alongside DeckGL release. This marks official shift to using the `pipe_ais_v3_published.product_events_fishing` table, and retiring the pipe 2.5 fishing events table for both internal/staff and external users.
+ **[2024 - Pipeline 3.0]** In Pipeline 3.0, published_events_fishing_v2 renamed to `product_events_fishing`. A second table of fishing events with a less restrictive list of potential fishing vessels was created in pipe 3 as well. This table is named `fishing_events`. Both tables are in `pipe_ais_v3_published` bin. Both tables use the exact same logic. The only difference is the list of vessels the query is run on. 
+ **[October 2023]** The `published_events_fishing` was re-run on an expanded list of fishing vessels using information from the `all_vessels_v2` table. Now vessels that are on_fishing_list_best, on_fishing_list_inferred, on_fishing_list_known, or on_fishing_list_sr have fishing events populated. This means that to reproduce the fishing events in Products, the fishing events table will need to be filtered for vessel_ids which are prod_shiptype = fishing in the all_vessels_v2 table. 
+ **[May 2023]** The `proto_events_fishing` table which was used in Products was updated to the `published_events_fishing` table, and the `proto_events_fishing` table was removed in June 2023. The published events fishing table now includes information on vessel authorization status, consistent with how authorization status is determined in vessel encounters. 
+ **[Spring 2023]** Authorization information is now included for fishing events, following the same logic as encounters for determining authorization status. Note, authorization information is limited to 7 RFMOs (5 tuna RFMOs, NPFC, and SPRFMO).
+ **[Aug 24 2021]** In the previous version for fishing events (prior to `world-fishing-827.pipe_production_v20201001.proto_events_fishing`) fishing events were more simply defined. Events were created if consecutive AIS points were considered ‘fishing’ by the neural net, and fishing events were grouped if they were only separated in time by a short duration (e.g., 10 minutes). There were no restrictions to remove likely noise nor to separate events made of AIS positions in different seg_id's.
