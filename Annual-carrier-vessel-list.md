# Annual carrier vessel list 

The **annual carrier vessel list** table contains likely carrier vessels that are active in AIS and matched to one or more registries by year. The information is pulled from the [vessel database](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-database) and includes only the following fields (year, MMSI number, and carrier classification) that helps identify carrier vessels in other tables including pipeline tables, vessel info table, encounter/loitering tables among others. It is mainly used for the Carrier Vessel Portal to have the base list of carriers for further data process. The carrier MMSI is presented for each year that it was AIS-active. Currently, no filters have been applied to remove noisy MMSIs. As a consequence, it may include some MMSIs that are used by multiple (possibly non-carrier) vessels, which should be treated according to the user's objective. For the further information about the query that creates the table, go to the [link](https://github.com/GlobalFishingWatch/vessel-identity/blob/master/vessel-database/push_staging_data/carrier_vessels_byyear.sql.j2). 

## Key Tables
+ `vessel_database.carrier_vessels_byyear_vYYYYMMDD` - This table contains the list of annual carrier vessels that are active in AIS and matched to vessel registries, extracted from the vessel database. The table includes `year`, `mmsi`, `flag`, and `vessel class`, and is automatically created on the 1st day of the month with the up-to-date information. The creation of the table has a lag time of one month (e.g. `vessel_database.carrier_vessels_byyear_v20211101` is created on the Dec 1, 2021, and includes the carrier list using data up to `2020-10-31 23:59:59`. 

## Data Description
The annual carrier vessel list extracts the information about likely carrier vessels by year from the vessel database (only AIS-registry matched vessels), therefore all likely carrier vessels (based on `is_carrier` fields in the vessel database) for each year. It includes `year`, `mmsi`, `flag`, and `vessel class` in the table. Some carrier vessels may appear in multiple years. 

* `year`: Year in which the given MMSI number is likely associated with a carrier vessel
* `mmsi`: MMSI number likely assigned to a carrier vessel at the given year
* `flag`: Flag of a vessel represented by ISO-3 country code
* `vessel_class`: It represents the classification of a carrier vessels, which is one of `reefer`, `specialized_reefer`, `container_reefer`, `fish_factory`, `well_boat`, and `fish_tender`

## Caveats & Known Issues
The database may contain information that is out of date or incorrect due to wrong information available in one or multiple registries on which the database relies, or a carrier vessel misuses AIS. The MMSI numbers are not cleaned up so there may be noisy MMSIs that are shared by multiple vessels (not necessarily only by carriers) in the dataset. Users are expected to apply appropriate filters to remove noisy MMSIs according to their goals.
In case of any questions or suggestions, please contact jaeyoon@globalfishingwatch.org