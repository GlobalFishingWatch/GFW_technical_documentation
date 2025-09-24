# VIIRS AIS matching

Each row of this table is corresponding to VIIRS detection and this table also contains the AIS/VMS vessel's IDs (`ssvid`) corresponding to each VIIRS detection. 

This table was created from the VIIRS table after excluding [the South American noise](VIIRS-noise-filter). Thus, **this table can also be used as noise-free VIIRS table**.


# Repository

Please look at README in the repository for how to update this table.

- https://github.com/GlobalFishingWatch/prj-viirs-matching

# Key tables

Current version

- `pipe_production_v20201001.proto_matches_raw_vbd_global_3top_v20210514`
  - VIIRS matches to AIS and VMS
- `pipe_production_v20201001.proto_viirs_match_ais_matches`
  - VIIRS matches to only AIS


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


# VIIRS boat detections

VIIRS, or Visible Infrared Imaging Radiometer Suite, is a sensor onboard the Suomi National Polar-orbiting Partnership and NOAA-20 weather satellites. VIIRS collects imagery and radiometric measurements that, among other applications, are used to detect bright lights at night. VIIRS can be used to detect vessels at night and [NOAA’s Earth Observation Group](https://www.ngdc.noaa.gov/eog/index.html) produces a nightly VIIRS Boat Detection (VBD) dataset. The VBD reports the locations of boats detected based on lights and is directly used by GFW.

## Key Tables

### Current version

- `pipe_viirs_production_v20220112.raw_vbd_global`
  - This table contains all the VBDs including non-vessel detections.
  - partitioned by `Date_Mscan`
- `pipe_viirs_production_v20220112.raw_vbd_global_without_noise`
  - This table contains **only true vessel detections**. The noise removal query [can be found here](https://github.com/GlobalFishingWatch/pipe-viirs/blob/main/assets/load-vbd-without-noise.sql.j2#L51).
  - partitioned by `Date_Mscan`
  - This table has a subset of columns from the `raw_vbd_global` table and some additional columns for the convenience of analysts. If you want columns not included in this table, you need to get it from `raw_vbd_global` table by joining with `id_Key` field. 

### Older version

- `pipe_viirs_production_v20180723.raw_vbd_global`
  - Global VBD dataset. 
- `pipe_viirs_production_v20180723.raw_vbd_redacted`
  - VBD dataset without detections around South America (see Caveats).
  - **No need to use this table**. Use [VIIRS noise filter](VIIRS-noise-filter) to avoid false detection around South America. 
  - **Not updated since `2019-11-30`**

## Data Description

The VBD dataset and underlying methods are outlined in the following publication:

[Elvidge, C., Zhizhin, M., Baugh, K., & Hsu, F. C. (2015). An automatic boat identification system for VIIRS low light imaging data. Remote Sensing, 7(3), 3020-3036.](https://www.mdpi.com/2072-4292/7/3/3020). 

GFW retains all VBD, including the following key fields:

+ `Date_Mscan`: VBD pixel date-time at mid-point of DNB scan reported in Universal Time.
+ `id_Key`: Unique VBD ID.
+ `Lat_DNB`: VBD pixel latitude from VIIRS DNB geolocation file.
+ `Lon_DNB`: VBD pixel longitude from VIIRS DNB geolocation file.
+ `Date_LTZ`: VBD pixel date-time at mid-point of DNB scan adjusted to standard time in the local time zone (LTZ). No adjustments for Daylight Savings Time (DST) are made.
+ `EEZ`: Exclusive Economic Zone for VBD pixel.
+ `FMZ`: Fishery Management Zone for VBD pixel
+ `MPA`: Marine Protected Area for VBD pixel
+ `RAD_DNB` : The brightness (radiance) of the VBD pixel (Unit: nW/cm2/sr)
+ `QF_Detect`: Integer quality flag for VBD pixel, yielding information about quality and type of detection.
  + `1`: Strong detection. Detection surpassed all VBD threshold tests
  + `2`: Weak detection. Detection did not pass SHI threshold test.
  + `3`: Blurry detection. Detection did not pass SI threshold test.
  + `4`: Gas flare. Detection has a concurrent Nightfire detection or is in the location of a known gas flare.
  + `5`: False detection: Detection is from high-energy particles impacting the DNB sensor, usually due to the South Atlantic anomaly.
  + `6`: False detection: Detection is from lunar glint.
  + `7`: False detection: Detection is from atmospheric glow around bright sources.
  + `8`: Recurring detection. Detection is in locations where boats are known to recur.
  + `9`: False detection: Detection is from sensor crosstalk around extremely bright sources, usually flares.
  + `10`: Weak and blurry detection. Detection did not pass either the SHI or SI threshold tests.
  + `11`: Offshore platform. Detection is in the location of a known stable light.
+ `SATZ_GDNBO` : Satelite Zenith Angle
+ `SOLZ_GDNBO`: Sun Zenith Angle
+ `LUNZ_GDNBO`: Moon Zenith Angle

  (See [document on Colorado School of Mines: Earth Observation Group](https://eogdata.mines.edu/vbd/vbd_readme_v23_r20180824.xlsx) for a description of all the columns.)

Other useful fields you can create from this table: 

- `detect_id`
    - unique id for each record (VIIRS detection).
    - Using this key, you can join with [VIIRS-AIS matching table](VIIRS-AIS-matching).
    - `concat(cast(Date_Mscan as string),concat(cast(Lat_DNB as string),cast(Lon_DNB as string))) as detect_id,`
- `OrbitNumber`
  - Single overpass of VIIRS satellite (from North to South at night) can be distinguishable by `OrbitNumber`.
  - This field can be used to eliminate overlap between successive orbits.
  - `CAST(SUBSTR(File_DNB, 40,5) AS INT64) AS OrbitNumber,`
- `GranuleID`
  - This field can be used to join with [VIIRS footprint table](VIIRS-footprint) and [VIIRS cloud mask](VIIRS-cloud-mask).
  - A + Year (2017) + Day_of_the_year(001 ~ 365) + hour (00~24) + minutes (00 ~ 60)
  - Each `GranuleID` is corresponding to 6 minutes time window. for example `GranuleID = 'A2017001.0024'` contains VIIRS detection from `2017-01-01 00:24` to `2017-01-01 00:30`.
  - `CONCAT("A", CAST(extract(YEAR from Date_Mscan) as STRING), LPAD(CAST(extract(DAYOFYEAR from Date_Mscan) as STRING), 3, "0"), ".", LPAD(CAST(extract(HOUR from Date_Mscan) as STRING), 2, "0"), LPAD(CAST(DIV(extract(MINUTE from Date_Mscan), 6)*6 as STRING), 2, "0") ) as GranuleID`



## Caveats & Known Issues

### EEZ/FMZ/MPA

The EEZ/FMZ/MPA values in the VBD data are not modified by GFW and may differ from the AIS/VMS data (**Verify this is the case**).

### The South Atlantic Anomaly

The VIIRS contains a lot of false detection around South America, which is caused by the South Atlantic Anomaly (SAA). The SAA is an area where the Earth's inner Van Allen radiation belt is at its lowest altitude, allowing more energetic particles from space to penetrate. When such a particle hits the sensors on board the satellite, it creates a false signal which might cause the VBD algorithm to recognize it as a boat detection.

To avoid false positives due to the South Atlantic Anomaly, we've developed a [VIIRS noise filter]( VIIRS-noise-filter). 



### Overlapping detection

VIIRS satellite may scan the same area twice a night, thus VIIRS may double count the same vessel. To avoid double-counting, you can select a single scan for each 1-degree grid for a night. [See example query](Excluding-overlapping-detection-of-VIIRS).


### Known data disruptions
- **June 28 to July 7, 2022**: The Suomi NPP satellite experienced [issues](https://www.ospo.noaa.gov/data/messages/2022/07/MSG_20220707_2318.html) on June 28, 2022 that resulted in excessive edge noise in the day / night band. There’s an unusually high number of VBD in some regions (e.g., within Gabon, Pakistan, India, Myanmar, Taiwan, China, and a few other places in Africa, Mediterranean Sea, and Asia), on June 28 and 29 and significantly fewer detections (globally) after, until July 7th.
- **July 26 to August 20, 2022**: VIIRS Boat Detection data are not available from July 26 to August 20, 2022. The Suomi NPP satellite entered a non-nominal state during this period ([announcement](https://lpdaac.usgs.gov/news/suomi-npp-recovers-from-safe-mode2/)).

## Example queries

- [How to extract VBDs from specific dates and areas?](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/viirs_get_points_date_area.sql)

- [How to eliminate double-counting from VIIRS?](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/viirs_get_daily_count_without_double_counting.sql)
- Get VIIRS detection in the squid fishing area
- Join with VIIRS footprint
- Join with VIIRS-AIS matching

## Related tables

- [VIIRS AIS matching](VIIRS-AIS-matching)
- [VIIRS cloud mask](VIIRS-cloud-mask)
- [VIIRS footprint](VIIRS-footprint)
- [Logic of VIIRS noise filter](VIIRS-noise-filter): eliminate VIIRS noise especially the South Atlantic Anomaly.

## Updates

- `pipe_viirs_production_v20220112.raw_vbd_global`
  - Missing VBD issue in the previous version is solved for the dates after `2020-01-07`. There may be still issues before the date. 
- `pipe_viirs_production_v20180723.raw_vbd_global`
  - It turns out that this table is missing some detections in the Asian region for some period.
  
  
# VIIRS cloud mask

This table contains information on the Cloud presence/absence data derived from VIIRS satellite (Suomi-NPP). The data was obtained from [CLDMSK_L2_VIIRS_SNPP](https://ladsweb.modaps.eosdis.nasa.gov/missions-and-measurements/products/CLDMSK_L2_VIIRS_SNPP/). The table is aggregated into a 0.1-degree grid (represented by `lat_bin`, `lon_bin`).

Each record of this table represents a grid cell of a granule (i.e. this table will be unique using fields `GranuleId`, `lat_bin`and `lon_bin`).


# Repository

Non but the codes exist in the VM instance `viirs-cloud-mask`.

# Key tables

Current version

- `scratch_masaki.cloud_mask_10th_degree_grid`


Older version

non



# Data descriptions


- `GranuleID` : ID of this granule. See [VIIRS footprint](VIIRS-footprint) for the explanation of granule.
- `lat_bin`	: 0.1-degree grid latitude. `round(lat*10)`
- `lon_bin`	: 0.1-degree grid longitude. `round(lon*10)`
- `OrbitNumber` : ID of this orbit.
- `StartDateTime` : Start datetime of this granule.
- `mean_integer_cloud_mask` : Cloud condition of this pixel (0 = cloudy, 1= probably cloudy, 2 = probably clear, 3 = confident clear). The original VIIRS CLOUD MASK have integer value, but the value is averaged for each 0.1-degree grid in this table. Thus, this field contains real number from 0 to 3.
- `mean_sensor_zenith` : Mean zenith angle of VIIRS satelite in this pixel.
- `lat` : 0.1-degree grid latitude. `0.1*round(lat*10)`
- `lon` : 0.1-degree grid longitude. `0.1*round(lat*10)`



# Caveats & Known Issues

- This table does not include cloud information over land.
- `mean_integer_cloud_mask` represents only presense/absence of the cloud, thus the cloud thickness is not taken into account.


# Example queries




# Related tables

<br>

# VIIRS - excluding overlappint detections to AIS Matching tables

Single VIIRS overpass from North to South at night can be distinguishable using OrbitNumber. And Successive VIIRS overpasses have overlapping regions. To eliminate the overlapping, We will choose an overpass (i.e OrbitNumber) with a smaller satellite zenith angle for each 1-degree grid area for each day.


# Example query

```sql
CREATE TEMP FUNCTION start_date() AS (DATE('2020-01-01'));
CREATE TEMP FUNCTION end_date() AS (DATE('2020-01-02'));

# Eliminate overlapping detection from VIIRS-AIS matching table
# Single VIIRS overpass from North to South at night can be distinguishable using OrbitNumber.
# And Successive VIIRS overpasses have overlapping regions. 
# To eliminate the overlapping, we will choose an overpass (i.e OrbitNumber) with a smaller satellite zenith angle for each 0.1 degree grid area for each day.

WITH 


# VIIRS-AIS matching table
viirs_matching AS (

SELECT  
    DATE(detect_timestamp) as date,
    # 1 degree grid bin
    CAST(round(detect_lat) as INT64) as lat_bin,
    CAST(round(detect_lon) as INT64) as lon_bin,
    *,
FROM
    `world-fishing-827.gfw_research.matches_raw_vbd_global_3top_v20210514`
WHERE
      DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

),



# VIIRS table
viirs AS (

SELECT
    # date
    Date(Date_Mscan) as date,    
    # 1 degree grid bin
    CAST(round(Lat_DNB) as INT64) as lat_bin,
    CAST(round(Lon_DNB) as INT64) as lon_bin,
    # OrbitNumber
    CAST(SUBSTR(File_DNB, 40,5) AS INT64) AS OrbitNumber,
    # Satelite Zenith Angle
    SATZ_GDNBO,

FROM
    `world-fishing-827.pipe_viirs_production_v20180723.raw_vbd_global`
WHERE
    DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

),


# Aggregate Satelite Zenith Angle for each grid-Orbit-date
date_orbit_grid_zenith as (
  select
    date,
    OrbitNumber,
    lat_bin,
    lon_bin,
    0.5*(max(SATZ_GDNBO) + min(SATZ_GDNBO)) as SATZ_GDNBO,
  from
    viirs
  GROUP BY 
    date, OrbitNumber, lat_bin, lon_bin

),


# Select smallest zenith orbit for each grid and day
# This is used for eliminate overlapping area from successive orbits
smallest_zenith_orbit as (
  SELECT
    date,
    lat_bin,
    lon_bin,
    OrbitNumber,
    SATZ_GDNBO,
    row_num
  FROM
    (
      select 
        *,
        ROW_NUMBER() OVER (PARTITION BY date, lat_bin, lon_bin ORDER BY SATZ_GDNBO ) AS row_num
      FROM
        date_orbit_grid_zenith
    )
  WHERE
    row_num	 = 1
)


# Extract detection only from smallest zenith orbits for each grid and day
select
  a.*
from
  viirs_matching a
  inner join
  smallest_zenith_orbit b
  using(date, OrbitNumber, lat_bin, lon_bin)
```

# VIIRS excluding overlapping detection

Single VIIRS overpass from North to South at night can be distinguishable using OrbitNumber. And Successive VIIRS overpasses have overlapping regions. To eliminate the overlapping, We will choose an overpass (i.e OrbitNumber) with a smaller satellite zenith angle for each 1-degree grid area for each day.


# Example query

```sql
# Eliminate overlapping detection from VIIRS
# Single VIIRS overpass from North to South at night can be distinguishable using OrbitNumber.
# And Successive VIIRS overpasses have overlapping region. 
# To eliminate the overlapping, We will choose an overpass (i.e OrbitNumber) with a smaller satellite zenith angle for each 1-degree grid area for each day.

WITH 
  
# VIIRS detection
viirs AS (

SELECT  
    Date(Date_Mscan) as date,
    Date_Mscan,
    Lat_DNB,
    Lon_DNB,
    Rad_DNB,
    QF_Detect,
    
    # 1 degree grid bin
    CAST(round(Lat_DNB) as INT64) as lat_bin,
    CAST(round(Lon_DNB) as INT64) as lon_bin,
    # OrbitNumber
    CAST(SUBSTR(File_DNB, 40,5) AS INT64) AS OrbitNumber,
    # Satelite Zenith Angle
    SATZ_GDNBO,

FROM
    `world-fishing-827.pipe_viirs_production_v20180723.raw_vbd_global`
WHERE
      DATE(_PARTITIONTIME) BETWEEN "2020-01-01" AND "2020-01-01"
),


# Calculate Satelite Zenith Angle for each grid-Orbit-date
date_orbit_grid_zenith as (
  select
    date,
    OrbitNumber,
    lat_bin,
    lon_bin,
    0.5*(max(SATZ_GDNBO) + min(SATZ_GDNBO)) as SATZ_GDNBO,
  from
    viirs
  GROUP BY 
    date, OrbitNumber, lat_bin, lon_bin

),



# Select smallest zenith orbit for each grid and day
# This is used for eliminate overlapping area from successive orbits
smallest_zenith_orbit as (
  SELECT
    date,
    lat_bin,
    lon_bin,
    OrbitNumber,
    SATZ_GDNBO,
    row_num
  FROM
    (
      select 
        *,
        ROW_NUMBER() OVER (PARTITION BY date, lat_bin, lon_bin ORDER BY SATZ_GDNBO ) AS row_num
      FROM
        date_orbit_grid_zenith
    )
  WHERE
    row_num	 = 1
)


# Extract detection only from smallest zenith orbit for each grid and day
select
  a.*
from
  viirs a
  inner join
  smallest_zenith_orbit b
  using(date, OrbitNumber, lat_bin, lon_bin)
where
  QF_Detect IN (1,2,3,5,7,10)
```

# VIIRS footprint

VIIRS footprint table contains information on the area observed by the VIIRS satellite (Suomi-NPP). The data is obtained from [NASA LAADS](https://ladsweb.modaps.eosdis.nasa.gov/archive/geoMetaJPSS/5110/NPP/).

## Repository

https://github.com/GlobalFishingWatch/viirs-footprint

## Key tables

Current version

- `scratch_masaki.viirs_footprint_granule_v20210208`


Older version

non



# Data descriptions

Each record of this table can be distinguished by `GranuleID` and a single record (granule) represents 6 minutes time window of VIIRS scan. The shape of the granule is contained in the `wkt` column (Well Known Text format). The `DayNightFlag` contains the information on whether the granule was scanned at night (`N`), day (`D`), or day/night boundary (`B`), and VIIRS boat detection only present in `N` and `B`. 


- `GranuleID` : Unique id for each records (granules)
- `StartDateTime` : Start datetime of this granule.
- `MidDateTime` : Median datetime of this granule (StartDateTime + 3 minutes).
- `EndDateTime` : End datetime of this granule (StartDateTime + 6 minutes).
- `date` : Date at StartDateTime
- `OrbitNumber` : `OrbitNumber` of this granule
- `DayNightFlag` : day/night information of this granule, night (`N`), day (`D`) or day/night boundary (`B`).
- `wkt` : The shape of this granule in WKT format.
- `NorthBoundingCoord` : North bound latitude of this granule
- `SouthBoundingCoord` : South bound latitude of this granule
- `EastBoundingCoord` : East bound longitude of this granule
- `WestBoundingCoord` : West bound longitude of this granule
- `GRingLongitude1` : Longitude of vertex_1 of this granule
- `GRingLongitude2` : Longitude of vertex_2 of this granule
- `GRingLongitude3` : Longitude of vertex_3 of this granule
- `GRingLongitude4` : Longitude of vertex_4 of this granule
- `GRingLatitude1` : Latitude of vertex_1 of this granule
- `GRingLatitude2` : Latitude of vertex_2 of this granule
- `GRingLatitude3` : Latitude of vertex_3 of this granule
- `GRingLatitude4` : Latitude of vertex_4 of this granule

The `wkt` was created by connecting four GRing vertices using great circle line.



# Caveats & Known Issues





# Example queries




# Related tables


# Join with VIIRS table

```sql
CREATE TEMP FUNCTION start_date() AS (DATE('2020-01-01'));
CREATE TEMP FUNCTION end_date() AS (DATE('2020-01-02'));

# Add VIIRS information to the VIIRS-AIS matching table

WITH 


# VIIRS-AIS matching table
viirs_matching AS (

SELECT  
    *
FROM
    `world-fishing-827.pipe_production_v20201001.proto_matches_raw_vbd_global_3top_v20210514`
WHERE
      DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

),



# VIIRS table
viirs AS (

SELECT
    concat(cast(Date_Mscan as string),concat(cast(Lat_DNB as string),cast(Lon_DNB as string))) as detect_id,
    *

FROM
    `world-fishing-827.pipe_viirs_production_v20220112.raw_vbd_global`
WHERE
      QF_Detect IN (1,2,3,5,7,10)
      AND DATE(Date_Mscan) BETWEEN start_date() AND end_date()

)

# Extract detection only from smallest zenith orbits for each grid and day
select
  *
from
  viirs_matching a
  left join
  viirs b
  using(detect_id)

limit 100
```

# VIIRS noise filter

## Example query

VIIRS contains many false detections, especially around South America. The example query linked below reduces the noise in South America. (but it may eliminate some prat of true detection as well as South Atlantic Anomaly)

[Example query](https://github.com/GlobalFishingWatch/viirs-noise-filter/blob/main/code/get_viirs_without_noise.sql)

The filter basically adopts QF1,2,3,10 for outside of South America, while adopting some portion of QF1,2,3,5,7,10 based on the value of `Rad_DNB` and `SHI`.

## Repository

The filter was developed in the repository [viirs-noise-filter](https://github.com/GlobalFishingWatch/viirs-noise-filter).

You can see the visualization results we used to develop that filter in the link below.

https://github.com/GlobalFishingWatch/viirs-noise-filter/blob/main/rendered_output/01_develop_viirs_noise_filter.md






