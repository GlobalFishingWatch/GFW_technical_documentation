Synthetic Aperture Radar (SAR) object detection consists of vessels and offshore infrastructure detected, classified, and matched to AIS/VMS (when available) from images taken by ESA's Sentinel-1 radar mission. 

## Key Tables

- `table_name_to_be_determined_vessels_`
- `table_name_to_be_determened_infrastructure_`

## Key Fields

- `scene_id` - scene ID from where detection was extracted
- `detect_id` - unique ID for the detection as `scene_id;lon;lat`
- `detect_lon` - longitude coordinate
- `detect_lat` - latitude coordinate
- `detect_timestamp` - time the image was taken
- `ssvid` - vessel identifier if the detection matches to AIS/VMS
- `match_score` - score cutoff used to match the detection
- `match_confidence` - confidence cutoff used to match the detection
- `inferred_length_m` - length estimated with a Neural Network
- `inferred_presence` - probability of vessel vs. noise from a Neural Network
- `repeat_id` - if repeated object, link to offshore infrastructure table
- `uncertainty` - metric of overall confidence for detection

## Data Description

Detections are from ~2014-10-01 - present, updated intermittently. Imagery availability increases along data acquisition timeframe, especially from mid-2016 on. Various types of SAR imagery captured with differing resolution, beam mode, orbit pass, polarizations, etc. Majority are scene filenames starting with `S1A_IW_GRDH_1SDV` or `S1B_IW_GRDH_1SDV` (naming conventions detailed [here](https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/naming-conventions)) which are stored in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`. The remaining detections are stored in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190703`.

(OUTDATED: DAVID TO UPDATE) Each dataset has 3 tables:
1. `objs`: use `lon` and `lat` for detection location. `start_time` for timestamp.
2. `exts`: full scene footprint including ocean and land pixels.
3. `masked_exts`: scene footprint including only ocean pixels.

## Caveats & Known Issues:
- use `id` field to join detections from `objs` with associated footprints from `exts` or `masked_exts`
- currently higher confidence in detections from `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`
- only 1 full run on `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190703` so far. Appears noisier. Improvements TBD.
- sea ice is known false positive issue for both datasets