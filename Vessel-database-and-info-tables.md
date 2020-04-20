Vessel information such as identity (shipname, callsign, IMO number, flag), gear type, authorizations, and activity (e.g. fishing hours) are another primary GFW data product. These data are compiled from a combination of AIS data, vessel registries, and neural net inference. 

## Key Tables

+ `vessel_database.all_vessels_vYYYYMMDD` - This table contains all identity information for a vessel available from AIS and vessel registries, including where a vessel is authorized to operate.
+ `gfw_research.vi_ssvid_vYYYYMMDD` - This table includes the best activity and identity information available for the vessel based on its full timeseries.
+ `gfw_research.vi_ssvid_byyear_vYYYYMMDD` - This table includes annual best activity and identity information for the vessel based only on data in each year. As a result, the information for a given year in `gfw_research.vi_ssvid_byyear_vYYYYMMDD` may not match that in `gfw_research.vi_ssvid_vYYYYMMDD` (e.g. the vessel recently re-flagged).

## Data Description

Vessel information data are stored in two key locations. The `vessel_database` dataset contains vessel identity information gathered from AIS messages and vessel registries. This identity information is subsequently combined with summarized AIS activity data in the vessel info tables in `gfw_research`.

### Vessel Database

The Global Fishing Watch vessel database contains information about vessels that is/was recorded in various formats of vessel registries, including RFMO vessel lists and national vessel registers that are publicly available. It also includes lists of vessels that have been obtained through groups of researchers and analyzed by analysts from GFW and its partners. 

The vessel database uses a multi-step process to match identity information from AIS data with identity information from vessel registries. Vessels are matched according to shipname, callsign, IMO number, and MMSI. These matches are then summarized by MMSI to show all identities associated with each MMSI. For more detailed information on the identity matching process, see this document `https://docs.google.com/document/d/1esbAWgfIEcT-F2C35vKkdVa4NeGzSh9MV8dDGH1OCFI/edit`.

The database may contain information that is out of date or incorrect due to wrong information available in one or multiple registries on which the database relies. Users may want to double-check the validity of information found in the database. In case of any questions or suggestions, please contact jaeyoon@globalfishingwatch.org 

### Vessel Info Tables

Building off of the vessel database, the vessel info tables provide summary information by MMSI that include AIS activity (positions, hours, fishing hours, etc.) and neural net outputs (vessel class, length, tonnage, etc.). This information can/is subsequently used for numerous purposes, including quickly summarizing fleet activity and identifying active fishing vessels and those vessels that are spoofing or offsetting their position.

## Caveats & Known Issues