
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

