Synthetic Aperture Radar (SAR) object detections consist of vessels and offshore infrastructure detected, classified, and matched to AIS/VMS (when available) from satellite radar images taken by ESA's Sentinel-1 mission.

## 1st Release (2022-05-25)

What to know regarding this preliminary release:

- Fixed objects like infrastructure have been removed, so only vessels <sup>1</sup>
- Detections refer, for the most part, to fishing and non-fishing vessels
- Large areas with significant sea ice and icebergs have been excluded (high latitudes)
- False positives (noise) have been filtered out with ML (this is not perfect)
- Detections have been matched to available vessels with AIS
- Detections are classified and separated as matched and unmatched
- Detections are computed with a delay of 72 hours, and tables updated daily
- Detections are available globally from January 1st 2022 onward <sup>2</sup>
- Detection footprints (area of the ocean scanned) are also provided

<sup>1</sup> Future releases will include all offshore infrastructure  
<sup>2</sup> Future releases will include detections from 2015/2017 onward

[Link to Map](https://#)

[Link to Data](https://#)

## Key Concepts

- Data point: individual detection of maritime object (from single SAR scene)
- Polygon: outline of the (valid) area within the scene used for detection
- Matched: individual detection has been paired with a specific AIS message

## Key Tables

BQ: `world-fishing-827.proj_sentinel1_v20210924`

- `detect_scene_raw_YYYYMMDD` - id/lon/lat/time from Earth Engine detections
- `detect_foot_raw_YYYYMMDD` - id/time/wkt-polygons from Earth Engine scene footprints
- `detect_scene_pred_YYYYMMDD` - id/presence/length from machine learning prediction
- `detect_scene_match` - id/matching-info/vessel-info from matching algorithm

## Key Fields

- `scene_id` - ID of Sentinel-1 scene from where detections were extracted
- `detect_id` - ID unique to every detection, composed as `scene_id;lon;lat`
- `detect_lon` - longitude coordinate
- `detect_lat` - latitude coordinate
- `detect_timestamp` - time the image was taken
- `ssvid` - vessel identifier if the detection matches to AIS/VMS
- `score` - score cutoff used to match the detection
- `confidence` - confidence cutoff used to match the detection
- `presence` - probability of vessel presence from a Conv Neural Net
- `length_m` - length estimated with a Conv Neural Net
- `footprint_wkt` - WKT multipolygon of image area used for detection
- `var1/var2` - vessel info before and after the SAR image used for matching

## Key Buckets

GCS: Data output from Earth Engine used to produce the BQ Tables:

Detections: `gs://gfw-production-sentinel1-detection-gee/v20210924/<date>`  
Footprints: `gs://gfw-production-sentinel1-detection-footprints/v20210924/<date>`  
Thumbnails: `gs://gfw-production-sentinel1-detection-thumbnails/v20210924/<date>`   

## Data Description

The data tables contain all detections from 2017-01-01 to present, updated intermittently. Imagery availability increases along data acquisition timeframe, especially from mid-2016 on. Vessels are extracted from single scenes while fixed infrastructure is extracted from 6-month median composites, constructed every month with a moving window. Detection was performed on Google Earth Engine with a Constant False Alarm Rate (CFAR) algorithm using the GRD product, Interferometric Wide Mode, VH band at 20 m resolution. Classification (to identify noise) and regression (to estimate length) was performed with a Convolutional Neural Network using both the VH and VV bands.

## Caveats & Known Issues:

- Sentinel-1 does not take images over the open ocean, mostly near coastal waters
- spatial coverage is not homogeneous, e.g. European waters are imaged more frequently
- temporal coverage is not homogenous, e.g. 2017 has less area covered than 2021
- Sentinel-1B stopped operating on 2021-12-23, reducing daily images from ~400 to ~250
- sea ice and icebergs, at high latitudes, are known to introduce false positives
- images prior 2018 are of lower quality, introducing potential false positives

## Example Queries
+ [SAR_dark_vessel_locations.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/SAR_dark_vessel_locations.sql)
+ [SAR_dark_vessels_eezs.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/SAR_dark_vessels_eezs.sql) 
+ [SAR_matched_vessels.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/SAR_matched_vessels.sql)

## Additional Information

- [SAR Detection Slides](https://docs.google.com/presentation/d/1Rzsz6roQU-QfEdGTq33fApBTkZwsBelzKBHBKWrSALM/edit?usp=sharing)
- [Technology Video](https://#)
- [Detection GEE Code](https://github.com/GlobalFishingWatch/sentinel-1-ee/tree/develop/detection)
- [Machine Learning Code](https://github.com/GlobalFishingWatch/sentinel-1-ee/tree/develop/classification)
- [Matching Algorithm Code](https://#)