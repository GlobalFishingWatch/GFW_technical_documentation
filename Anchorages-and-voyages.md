> **Update** *As a result of the All Vessels v2 table, all AIS published event tables now have a new v2 version which should be used moving forward, `pipe_production_v20201001.published_events_port_visits_v2`, until we switch to using pipeline 3.0, where all v2 published event tables will become the only maintained versions of the tables. The logic for the events themselves haven’t changed, but due to the change in how shiptype is determined in products, the published events tables have been re-run and ingested into products with the updated shtipetype designation. All example queries referencing published events tables should use the v2 versions of the tables.* 

GFW maintains a database of `anchorages` and identifies when vessels enter/exit these locations (`port visits`). By connecting a vessel's port EXIT and next port ENTRY it is also possible to identify vessel `voyages`. 

`Port events` serve as the fundamental units used to create `port visits` or `voyages`. There are four types of port events:  

`PORT ENTRY`: occurs when vessel that was not in port travels within 3km of an anchorage point  
`PORT STOP`: begin: speed < 0.2 knots; end: speed > 0.5 knots  
`PORT GAP`: AIS gap > 4 hours; start is recorded 4 hours after the last message before the gap; end at next message after gap.  
`PORT_EXIT`: occurs when vessel that was in port travels more than 4km from an anchorage point  
  <br> 

All distances are from an anchorage point. If a "port" is made up of many anchorage points the vessel "enters" the port when it is within 3 km of any anchorage point (the first one it is within 3 km of). And when the vessel leaves it simple leaves from the last anchorage point it is within 4km of. These may be different anchorage points.
  
**Updates in 2021**

The `seg_id` is now used rather than `vessel_id` to estimate port events. By waiting to join the segments together until the visits table, we are able to remove noisy segments from visits. This is possible because the visits table is regenerated every day so we can use the most up to date noisy segment data when generating the table.  Removing the noise segments reduces the number of times when visits are skipped, ended prematurely, or inappropriately merged because of a noisy segment. Noisy segments were removed from the estimation (`overlapping_and_short = FALSE`)  
  
All port events were used to create port visits, and these port visits have ‘confidence’ levels based on the events within the visit. A confidence value ranging from 1-4 is now assigned to port visits.  In the past, all port visits needed to have a confidence of 4, but now we have a range of confidences.  
  
These are defined as :  
* 1 -->  port entry AND/OR exit, but no stop OR gap  
* 2 -->  only stop AND/OR gap, but no port entry OR exit  
* 3 --> port entry OR exit + stop AND/OR gap  
* 4 --> port entry AND exit + stop AND/OR gap.  
	
An example of a port visit that has a confidence of 2 is shown in the [Training Slide Deck](https://docs.google.com/presentation/d/1CNL-hUbZGkj41siUWPi4QvHgz82Ohe_G1fLHMfSjXu4/edit#slide=id.g7b6fe9f445_0_0): In this instance the vessel turns AIS off, then it transmits only in port, it turns AIS off again and appears on the high seas. In the database this only shows up as a `PORT_STOP` with NO Entry or Exit. 
 
An example of a port visit that has a confidence of 3 is shown the [Training Slide Deck](https://docs.google.com/presentation/d/1CNL-hUbZGkj41siUWPi4QvHgz82Ohe_G1fLHMfSjXu4/edit#slide=id.g7b6fe9f445_0_0): In this instance the vessel went to port but changed IDs in port, so under this ID we only see a PORT_ENTRY and PORT_STOP.
  
 <br>
 
**Key Tables**

The tables associated with ports and voyages include the following:

+ `anchorages.named_anchorages_vYYYYMMDD` - GFW database of ports/anchorages. Includes the name and location of all ports/anchorages as well as summary stats regarding anchorage at the time of creation.
+ `gfw_research.named_anchorages` - View of most the recent version of `anchorages.named_anchorages_vYYYYMMDD`
+ `pipe_production_vYYYYMMDD.proto_raw_port_events_*` - Port events by seg_id. Each port activity by a vessel (e.g. port entry, port exit) recorded in a single row. 
+ `pipe_production_vYYYYMMDD.proto_port_visits` - Port visits by vessel. Includes confidence value to filter by port visits of interest. All events in a port visit are included in the `events` array. 
+ `pipe_production_vYYYYMMDD.published_events_port_visits` - Proto_port_visits table version, limited to confidence >=2, that is used in API (different scheme than original proto_port_visits table). Does not include nested information on individual port events, but does have information on start, intermediate, and end anchorage within the port visit.
+ `pipe_production_vYYYYMMDD.proto_voyages_c2` - Voyages by vessels with a port visit confidence of >=2. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.
+ `pipe_production_vYYYYMMDD.proto_voyages_c3` - Voyages by vessels with a port visit confidence of >=3. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.
+ `pipe_production_vYYYYMMDD.proto_voyages_c4` - Voyages by vessels with a port visit confidence of 4. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.


>**Note** 
> Always use the `published_events_x` or `proto_events_x` (if still considered a prototype) view version of a table rather than `published_events_x_v` (for example, if you want to use `pipe_production_v20201001.published_events_port_visits` then use that view table rather than `pipe_production_v20201001.published_events_port_visits_v`. The _v form of a published events table only exists for internal engineering purposes. When the tables are updated daily we must calculate if any events have changed from yesterday to today and to add those into the `published_events`. The _v form of a table is created for this calculation, but is not to be used by anyone. 


**Original data tables still available**
+ `pipe_production_vYYYYMMDD.port_visits_` - Previously used port_visits table, which can still be used now if you do not want to shift to the 'proto_' data. This data includes port visits with a confidence of 4 only. 
+ `pipe_production_vYYYYMMDD.voyages` - Previously used voyages table, which can still be used now. Creates voyages from `pipe_production_vYYYYMMDD.port_visits_`. 


## Data Description
There are three primary forms of port activity data - events, visits, and voyages. They are described here briefly from the rawest to the most processed form.  

Note that all data points involve `anchorage_points` for visits and to convert to what more broadly would be considered "ports" additional aggregation will be needed, likely by grouping using `gfw_research.named_anchorages`, specifically the `label` and `iso3` fields.   
  
### Port Events:  
  
Port events data are stored in `pipe_production_vYYYYMMDD.proto_raw_port_events_*`. There are four different types of port events: `PORT_ENTRY`, `PORT_STOP`, `PORT_GAP`, `PORT_EXIT`. 

A `PORT_ENTRY` event occurs when a vessel gets within 3 km of an anchorage point and a `PORT_EXIT` event occurs when it exceeds 4 km. These points can be considered changes in state, between "AT SEA" and "IN PORT". The use of two different limits for entry and exit, prevents a vessel from sitting at a single entry/exit boundary and repeatedly falsely entering and exiting the port.   

Nested between a `PORT_ENTRY` and a `PORT_EXIT`, a vessel may have a `PORT_STOP` or `PORT_GAP` or both (these events require a state = "in port"), though these events don't necessarily occur. A `PORT_STOP_BEGIN` occurs when the vessel is "in port" and the vessel's speed drops under 0.2 knots and `PORT_STOP_END` occurs when the speed climbs over 0.5 (thus `PORT_STOP`s may a start and an end.  

A `GAP_EVENT` occurs when a vessel is in "in_port" and has a >4 hour AIS gap.  The start (`GAP_EVENT_BEGIN`) of the gap is recorded at 4 hours after the last message before the gap and the end (`GAP_EVENT_END`) is recorded at the time of the message that ends the gap.

As stated previously, not every `PORT_ENTRY`/`PORT_EXIT` pair will exhibit a `PORT_GAP` or a `PORT_STOP`. These events may not occur if an AIS signal is not seen in port (the AIS may be turned off such that we cannot detect a `PORT_STOP` or an AIS gap in port may be less than 4 hours).  

### Port Visits:  
  
Port visits represents a second dataset built on the **Port Events**. **Port Visits** differ in that visits are generated based on the series of port events for a given vessel. This processed dataset is valuable because the events it contains require some evidence that the vessel actually spent time within the port. Vessels can have variable AIS transmission when entering port. For example, a vessel may have a series of `PORT_ENTRY`/`PORT_EXIT` events when transiting along a coastline, but never actually visit those ports. However, a `PORT_ENTRY`/`PORT_EXIT` event may also indicate a real port visit of interest where perhaps the vessel’s speed didn’t drop enough to trigger a PORT_STOP. The use of confidence levels in  **Port Visits** allows a user to view all possible grouped port events, but also decipher what type of visits they want to eliminate from the analysis. In the past, all **Port Visits** had a confidence of 4 to eliminate possible false positives.  

### Voyages:

Voyages are a further processed dataset as they represent two port visits that bracket a transit or voyage. Voyages connect two **Port Visits** (as described above), but have no other requirements. Thus voyages can vary significantly in length as well as the time a vessel spends in either the starting or ending port. Note that you should use the voyages table that corresponds to the confidence level you have restricted port visits to. For instance, if you set port visit `confidence >= 2`, then you should use the `pipe_production_vYYYYMMDD.proto_voyages_c2` table that shows voyages where both the start and end port visit had a confidence of `>=2`.

## Caveats & Known Issues

### Port Events:  
* These events use `seg_id` as the vessel identifier and may need to be mapped to `SSVID` for broader use.  

### Port Visits:
* Port visits with a confidence of 1 may identify port entry/exit for vessels transiting near anchorages, without stopping, and thus are largely false positives. Therefore, it is recommended that unless you are in a very exploratory stage, you should set confidence to at least 2. 
*Generally confidence of 4 will capture most port visits (see notes below)

	Looking at all confidence `>=2`port visits:  
	13.4% - confidence 2  
	8.6% - confidence 3   
	78.0% - confidence 4  
	
	Examining all confidence `>=2` ports visits by carrier/fishing vessels within the last 3 years to Abidjan, Côte d'Ivoire:  
	0.02% - confidence 2  
	0.05% - confidence 3  
	99.3% - confidence 4  
	
	There are more low confidence visits when looking at all data, because 'noisy' vessels dominate the results, but when filtering to a tidier vessel list there is considerably less 'noise'. However, even small percentages can provide key insight into voyage history.  

* These events use `vessel_id` as vessel identifier and may need to be mapped to `SSVID` for broader use.  

### Voyages
* These events use port visits and have similar caveats.  
* There are three tables of voyages based on a minimum confidence value for the start or end port visits of a voyage. 
* These tables require a minimum port visit confidence of 2

## Example queries

+ [proto_voyages_and_anchorages.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/proto_voyages_and_anchorages.sql) 

+ [proto_port_visits_and_events_example.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/proto_port_visits_and_events_example.sql) 

+ [proto_voyages_merge_loitering.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/proto_voyages_merge_loitering.sql) 

## Resources

[Training Slide Deck](https://docs.google.com/presentation/d/1CNL-hUbZGkj41siUWPi4QvHgz82Ohe_G1fLHMfSjXu4/edit#slide=id.g7b6fe9f445_0_0)