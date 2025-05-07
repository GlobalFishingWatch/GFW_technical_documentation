# Vessel info tables

Building on the vessel database, the vessel info tables provide summary information by MMSI that combine AIS activity (positions, hours, fishing hours, etc.), registry info (when available), and neural net outputs (vessel class, length, tonnage, etc.) 

A key purpose of the vessel info tables is to evaluate these multiple and potentially conflicting sources of information to determine the "best" values for various fields (e.g. vessel class, flag, dimensions) to be used by GFW. The vessel info tables are also used to identify active fishing vessels and those vessels that are spoofing or offsetting their position, as well as quickly summarizing fleet activity.

## Key Tables

+ `pipe_ais_v3_published.vi_ssvid_vYYYYMMDD` - This table includes the best activity and identity information available for the vessel based on its full AIS timeseries.
+ `pipe_ais_v3_published.vi_ssvid_byyear_vYYYYMMDD` - This table includes annual best activity and identity information for the vessel based only on data in each year. As a result, the information for a given year in `pipe_ais_v3_published.vi_ssvid_byyear_vYYYYMMDD` may not match that in `pipe_ais_v3_published.vi_ssvid_vYYYYMMDD` (e.g. the vessel recently re-flagged).  


## Data Description

### `vi_ssvid` and `vi_ssvid_byyear` tables

The information in the main vessel info tables (`vi_ssvid_vYYYYMMDD`; `vi_ssvid_byyear_vYYYYMMDD`) is organized into multiple `STRUCT` that summarize the MMSI's `activity`, `ais_identity`, `inferred` characteristics, `registry_info`, and `best` info to be used by GFW.

+ `activity`: Fields summarizing the amount and location of the MMSI’s AIS activity - positions, hours, fishing_hours, spoofing etc.
  + > `eez`: Helpful summary of the MMSI's activity (`hours`, `fishing_hours`) by EEZ. Useful for quick, non-spatial summaries of activity by EEZ 
  + > `offsetting`: Whether the MMSI offsetting its position
  + > `overlap_hours_multinames`: Spoofing flag. Indicates how many hours the MMSI has overlapping segments with multiple identities. MMSI with `overlap_hours_multinames > 24` in a given year are considered to be spoofed.

+ `ais_identity`: Fields summarizing the identities (ship name, callsign, IMO, flag, etc.) broadcast by that MMSI in AIS messages. Each identity type includes a `mostcommon` field providing the most-common value for that identity type.
  + > Fields prefixed with `n_` are normalized versions of the original values. 
  + > `likely_gear`: Whether the MMSI's most common shipname (`n_shipname_mostcommon`) suggests the MMSI is attached to fishing gear (e.g. `BUOY16`).

+ `inferred`: Fields summarizing vessel characteristics (length, tonnage, geartype) inferred by the neural net for the MMSI. 
  + > GFW uses a nested vessel class hierarchy and only "leaf" classes are scored by the neural net, which are recorded in `inferred.inferred_vessel_class`. However, the final inferred vessel class assigned to an MMSI is recorded in `inferred.inferred_vessel_class_ag` and corresponds to the lowest level vessel class with a cumulative neural net score > 0.5.

+ `registry_info`: The identity information associated with the MMSI sourced from vessel registries
  + > Values correspond to the `feature` `STRUCT` in the vessel database.

+ `best`: GFW’s assigned “best” vessel characteristics after considering all info from AIS, the neural
net, and vessel registries
  + > `registry_net_disagreement`: Do the neural net (`inferred.inferred_vessel_class_ag`) and vessel registries (`registry_info.best_known_vessel_class`) disagree about the vessel class of the MMSI

#### `on_fishing_list_` fields

GFW assigns MMSI to four types of fishing lists in the vessel info tables. Three lists indicate whether the MMSI is potentially a fishing vessel according to different sources. The fourth list, `on_fishing_list_best` combines the results of the previous three lists to make a final determination of whether each MMSI is likely a fishing vessel.

+ `on_fishing_list_known` - Listed as a fishing vessel on 1+ registries

+ `on_fishing_list_sr` - Consistently self reports as fishing in AIS messages >98% of messages; minimum 50 positions

+ `on_fishing_list_nn` - The neural net believes the vessel is more likely a fishing vessel than a non-fishing vessel (e.g.
combined neural net score for fishing classes `inferred.fishing_class_score` > 0.5)

+ `on_fishing_list_best` - GFW's best list of likely fishing MMSI. True in the following cases:
  + The vessel is `on_fishing_list_known` 
  + The vessel is `on_fishing_list_sr` and `on_fishing_list_nn`
  + The vessel’s highest `inferred.inferred_vessel_class_score` is for a fishing vessel class and exceeds 0.85
  + The vessel is `on_fishing_list_nn` and `on_fishing_list_known` is **not** False

## Caveats & Known Issues

### Fishing hours incorrect for squid jiggers

Currently, the vessel info table summarizes `fishing_hours` from the `pipe_ais_v3_published.segs_activity_daily` table. Fishing hours in this table are calculated solely from `nnet_score` and are therefore incorrect for `squid_jiggers`, which should be calculated using `night_loitering`. 

## Example Queries (need updating)

+ vessel_info_examples_1_v20211128.sql

+ vessel_info_examples_2_v20211128.sql

## Links

+ [Vessel info training slides](https://docs.google.com/presentation/d/1Eu3vVM2w2bhnbDYRgdNqV6fEMcC5AWX_5nnm6YRWqDw/edit?usp=sharing)

+ [Vessel class definitions](https://docs.google.com/document/d/1HCQbP_gU79CYjSL1qpSkS5q-W0Guw0W40q6rUL39GoQ/edit?usp=sharing)
