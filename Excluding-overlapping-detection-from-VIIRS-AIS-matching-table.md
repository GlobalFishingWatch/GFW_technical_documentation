Single VIIRS overpass from North to South at night can be distinguishable using OrbitNumber. and Successive VIIRS overpass have overlapping region. To eliminat the overlapping, We will choose overpass (i.e OrbitNumber) with smaller satelite zenith angle for each 0.1 degree grid area for each day.


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
    *
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
    # 0.1 degree grid bin
    CAST(round(Lat_DNB*10) as INT64) as lat_bin,
    CAST(round(Lon_DNB*10) as INT64) as lon_bin,
    # OrbitNumber
    CAST(SUBSTR(File_DNB, 40,5) AS INT64) AS OrbitNumber,
    # Satelite Zenith Angle
    SATZ_GDNBO,

FROM
    `world-fishing-827.pipe_viirs_production_v20180723.raw_vbd_global`
WHERE
      QF_Detect IN (1,2,3,5,7,10)
      AND DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

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

