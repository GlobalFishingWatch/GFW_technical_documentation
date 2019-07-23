GFW data is stored as BigQuery tables and organized in a handful of key datasets. These datasets contain the automated output of GFW's data pipeline (`pipe_production_vYYYYMMDD`), vessel identity information pulled from vessel registries (`vessel_database`), and streamlined versions of these tables and other derived data products relevant for research and analysis (`gfw_research`).    

### `pipe_production_vYYYYMMDD`

+ **Description:** This dataset includes AIS tables processed by the GFW fishing algorithms and derived event tables (e.g. encounters, loitering, port visits). All tables in `pipe_production_vYYYYMMDD` are automated and **should not be modified manually.**  

+ **When to use:**  

### `vessel_database`

### `gfw_research`

+ **Description:**  This dataset includes streamlined versions of the pipeline AIS tables in `pipe_production_vYYYYMMDD` as well as an additional AIS table subset to only include fishing vessels; vessel info tables that combine identity information from `vessel_database` with AIS activity information from `pipe_production_vYYYYMMDD`; derived data products (encounters, loitering, ports, etc.) that are not yet fully automated; and various reference tables important for analysis (e.g. EEZ info, flags of convenience, country codes, etc.). The versions of the pipeline AIS tables in `gfw_research` have been thinned (only 1 point per minute) and further processed to include additional info relevant for analysis (e.g. hours and regions). However, these tables are not currently automated by the pipeline and, as a result, may be a few days behind the tables in `pipe_production_vYYYYMMDD`. 

+ **When to use:** This dataset is the primary dataset to use for research and analysis. The streamlined AIS tables are smaller and cheaper to query than those in `pipe_production_vYYYYMMDD` and the vessel info tables include important activity data not available in `vessel_database`



