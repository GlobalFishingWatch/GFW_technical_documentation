# GFW BigQuery Documentation

This wiki includes the primary documentation of Global Fishing Watch's data and infrastructure. It outlines how data are processed, where data are located, and the key data products (e.g. fishing effort, encounters, ports, etc.) and how/when to use the associated BigQuery tables.

GFW uses data from various sources, including AIS, VMS, vessel registries, SAR, and VIIRS. These data are processed by GFW's automated [pipelines](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline). These raw and processed data are stored as tables and organized in a handful of [key BigQuery datasets](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/BigQuery-datasets). 

## Data Updates
Check the [Data updates](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Data-updates) for new changes to GFW data.

See the [BigQuery Table Reference](https://docs.google.com/spreadsheets/d/1B8Q04rzWRdffty2gVBlHiBDS9DYZLdW8J-4B3xja5ws/edit#gid=0) for checking the latest version of key tables to use.

## Key Concepts
Read the [Key concepts page](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Key-concepts) for important things to understand when working with GFW data in BigQuery, including BigQuery best practices.

## Data Products

GFW data are grouped into the following general categories, or data products, that are used for various analyses.

1. [Fishing effort and vessel presences](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-effort-and-vessel-presence) - Activity and fishing activity for all vessels
2. [Fishing events](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-events) - Activity by the world's fishing vessels grouped into events
3. [Vessel database](Vessel-database) and [Vessel info tables](Vessel-info-tables) - Database of vessel identity
4. [Encounters](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters) - Encounters between two vessels
5. [Loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Loitering) - Loitering events by potential transshipment vessels
6. [Ports & voyages](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) - Database of ports and vessel voyages between ports
7. [Country VMS](VMS) - Country-specific VMS data
8. [SAR Detections](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/SAR-Detections) - Vessel detections from Sentinel-1 SAR imagery
9. [AIS Gaps](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Gaps) - Dataset of all 6+ hour gaps in AIS signal by MMSI for all vessels.
