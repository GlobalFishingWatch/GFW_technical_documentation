## Vessel info

Vessel information such as identity (shipname, callsign, IMO number, flag), gear type, authorizations, and activity (e.g. fishing hours) are another primary GFW data product. These data are compiled from a combination of AIS data, vessel registries, and neural net inference. Use the following key tables for vessel information:

+ `gfw_research.vi_ssvid_vYYYYMMDD` - This table includes the best information available for the vessel based on its full timeseries.
+ `gfw_research.vi_ssvid_byyear_vYYYYMMDD` - This table includes the annual best information for the vessel based only on data in from that year. As a result, the information for a given year in `gfw_research.vi_ssvid_byyear_vYYYYMMDD` may not match that in `gfw_research.vi_ssvid_vYYYYMMDD` (e.g. the vessel recently re-flagged).
+ `vessel_database.all_vessels_vYYYYMMDD` - This table contains all identity information for a vessel available from AIS and vessel registries, including where a vessel is authorized to operate.