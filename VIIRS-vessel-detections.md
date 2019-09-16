VIIRS, or Visible Infrared Imaging Radiometer Suite, is a sensor onboard the Suomi National Polar-orbiting Partnership and NOAA-20 weather satellites. VIIRS collects imagery and radiometric measurements that, among other applications, are used to detect bright lights at night. VIIRS can be used to detect vessels at night and [NOAA’s Earth Observation Group](https://www.ngdc.noaa.gov/eog/index.html) produces a nightly VIIRS Boat Detection (VBD) dataset. The VBD reports the locations of boats detected based on lights and is directly used by GFW.

## Key Tables

+ `pipe_viirs_production_vYYYYMMDD.raw_vbd_global` - Global VBD dataset. 
+ `pipe_viirs_production_vYYYYMMDD.raw_vbd_redacted` - VBD dataset without detections in the South Atlantic Anomaly (see (Caveats).

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

## Caveats & Known Issues

The EEZ/FMZ/MPA values in the VBD data are not modified by GFW and may differ from the AIS/VMS data (**Verify this is the case**).

The South Atlantic Anomaly (SAA) is an area where the Earth's inner Van Allen radiation belt is at its lowest altitude, allowing more energetic particles from space to penetrate. When such particle hit the sensors on board of the satellite, it creates a false signal which might cause the VBD algorithm to recognize it as a boat detection. To avoid false positives, vessel detections in this area are omitted from `pipe_viirs_production_vYYYYMMDD.raw_vbd_redacted`.