
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


