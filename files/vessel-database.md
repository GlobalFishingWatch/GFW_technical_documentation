# Vessel Database

## Data Overview
The Global Fishing Watch vessel database contains information about vessels that is/was recorded in various formats of vessel registries, including RFMO vessel lists and national vessel registers that are publicly available. It also includes lists of vessels that have been obtained through groups of researchers and analyzed by analysts from GFW and its partners. The collected registry data are matched to AIS data to assign further identity, authorization, and owner information among others to the vessel found in AIS. For further reading about the matching, go to the [link](https://docs.google.com/document/d/1esbAWgfIEcT-F2C35vKkdVa4NeGzSh9MV8dDGH1OCFI/edit?usp=sharing). 
Owner: Jaeyoon

## Key Tables
+ `vessel_database.all_vessels_vYYYYMMDD` - This table contains all identity information for a vessel available from AIS and vessel registries, including where a vessel is authorized to operate, and who owns the vessel. The table is automatically created with the latest update on the 1st day of the month, and contains data until the date `YYYYMMDD`. For instance, `vessel_database.all_vessels_v20211101` is created on the Dec 1, 2021, and includes AIS/registry data up to `2020-10-31 23:59:59`. You can see some stats related to Nov 2024 database in [this dashboard](https://datastudio.google.com/s/kBjYRu6dTyo). 

+ `vessel_database_staging.registry_[registry_code]_vYYYYMMDD` - These tables represent raw registries scraped monthly. You may replace [registry_code] with the registry code to be found in the [registry sources](https://docs.google.com/spreadsheets/d/1mfRIlIcl7VLnHIsf0mioJruW2L9YzE0npOoEKdGnOkI/edit?usp=sharing). Check [this list as a public resource](https://globalfishingwatch.org/our-apis/documentation#vessel-api-registry-codes-data-sources) to share with Partners. The tables include data obtained from each registry before they go through the process of matching registry to AIS. 

+ `single_mmsi_matched_vessels_byyear_vYYYYMMDD` - This table includes the most representative vessel per MMSI by year from the vessel database. Therefore, a single MMSI represents one row (vessel) in the given year in this table. The fields are identical to the vessel database except this table has `year` column.

## Data Description
The vessel database uses a multi-step process to match identity information from AIS data with identity information from vessel registries. Vessels are matched according to shipname, callsign, IMO number, and MMSI. These matches are then summarized by MMSI to show all identities associated with each MMSI. For more detailed information on the identity matching process, see this [document](https://docs.google.com/document/d/1esbAWgfIEcT-F2C35vKkdVa4NeGzSh9MV8dDGH1OCFI/edit).

* `matched`: TRUE if the AIS identity standard-matches to one or more registries. The standard match indicates that the records from AIS and registry sufficiently close in multiple fields among name, callsign, IMO, MMSI number, and flag. In most cases, you would only need the standard matches. 
* `loose-match`: TRUE if the AIS identity loose-matches to one or more registries. The loose-match represents the records that matched between AIS and registry through only name and flag. This is sometimes useful for investigating additional Chinese/Taiwanese vessels as some do only broadcast ship name. Not broadcasting other identity fields prevents the more confident standard-match, but if they use distinct ship name, it is possible to identify them through loose-match. Beware that loose-matches may present wrong matches if more than one vessel from the same country exist (e.g. There are >20 vessels that are US-flagged and named `FREEDOM`)
* `identity`: This `STRUCT` includes identity fields that are MMSI number, shipname, callsign, IMO number, and flag. It relies on data recorded in AIS data if a match was not perfect but good enough to pass the matching process. (e.g. AIS: ACONCAGUA vs Registry: ACONCAGOUA → In this case, normalized value from AIS (ACONCAGUA) remains in final database) 
  * > `ssvid`: The source-specific vessel ID which is equivalent to MMSI number in this dataset
  * > `n_`: This prefix represents "normalized" for shipname and callsign
  * > `flag`: The flag of a vessel is represented by ISO-3 country code

* `feature`: This `STRUCT` presents representative (or best) value of vessel characteristics (length, tonnage, power, geartype, and crew number) derived from corresponding values registered in multiple registries. All raw values that are 2 standard deviations away from the average are discarded as an outlier. Each registry has only one vote for the calculation so as to avoid double counts for multiple scrapes of a registry. If some registries have higher levels of confidence (4 being the highest, 1 being the lowest), then they override the lower ones.  
  * > `geartype`: It represents the classification of a vessel. All (distinct) gears recorded in registries for a given vessel are stored in a form of ARRAY. Gears that can be merged will be merged to more fine-grained values (e.g. fishing | tuna_purse_seines | purse_seines → merged to only tuna_purse_seines)

* `is_fishing`, `is_carrier`, `is_bunker`: TRUE if the given vessel is categorized into the corresponding type based on feature.geartype values, FALSE otherwise

* `activity`: It is an `ARRAY of STRUCT` of time blocks (including first_timestamp, last_timestamp, messages). Time blocks are AIS activities of the given vessel separated by >90 days of gap from one another. For instance, if a vessel is AIS-active from `2020-01-01` to `2020-08-31`, and then active again from `2021-01-01` to `2021-10-31`, the `activity` field includes two elements in its `ARRAY` of the activity range in 2020 and 2021. However, if the first activity was up to `2020-11-30`, then the two activity ranges are merged as from `2020-01-01` to `2021-10-31`. 

* `registry`: It is an `ARRAY of STRUCT` of all relevant raw values from source registries. Often, this `ARRAY` includes a number of rows because a vessel may be registered to multiple registries, and those registries are scraped every month for a long period. 
  * > `list_uvi`: A unique vessel identifier within a given registry, it starts with registry code followed by ID code (e.g. WCPFC-1079)
  * > `authorized_from`, `authorized_to`: Information about authorization period (mostly from RFMO registries). If one of the fields is NULL, it means no authorization information is available publicly, not that it is unauthorized.
  * > `is_active`: TRUE if a given vessel is active in AIS (at least 1 day) during the authorized period, FALSE otherwise.
  * > `last_modified`: Date of known update made in registry
  * > `scraped`: Date of scraping (usually the 1st day of month)
  * > `owner`: Name of the owner (usually a company name). The owner is as indicated in a registry, which means it could be only registered owner instead of beneficial owner. We try to pull the beneficial owner if that information is available in the given registry.
  * > `owner_flag`: Nationality of the owner, based on the available information found in the given registry


## Caveats & Known Issues
The database may contain information that is out of date or incorrect due to wrong information available in one or multiple registries on which the database relies. Users may want to double-check the validity of the information found in the database. Users can provide the correct information in this [spreadsheet](https://docs.google.com/spreadsheets/d/1znSit_hHWf7X0PO5MyvbugnDc2q55NK2RN0CBnx2Ec4/edit?usp=sharing) which will override the current data in the table. 
In case of any questions or suggestions, please contact jaeyoon@globalfishingwatch.org


## Example Queries
You can find some example queries for the vessel database in this [link](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current)

## Links
  * [Training slides](https://docs.google.com/presentation/d/1S_ch-HPox3B-1IrQzSuRbS2iocrrJgTyMVWCZNJ7UdA/edit?usp=sharing)
  * [GitHub repo](https://github.com/GlobalFishingWatch/vessel-identity/tree/master/vessel-database)
  * [Data dashboard](https://datastudio.google.com/s/vf4bMOulEa8)
  * [Registry sources](https://docs.google.com/spreadsheets/d/1mfRIlIcl7VLnHIsf0mioJruW2L9YzE0npOoEKdGnOkI/edit?usp=sharing)
  * [User guide](https://docs.google.com/document/d/1esbAWgfIEcT-F2C35vKkdVa4NeGzSh9MV8dDGH1OCFI/edit?usp=sharing)

## Updates
  * All documents are updated on `2021-11-18`
  * All key tables are automatically updated every 1st day of the month. 
    * In the process in Nov 2021, some duplicates (mostly Chinese vessels) were merged together. It was due to some minor discrepancies that were not automatically detected/merged. For instance, two rows that have the same name but different call signs such as `B4X80` and `B4X8O`, or two rows that have the same identity except different suffix numbers (which is important to separate vessels) such as `LURONGYUANYU107` and `LURONGYUANYU1O7`. These cases are manually corrected and merged.