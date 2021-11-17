Vessels meet up at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and loitering events may indicate when a vessel has met up with another vessel that is not transmitting AIS and may be of interest for monitoring illegal behavior. 

## Key Tables

+ `world-fishing-827.pipe_production_v20201001.loitering` - loitering table is for all vessels types with no constraints on distance from shore and duration, and has not been filtered to good seg or overlapping and short. - analysts need to add restrictions. 
+ `world-fishing-827.pipe_production_v20201001.published_events_loitering` - Similar to loitering table, however the schema is modified to be consistent with the other `published_events_` tables AND the data is restricted to avg_distance_from_shore_nm>=20 and segments must be good_seg AND NOT overlapping_and_short , largely used in products APIs


## Source Tables
The tables that are used in the loitering query are:
gfw_research.pipe_v20201001


## Data Description and Definition

When two vessels are both observed moving very slowly in close proximity for an extended period of time it is considered an **encounter**. If only one vessel is observed operating in this fashion, it is considered a **loitering event**. Loitering events are important because they may indicate that the vessel is encountering another vessel that is not broadcasting AIS. For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

**Updates in 2021**

In summary, the loitering query groups data by hour by vessel. It gets the average speed of an hour block of time, and then identifies consecutive hour blocks where the average implied speed is < 2 knots. 
A loitering pipeline was developed to create and automate a daily updated loitering dataset. 
The dataset starts in 2012 and is updated up to 3 days prior to the current day (in order to aggregate multiday loitering events).
It is no longer filtered on any of the following to allow the analyst the flexibility to choose the desired parameters:
* vessel type
* distance from shore
* good segs or overlapping and short
* minimum loitering time
It is partition on the loitering_start_timetamp field, and clustering by ssvid.


#### definitions used in RFMO Transshipment Reports 

GFW, in partnership with The Pew Charitable Trusts, has produced annual reports covering the years of 2017-2019 on transshipment activity in the the five major tuna RFMOs. These reports compare reported activity to potential transshipment, fishing, and port activity detected on AIS. The definitions of encounter and loitering events used for these reports are as follows.

+ `Encounters`: when two vessels are within 500 meters of each other for at least 2 hours and traveling at < 2 knots, while at least 10 kilometers from a coastal anchorage ([Miller et al. 2018](https://www.frontiersin.org/articles/10.3389/fmars.2018.00240/full)). 
+ `Loitering events`: when a carrier vessel travelled at speeds of < 2 knots for at least 4 hours, while at least 20 nautical miles from shore (see Miller et al. 2018 for original methodology, however the original minimum of 8 hours has been changed to 4 hours for the purposes of this study).

## Caveats & Known Issues

+ Point nature of events (if poor ais transmission average location can be inconsistent with full location of tracks during time period)
+ Maintaining up to date identity changes of carrier and fishing vessels of interest 
+ Loitering table is for all vessels types with no constraints on distance from shore and duration. In addition, it has not been filtered on good segs or overlapping and short - analyst needs to add restrictions.
+ Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.

## Example queries

+ [loitering_carrier_basic.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_carrier_basic.sql) 
+ [loitering_carrier_tracks.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_carrier_tracks.sql) 
+ [loitering_ex_carrier_list_20211004.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_ex_carrier_list_20211004.sql) 
+ [loitering_overlap_encounters_r_v20210816.Rmd](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_overlap_encounters_r_v20210816.Rmd) 
+ [loitering_overlap_encounters_v20210813.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_overlap_encounters_v20210813.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 
