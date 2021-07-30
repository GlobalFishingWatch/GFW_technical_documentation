# GFW BigQuery Documentation

This wiki includes the primary documentation of Global Fishing Watch's data and infrastructure. It outlines how data are processed, where data are located, and the key data products (e.g. fishing effort, encounters, ports, etc.) and how/when to use the associated BigQuery tables.

GFW uses data from various sources, including AIS, VMS, vessel registries, SAR, and VIIRS. These data are processed by GFW's automated [pipelines](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline). These raw and processed data are stored as tables and organized in a handful of [key BigQuery datasets](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/BigQuery-datasets). 

Check the [Data updates](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Data-updates) for new changes to GFW data

## Data Products

GFW data are grouped into the following general categories, or data products, that are used for various analyses.

See [BigQuery Table Reference](https://docs.google.com/spreadsheets/d/1B8Q04rzWRdffty2gVBlHiBDS9DYZLdW8J-4B3xja5ws/edit#gid=0) for checking the latest pipeline version.

1. [Fishing effort](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-Effort) - Activity by the world's fishing vessels
2. [Vessel database and info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-database-and-info-tables) - Database of vessel identity
3. [Encounters & loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters-and-loitering) - Encounters between two vessels and loitering events by potential transshipment vessels
4. [Ports & voyages](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) - Database of ports and vessel voyages between ports
5. [Country VMS]() - Country-specific VMS data
6. [SAR Detections](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/SAR-Detections) - Vessel detections from Sentinel-1 SAR imagery
7. [AIS Gaps](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Gaps) - Dataset of all 6+ hour gaps in AIS signal by MMSI for all vessels.
