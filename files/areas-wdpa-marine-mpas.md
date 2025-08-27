---
date-modified: last-modified
---

<!--Owner: Hannah Linder
Last edited time: 21 de mayo de 2024 20:57
Created time: 1 de marzo de 2024 13:14-->

# WDPA Marine MPAs

## Key Attributes

| **Layer name** | mpa, mpa_no_take, mpa_no_take_partial (see below for filters used) |
| --- | --- |
| **Unique id field** | WDPA_PID |
| **Short description** | Marine protected areas from The World Database on Protected Areas (WDPA) via protectedplanet.net |
| **Source** | [https://www.protectedplanet.net/en/search-areas?filters[db_type][]=wdpa&filters[is_type][]=marine](https://www.protectedplanet.net/en/search-areas?filters%5Bdb_type%5D%5B%5D=wdpa&filters%5Bis_type%5D%5B%5D=marine) |
| **Citation** | UNEP-WCMC and IUCN (2022), Protected Planet: The World Database on Protected Areas (WDPA) and World Database on Other Effective Area-based Conservation Measures (WD-OECM) [Online], August 2022, Cambridge, UK: UNEP-WCMC and IUCN. Available at: [www.protectedplanet.net](http://protectedplanet.net/). |
| **Attribution** | You must ensure that one of the following citations is always clearly reproduced in any publication or analysis involving the Protected Planet Materials in any derived form or format: Use this citation for any downloads of Protected Planet Materials from [ProtectedPlanet.net](http://protectedplanet.net/):
**UNEP-WCMC and IUCN (year), Protected Planet: The World Database on Protected Areas (WDPA) [On-line], [insert month/year of the version downloaded], Cambridge, UK: UNEP-WCMC and IUCN. Available at: [www.protectedplanet.net](http://www.protectedplanet.net/).** |
| **Last updated** | 2020-11-01 (Updated monthly) |
| **License** | Non-commecrial use [https://www.protectedplanet.net/en/legal](https://www.protectedplanet.net/en/legal) |

## Description

The World Database on Protected Areas (WDPA) is the most comprehensive global database of marine and terrestrial protected areas. It is a joint project between UN Environment Programme and the International Union for Conservation of Nature (IUCN), and is managed by UN Environment Programme World Conservation Monitoring Centre (UNEP-WCMC), in collaboration with governments, non-governmental organizations, academia and industry. The WDPA is updated on a monthly basis and can be downloaded using the button in the top right of this page.

We use just the marine protected areas, which is a subset of the full WDPA.

- Formatting is different than the previous MPA source we used (previous to 2022 pipeline updates). Many more columns. One column that is ‘NO_TAKE’ with different categories (‘All’, ‘Part’, ‘Not Reported’, ‘Not Applicable’, ‘None’). Should make sure to capture this information and as many of the other fields as we can.
- The source is either .csv, or .shp. Due to the size of the data the .shp folder breaks it down into three layers that need to be merged to get full resolution. See ‘Shapefile_splitting_README.txt’ in folder. There is a utility in pipe_regions that will download and merge the multiple shapefiles.

## No-Take MPAs

in addition to the full set of MPAs, there are two filtered subsets published as separate layers.

| **Layer ID** | **Filter** |
| --- | --- |
| mpa_no_take | NO_TK_AREA > 0 AND NO_TAKE = 'All' |
| mpa_no_take_partial | NO_TK_AREA > 0 |

## Data access

**GCS**

- Original: gs://pipe-regions-layers-us-central1/mpa/original/WDPA_WDOECM_Aug2022_Public_marine_shp.zip
- Normalized: gs://pipe-regions-layers-us-central1/wdpa/WDPA_WDOECM_Aug2022_Public_marine_shp.shp.zip
- No Take: gs://pipe-regions-layers-us-central1/wdpa/NO_TAKE_WDPA_WDOECM_Aug2022_Public_marine_shp.shp.zip
- No Take Partial: gs://pipe-regions-layers-us-central1/wdpa/NO_TAKE_PARTIAL_WDPA_WDOECM_Aug2022_Public_marine_shp.shp.zip

**Bigquery**

- Normalized:
    - world-fishing-827.pipe_regions_layers.WDPA_WDOECM_Nov2022_Public_marine_shp
    - world-fishing-827.pipe_regions_layers.NO_TAKE_PARTIAL_WDPA_WDOECM_Nov2022_Public_marine_shp
    - world-fishing-827.pipe_regions_layers.NO_TAKE_WDPA_WDOECM_Nov2022_Public_marine_shp
- Processed for S2 lookup::
    - world-fishing-827.pipe_regions_layers.WDPA_WDOECM_Nov2022_Public_marine_shp_s2_10
    - world-fishing-827.pipe_regions_layers.NO_TAKE_PARTIAL_WDPA_WDOECM_Nov2022_Public_marine_shp_s2_10
    - world-fishing-827.pipe_regions_layers.NO_TAKE_WDPA_WDOECM_Nov2022_Public_marine_shp_s2_10
