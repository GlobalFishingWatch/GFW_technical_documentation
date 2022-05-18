Synthetic Aperture Radar (SAR) object detections consist of vessels and offshore infrastructure detected, classified, and matched to AIS/VMS (when available) from satellite radar images taken by ESA's Sentinel-1 mission. 

## Release #1 (DATE)

Key things to know regarding this preliminary release:

- Fixed objects like infrastructure have been removed, so only vessels
- Detections refer, for the most part, to fishing and non-fishing vessels
- Large areas with significant sea ice and icebergs have been excluded
- Detections have been matched to available AIS
- Detections are separated/classified as matched and unmatched
- Detection footprints (area of the ocean scanned) is also provided

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
- `uncertainty` - metric of overall quality of detection

## Data Description

The data tables contain all detections from 2017-01-01 to present, updated intermittently. Imagery availability increases along data acquisition timeframe, especially from mid-2016 on. Vessels are extracted from single scenes while fixed infrastructure is extracted from 6-month median composites, constructed every month with a moving window. Detection was performed on Google Earth Engine with a Constant False Alarm Rate (CFAR) algorithm using the GRD product, Interferometric Wide Mode, VH band at 20 m resolution. Classification (to identify noise) and regression (to estimate length) was performed with a Convolutional Neural Network using both the VH and VV bands.

## Caveats & Known Issues:

- Sentinel-1 does not sample over the open ocean, mostly along shorelines
- spatial coverage is not homogeneous, European waters are imaged more frequently
- sea ice, in high latitudes, is known to introduce false positives
- images prior 2018 are of lower quality, introducing potential false positives

## Links

- [Training Slides](https://docs.google.com/presentation/d/1Rzsz6roQU-QfEdGTq33fApBTkZwsBelzKBHBKWrSALM/edit?usp=sharing)
- [Tech Video](https://#)
- [Detection GEE Code](https://github.com/GlobalFishingWatch/sentinel-1-ee/tree/develop/detection)
- [Machine Learning Code](https://github.com/GlobalFishingWatch/sentinel-1-ee/tree/develop/classification)