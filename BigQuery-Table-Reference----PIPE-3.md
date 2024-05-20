This page contains the published tables of AIS Pipeline 3 `pipe_ais_v3_published.` and their previous names in the AIS Pipeline 2.5 `pipe_production_v20201001.` If you do NOT see a table listed below it has either been archived or placed in the `internal` bucket within BQ. This page does NOT contain the description of the internal tables of AIS Pipeline 3 `pipe_ais_v3_internal.` Please contact the engineers and pipeline manager if you request to use an internal table for your work. 

Table Development: 

+ Andres Arana: lead engineer
+ Matias Piano: core engineer
+ Jenn Van Osdel: Researcher lead - Identity and Review of Segmenter
+ Tim Hochberg: Researcher lead - Segmenter
+ Chris Homberg: QA Analyst
+ Hannah Linder: AIS Pipeline Manager


Data pipeline ownership falls with the pipeline engineering team, currently Andres Arana and Matias Piano. This means they operate, maintain, develop and deprecate the core pipeline data outputs. The naming, security, versioning control of these tables is decided upon across all engineers, Pipeline Manager, and CTO. Development of the pipeline logic and methodology was originally created outside of the pipeline team that developed Pipeline 3. In the future the pipeline methodology is developed by partnership between the Research team and Engineering team. 


Last update:
   * Date: `2024-04-18`
   * By: `Hannah Linder`


## AIS Pipe 3 -- Published
| Description | BQ Table| Previous BQ Table |
| --- | --- | --- |
| AIS pipeline dataset to use | `pipe_ais_v3`| `pipe_production_v20201001` | 
| Summary of AIS information on a segment level, includes ssvid and vessel id | `pipe_ais_v3_published.segment_info` | `pipe_production_v20201001.segment_info` | 
| Aggregated vessel identity information by vessel id and ssvid| `pipe_ais_v3_published.vessel_info` | `pipe_production_v20201001.vessel_info` | 
| Encounter events| `pipe_ais_v3_published.encounters` | `pipe_production_v20201001.encounters` | 
| Encounter events used in products (adjustment to schema and additional fields compared to encounters)| `pipe_ais_v3_published.product_events_encounter_v` | `pipe_production_v20201001.published_events_encounters_v` | 
| Loitering events| `pipe_ais_v3_published.loitering` | `pipe_production_v20201001.loitering` | 
| Loitering events used in products (adjustment to schema and additional fields compared to encounters)| `pipe_ais_v3_published.product_events_loitering_v` | `pipe_production_v20201001.published_events_loitering_v` | 
| Port visits by vessels| `pipe_ais_v3_published.port_visits` | `pipe_production_v20201001.proto_port_visits` | 
| Port visits by vessels used in products (adjustment to schema and additional fields compared to port visits| `pipe_ais_v3_published.product_events_port_visit_v` | `pipe_production_v20201001.published_events_port_visits_v` | 
| Confidence 3 vessel voyages| `pipe_ais_v3_published.voyages_c3` | `pipe_production_v20201001.proto_voyages_c3` | 
| Confidence 4 vessel voyages| `pipe_ais_v3_published.voyages_c4` | `pipe_production_v20201001.proto_voyages_c4` |  
| Vessel database (registry) table aggregated authorization records| `pipe_ais_v3_published.identity_authorization_v` | `vessel_identity.identity_authorization_v` | 
| Vessel database (registry) table aggregated owner records| `pipe_ais_v3_published.identity_owner_v` | `vessel_identity.identity_owner_v` | 
| Vessel database (registry) table core identity matched| `pipe_ais_v3_published.identity_core_v` | `vessel_identity.identity_core_v` | 
| Merged identity and class summary table used in products| `pipe_ais_v3_published.product_vessel_info_summary` | `pipe_production_v20201001.all_vessels_byyear_v2_v` | 
| Vessel identity match between pipe vessel info and registry table| `pipe_ais_v3_published.product_vessel_info_match` | `pipe_production_v20201001.base_vessel_identity_info_match_v` | 
| Aggregated vessel classification and activity summaries across all time by ssvid| `pipe_ais_v3_published.vi_ssvid_v` | `pipe_production_v20201001.vi_ssvid_v` |
| Aggregated vessel classification and activity summaries (difference from vi_ssvid_v is AIS based summaries by year) by ssvid| `pipe_ais_v3_published.vi_ssvid_byyear_v` | `pipe_production_v20201001.vi_ssvid_byyear_v` |
| Prototype gaps| `pipe_ais_v3_published.product_events_gaps_proto` | `pipe_production_v20201001.proto_published_events_ais_gaps` |
| Satellite timing offsets | `pipe_ais_v3_published.satellite_timing_offsets` | `pipe_production_v20201001.research_satellite_timing` | 
| AIS activity aggregated at the segment level with noise indicators| `pipe_ais_v3_published.segs_activity` | `pipe_production_v20201001.research_segs` | 
| AIS activity aggregated at the daily segment level with noise indicators| `pipe_ais_v3_published.segs_activity_daily` | `pipe_production_v20201001.research_segs_daily` | 
| Aggregated raw AIS identity information by ssvid| `pipe_ais_v3_published.ssvids_identities` | `pipe_production_v20201001.research_ids` | 
| Daily raw AIS identity information by ssvid| `pipe_ais_v3_published.ssvids_identities_daily` | `pipe_production_v20201001.research_ids_daily` | 
| Stats on AIS data daily| `pipe_ais_v3_published.stats_daily` | `pipe_production_v20201001.research_stats` | 

