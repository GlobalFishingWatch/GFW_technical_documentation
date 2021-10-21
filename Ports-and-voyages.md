
[Training Slide Deck](https://docs.google.com/presentation/d/1CNL-hUbZGkj41siUWPi4QvHgz82Ohe_G1fLHMfSjXu4/edit#slide=id.g7b6fe9f445_0_0)


GFW maintains a database of anchorages/ports and tracks when vessels enter/exit these ports. The trips in between these port visits are then grouped to track vessel voyages. 

“Port events” are the single events that occur within a port, including:  
  
* PORT ENTRY: vessel that was not in port gets within 3km of anchorage point  
* PORT STOP: begin: speed < 0.2 knots; end: speed > 0.5 knots  
* PORT GAP: AIS gap  > 4 hours; start is recorded 4 hours after the last message before the gap; end at next message after gap.  
* PORT_EXIT: vessel that was in port moves more than 4km from anchorage point  
  
“Port visits” are when more than one single port events are grouped into a ‘visit’   
  
“Voyage” is the end of one port visit until the beginning of the next port visits aka the trip of the vessel between port visits  
  
  
**Updates in 2021**

The seg_id is now used rather than vessel_id to estimate port events. By waiting to join the segments together until the visits table, we are able to remove noisy segments from visits. This is possible because the visits table is regenerated every day so we can use the most up to date noisy segment data when generating the table.  Removing the noise segments reduces the number of times when visits are skipped, ended prematurely, or inappropriately merged because of a noisy segment. Noisy segments were removed from the estimation (overlapping_and_short = FALSE)  
  
All port events were used to create port visits, and these port visits have ‘confidence’ levels based on the events within the visit. A confidence value ranging from 1-4 is now assigned to port visits.  In the past, all port visits needed to have a confidence of 4, but now we have a range of confidences.  
  
These are defined as :  
* 1 -> no stop/gap  
* 2 -> only stop / gap  
* 3 -> port entry or exit + stop/gap  
* 4 -> port entry and exit + stop/gap.  
	
An example of a port visit that has a confidence of 2 is shown in the Training Slide Deck referenced at the top of the page: In this instance the vessel turns AIS off, then it transmits only in port, it turns off again and it appears on the high seas. In the database this only shows up as a PORT_STOP with NO Entry or Exit. 
 
An example of a port visit that has a confidence of 3 is shown the Training Slide Deck referenced at the top of the page: In this instance the vessel went to port but changed IDs in port, so under this ID we only see a PORT_ENTRY and PORT_STOP.

**Key Tables**

The tables associated with ports and voyages include the following:

+ `anchorages.named_anchorages_vYYYYMMDD` - GFW database of ports/anchorages. Includes the name and location of all ports/anchorages as well as summary stats about visits to that port.
+ `gfw_research.named_anchorages` - View of most recent version of `anchorages.named_anchorages_vYYYYMMDD`
+ `pipe_production_vYYYYMMDD.proto_raw_port_events_*` - Port events by seg_id. Each port activity by a vessel (e.g. port entry, port exit) recorded in a single row. 
+ `pipe_production_vYYYYMMDD.proto_port_visits` - Port visits by vessel. Includes confidence value to filter by port visits of interest. All events in a port visit are included in the `events` array. 
+ `pipe_production_vYYYYMMDD.published_events_port_visits` - Proto_port_visits table version, limited to confidence >=2, that is used in API (different scheme than original proto_port_visits table). Does not include nested information on individual port events, but does have information on start, intermediate, and end anchorage within the port visit.
+ `pipe_production_vYYYYMMDD.proto_voyages_c2` - Voyages by vessels with a port visit confidence of >=2. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.
+ `pipe_production_vYYYYMMDD.proto_voyages_c3` - Voyages by vessels with a port visit confidence of >=3. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.
+ `pipe_production_vYYYYMMDD.proto_voyages_c4` - Voyages by vessels with a port visit confidence of 4. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.

**Original data tables still available**
+ `pipe_production_vYYYYMMDD.port_visits_` - Previously used port_visits table, which can still be used now if you do not want to shift to the 'proto_' data. This data includes port visits with a confidence of 4 only. 
+ `pipe_production_vYYYYMMDD.voyages` - Previously used voyages table, which can still be used now. Creates voyages from `pipe_production_vYYYYMMDD.port_visits_`. 


## Data Description
There are three primary forms of port activity data - events, visits, and voyages. They are described here briefly from the rawest to the most processed form.  

Note that all data points involve `anchorage_points` for visits and to convert to what more broadly would be considered "ports" additional aggregation will be needed, likely by grouping using `gfw_research.named_anchorages`, specifically the `label` and `iso3` fields.   
  
### Port Events:  
  
Port events data are stored in `pipe_production_vYYYYMMDD.proto_raw_port_events_*`. There are four different types of port events: `PORT_ENTRY`, `PORT_STOP`, `PORT_GAP`, `PORT_EXIT`. 

A `PORT_ENTRY` event occurs when a vessel gets within 3 km of an anchorage point and a `PORT_EXIT` event occurs when it exceeds 4 km. These points can be considered changes in state, between "out of port" and "in port". The use of two different limits for entry and exit, prevents a vessel from sitting at a single entry/exit boundary and repeatedly falsely entering and exiting the port.   

Nested between a `PORT_ENTRY` and a `PORT_EXIT`, a vessel may have a `PORT_STOP` or `PORT_GAP` (these events require a state = "in port"), though these events don't necessarily occur. A `PORT_STOP` occurs when the vessel is "in port" and the vessel's speed drops under 0.2 knots and `PORT_STOP` ends when the speed climbs over 0.5 (thus `PORT_STOP`s have a start and an end.  

A `GAP_EVENT` occurs when a vessel is in "in_port" and has a >4 hour AIS gap.  The start of the gap is recorded at 4 hours after the last message before the gap and the end is recorded at the time of the message that ends the gap.

As stated previously, not every `PORT_ENTRY`/`PORT_EXIT` pair will exhibit a `PORT_GAP` or a `PORT_STOP`. These events may not occur if an AIS signal is not seen in port (the AIS may be turned off such that we cannot detect a `PORT_STOP` or an AIS gap in port may be less than 4 hours).  

### Port Visits:  
  
Port visits represents a second dataset built on the **Port Events**. **Port Visits** differ in that visits are generated based on the series of port events for a given vessel. This processed dataset is valuable because the events it contains require some evidence that the vessel actually spent time within the port. Vessels can have variable AIS transmission when entering port. For example, a vessel may have a series of `PORT_ENTRY`/`PORT_EXIT` events when transiting along a coastline, but never actually visit those ports. However, a `PORT_ENTRY`/`PORT_EXIT` event may also indicate a real port visit of interest where perhaps the vessel’s speed didn’t drop enough to trigger a PORT_STOP. The use of confidence levels in  **Port Visits** allows a user to view all possible grouped port events, but also decipher what type of visits they want to eliminate from the analysis. In the past, all **Port Visits** had a confidence of 4 to eliminate possible false positives.  

### Voyages:

Voyages are a further processed dataset as they represent two port visits that bracket a transit or voyage. Voyages connect two **Port Visits** (as described above), but have no other requirements. Thus voyages can vary significantly in length as well as the time a vessel "spent" in starting or ending port. Note that you should use the voyages table that corresponds to the confidence level you restricted port visits to. For instance, if you set port visit confidence >= 2, then you should use the pipe_production_vYYYYMMDD.proto_voyages_c2 table that shows voyages where both the start and end port visit had a confidence of >=2.

## Caveats & Known Issues

### Port Events:  
* These events use `seg_id` as the vessel identifier and may need to be mapped to `SSVID` for broader use.  

### Port Visits:
* Port visits with a confidence of 1 may identify port entry/exit for vessels transiting near anchorages, without stopping, and thus are largely false positives. Therefore, it is recommended that unless you are in a very exploratory stage, you should set confidence to at least 2. 
*Generally confidence of 4 will capture most port visits (see notes below)
 looking across all port visits with confidence >= 2::
13.4% have confidence 2, 8.6% confidence 3, and 78% confidence of 4
when looking at all port visits to Abidjan in last 3 years by carrier/fishing vessels with confidence >= 2:
.02% - confidence 2
.05% confidence 3
rest confidence 4
When the percentages are higher for lower confidence when looking at the whole data, that is because a lot of the 'noisey' vessels start to overpower the results, but when you are filtering on specific fishing/carrier lists you get less 'noise'. However, those small percentages can provide key insight into voyage history
* These events use `vessel_id` as vessel identifier and may need to be mapped to `SSVID` for broader use.  

### Voyages
* These events use port visits and have similar caveats.  
* There are three tables of voyages based on a minimum confidence value for the start or end port visits of a voyage. 
* These tables were only created when there is at least a port visit confidence of 2

## Example queries

+ [proto_voyages_and_anchorages_20210730.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/proto_voyages_and_anchorages_20210730.sql) 
+ [proto_port_visits_and_events_example_20210730.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/proto_port_visits_and_events_example_20210730.sql) 
+ [proto_voyages_merge_loitering_20210730.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/proto_voyages_merge_loitering_20210730.sql) 