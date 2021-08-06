The Global Fishing Watch vessel database contains information about vessels that is/was recorded in various formats of vessel registries, including RFMO vessel lists and national vessel registers that are publicly available. It also includes lists of vessels that have been obtained through groups of researchers and analyzed by analysts from GFW and its partners.

## Key Tables
+ `vessel_database.all_vessels_vYYYYMMDD` - This table contains all identity information for a vessel available from AIS and vessel registries, including where a vessel is authorized to operate.

## Data Description
The vessel database uses a multi-step process to match identity information from AIS data with identity information from vessel registries. Vessels are matched according to shipname, callsign, IMO number, and MMSI. These matches are then summarized by MMSI to show all identities associated with each MMSI. For more detailed information on the identity matching process, see this document [https://docs.google.com/document/d/1esbAWgfIEcT-F2C35vKkdVa4NeGzSh9MV8dDGH1OCFI/edit]().

## Caveats & Known Issues
The database may contain information that is out of date or incorrect due to wrong information available in one or multiple registries on which the database relies. Users may want to double-check the validity of information found in the database. In case of any questions or suggestions, please contact jaeyoon@globalfishingwatch.org