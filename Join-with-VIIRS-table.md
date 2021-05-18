
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
    `world-fishing-827.gfw_research.matches_raw_vbd_global_3top_v20210514`
WHERE
      DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

),



# VIIRS table
viirs AS (

SELECT
    concat(cast(Date_Mscan as string),concat(cast(Lat_DNB as string),cast(Lon_DNB as string))) as detect_id,
    *

FROM
    `world-fishing-827.pipe_viirs_production_v20180723.raw_vbd_global`
WHERE
      QF_Detect IN (1,2,3,5,7,10)
      AND DATE(_PARTITIONTIME) BETWEEN start_date() AND end_date()

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