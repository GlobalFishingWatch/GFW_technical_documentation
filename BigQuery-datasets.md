GFW data is stored as BigQuery tables and organized in a handful of key datasets. These datasets contain the automated output of GFW's data pipeline (`pipe_production_vYYYYMMDD`), vessel identity information pulled from vessel registries (`vessel_database`), and streamlined versions of these tables and other derived data products relevant for research and analysis (`gfw_research`).    

## Key Datasets

### `pipe_production_vYYYYMMDD`

+ **Description:** This is the dataset for the AIS pipeline and includes tables processed by the GFW fishing algorithms and derived event tables (e.g. encounters, loitering, port visits). All tables in `pipe_production_vYYYYMMDD` are automated by the pipeline and **should not be modified manually.**  

+ **When to use:** The tables in the pipeline dataset contain the most up-to-date event data (encounters, port visits, port visits, etc.) and should also be used whenever access to the full AIS data is desired (e.g. every AIS position). 

### `vessel_database`

+ **Description:** The Global Fishing Watch vessel database contains identity and authorization information about vessels that is/was recorded in various formats of vessel registries, including RFMO vessel lists and national vessel registers that are publicly available. It also includes lists of vessels that have been obtained through groups of researchers and analyzed by analysts from GFW and its partners.

+ **When to use:** The vessel database should be used when searching for identity information about an MMSI, particularly if interested in when/if/where an MMSI is authorized to fish.

### `gfw_research`

+ **Description:**  This dataset includes streamlined versions of the AIS tables in `pipe_production_vYYYYMMDD`, as well as an additional AIS table subset to only include fishing vessels; vessel info tables that combine identity information from `vessel_database` with AIS activity information from `pipe_production_vYYYYMMDD`; derived data products (encounters, loitering, ports, etc.) that are not yet fully automated; and various reference tables important for analysis (e.g. EEZ info, flags of convenience, country codes, etc.). The versions of the pipeline AIS tables in `gfw_research` have been thinned (only 1 point per minute) and further processed to include additional info relevant for analysis (e.g. hours and regions). 

+ **When to use:** This dataset is the primary dataset to use for research and analysis. The streamlined AIS tables are smaller and cheaper to query than those in `pipe_production_vYYYYMMDD` and the vessel info tables include important activity data not available in `vessel_database`

## Other Relevant Datasets

### `anchorages`