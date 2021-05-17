
This table contains the AIS/VMS vessel's IDs (`ssvid`) corresponding to each VIIRS detection. Each row of this table is corresponding to VIIRS detection.

This table was created from the VIIRS detection after excluding the South American noise. Thus, **this table can also be used as noise-free VIIRS table**.


# Repository

https://github.com/GlobalFishingWatch/prj-viirs-matching

# Key tables

Current version

- `gfw_research.matches_raw_vbd_global_3top`
    - this table is based on the AIS pipeline version `v20201001`. Thus if you want to join with other AIS information, you need to use the pipeline version.

Older version

- `gfw_research.matches_raw_vbd_global_3top_qf12310`
- `gfw_research.matches_raw_vbd_global_3top_qf1235710`



# Data descriptions

- `detect_id`
    - unique id for each records (i.e. VIIRS detection).
    - Using this key, you can join with VIIRS table (`pipe_viirs_production_v20180723.raw_vbd_global`).
    - `concat(cast(Date_Mscan as string),concat(cast(Lat_DNB as string),cast(Lon_DNB as string))) as detect_id,`
- `detect_timestamp`
    - timespamp of VIIRS detection in UTC.
    - Same as `Date_Mscan`
- `detect_lat`
    - latitude of VIIRS detection.
    - Same as `Lat_DNB`
- `detect_lon`
    - longitude of VIIRS detection.
    - Same as `Lon_DNB`
- `QF_Detect`
    - Integer quality flag for VIIRS detection, yielding information about quality and type of detection
    - See [Data description of VIIRS table](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/VIIRS-boat-detections#data-description)
- `OrbitNumber`
  - Single overpass of VIIRS satelite (from North to South at night) can be distinguishable by `OrbitNumber`.
  - `CAST(SUBSTR(File_DNB, 40,5) AS INT64) AS OrbitNumber,`
- `GranuleID`
  - This field can be used to join with [VIIRS footprint table](VIIRS-footprint) and [VIIRS-cloud-mask](VIIRS-cloud-mask).
  - Format: `A` + Year (2017 ~ ) + Day_of_the_year(001 ~ 366) + hour (00~24) + minutes (00 ~ 60)
  - Each `GranuleID` is corresponding to 6 minutes time window. for example `GranuleID = 'A2017001.0024'` contains VIIRS detection from `2017-01-01 00:24` to `2017-01-01 00:30`.
  - `CONCAT("A", CAST(extract(YEAR from Date_Mscan) as STRING), LPAD(CAST(extract(DAYOFYEAR from Date_Mscan) as STRING), 3, "0"), ".", LPAD(CAST(extract(HOUR from Date_Mscan) as STRING), 2, "0"), LPAD(CAST(DIV(extract(MINUTE from Date_Mscan), 6)*6 as STRING), 2, "0") ) as GranuleID`
+ `RAD_DNB`
    - The brightness (radiance) of the VBD pixel (Unit: nW/cm2/sr)
- `source`
    - source of vessel track data (i.e. `AIS`or VMS like `indovms`, `panama_vms`) that matched with the VIIRS
- `ssvid`
    - `ssvid` of the vessel that matched with the VIIRS
- `seg_id`
    - `seg_id` of the vessel that matched with the VIIRS
- `score`
    - Matching score. Higher value indicates good matching (`score > 0.01` is a tentative threshold for good matching.)




# Caveats & Known Issues

- Overlapping detections



# Example queries


- [VIIRS noise filter](VIIRS-noise-filter): eliminate VIIRS noise especially the South Atlantic Anomaly.
- [Excluding overlapping detection of VIIRS](Excluding-overlapping-detection-of-VIIRS): eliminate possible double counting
- Get VIIRS detection in the squid fishing area
- Join with VIIRS footprint
- Join with VIIRS-AIS matching

# Related tables

- [VIIRS boat detections](VIIRS-boat-detections)
- [VIIRS footprint](VIIRS-footprint)
- [VIIRS cloud mask](VIIRS-cloud-mask)


