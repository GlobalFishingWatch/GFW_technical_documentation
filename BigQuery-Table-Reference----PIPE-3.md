This page contains the list and location of the BigQuery datasets and tables composing AIS Pipe 3. These BQ tables are for internal testing only for Q3 2023, but will replace all previous pipe_production_v20201001 tables upon completion of internal testing. 

Last update:
   * Date: `2023-06-26`
   * By: `Hannah Linder`


| Subject | Description | BQ Table| Previous BQ Table | Owner |
| --- | --- | --- | --- | --- |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS pipeline dataset to use | `pipe_ais_v3_alpha`| `pipe_production_v20201001` | Andres |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (thinned) | `pipe_production_v3_alpha_published.messages` | `pipe_production_v20201001.research_messages` | Andres |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (all messages) | `pipe_production_v3_alpha_internal` | `messages_scored_` | 3.0 | Andres |
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
| [AIS gaps](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Gaps) | AIS gaps and suspected disabling events | `pipe_production_v20201001` | `proto_ais_gap_events` | 2.5 | Tyler |
| AIS offsetting | AIS offsetting by `seg_id` | `gfw_research_precursors` | `offsetting_year_seg_v20220609` | 2.5 | Pete |



