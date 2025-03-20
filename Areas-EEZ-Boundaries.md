## EEZ Boundaries

Owner: Hannah Linder
Last edited time: 1 de marzo de 2024 13:30
Created time: 1 de marzo de 2024 13:13


### Key Attributes

| **Layer name** | eez |
| --- | --- |
| **Unique id field** | MRGID_EEZ |
| **Short description** | Maritime exclusive economic zones (200 nautical mile boundary) for all coastal nations. |
| **Source** | [https://www.marineregions.org/downloads.php](https://www.marineregions.org/downloads.php) |
| **Citation** | Flanders Marine Institute (2019). Maritime Boundaries Geodatabase: Maritime Boundaries and Exclusive Economic Zones (200NM), version 11. Available online at [https://www.marineregions.org/](https://www.marineregions.org/). [https://doi.org/10.14284/386](https://doi.org/10.14284/386) |
| **Attribution** | Copyright Flanders Marine Institute (2020) - marineregions.org. |
| **Last updated** | 2020-03-17 |
| **License** | [CC-BY](https://creativecommons.org/licenses/by/4.0/) |

### Description

We use EEZ boundaries that are freely available from [Marine Regions](https://www.marineregions.org/)

Note that we use the product **Marine and land zones: the union of world country boundaries and EEZ's** version 3 2020-03-17. This dataset combines the boundaries of the world countries and the Exclusive Economic Zones of the world. It was created by combining the ESRI world country database and the EEZ V11 dataset. [[Known Issues]](https://www.marineregions.org/files/EEZ_land_union_v3_known_issues.txt)

### Modifications to Original

The original file from Marine Regions contains all countries, even those that have no coast, and therefore no EEZ. It also contains the entire area of each country. This creates some problems for us because EEZ-based activity picks up in-shore activity in rivers, and this is confusing when we expect things to be limited to just the EEZ marine area. So what we do is clip the final polygons in the layer to include only a 0.1 degree (about 10km) buffer in from the coastline for each country. Countries with no coastline are filtered out. We include the buffer because if we only include the EEZ polygon, the lack of precision at the coastline causes some activity that is in the marine area to be excluded.

![](EEZ%20Boundaries%20c9d7f29231374691b9647a4120231be5/image1.png)

The buffering is done using a land mask from the [Natural Earth 10m Land](https://www.naturalearthdata.com/downloads/10m-physical-vectors/10m-land/) data set. The file used for this is in GCS and the script that does the buffering, clipping and filtering is in github [https://github.com/GlobalFishingWatch/pipe-regions/blob/master/layer-preprocessing/eez-land-buffered.sh](https://github.com/GlobalFishingWatch/pipe-regions/blob/master/layer-preprocessing/eez-land-buffered.sh)

### Data access

**GCS**

- Original: gs://pipe-regions-layers-us-central1/eez/original/EEZ_land_union_v3_202003.zip
- Land Mask: gs://pipe-regions-layers-us-central1/eez/original/ne_10m_land.zip
- Normalized, buffered and clipped: gs://pipe-regions-layers-us-central1/eez/EEZ_land_union_v3_202003_buffered.shp.zip

**Bigquery**

- Normalized: world-fishing-827.pipe_regions_layers.EEZ_land_union_v3_202003_buffered
- Processed for S2 lookup:: world-fishing-827.pipe_regions_layers.EEZ_land_union_v3_202003_buffered_marine_s2_10
