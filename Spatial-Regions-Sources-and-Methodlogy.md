This page contains the list and location of the most current BigQuery datasets and tables for the various GFW data products. The BQ tables in the below table are the default tables to be used when starting a new analysis. 

Last update:
   * Date: `2023-06-26`
   * By: `Hannah Linder`
   * Note on update cycle: All layers in pipe_regions are updated on an annual basis EXCEPT for Marine protected areas from The World Database 
     on Protected Areas (WDPA) via protectedplanet.net. These layers are updated on a monthly basis. 

## Data Description

Regional source layers are retrieved from publicly available sources for key boundaries and areas of interest in the world's oceans. These layers are converted to geojson and loaded into bigquery here: `world-fishing-827.pipe_regions_layers`

Layers are also processed to optimize for point in polygon testing using S2 cells in bigquery.  The optimized tables are in world-fishing-827.pipe_regions_layers in tables with suffix  _S2_10 which indicates it is optimized for S2 cells at level 10 (roughly 7-10km grid size).

Detailed information on all layers is found and updated here: https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/459014145/Regions

A simplified list of sources is provided below:

| Region Layer | Description | Source | Detailed Attributes | Modifications? |
| --- | --- | --- | --- | --- | 
| EEZ | AIS pipeline dataset to use | `[Marine Regions](https://www.marineregions.org/) - Marine and land zones: the union of world country boundaries and EEZ's version 3 2020-03-17. | https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/459014157/EEZ+Boundaries | Yes |
