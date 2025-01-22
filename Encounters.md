> **Update** *As a result of the All Vessels v2 table, all AIS published event tables now have a new v2 version which should be used moving forward, `pipe_production_v20201001.published_events_encounters_v2`, until we switch to using pipeline 3.0, where all v2 published event tables will become the only maintained versions of the tables. The logic for the events themselves haven’t changed, but due to the change in how shiptype is determined in products, the published events tables have been re-run and ingested into products with the updated shtipetype designation. All example queries referencing published events tables should use the v2 versions of the tables.* 

Vessels meet at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and these encounters between vessels are of interest for monitoring illegal behavior. 

## Key Tables

+ `pipe_production_v20201001.published_events_encounters_v2` - same as the encounters table but with a schema intended for use in products and consumed by the APIs. This means that there is one row for each of the two vessels in an encounter and the two rows must be joined together to have the information associated with both vessels on the same row (see query examples). This table also includes information on vessel authorization status. 
+ `pipe_production_v20201001.encounters` - Source table for published events encounters table. This is not filtered by speed and has one row per encounter.

> **Note**   
> Always use the `published_events_x` view version of a table rather than `published_events_x_v` 
(for example, if you want to use `pipe_production_v20201001.published_events_encounters` then use that view table rather than `pipe_production_v20201001.published_events_encounters_v`. The _v form of a published events table only exists for internal engineering purposes. When the tables are updated daily we must calculate if any events have changed from yesterday to today and to add those into the `published_events`. The _v form of a table is created for this calculation, but is not to be used by anyone. 

### Source Tables
 * `world-fishing-827:pipe_production_v20201001.raw_encounters_*`
 * `world-fishing-827:pipe_static.spatial_measures_20200311`
 * `world-fishing-827:pipe_production_v20201001.segment_info`
 * `world-fishing-827:pipe_production_v20201001.position_messages_`
 * `world-fishing-827.pipe_production_v20201001.all_vessels_byyear`
 * `world-fishing-827.pipe_production_v20201001.vessel_info`
 * `world-fishing-827.vessel_identity.identity_core`
 * `world-fishing-827.vessel_identity.identity_authorization`

## Data Description

When two vessels are observed in close proximity for an extended period of time it is considered an **encounter**.  `published_events_encounters` applies the additional constraint that both vessels must be moving very slowly.
Encounters occur when:

2 vessels within 500 meters of each other
Minimum duration of 2 hours
10 kilometers from a coastal anchorage
 traveling <2 knots [`published_events_encounters` only]

 For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

#### Definitions used in RFMO Transshipment Reports 

GFW, in partnership with The Pew Charitable Trusts, has produced annual reports covering the years of 2017-2019 on transshipment activity in the the five major tuna RFMOs. These reports compare reported activity to potential transshipment, fishing, and port activity detected on AIS. The definitions of encounter and loitering events used for these reports are as follows.

+ `Encounters`: when two vessels are within 500 meters of each other for at least 2 hours and traveling at < 2 knots, while at least 10 kilometers from a coastal anchorage ([Miller et al. 2018](https://www.frontiersin.org/articles/10.3389/fmars.2018.00240/full)). 

## Caveats & Known Issues

+ GFW has never created encounters for short events on either side of the UTC day boundary - specifically those that start between 10pm and 12am or end between 12am and 2am. This is a known issue that will be fixed in the future, for more information see [PIPELINE-2137](https://globalfishingwatch.atlassian.net/browse/PIPELINE-2137) and [this Slack thread](https://globalfishingwatch.slack.com/archives/CHBNB2JAE/p1737127108538739?thread_ts=1737046100.552259&cid=CHBNB2JAE)
+ Point nature of events; if ais transmission is poor, the average location can be inconsistent with full location of tracks during time period. In addition, due to the way events are combined across days, average event parameters are approximate.
+ Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.
+ Maintaining up to date identity changes of carrier and fishing vessels of interest in encounters between carrier and fishing vessels. Always use the most up-to-date identity tables, and verify results when possible.
+ Vessels may encounter more than one vessel at a time, an issue that comes up frequently in the presence of gear, but also occasionally with multiple vessels meeting. 

## Example queries
+ [encounters_1_ssvid.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_1_ssvid.sql) - this query pulls information on both vessels in the encounter, including vessel attributes that are nested within the `event_vessels` field  
+ [encounters_2_carriers_fishing.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_2_carriers_fishing.sql) - this query identifies encounters between carrier and fishing vessels, pulling from the shiptype attribute used in Products (eg mirroring the encounters you would expect to see in Products as of June 2023 when this query was drafted).
+ [encounters_3_iccat.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_3_iccat.sql) - new example query that pulls out encounters that occurred inside ICCAT. Pulls using the ICCAT shapefile in the `pipe_regions_layers` bin. Technically you could also use the method in the below example query to identify encounters in ICCAT - both pulling region information from within the published events table, and using the regions shapefiles in the `pipe_regions_layers` should yield the same results. 
+ [encounters_3_no_take_mpa.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_3_no_take_mpa.sql) - new example query that pulls out encounters that occurred inside a no take MPA based on the region information in the Map.
+ [encounters_4_original_table.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_4_original_table.sql) - this query pull encounters with an average speed of greater than 2 knots from the original encounter table that is the base for the published event encounter table. Note, for the most part, the only encounters in the original table that are not in the `published_event_encounter` table, are those encounters with an average speed above 2 knots. 
+ [published_events_unnest_auth_info.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_events_unnest_auth_info.sql) - how to pull event authorization from published event table schema  
+ [loitering_overlap_encounters.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_overlap_encounters.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 


