This page contains the list and location of the BigQuery datasets and tables composing AIS Pipe 3. These BQ tables are for internal testing only for Q3 2023, but will replace all previous pipe_production_v20201001 tables upon completion of internal testing. 

Table Development and Ownership: 

+ Andres Arana: lead engineer
+ Matias Piano: core engineer
+ Jenn Van Osdel: Researcher lead - Identity and Review of Segmenter
+ Tim Hochberg: Researcher lead - Segmenter
+ Chris Homberg: QA Analyst
+ Hannah Linder: AIS Pipeline Manager


Data pipeline ownership falls with the pipeline engineering team, currently Andres Arana and Matias Piano. This means they operate, maintain, and develop and deprecate  the core pipeline data outputs. The naming, security, versioning control of these tables is decided upon across all engineers, Pipeline Manager, and CTO. Development of the pipeline logic and methodology was originally created outside of the pipeline team that developed Pipeline 3. In the future the pipeline methodology is developed by partnership between the Research team and Engineering team. 


Last update:
   * Date: `2023-08-21`
   * By: `Hannah Linder`


## AIS Pipe 3 -- Published
| Description | BQ Table| Previous BQ Table |
| --- | --- | --- |
| AIS pipeline dataset to use | `pipe_ais_v3_alpha`| `pipe_production_v20201001` | 
| AIS position table (thinned) | `pipe_ais_v3_alpha_published.messages` | `pipe_production_v20201001.research_messages` | 
| Satellite timing offsets | `pipe_ais_v3_alpha_published.satellite_timing_offsets` | `pipe_production_v20201001.research_satellite_timing` | 
| AIS activity aggregated at the segment level with noise indicators| `pipe_ais_v3_alpha_published.segs_activity` | `pipe_production_v20201001.research_segs` | 
| AIS activity aggregated at the daily segment level with noise indicators| `pipe_ais_v3_alpha_published.segs_activity_daily` | `pipe_production_v20201001.research_segs_daily` | 
| Aggregated raw AIS identity information by ssvid| `pipe_ais_v3_alpha_published.ssvids_identities` | `pipe_production_v20201001.research_ids` | 
| Daily raw AIS identity information by ssvid| `pipe_ais_v3_alpha_published.ssvids_identities_daily` | `pipe_production_v20201001.research_ids_daily` | 
| Stats on AIS data daily| `pipe_ais_v3_alpha_published.stats_daily` | `pipe_production_v20201001.research_stats` | 
| Aggregated vessel identity information by vessel id and ssvid| `pipe_ais_v3_alpha_published.vessel_info` | `pipe_production_v20201001.vessel_info` | 
| Encounter events| `pipe_ais_v3_alpha_published.encounters` | `pipe_production_v20201001.encounters` | 
| Encounter events used in products (additional fields compared to encounters)| `pipe_ais_v3_alpha_published.product_events_encounter_v` | `pipe_production_v20201001.published_events_encounters_v` | 
| Loitering events| `pipe_ais_v3_alpha_published.loitering` | `pipe_production_v20201001.loitering` | 
| Port visits by vessels| `pipe_ais_v3_alpha_published.port_visits` | `pipe_production_v20201001.proto_port_visits` | 
| Confidence 3 vessel voyages| `pipe_ais_v3_alpha_published.voyages_c3` | `pipe_production_v20201001.proto_voyages_c3` | 
| Confidence 4 vessel voyages| `pipe_ais_v3_alpha_published.voyages_c4` | `pipe_production_v20201001.proto_voyages_c4` | 
| Vessel database (registry) table| `pipe_ais_v3_alpha_published.identity_all_vessels_v` | `vessel_database.all_vessels_v` | 
| Vessel database (registry) table| `pipe_ais_v3_alpha_published.identity_authorization_v` | `vessel_identity_staging.identity_authorization_v` | 
| Vessel database (registry) table| `pipe_ais_v3_alpha_published.identity_owner_v` | `vessel_identity_staging.identity_owner_v` | 
| Vessel database (registry) table| `pipe_ais_v3_alpha_published.identity_core_v` | `vessel_identity_staging.identity_core_v` | 

## AIS Pipe 3 -- Internal
| Description | BQ Table| Previous BQ Table |
| --- | --- | --- |
| AIS ML features used in the fishing model estimates | `pipe_ais_v3_alpha_internal.features_` | `pipe_production_v20201001.features_` | 
| Fishing score estimated using features | `pipe_ais_v3_alpha_internal.fishing_score_` | `pipe_production_v20201001.fishing_score_` | 
| Daily segments considered 'fragments' used to stitch together segments | `pipe_ais_v3_alpha_internal.fragments_` | NA |
| AIS position table (all messages) | `pipe_ais_v3_alpha_internal.messages_positions` | `pipe_production_v20201001.position_messages_` |
| AIS position table (all messages) merged with estimated fishing scores | `pipe_ais_v3_alpha_internal.messages_scored_` | `pipe_production_v20201001.messages_scored_` |
| AIS position table (all messages) merged with segment ids | `pipe_ais_v3_alpha_internal.messages_segmented_` | `pipe_production_v20201001.messages_segmented_` |
| AIS daily SSVID identity information merged with associated seg id | `pipe_ais_v3_alpha_internal.segment_identity_daily_` | `pipe_production_v20201001.segment_identity_daily_` |
| Aggregated segment and vessel id information | `pipe_ais_v3_alpha_internal.segment_info` | `pipe_production_v20201001.segment_info` |
| Aggregated matching of segment, vessel id, and ssvid | `pipe_ais_v3_alpha_internal.segment_vessel` | `pipe_production_v20201001.segment_vessel` |
| Daily matching of segment, vessel id, and ssvid | `pipe_ais_v3_alpha_internal.segment_vessel_daily_` | `pipe_production_v20201001.segment_vessel_daily_` |
| Daily determination of seg id from frag ids | `pipe_ais_v3_alpha_internal.segments_` | `pipe_production_v20201001.segments_` |
| Intermediate internal table that's
used to later aggregate into merged final encounter events | `pipe_ais_v3_alpha_internal.raw_encounters_` | `pipe_production_v20201001.raw_encounters_` |
| Intermediate internal table that's
used to later aggregate into actual loitering events | `pipe_ais_v3_alpha_internal.raw_loitering_` | `pipe_production_v20201001.raw_loitering_` |
| Port events before developed into port visits | `pipe_ais_v3_alpha_internal.raw_port_events_` | `pipe_production_v20201001.proto_raw_port_events_` |
| Estimate of confidence 2 level voyages | `pipe_ais_v3_alpha_internal.voyages_c2` | `pipe_production_v20201001.proto_voyages_c2` |




