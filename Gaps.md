
AIS devices were designed to continually broadcast a vessel's position in order to serve as a collision avoidance system. Indeed, it is not uncommon to observe hundreds or thousands of AIS positions for a vessel in a single day. Thus, when a vessel has an extended gap in AIS positions it can potentially indicate illegal behavior. GFW's AIS gaps dataset catalogues instances of long gaps in vessel's AIS signal and identifies which gaps are likely due to intentional AIS disabling. 

## Key Tables

+ `world-fishing-827.pipe_production_v20201001.proto_ais_gap_events`: Date-partitioned table of AIS gap events of 6+ hours in duration. This is the primary table for analyses of AIS gaps. 
+ `gfw_research.sat_reception_smoothed_one_degree_v20210722`: Monthly satellite AIS reception quality (modeled) for 2017-2020. This table is used to exclude AIS gaps from areas with unreliable AIS reception.

### Precursor Tables
+ `world-fishing-827.pipe_production_v20201001.proto_ais_off_events`: Date-partitioned table of AIS positions that are followed by a 6+ hour gap in AIS signal  
+ `world-fishing-827.pipe_production_v20201001.proto_ais_on_events`: Date-partitioned table of AIS positions that are preceeded by a 6+ hour gap in AIS signal 

### Key Fields

+ `ssvid`: MMSI with the AIS gap event
+ `gap_id`: Unique ID for the AIS gap event
+ `gap_distance_m`: The distance (meters) between the off and on events
+ `gap_implied_speed_knots`: The average implied speed of the vessel during the AIS gap event. Events with a high values for `gap_implied_speed_knots` could indicate the vessel was likely transiting during the event.
+ `gap_start_*`: Fields with information associated with the AIS off event
+ `gap_end_*`: Fields with information associated with the AIS on event
+ `positions_12_hours_before_*`: These fields are measures of the number of AIS messages received in the 12 hours prior to the off event. They are used in the gap classification model to identify suspected disabling events.
+ `is_closed`: Indicates whether the AIS gap event has ended or is currently ongoing

## Data Description

### AIS gap events

The primary AIS gaps table is the `proto_ais_gap_events` table, which is updated daily by combining all events in the two precursor tables - `proto_ais_off_events` and `proto_ais_on_events`. The `proto_ais_gap_events` table includes all fields in each precursor table, as well as the `is_closed` field to indicate whether an AIS gap has not ended. Unlike the precursor tables, `proto_ais_gap_events` is date-partitioned on the `gap_start` field, allowing it to be filtered for AIS gap events that started on a specific date. 

#### AIS off/on events

AIS off events are defined as an AIS position that is followed by a six hour gap and AIS on events are AIS position preceded by a six hour gap. However, only gaps >= 12 hours should be included analyses. The 12 hour threshold accounts for the approximate amount of time required for the swath of an individual AIS satellite in a sun-synchronous orbit to cover the same location. Each day, the `proto_ais_off_events` and `proto_ais_on_events` tables are updated with new events that occur on that date. Both tables are date partitioned and should be filtered using the `_partitiontime` field.

### Suspected disabling events

For several reasons, not all AIS signals that are broadcast are received by satellite or terrestrial AIS receivers and it is not uncommon to observe transmission gaps in a vessel’s AIS signal that are many hours long. Satellites must be overhead and terrestrial receivers require line of sight to receive AIS messages. AIS messages may overlap in time, especially in areas of high vessel density, causing signal interference and preventing messages from being received by satellites. 

To identify AIS transmission gaps that are most likely due to intentional disabling rather than technical issues, GFW developed a classification model based on the following rules, which are explained in more detail in the [Caveats and Known Issues](#Caveats-and-known-issues) section:

1. `gap_hours >= 12`: The gap event must be at least 12 hours.
2. `gap_start_distance_from_shore_m > 50 AND gap_start_distance_from_shore_m > 50`: The gap must start and end at least 50 nautical miles from shore. 
3. The vessel must have averaged at least 20 satellite positions per 12 hours during the 48 hours prior to the gap event. This `gap_score` is not included in the table and must be calculated from the `positions_12_hours_before_sat` field. This is because the metric is still in development and subject to change.

```
positions_12_hours_before_sat * if(gap_hours > 12*4, 12*4, gap_hours)/12 as gap_score
```
Combining these filters to select suspected disabling events:

```
    SELECT 
    *
    FROM `world-fishing-827.pipe_production_v20201001.proto_ais_gap_events`
    WHERE gap_hours >= 12 
    AND is_closed
    AND gap_start_distance_from_shore_m > 50 * 1852
    AND gap_end_distance_from_shore_m > 50 * 1852
    AND positions_12_hours_before_sat * if(gap_hours > 12*4, 12*4, gap_hours)/12 >= 20
```

## Caveats and Known Issues

### Disabling events less than 50 nautical miles from shore are unreliable

Satellite AIS reception generally decreases closer to shore as high vessel densities lead to signal interference. At the same time, >99% of GFW's terrestrial AIS messages are less than 50 nautical miles from shore - generally the [upper range of terrestrial AIS receivers](https://help.marinetraffic.com/hc/en-us/articles/203990918--What-is-the-typical-range-of-the-AIS-#:~:text=Normally%2C%20an%20AIS%2DReceiving%20station,20%20nautical%20miles%20around%20it.) - and terrestrial AIS coverage varies considerably around the world. Through a combination of these factors, AIS gaps that start within 50 nautical miles could be due to numerous technical reasons, such as: 
    
  + Transitioning from areas with terrestrial AIS coverage to poor satellite AIS reception
  + Poor satellite reception while approaching port followed by turning off AIS upon arrival. These situations are likely responsible for many of the very long AIS gaps (e.g. several months) in the data

### The `gap_score` used in production differs from the peer-reviewed method

Our paper on AIS gaps identifies suspected disabling events based on a metric of how many AIS positions a vessel had in the _t_ hours prior to the AIS gap event, where _t_ is equal to the duration of the gap event. In production, however, we want to determine in near real-time if an AIS gap event is due to suspected disabling, even if the AIS gap event has not yet ended (e.g. `is_closed = FALSE`). As a result, we developed a modified metric 

### Satellite coverage varies considerably for periods less than 12 hours

The number of satellites over the horizon at different places on earth varies considerably hour to hour. At all latitudes under 60, the major peak is at 12 hours (on half a day). Near the poles (85 latitude), the periodicity peaks at about 150 minutes, which is roughly the time for a satellite to orbit the earth once, and most of these satellites are in polar orbits and thus pass over the poles. Taking a standard deviation of these time series with different windows shows that the standard deviation of the number of satellites overhead is very high when considering time periods under 12 hours. For this reason, *AIS gaps less than 12 hours should not be considered suspected disabling events*

## Example Queries

+ [ais_disabling_events_v20211119.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/ais_disabling_events_v20211119.sql): Get all suspected AIS disabling events.

+ [ais_disabling_events_reception_v20211119.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/ais_disabling_events_reception_v20211119.sql): Get all suspected AIS disabling events, filtering out events from low reception areas.

## Links

+ [AIS Gaps Training Slides](https://docs.google.com/presentation/d/1g-iQxPrpmuMCvnVLm4z2rnzuqXQSzbrnfO5EUiWzK2Y/edit?usp=sharing)

## Updates