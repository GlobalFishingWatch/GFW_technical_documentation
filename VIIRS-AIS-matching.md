
Each row of this table is corresponding to VIIRS detection and this table also contains the AIS/VMS vessel's IDs (`ssvid`) corresponding to each VIIRS detection. 

This table was created from the VIIRS table after excluding [the South American noise](VIIRS-noise-filter). Thus, **this table can also be used as noise-free VIIRS table**.


# Repository

Please look at README in the repository for how to update this table.

- https://github.com/GlobalFishingWatch/prj-viirs-matching

# Key tables

Current version

- `pipe_production_v20201001.proto_matches_raw_vbd_global_3top_v20210514`

Old version

- None


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
    - Matching score between VIIRS and AIS/VMS. Higher value indicates good matching (`score > 0.01` is a tentative threshold for good matching.)




# Caveats & Known Issues

- Overlapping detection
VIIRS satelite may scan the same area twice a night, thus VIIRS may double count the same vessel. To avoid double counting, you can select single scan for each 1 degree grid for a night. See example query.


# Example queries

- [Join with VIIRS table](Join-with-VIIRS-table)
- [Excluding overlapping detection from VIIRS-AIS matching table](Excluding-overlapping-detection-from-VIIRS-AIS-matching-table): eliminate possible double-counting of VIIRS detection.
- Get VIIRS detection in the squid fishing area
- Join with VIIRS footprint
- Join with VIIRS-AIS matching

# Related tables

- [VIIRS boat detections](VIIRS-boat-detections)
- [VIIRS footprint](VIIRS-footprint)
- [VIIRS cloud mask](VIIRS-cloud-mask)


