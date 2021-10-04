Vessels meet at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and these encounters between vessels are of interest for monitoring illegal behavior. 

## Key Tables

+ `pipe_production_v20201001.published_events_encounters` - same as the encounters table but with a schema intended for use in products and consumed by the APIs. This means that there is one row for each of the two vessels in an encounter and the two rows must be joined together to have the information associated with both vessels on the same row (see query examples). 
+ `pipe_production_v20201001.encounters` - Source table for published events encounters table. This is not filtered by speed and has one row per encounter.

### Source Tables
 * `world-fishing-827:pipe_production_v20201001.raw_encounters_*`
 * `world-fishing-827:pipe_static.spatial_measures_20200311`
 * `world-fishing-827:pipe_production_v20201001.segment_info`
 * `world-fishing-827:pipe_production_v20201001.position_messages_`

##Updates in 2021

Encounters are generated in a two stage process: first, “raw” encounters are generated daily then these encounters are joined together across multiple days. Prior to 2021 both of these steps were performed on the basis of vessel id. However, in 2021, the first step was changed to use segment id instead. This fixes some inconsistencies in the data and allows bad segments to be dropped from encounters reducing overall noise. A second change allowed vessels to encounter more than one vessel at a time, an issue that comes up frequently in the presence of gear, but also occasionally with multiple vessels meeting.

## Data Description

When two vessels are observed in close proximity for an extended period of time it is considered an **encounter**.  `published_events_encounters` applies the additional constraint that both vessels must be moving very slowly.
Encounters occur when:

2 vessels within 500 meters of each other
Minimum duration of 2 hours
10 kilometers from a coastal anchorage
 traveling <2 knots [`published_events_encounters` only]

 For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

#### definitions used in RFMO Transshipment Reports 

GFW, in partnership with The Pew Charitable Trusts, has produced annual reports covering the years of 2017-2019 on transshipment activity in the the five major tuna RFMOs. These reports compare reported activity to potential transshipment, fishing, and port activity detected on AIS. The definitions of encounter and loitering events used for these reports are as follows.

+ `Encounters`: when two vessels are within 500 meters of each other for at least 2 hours and traveling at < 2 knots, while at least 10 kilometers from a coastal anchorage ([Miller et al. 2018](https://www.frontiersin.org/articles/10.3389/fmars.2018.00240/full)). 

## Caveats & Known Issues

+ Point nature of events; if ais transmission is poor, the average location can be inconsistent with full location of tracks during time period. In addition, due to the way events are combined across days, average event parameters are approximate.
+ Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.
+ Maintaining up to date identity changes of carrier and fishing vessels of interest in encounters between carrier and fishing vessels. Always use the most up-to-date identity tables, and verify results when possible. 

## Example queries
+ [encounters_1_ssvid_v20211004.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_1_ssvid_v20211004.sql)  
+ [encounters_2_carriers_v20211004.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_2_carriers_v20211004.sql) 
+ [encounters_3_carriers_vessel_info_v20211004.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_3_carriers_vessel_info_v20211004.sql) 
+
[encounters_4_original_table_v20211004.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/enencounters_4_original_table_v20211004.sql) 
+
[loitering_overlap_encounters_r_v20210816.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_overlap_encounters_r_v20210816.sql) 
+
[loitering_overlap_encounters_v20210813.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_overlap_encounters_v20210813.sql) 
+ 
[analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 

