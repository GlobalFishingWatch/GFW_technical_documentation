
::: {.callout-warning}

**WORK IN PROGRESS**. Information on this book is not up to date. We are currently working on improving GFW documentation.

:::

<br>

# Welcome! {-}

This book includes the primary documentation of Global Fishing Watch's data and infrastructure. It outlines how data are processed, where data are located, and the key data products (e.g. fishing effort, encounters, ports, etc.) and how/when to use the associated BigQuery tables.

GFW uses data from various sources, including AIS, VMS, vessel registries, SAR, and VIIRS. These data are processed by GFW's automated [pipelines](files/pipeline.md). These raw and processed data are stored as tables and organized in a handful of [key BigQuery datasets](files/bigquery-datasets.md). 

## Data Updates
Check the [Data updates](files/data-updates.md) for new changes to GFW data.

See the [BigQuery Table Reference](files/bigquery-pipe3-table-reference.md) for checking the latest version of key tables to use.

## BigQuery Key Concepts
Read the [BigQuery Key Concepts page](files/bigquery-key-concepts.md) for important things to understand when working with GFW data in BigQuery, including BigQuery best practices.

## Data Products

GFW data are grouped into the following general categories, or data products, that are used for various analyses.

1. [Fishing effort and vessel presences](files/fishing-effort-and-vessel-presence.md) - Activity and fishing activity for all vessels
2. [Fishing events](files/fishing-events.md) - Activity by the world's fishing vessels grouped into events
3. [Vessel database](files/vessel-database.md) and [Vessel info tables](files/vessel-info-tables.md) - Database of vessel identity
4. [Encounters](files/encounters.md) - Encounters between two vessels
5. [Loitering](files/loitering.md) - Loitering events by potential transshipment vessels
6. [Anchorages & voyages](files/anchorages-voyages-port-visits.md) - Database of ports and vessel voyages between ports
7. [Country VMS](files/vms-data-sources.md) - Country-specific VMS data
8. [SAR Detections](files/sar-object-detections.md) - Vessel detection from Sentinel-1 SAR imagery
9. [AIS Gaps](files/gaps.md) - Dataset of all 6+ hour gaps in AIS signal by MMSI for all vessels.





