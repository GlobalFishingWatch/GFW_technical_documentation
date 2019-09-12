# GFW BigQuery Documentation

This wiki includes the primary documentation of Global Fishing Watch's data and infrastructure. It outlines how data are processed, where data are located, and the key data products (e.g. fishing effort, encounters, ports, etc.) and how/when to use the associated BigQuery tables.

GFW uses data from various sources, including AIS, VMS, vessel registries, SAR, and VIIRS. These data are processed by GFW's automated [pipelines](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Pipeline). These raw and processed data are stored as tables and organized in a handful of [key BigQuery datasets](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/BigQuery-datasets). 

## Data Products

GFW data are grouped into the following general categories, or data products, that are used for various analyses.

1. [Fishing effort](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-Effort) - Activity by the world's fishing vessels
2. [Vessel database and info tables](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Vessel-database-and-info-tables) - Database of vessel identity
3. [Encounters & loitering](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Encounters-and-loitering) - Encounters between two vessels and loitering events by potential transshipment vessels
4. [Ports & voyages](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Ports-and-voyages) - Database of ports and vessel voyages between ports
5. [Country VMS]() - Country-specific VMS data
6. [SAR Detections](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/SAR-Detections) - Vessel detections from Sentinel-1 SAR imagery
