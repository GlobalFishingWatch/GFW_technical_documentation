Building on the vessel database, the vessel info tables provide summary information by MMSI that combine AIS activity (positions, hours, fishing hours, etc.), registry info (when available), and neural net outputs (vessel class, length, tonnage, etc.). 

A key purpose of the vessel info tables is to evaluate these multiple and potentially conflicting sources of information to determine the "best" values for various fields (e.g. vessel class, flag, dimensions) to be used by GFW. The vessel info tables are also used to identify active fishing vessels and those vessels that are spoofing or offsetting their position, as well as quickly summarizing fleet activity.

## Key Tables

+ `gfw_research.vi_ssvid_vYYYYMMDD` - This table includes the best activity and identity information available for the vessel based on its full timeseries.
+ `gfw_research.vi_ssvid_byyear_vYYYYMMDD` - This table includes annual best activity and identity information for the vessel based only on data in each year. As a result, the information for a given year in `gfw_research.vi_ssvid_byyear_vYYYYMMDD` may not match that in `gfw_research.vi_ssvid_vYYYYMMDD` (e.g. the vessel recently re-flagged).  
+ `gfw_research.fishing_vessels_ssvid_vYYYYMMDD` - This table includes the GFW yearly list of active non spoofing/offsetting fishing vessels. It is GFW's most restrictive list of fishing vessels and the default list to use in research/analysis.

## Data Description

### `vi_ssvid` Tables

The main vessel info tables (`vi_ssvid_vYYYYMMDD`; `vi_ssvid_byyear_vYYYYMMDD`) include a lot of information about each MMSI. The information is organized into multiple `STRUCT`s.

#### `activity`
This STRUCT contains numerous fields summarizing the MMSI's AIS activity. 

#### `ais_identity`, `inferred`, `registry_info`


#### `best`

#### `on_fishing_list_[nn;known;sr;best]`



## Caveats & Known Issues