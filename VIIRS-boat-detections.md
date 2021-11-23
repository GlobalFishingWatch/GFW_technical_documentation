

VIIRS, or Visible Infrared Imaging Radiometer Suite, is a sensor onboard the Suomi National Polar-orbiting Partnership and NOAA-20 weather satellites. VIIRS collects imagery and radiometric measurements that, among other applications, are used to detect bright lights at night. VIIRS can be used to detect vessels at night and [NOAA’s Earth Observation Group](https://www.ngdc.noaa.gov/eog/index.html) produces a nightly VIIRS Boat Detection (VBD) dataset. The VBD reports the locations of boats detected based on lights and is directly used by GFW.

## Key Tables

### current version

- `pipe_viirs_production_v20180723.raw_vbd_global`
  - Global VBD dataset. 
- `pipe_viirs_production_v20180723.raw_vbd_redacted`
  - VBD dataset without detections around South America (see Caveats).
  - **No need to use this table**. Use [VIIRS noise filter](VIIRS-noise-filter) to avoid false detection around South America. 
  - **Not updated since `2019-11-30`**



### older version

non

## Data Description

The VBD dataset and underlying methods are outlined in the following publication:

[Elvidge, C., Zhizhin, M., Baugh, K., & Hsu, F. C. (2015). An automatic boat identification system for VIIRS low light imaging data. Remote Sensing, 7(3), 3020-3036.](https://www.mdpi.com/2072-4292/7/3/3020). 

GFW retains all VBD, including the following key fields:

+ `id_Key`: Unique VBD ID.
+ `Lat_DNB`: VBD pixel latitude from VIIRS DNB geolocation file.
+ `Lon_DNB`: VBD pixel longitude from VIIRS DNB geolocation file.
+ `Date_Mscan`: VBD pixel date-time at mid-point of DNB scan reported in Universal Time.
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


## Example queries

- [How to extract VBDs from specific dates and areas?](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/viirs_get_points_date_area.sql)
- [How to remove noise from VIIRS](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/viirs_get_daily_count_without_noise.sql)
- [How to eliminate double-counting from VIIRS?](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/viirs_get_daily_count_without_double_counting.sql)
- Get VIIRS detection in the squid fishing area
- Join with VIIRS footprint
- Join with VIIRS-AIS matching

## Related tables

- [VIIRS AIS matching](VIIRS-AIS-matching)
- [VIIRS cloud mask](VIIRS-cloud-mask)
- [VIIRS footprint](VIIRS-footprint)
- [Logic of VIIRS noise filter](VIIRS-noise-filter): eliminate VIIRS noise especially the South Atlantic Anomaly.



