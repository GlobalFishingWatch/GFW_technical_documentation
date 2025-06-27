# Gaps

GFW's AIS gaps dataset catalogues instances of long gaps in a vessel's AIS signal and identifies which gaps are likely due to intentional AIS disabling. 

## Key Tables

+ `pipe_ais_v3_published.product_events_ais_disabling`: Date-partitioned table of AIS intentional disabling events. This is a subset of the `product_events_ais_gaps` table that has a modified schema consistent with the other `product_events_` tables largely used in products APIs. **This is the primary table for analyses of AIS disabling.**

+ `pipe_ais_v3_published.product_events_ais_gaps`: Date-partitioned table of AIS gap events of 6+ hours in duration. This table can be used to identify gap events that are currently ongoing (`is_closed = False`) but must be filtered to select AIS disabling events. **Note: this table does not currently conform to the same schema of other `product_event` tables.**

### Secondary Tables

+ `pipe_ais_v3_internal.proto_ais_off_events`: Date-partitioned table of AIS positions that are followed by a 6+ hour gap in AIS signal 

+ `pipe_ais_v3_internal.proto_ais_on_events`: Date-partitioned table of AIS positions that are preceeded by a 6+ hour gap in AIS signal 

+ `gfw_research.sat_reception_smoothed_one_degree_v20210722`: Monthly satellite AIS reception quality (modeled) for 2017-2020. This table is used to exclude AIS gaps from areas with unreliable AIS reception.

## Data Description

AIS devices were designed to continually broadcast a vessel's position in order to serve as a collision avoidance system. Indeed, it is not uncommon to observe hundreds or thousands of AIS positions for a vessel in a single day. Thus, when a vessel has an extended gap in AIS positions it can potentially indicate illegal behavior. However, for several reasons, not all AIS signals that are broadcast are received by satellite or terrestrial AIS receivers and it is not uncommon to observe transmission gaps in a vessel’s AIS signal that are many hours long. Satellites must be overhead and terrestrial receivers require line of sight to receive AIS messages. AIS messages may overlap in time, especially in areas of high vessel density, causing signal interference and preventing messages from being received by satellites. As a result, GFW records all AIS gap events over six hours (referred to as "naive gaps") and then uses a set of filters to identify those gaps we believe to be caused by intentional AIS disabling.  

### AIS gap events ("naive gaps")

The `product_events_ais_gaps` table contains all 6+ hour gaps in the AIS data. These gap events are created by combining all events in the two internal precursor tables - `proto_ais_off_events` and `proto_ais_on_events`. The `product_events_ais_gaps` table includes all fields in each precursor table, as well as the `is_closed` field to indicate whether an AIS gap is currently ongoing. The table is date-partitioned on the `gap_start` field, allowing it to be filtered for AIS gap events that started on a specific date. Gaps in this table are considered "naive gaps" because additional filtering is required to identify gaps due to intentional disabling.

#### AIS off/on events

AIS off events are defined as an AIS position that is followed by a six hour gap and AIS on events are AIS position preceded by a six hour gap. Each day, the `proto_ais_off_events` and `proto_ais_on_events` tables are updated with new events that occur on that date. Both tables are date partitioned and should be filtered using the `_partitiontime` field.

### AIS disabling events

To identify AIS gaps that are most likely due to intentional disabling rather than technical issues, GFW developed a classification model based on the following rules, which are explained in more detail in the [Caveats and Known Issues](#Caveats-and-known-issues) section:

1. The gap event must be at least 12 hours
3. The gap must start at least 50 nautical miles from shore 
4. The gap must start in an area with a satellite reception quality greater than 10 positions per day
5. The vessel must have at least 14 satellite positions in the 12 hours prior to the gap 

The `product_events_ais_disabling` table applies these filters to restrict to AIS gaps that are likely AIS disabling events. These filters are not perfect, however, and analysts may consider slightly relaxing certain filters (e.g. distance from shore) if they want to capture potential disabling events of lower confidence.

## Caveats and Known Issues

### Reception quality for disabling events not updated past 2020

The layer of satellite reception quality used to identify potential AIS disabling events has not been updated for data past 2020. AIS satellite reception has improved significantly in recent years, and dynamic AIS data has further complicated our understanding of AIS reception in relation to AIS disabling.

### Gap events less than 50 nautical miles from shore are unreliable due to differences in satellite and terrestrial AIS

Satellite AIS reception generally decreases closer to shore as high vessel densities lead to signal interference. At the same time, >99% of GFW's terrestrial AIS messages are less than 50 nautical miles from shore - generally the [upper range of terrestrial AIS receivers](https://help.marinetraffic.com/hc/en-us/articles/203990918--What-is-the-typical-range-of-the-AIS-#:~:text=Normally%2C%20an%20AIS%2DReceiving%20station,20%20nautical%20miles%20around%20it.) - and terrestrial AIS coverage varies considerably around the world. Through a combination of these factors, AIS gaps that start within 50 nautical miles could be due to numerous technical reasons, such as: 
    
  + Transitioning from areas with terrestrial AIS coverage to poor satellite AIS reception
  + Poor satellite reception while approaching port followed by turning off AIS upon arrival. These situations are likely responsible for many of the very long AIS gaps (e.g. several months) in the data

### Gaps shorter than 12 hours are unreliable due to satellite periodicity

The number of satellites over the horizon at different places on earth varies considerably hour to hour. At all latitudes under 60, the major peak is at 12 hours (one half a day) and the standard deviation of the number of satellites overhead is very high when considering time periods under 12 hours. For this reason, **only gaps >= 12 hours can be considered intentional disabling events**. The 12 hour threshold accounts for the approximate amount of time required for the swath of an individual AIS satellite in a sun-synchronous orbit to cover the same location.

## Example Queries

+ [ais_disabling_published_events.sql](): Get all (completed) likely AIS disabling events during a certain period.

+ [ais_disabling_published_events_fishing_vessels.sql](): Get all (completed) likely AIS disabling events by fishing vessels during a certain period.

+ [ais_disabling_ongoing_events.sql](): Get all likely AIS disabling events that are currently ongoing (e.g. `is_closed = false`). This query demonstrates how to combine naive gap events with reception quality to identify likely disabling events in the way gaps in the `product_events_ais_gaps` table are added to the `product_events_ais_disabling` table.

## Links

+ [AIS Gaps Training Slides](https://docs.google.com/presentation/d/1g-iQxPrpmuMCvnVLm4z2rnzuqXQSzbrnfO5EUiWzK2Y/edit?usp=sharing)
