
AIS devices were designed for collision avoidance and continually broadcast a vessel's position every few seconds (Class A) or every 30 seconds (Class B). Indeed, it is not uncommon to observe hundreds or thousands of AIS positions for a vessel in a single day. Thus, when a vessel has an extended gap in AIS positions it can potentially indicate illegal behavior. 

## Key Tables

+ `world-fishing-827.pipe_production_v20201001.proto_ais_gap_events`: Date-partitioned table of AIS gap events of 6+ hours in duration. This is the primary table for analyses of AIS gaps. 
+ `world-fishing-827.pipe_production_v20201001.proto_ais_off_events`: Date-partitioned table of AIS positions that are followed by a 6+ hour gap in AIS signal  
+ `world-fishing-827.pipe_production_v20201001.proto_ais_on_events`: Date-partitioned table of AIS positions that are preceeded by a 6+ hour gap in AIS signal 
+ [ INSERT RECEPTION QUALITY TABLE ]

## Data Description

### AIS off/on events
AIS off events are defined as an AIS position that is followed by a six hour gap and AIS on events are AIS position preceded by a six hour gap. However, only gaps >= 12 hours should be included analyses. The 12 hour threshold accounts for the approximate amount of time required for the swath of an individual AIS satellite in a sun-synchronous orbit to cover the same location. Each day, the `proto_ais_off_events` and `proto_ais_on_events` tables are updated with new events that occur on that date. Both tables are date partitioned and should be filtered using the `_partitiontime` field.

### AIS gap events
The primary AIS gaps table is the `proto_ais_gap_events` table, which is updated daily by combining all events in the two precursor tables - `proto_ais_off_events` and `proto_ais_on_events`. The `proto_ais_gap_events` table includes all fields in each precursor table, as well as the `is_closed` field to indicate whether an AIS gap has not ended. Unlike the precursor tables, `proto_ais_gap_events` is date-partitioned on the `gap_start` field, allowing it to be filtered for AIS gap events that started on a specific date. 

### Filtering for suspected disabling events
For several reasons, not all AIS signals that are broadcast are received by satellite or terrestrial AIS receivers and it is not uncommon to observe transmission gaps in a vessel’s AIS signal that are many hours long. Satellites must be overhead and terrestrial receivers require line of sight to receive AIS messages. AIS messages may overlap in time, especially in areas of high vessel density, causing signal interference and preventing messages from being received by satellites. Therefore, GFW developed a classification model that  defines a set of filters to identify AIS transmission gaps that are most likely due to intentional disabling.  

```
INSERT EXAMPLE QUERY
```

### Key Fields

+ `ssvid`: MMSI with the AIS gap event
+ `gap_id`: Unique ID for the AIS gap event
+ `gap_distance_m`: The distance (meters) between the off and on events
+ `gap_implied_speed_knots`: The average implied speed of the vessel during the AIS gap event. Events with a high values for `gap_implied_speed_knots` could indicate the vessel was likely transiting during the event.
+ `gap_start_*`: Fields with information associated with the AIS off event
+ `gap_end_*`: Fields with information associated with the AIS on event
+ `positions_12_hours_before_*`: These fields are measures of the number of AIS messages received in the 12 hours prior to the off event. They are used in the gap classification model to identify suspected disabling events.
+ `is_closed`: Indicates whether the AIS gap event has ended or is currently ongoing

## Caveats & Known Issues

1. AIS reception
2. Distance from shore
3. 12 hour minimum