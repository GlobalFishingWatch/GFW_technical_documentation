## High Seas Pockets

Owner: Hannah Linder
Last edited time: 11 de febrero de 2025 19:16
Created time: 1 de marzo de 2024 13:14


World High Seas (Areas outside EEZs), Including only the small pockets of isolated, non-contiguous high seas area

### Key Attributes

| **Layer name** | hsp |
| --- | --- |
| **Unique id field** | name |
| **Short description** | High Seas Pockets |
| **Source** | Exceprted from [https://www.marineregions.org/downloads.php](https://www.marineregions.org/downloads.php) |
| **Citation** | Flanders Marine Institute (2024). Maritime Boundaries Geodatabase: High Seas, version 2. Available online at [https://www.marineregions.org/](url). [https://doi.org/10.14284/696](url). |
| **Attribution** | Copyright Flanders Marine Institute - [http://marineregions.org](http://marineregions.org/) |
| **Last updated** | 2024-10-10 (marine regions), 2022-12-16 (GFW) |
| **License** | [CC-BY](https://creativecommons.org/licenses/by/4.0/) |

### Description

The regions of the high seas excluding the major contiguous area of high seas and the arctic ocean.

![](High%20Seas%20Pockets%20245bd906a21842a3af217ae7acca17ef/image1.png)

This data set is derived from the Marine Regions high-seas data set by extracting just the polygons corresponding to the 15 high seas pockets that GFW has labeled from HSP-1 to HSP-15. Note that HSP-15 is split across the anti-meridian, so it appears in two pieces that have the same label. Naming of the pockets is consistent with the naming that WCPFC uses to identify HSP 1-4. The rest of the pockets are enumerated in arbitrary order by GFW.

The script used to create the layer is here: [[https://github.com/GlobalFishingWatch/pipe-regions/blob/PIPELINE-1049/layer-preprocessing/high-seas-pockets.sh](https://github.com/GlobalFishingWatch/pipe-regions/blob/PIPELINE-1049/layer-preprocessing/high-seas-pockets.sh)]([https://github.com/GlobalFishingWatch/pipe-regions/blob/develop/layer-preprocessing/high-seas-pockets.sh](url))

### Data access

**GCS**

- Original: gs://pipe-regions-layers-us-central1/high_seas/original/World_High_Seas_v2_20241010.zip
- GFW Labels: gs://pipe-regions-layers-us-central1/hsp/original/hsp_centroids.shp.zip
- Normalized: gs://pipe-regions-layers-us-central1/hsp/high_seas_pockets_v20241010.shp.zip

**Bigquery**

- Normalized: world-fishing-827.pipe_regions_layers.high_seas_pockets_v20241010
- Processed for S2 lookup:: world-fishing-827.pipe_regions_layers.high_seas_pockets_v20241010_S2_10

## Old Versions

### Data access

**GCS**

- Original: gs://pipe-regions-layers-us-central1/high_seas/original/World_High_Seas_v1_20200826.zip
- GFW Labels: gs://pipe-regions-layers-us-central1/hsp/original/hsp_centroids.shp.zip
- Normalized: gs://pipe-regions-layers-us-central1/hsp/high_seas_pockets_v20221216.shp.zip

**Bigquery**

- Normalized: world-fishing-827.pipe_regions_layers.high_seas_pockets_v20221216
- Processed for S2 lookup:: world-fishing-827.pipe_regions_layers.high_seas_pockets_v20221216_s2_10
