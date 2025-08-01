# Loitering

Vessels meet up at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and loitering events may indicate when a vessel has met up with another vessel that is not transmitting AIS and may be of interest for monitoring illegal behavior. 

## Key Tables
 
+ `pipe_ais_v3_published.product_events_loitering` - A view of `product_events_loitering_vYYYYMMDD` that always provides data from the latest version (`vYYYYMMDD`) of the table.
+ `pipe_ais_v3_published.product_events_loitering_vYYYYMMDD` - Similar to `loitering` table, however the schema is modified to be consistent with the other `product_events_` tables largely used in products APIs This table may contain multiple versions, indicated by the `_vYYYYMMDD` and should be used instead of the `product_events_loitering` view for analyses that need to be reproducible.
+ `pipe_ais_v3_published.loitering` - Source loitering table for `product_events_loitering_vYYYYMMDD` that includes all vessels types with no constraints on distance from shore and duration, and has not been filtered to good seg or overlapping and short. - analysts need to add restrictions.

> **Note: Using event views**   
> Use the `product_events_{EVENT_TYPE}` view version of rather than a specific `product_events_{EVENT_TYPE}_vYYYYMMDD` table if you want the most recent available date. However, if it's important that an analysis be reproducible, you should use a specific version of the events table (e.g. `product_events_{EVENT_TYPE}_vYYYYMMDD`) so that it's clear what data was used in the query. 

> **Note: Compatability views** 
> When event tables are updated daily, we must calculate if any events have changed from yesterday to today and to add those into the `published_events_{EVENT_TYPE}` table. The `_v` form of a table is created for this calculation. This "compatability view" is not to be used by anyone as it only exists for internal engineering purposes. Always use the `published_events_{EVENT_TYPE}` or `proto_events_{EVENT_TYPE}` (if still considered a prototype) view table rather than the `published_events_{EVENT_TYPE}_v` view. For example, if you want to look at port visits, use the `published_events_loitering` view table rather than the `published_events_loitering_v` view table.

## Source Tables

## Data Description and Definition

When two vessels are both observed moving very slowly in close proximity for an extended period of time it is considered an **encounter event**. If only one vessel is observed operating in this fashion, it is considered a **loitering event**. Loitering events are important because they may indicate that the vessel is encountering another vessel that is not broadcasting AIS. 

The loitering query groups data by hour by vessel. It gets the average speed of an hour block of time, and then identifies consecutive hour blocks where the average implied speed is < 2 knots. Points that fall within these periods of time are aggregated and classified as loitering events. 

The loitering pipeline was developed to create and automate a daily updated loitering dataset. The dataset starts in 2012 and is updated up to 3 days prior to the current day (in order to aggregate multi-day loitering events). It is no longer filtered on any of the following to allow the analyst the flexibility to choose the desired parameters:

+ Vessel type
+ Distance from shore
+ `good_seg` or `overlapping_and_short`
+ minimum loitering time (beyond the minimum 2 hour threshold)

It is partitioned on the `loitering_start_timetamp` field, and clustered by `ssvid`.

For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

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

>Needs updating. Removed links until queries are updated/removed.

+ [loitering_carrier_basic.sql]() 
+ [loitering_carrier_tracks.sql]() 
+ [loitering_ex_carrier_list.sql]() 
+ [loitering_overlap_encounters.sql]() 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 
