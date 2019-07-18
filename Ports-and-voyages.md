## Ports and voyages

GFW maintains a database of ports and tracks when vessels enter/exit these ports. The trips in between these port visits are then grouped to track vessel voyages. The tables associated with ports and voyages include the following:

+ `gfw_research.named_anchorages_vYYYYMMDD` - GFW database of ports/anchorages. Includes the name and location of all ports/anchorages as well as summary stats about visits to that port.
+ `pipe_production_b.port_events_` - Port events by vessels. Each port activity by a vessel (e.g. port entry, port exit) recorded in a single row. 
+ `pipe_production_b.port_visits_` - Port visits by vessels. All events in a port visit are included in the `events` array.  
+ `pipe_production_b.published_events_ports` - Port events by vessels with event id.
+ `gfw_research.vessel_voyages_vYYYYMMDD` - Voyages by vessels. Includes the departure time and id for the departure port and the arrival time and id for the destination port.