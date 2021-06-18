
VIIRS footprint table contains information on the area observed by the VIIRS satellite (Suomi-NPP). The data is obtained from [NASA LAADS](https://ladsweb.modaps.eosdis.nasa.gov/archive/geoMetaJPSS/5110/NPP/).

# Repository

non

# Key tables

Current version

- `scratch_masaki.viirs_footprint_granule_v20210208`


Older version

non



# Data descriptions

Each records of this table can be distinguished by `GranuleID` and single record (granule) represents 6 minutes time window of VIIRS scan. The shape of the granule is contained in the `wkt` column (Well Known Text format). The `DayNightFlag` contains the informaton on whether the granule was scaned at night (`N`), day (`D`) or day/night boundary (`B`), and VIIRS boat detection only present in `N` and `B`. 


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


