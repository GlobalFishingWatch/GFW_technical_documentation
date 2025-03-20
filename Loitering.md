# Loitering

> **Update** *As a result of the All Vessels v2 table, all AIS published event tables now have a new v2 version which should be used moving forward, `pipe_production_v20201001.published_events_loitering_v2`, until we switch to using pipeline 3.0, where all v2 published event tables will become the only maintained versions of the tables. The logic for the events themselves haven’t changed, but due to the change in how shiptype is determined in products, the published events tables have been re-run and ingested into products with the updated shtipetype designation. All example queries referencing published events tables should use the v2 versions of the tables.* 

Vessels meet up at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and loitering events may indicate when a vessel has met up with another vessel that is not transmitting AIS and may be of interest for monitoring illegal behavior. 

## Key Tables
 
+ `world-fishing-827.pipe_production_v20201001.published_events_loitering_v2` - Similar to loitering table, however the schema is modified to be consistent with the other `published_events_` tables AND the data is restricted to avg_distance_from_shore_nm>=20 and segments must be good_seg AND NOT overlapping_and_short , largely used in products APIs
+ `world-fishing-827.pipe_production_v20201001.loitering` - loitering table is for all vessels types with no constraints on distance from shore and duration, and has not been filtered to good seg or overlapping and short. - analysts need to add restrictions.

> **Note** 
> Always use the `published_events_x` view version of a table rather than `published_events_x_v` (for example, if you want to use `pipe_production_v20201001.published_events_loitering` then use that view table rather than `pipe_production_v20201001.published_events_loitering_v`. The _v form of a published events table only exists for internal engineering purposes. When the tables are updated daily we must calculate if any events have changed from yesterday to today and to add those into the `published_events`. The _v form of a table is created for this calculation, but is not to be used by anyone. 



## Source Tables
The tables that are used in the loitering query are:
gfw_research.pipe_v20201001


## Data Description and Definition

When two vessels are both observed moving very slowly in close proximity for an extended period of time it is considered an **encounter**. If only one vessel is observed operating in this fashion, it is considered a **loitering event**. Loitering events are important because they may indicate that the vessel is encountering another vessel that is not broadcasting AIS. For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

**Updates in 2021**

In summary, the loitering query groups data by hour by vessel. It gets the average speed of an hour block of time, and then identifies consecutive hour blocks where the average implied speed is < 2 knots. Points that fall within these periods of time are aggregated and classified as loitering events. 
A loitering pipeline was developed to create and automate a daily updated loitering dataset. 
The dataset starts in 2012 and is updated up to 3 days prior to the current day (in order to aggregate multiday loitering events).
It is no longer filtered on any of the following to allow the analyst the flexibility to choose the desired parameters:
* vessel type
* distance from shore
* good segs or overlapping and short
* minimum loitering time
It is partition on the loitering_start_timetamp field, and clustering by ssvid.


#### Definitions used in RFMO Transshipment Reports 

GFW, in partnership with The Pew Charitable Trusts, has produced annual reports covering the years of 2017-2019 on transshipment activity in the the five major tuna RFMOs. These reports compare reported activity to potential transshipment, fishing, and port activity detected on AIS. The definitions of encounter and loitering events used for these reports are as follows.

+ `Encounters`: when two vessels are within 500 meters of each other for at least 2 hours and traveling at < 2 knots, while at least 10 kilometers from a coastal anchorage ([Miller et al. 2018](https://www.frontiersin.org/articles/10.3389/fmars.2018.00240/full)). 
+ `Loitering events`: when a carrier vessel travelled at speeds of < 2 knots for at least 4 hours, while at least 20 nautical miles from shore (see Miller et al. 2018 for original methodology, however the original minimum of 8 hours has been changed to 4 hours for the purposes of this study).

## Caveats & Known Issues

+ Point nature of events (if poor ais transmission average location can be inconsistent with full location of tracks during time period)
+ Maintaining up to date identity changes of carrier and fishing vessels of interest 
+ Loitering table is for all vessels types with no constraints on distance from shore and duration. In addition, it has not been filtered on good segs or overlapping and short - analyst needs to add restrictions.
+ Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.

## Example queries

+ [loitering_carrier_basic.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_carrier_basic.sql) 
+ [loitering_carrier_tracks.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_carrier_tracks.sql) 
+ [loitering_ex_carrier_list.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_ex_carrier_list.sql) 
+ [loitering_overlap_encounters.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_overlap_encounters.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 
