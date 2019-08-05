# SAR Detections

SAR (synthetic aperture radar) detections are vessel detection data products derived from two ESA satellites: S1A and S1B of Sentinel-1 mission. Detections from ~2014-10-01 - present, updated intermittently. Imagery availability increases along data acquisition timeframe, especially from mid-2016 on. Various types of SAR imagery captured with differing resolution, beam mode, orbit pass, polarizations, etc. Majority are scene filenames starting with `S1A_IW_GRDH_1SDV` or `S1B_IW_GRDH_1SDV` (naming conventions detailed [here](https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/naming-conventions)) which are stored in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`. The remaining detections are stored in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190703`.

Two primary datasets of SAR detections:
- `gfw-research.sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`: contains majority of SAR detections. Generally faster revisit time, all 10m imagery.
- `gfw-research.sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190703`: contains everything not in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`. Much sparser coverage and fewer total image count, but adds new acquisitions across oceans (even some high seas) not found in `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`.

Each dataset has 3 tables:
1. `objs`: use `lon` and `lat` for detection location. `start_time` for timestamp.
2. `exts`: full scene footprint including ocean and land pixels.
3. `masked_exts`: scene footprint including only ocean pixels.

Notes and caveats:
- use `id` field to join detections from `objs` with associated footprints from `exts` or `masked_exts`
- currently higher confidence in detections from `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190306`
- only 1 full run on `sentinel_ds3_fmean250_e10_d70_s20_8xmean_ns_v20190703` so far. Appears noisier. Improvements TBD.
- sea ice is known false positive issue for both datasets