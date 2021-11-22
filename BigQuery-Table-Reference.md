This page contains the list and location of the most current BigQuery datasets and tables for the various GFW data products. The BQ tables in the below table are the default tables to be used when starting a new analysis. 

Last update:
   * Date: `2021-11-11`
   * By: `Tyler Clavelle`


| Subject | Description | BQ Dataset | BQ Table | Pipeline version | Owner |
| --- | --- | --- | --- | --- | --- |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS pipeline dataset to use | `pipe_production_v20201001` | | 2.5 | Andres |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (all messages) | `pipe_production_v20201001` | `messages_scored_` | 2.5 | Andres |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (thinned) | `gfw_research` | `pipe_v20201001` | 2.5 | Tyler |
| [Vessel identity](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-identity) | [Vessel registry database](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-database) | `vessel_database` | `all_vessels_v20211001` | 2.5 | Jaeyoon |
| [Vessel identity](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-identity) | [Vessel info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-info-tables) | `gfw_research` | `vi_ssvid_[byyear]_v20210913` | 2.5 | Tyler |
| [Vessel identity](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-identity) | Annual fishing vessel list | `gfw_research` | `fishing_vessels_ssvid_v20210913` | 2.5 | Tyler |
| [Vessel identity](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-identity) | Annual carrier vessel list | `vessel_database` | `carrier_vessels_byyear_v20211001` | 2.5 | Jaeyoon |
| [Encounters](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters) | Encounter events no speed restrictions | `pipe_production_v20201001` | `encounters` | 2.5 | Nate |
| [Encounters](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters) | Encounter events in Published format for Products | `pipe_production_v20201001` | `published_events_encounters` | 2.5 | Nate |
| [Loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Loitering) | Loitering events | `pipe_production_v20201001` | `loitering` `published_events_loitering` | 2.5 | Pete |
| [Loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Loitering) | Loitering events in Published format for Products | `pipe_production_v20201001` | `published_events_loitering` | 2.5 | Hannah |
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Anchorage locations | `anchorages` | `named_anchorages_v20211026` | 2.5 | Nate |
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Individual port events (entry, start, gap, stop, exit) | `pipe_production_v20201001` | `proto_raw_port_events_*` | 2.5 | Tim H| 
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Archived original individual port events (entry, start, gap, stop, exit) | `pipe_production_v20201001` | `archive_20201001_port_events_*` | 2.5 | Tim H | 
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Port visits (grouped port events) | `pipe_production_v20201001` | `proto_port_visits` | 2.5 | Tim H |  
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Port visits (grouped port events) in Published format for Products | `pipe_production_v20201001` | `published_events_port_visits` | 2.5 | Hannah | 
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Previous version of Port visits version (grouped port events) | `pipe_production_v20201001` | `port_visits_`| 2.5 | Tim H | 
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Vessel voyages between anchorages | `pipe_production_v20201001` | `proto_voyages_c2` `proto_voyages_c3` `proto_voyages_c4` | 2.5 | Tim H |
| [Ports](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) | Previous version of Vessel voyages between anchorages | `pipe_production_v20201001` | `voyages` | 2.5 | Tim H |
| [VIIRS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VIIRS-boat-detections) | VIIRS vessel detections | `pipe_viirs_production_v20180723` | `raw_vbd_global` |  | Masaki | 
| [VIIRS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VIIRS-boat-detections) | [VIIRS vessel detections that matched/unmatched with AIS/VMS](VIIRS-AIS-matching) | `pipe_production_v20201001` | `proto_matches_raw_vbd_global_3top_v20210514` | 2.5 | Masaki | 
| [AIS gaps](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Gaps) | AIS gaps and suspected disabling events | `pipe_production_v20201001` | `proto_ais_gap_events` | 2.5 | Tyler |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Peru VMS | `pipe_peru_production_v20200324` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Panama VMS | `pipe_panama_production_v20200331` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Chile VMS | `pipe_chile_production_v20200331` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Indonesia VMS | `pipe_indonesia_production_v20200320` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Brazil VMS | `pipe_brazil_production_v20210910` | | 2.5 | Claudino |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Mexico VMS | `pipe_mexico_oceana_production_v20210811` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Costa Rica VMS | `pipe_costarica_production_v20210830` | | 2.5 | Andres |
| [VMS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VMS) | Ecuador VMS | `pipe_ecuador_production_v20210612` | | 2.5 | Andres |




