This page contains the list and location of the BigQuery datasets and tables composing AIS Pipe 3. These BQ tables are for internal testing only for Q3 2023, but will replace all previous pipe_production_v20201001 tables upon completion of internal testing. 

Last update:
   * Date: `2023-06-26`
   * By: `Hannah Linder`


| Subject | Description | BQ Table| Previous BQ Table | Research Owner |
| --- | --- | --- | --- | --- |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS pipeline dataset to use | `pipe_ais_v3_alpha`| `pipe_production_v20201001` | Tim H, Jenn, Andres, Mathias |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (thinned) | `pipe_production_v3_alpha_published.messages` | `pipe_production_v20201001.research_messages` | Andres |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Satellite timing offsets | `pipe_production_v3_alpha_published.satellite_timing_offsets` | `pipe_production_v20201001.research_satellite_timing` | Tim Hochberg |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS activity aggregated at the segment level with noise indicators| `pipe_production_v3_alpha_published.segs_activity` | `pipe_production_v20201001.research_segs` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS activity aggregated at the daily segment level with noise indicators| `pipe_production_v3_alpha_published.segs_activity_daily` | `pipe_production_v20201001.research_segs_daily` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Aggregated raw AIS identity information by ssvid| `pipe_production_v3_alpha_published.ssvids_identity` | `pipe_production_v20201001.research_ids` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Daily raw AIS identity information by ssvid| `pipe_production_v3_alpha_published.ssvids_identities_daily` | `pipe_production_v20201001.research_ids_daily` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Stats on AIS data daily| `pipe_production_v3_alpha_published.stats_daily` | `pipe_production_v20201001.research_stats` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Aggregated vessel identity information by vessel id and ssvid| `pipe_production_v3_alpha_published.vessel_info` | `pipe_production_v20201001.vessel_info` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS ML features used in the fishing model estimates | `pipe_production_v3_alpha_internal.features_` | `pipe_production_v20201001.features_` | Tim Hochberg |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Fishing score estimated using features | `pipe_production_v3_alpha_internal.fishing_score_` | `pipe_production_v20201001.fishing_score_` | Tim Hochberg |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Daily segments considered 'fragments' used to stitch together segments | `pipe_production_v3_alpha_internal.fragments_` | NA | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (all messages) | `pipe_production_v3_alpha_internal.messages_positions` | `pipe_production_v20201001.position_messages_` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (all messages) merged with estimated fishing scores | `pipe_production_v3_alpha_internal.messages_scored_` | `pipe_production_v20201001.messages_scored_` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS position table (all messages) merged with segment ids | `pipe_production_v3_alpha_internal.messages_segmented_` | `pipe_production_v20201001.messages_segmented_` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | AIS daily SSVID identity information merged with associated seg id | `pipe_production_v3_alpha_internal.segment_identity_daily_` | `pipe_production_v20201001.segment_identity_daily_` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Aggregated segment and vessel id information | `pipe_production_v3_alpha_internal.segment_info` | `pipe_production_v20201001.segment_info` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Aggregated matching of segment, vessel id, and ssvid | `pipe_production_v3_alpha_internal.segment_vessel` | `pipe_production_v20201001.segment_vessel` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Daily matching of segment, vessel id, and ssvid | `pipe_production_v3_alpha_internal.segment_vessel_daily_` | `pipe_production_v20201001.segment_vessel_daily` | ? |
| [AIS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline) | Daily determination of seg id from frag ids | `pipe_production_v3_alpha_internal.segments_` | `pipe_production_v20201001.segments_` | ? |
