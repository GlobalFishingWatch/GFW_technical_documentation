## FAO Areas

Owner: Hannah Linder
Last edited time: 1 de marzo de 2024 13:39
Created time: 1 de marzo de 2024 13:08


FAO Statistical Areas

### Key Attributes

| **Layer names** | fao, major_fao |
| --- | --- |
| **Unique id field** | ID |
| **Short description** | FAO Statistical Areas |
| **Source** | [https://data.apps.fao.org/map/catalog/srv/eng/catalog.search#/metadata/ac02a460-da52-11dc-9d70-0017f293bd28](https://data.apps.fao.org/map/catalog/srv/eng/catalog.search#/metadata/ac02a460-da52-11dc-9d70-0017f293bd28) |
| **Citation** | (c) FAO, 2020. FAO Statistical Areas for Fishery Purposes. In: FAO Fisheries and Aquaculture Department [online]. Rome. [Cited <DATE>] [http://www.fao.org/fishery/area/search/en](http://www.fao.org/fishery/area/search/en) |
| **Attribution** | (c) FAO, 2020 |
| **Last updated** | Manual update 2022-01-19, original source revision date 2014-04-17 |
| **License** | Public |

### Description

![](FAO%20Areas%20d88199bc7cf048e497ca9450225370b2/image1.png)

FAO Major Fishing Areas for Statistical Purposes are arbitrary areas, the boundaries of which were determined in consultation with international fishery agencies on various considerations, including (i) the boundary of natural regions and the natural divisions of oceans and seas; (ii) the boundaries of adjacent statistical fisheries bodies already established in inter-governmental conventions and treaties; (iii) existing national practices; (iv) national boundaries; (v) the longitude and latitude grid system; (vi) the distribution of the aquatic fauna; and (vii) the distribution of the resources and the environmental conditions within an area.

NB: These two layers were manually tweaked by Pete. See [PIPELINE-558](https://globalfishingwatch.atlassian.net/browse/PIPELINE-558) - Update FAO regions in the next pipeline run Done

Original Source: [https://data.apps.fao.org/map/catalog/srv/eng/catalog.search#/metadata/ac02a460-da52-11dc-9d70-0017f293bd28](https://data.apps.fao.org/map/catalog/srv/eng/catalog.search#/metadata/ac02a460-da52-11dc-9d70-0017f293bd28)

Normalized shapefile produced from original (manually edited) geojson like this:

ogr2ogr -f "ESRI Shapefile" -lco ENCODING=UTF-8 \

FAO_MAJOR_AREAS_NOCOASTLINE.shp.zip \

FAO_MAJOR_AREAS_NOCOASTLINE.geojson

### FAO Areas

This is the full data set

**Layer Name:** fao

**Original**: gs://pipe-regions-layers-us-central1/fao_areas/original/FAO_AREAS_NOCOASTLINE.geojson

**Normalized**: gs://pipe-regions-layers-us-central1/fao_areas/FAO_AREAS_NOCOASTLINE.shp.zip

**Bigquery Normalized**:

**Bigquery S2 Optimized:**

### FAO Major Areas

This is just the 19 major areas. All the other areas are subdivisions of these 19

**Layer Name:** major_fao

**Original**: gs://pipe-regions-layers-us-central1/fao_major_areas/original/FAO_MAJOR_AREAS_NOCOASTLINE.geojson

**Normalized**: gs://pipe-regions-layers-us-central1/fao_major_areas/FAO_MAJOR_AREAS_NOCOASTLINE.shp.zip

**Bigquery Normalized**:

**Bigquery S2 Optimized:**
