VIIRS, or Visible Infrared Imaging Radiometer Suite, is a sensor onboard the Suomi National Polar-orbiting Partnership and NOAA-20 weather satellites. VIIRS collects imagery and radiometric measurements that, among other applications, are used to detect bright lights at night. VIIRS can be used to detect vessels at night and [NOAA’s Earth Observation Group](https://www.ngdc.noaa.gov/eog/index.html) produces a nightly VIIRS Boat Detection (VBD) dataset. The VBD reports the locations of boats detected based on lights and is directly used by GFW.

## Key Tables

+ `pipe_viirs_production_v20180723.raw_vbd_global` - Global VBD dataset. 
+ `pipe_viirs_production_v20180723.raw_vbd_redacted` - VBD dataset without detections in the South Atlantic Anomaly (see (Caveats). Not updated since `2019-11-30`

## Data Description

The VBD dataset and underlying methods are outlined in the following publication:

[Elvidge, C., Zhizhin, M., Baugh, K., & Hsu, F. C. (2015). Automatic boat identification system for VIIRS low light imaging data. Remote Sensing, 7(3), 3020-3036.](https://www.mdpi.com/2072-4292/7/3/3020). 

GFW retains all VBD, including the following key fields:

+ `id_Key`: Unique VBD ID.
+ `Lat_DNB`: VBD pixel latitude from VIIRS DNB geolocation file.
+ `Lon_DNB`: VBD pixel longitude from VIIRS DNB geolocation file.
+ `Date_Mscan`: VBD pixel date-time at mid-point of DNB scan reported in Universal Time.
+ `Date_LTZ`: VBD pixel date-time at mid-point of DNB scan adjusted to standard time in the local time zone (LTZ). No adjustments for Daylight Savings Time (DST) are made.
+ `EEZ`: Exclusive Economic Zone for VBD pixel.
+ `FMZ`: Fishery Management Zone for VBD pixel
+ `MPA`: Marine Protected Area for VBD pixel
+ `QF_Detect`: Integer quality flag for VBD pixel, yielding information about quality and type of detection.
  + `1`: Strong detection. Detection surpassed all VBD threshold tests
  + `2`: Weak detection. Detection did not pass SHI threshold test.
  + `3`: Blurry detection. Detection did not pass SI threshold test.
  + `4`: Gas flare. Detection has a concurrent Nightfire detection, or is in the location of a known gas flare.
  + `5`: False detection: Detection is from high energy particles impacting the DNB sensor, usually due to the South Atlantic anomaly.
  + `6`: False detection: Detection is from lunar glint.
  + `7`: False detection: Detection is from atmospheric glow around bright sources.
  + `8`: Recurring detection. Detection is in location where boats are known to recur.
  + `9`: False detection: Detection is from sensor crosstalk around extremely bright sources, usually flares.
  + `10`: Weak and blurry detection. Detection did not pass either the SHI or SI threshold tests.
  + `11`: Offshore platform. Detection is in location of a known stable light.

(See [document on Colorado School of Mines : Earth Observation Group](https://eogdata.mines.edu/vbd/vbd_readme_v23_r20180824.xlsx) for description of all the columns.)


## Caveats & Known Issues

The EEZ/FMZ/MPA values in the VBD data are not modified by GFW and may differ from the AIS/VMS data (**Verify this is the case**).

The South Atlantic Anomaly (SAA) is an area where the Earth's inner Van Allen radiation belt is at its lowest altitude, allowing more energetic particles from space to penetrate. When such particle hit the sensors on board of the satellite, it creates a false signal which might cause the VBD algorithm to recognize it as a boat detection. To avoid false positives, vessel detections in this area are omitted from `pipe_viirs_production_vYYYYMMDD.raw_vbd_redacted`.

## Example queries

- VIIRS noise filter: eliminate VIIRS noise especially the South Atlantic Anomaly.


## Related tables

- VIIRS footprint
- VIIRS-AIS matching
- VIIRS cloud mask