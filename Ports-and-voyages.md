# Ports and voyages

GFW maintains a database of anchorages/ports and tracks when vessels enter/exit these ports. The trips in between these port visits are then grouped to track vessel voyages. 

## Key Tables

The tables associated with ports and voyages include the following:

+ `gfw_research.named_anchorages_vYYYYMMDD` - GFW database of ports/anchorages. Includes the name and location of all ports/anchorages as well as summary stats about visits to that port.
+ `pipe_production_vYYYYMMDD.port_events_` - Port events by vessels. Each port activity by a vessel (e.g. port entry, port exit) recorded in a single row. 
+ `pipe_production_vYYYYMMDD.port_visits_` - Port visits by vessels. All events in a port visit are included in the `events` array.  
+ `pipe_production_vYYYYMMDD.published_events_ports` - Port events by vessels with event id.
+ `pipe_production_vYYYYMMDD.voyages` - Voyages by vessels. Includes the departure time and id for the departure anchorage and the arrival time and id for the destination anchorage.

## Data Description
There are three primary forms of port activity data - events, visits, and voyages. They are described here briefly from the rawest to the most processed form.  

Note that all data points involve `anchorage_points` and for visits to what more broadly be considered "ports" additional aggregation will be needed, likely by grouping using `named_anchorages`, specifically the `label` and `iso3` fields.   
  
### Port Events:  
  
Port events data are stored in `pipe_production_vYYYYMMDD.port_events_`. There are four different types of port events: `PORT_ENTRY`, `PORT_STOP`, `PORT_GAP`, `PORT_EXIT`. 

A `PORT_ENTRY` event occurs when a vessel gets with 3 km of an anchorage point and a `PORT_EXIT` event when it exceeds 4 km.  Speed does not enter into it and can be considered changes in state, between "out of port" and "in port". The use of two different limits for entry and exit, prevents a vessel from sitting at a single entry/exit boundary and repeatedly falsely entering and exiting the port.   

Nested between a `PORT_ENTRY` and a `PORT_EXIT`, a vessel may have a `PORT_STOP` or `PORT_GAP` (these events require a state = "in port"), though these events don't necessarily occur. A `PORT_STOP` occurs when the vessel is "in port" and the vessel's speed drops under 0.2 knots and `PORT_STOP` ends when the speed climbs over 0.5 (thus `PORT_STOP`s have a start and an end.  

A `GAP_EVENT` occurs when a vessel is in "in_port" and has a >4 hour AIS gap.  

As stated previously, not every `PORT_ENTRY`/`PORT_EXIT` pair will exhibit a `PORT_GAP` or a `PORT_STOP`. These events may not occur if an AIS signal is not seen in port (the AIS may be turned turned off such that we cannot detect a `PORT_STOP` or a AIS gap in port may be less than 4 hours).  

### Port Visits:  
  
Port visits represents a second dataset built on the **Port Events**. **Port Visits** differ in that a visit is not generated unless an entry/exit pair brackets either a `PORT_STOP` or `PORT_GAP` event (described above). This processed dataset is valuable because the events in contains require some evidence that the vessel actually spent time or stopped within the port _(it either was detected "stopping" there or it had a gap in AIS bracketed by being seen within the port)_.  
  
Because `PORT_EVENTS` few restrictions, except that the vessel be within 3km of an anchorage point a vessel may have a series of `PORT_ENTRY`/`PORT_EXIT` events when transiting along a coastline, but never actually visit those ports. The use of **Port Visits** eliminates many of these false positives.  

_Note that while potentially unlikely it is possible for a vessel to visit a port, but not meet the requirements for a **Port Visits** and not be recorded here. However, this 'port event' may be recorded in the **Port Events** table which does not require port stops or port gaps. Examining both tables may at times be useful_  

### Voyages:

Voyages are a further processed dataset as they represent two port visits that bracket a transit or voyage. Voyages connect two **Port Visits** (as described above), but have no other requirements. Thus voyages can vary significantly in length as will the time a vessel "spent" in starting or ending port. Note that this table has the same restrictions as the **Port Visits** and avoiding many false visits, but potentially missing a few events lacking port stops or port gaps. 