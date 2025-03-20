# Spatial Region Sources

Owner: Hannah Linder
Last edited time: 20 de febrero de 2025 11:49
Created time: 28 de febrero de 2024 15:14

# Raster information

Raster Information can be found in `pipe_static.spatial_measures_clustered_20230307`

# Regions

Regions are polygon layers that are used in the later stages of the pipeline to determine whether a given position (such as the location of a fishing event) is inside or outside the individual polygons in the layer. Example layers are WDPA marine protected areas, EEZs, RFMO regions, etc.

## Description of Region Layers

Each set of region layers comes from a different source and is composed of a set of polygons that define a number of regions within that layer. Each layer has a unique ID field that identifies the real-world feature which may be represented by multiple polygons. For instance, the original EEZ layer has 323 polygon features which represent 281 distinct EEZs. See the child pages for details on where the layers come from, attribution and citation requirements and how to access the data.

### Available Regions Layers

**Pipeline bucket: pipe_regions_layers**

- [12 NM Boundary](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/12%20NM%20Boundary%2058fd490280ff4a20a8442d978eb40bb0.md)
- [EEZ Boundaries](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/EEZ%20Boundaries%20c9d7f29231374691b9647a4120231be5.md)
- [FAO Areas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/FAO%20Areas%20d88199bc7cf048e497ca9450225370b2.md)
- [High Seas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/High%20Seas%204cfe42cc9f6f4dc1954613c1534fe737.md)
- [High Seas Pockets](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/High%20Seas%20Pockets%20245bd906a21842a3af217ae7acca17ef.md)
- [Oceans and Seas](https://www.notion.so/Ocean-and-Seas-33055608a7e94b6587354537d73f06fd?pvs=21)
- [Protected Seas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/Protected%20Seas%2029ecb337972e4c91a65edcb7bd0fdfe9.md)
- [RFMOs](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/RFMOs%204516ced0f92b41a29da446c9593a1dfc.md)
- [WDPA Marine MPAs](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/WDPA%20Marine%20MPAs%20d03a66feed5649e4b15faa5c3f28a457.md)
- [HOWTO: Do point in polygon test in bigquery using regions](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/HOW%20TO%20-%20Do%20point-in-polygon%20test%20in%20BigQuery%20(BQ)%203ce81b2b246047719a9effdbd4bee72e.md)
- [HOWTO: Update a layer in pipe_regions](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/HOW%20TO%20-%20Update%20layer%20in%20pipe_regions%20bfcbdf4501ba415686253c04d8f0600b.md)

## Data Access

All these regions are managed via the pipe-regions tool. Regions are available in GCS and Bigquery with all attributes from the source shapefiles.

Each layer is normalized into a single zipped shapefile which is stored in GCS here **gs://pipe-regions-layers-us-central1/**

Layers are then converted to geojson and loaded into bigquery here: **world-fishing-827.pipe_regions_layers**

Layers are also processed to optimize for point in polygon testing using S2 cells in bigquery. The optimized tables are in **world-fishing-827.pipe_regions_layers** in tables with suffix **_S2_10** which indicates it is optimized for S2 cells at level 10.

## Background

Previously regions were included in the [Spatial Measures](file:////wiki/spaces/TD/pages/7405581/Spatial+Measures) pipeline stage, along with other scalar measures like bathymetry depth and distance from shore. There were some problems with the old approach that have been fixed with the new implementation in pipe-regions.

- In Spatial Measures regions were gridded to 1km resolution, so positions near the boundary were sometimes mis-classified as being inside or outside. This gridded approach was a compromise made for performance reasons. With the new pipe-regions implementation, every lat/lon position is tested against the full resolution polygon using ST_INTERSECTS, so it is very accurate for positions near the polygon boundary
- The Spatial Measures step comes early in the pipeline, so this meant that updating the polygons used for computing regions was expensive since all downstream pipeline stages would need to be re-run. The WDPA is updated monthly, and we would like for these monthly updates to be automatically included in the pipeline. The new pipe-regions implementation comes later in the pipeline, and has enhanced support for automated updates of polygon layers, so it is easier to make changes and keep the pipeline output up to date
- Mismatch between compute and display - the polygons that are displayed on the map are the true polygons, and this means that with the Spatial Measures approach, there was a mismatch between what was computed and what was displayed.

## Legacy Regions

List of regions that were present in the old spatial_measures table. Need to figure out what to do with these.

Reference docs

- [https://docs.google.com/document/d/1DdzRGYGMHfomxzetel_eNeaAmgl7sX7AD7N12lcvW_M/edit](https://docs.google.com/document/d/1DdzRGYGMHfomxzetel_eNeaAmgl7sX7AD7N12lcvW_M/edit)
- [https://drive.google.com/drive/u/0/folders/1-udRAn3Tj_OLX8r79KNi7qUtQAYo8pfJ](https://drive.google.com/drive/u/0/folders/1-udRAn3Tj_OLX8r79KNi7qUtQAYo8pfJ)

| **legacy layer id from Spatial Mesaures** | **New Regions S2 layer** |
| --- | --- |
| mregion | Not sure what this is. there are 312 unique id values |
| arg | Argentina? |
| eez | [EEZ Boundaries](file:////wiki/spaces/TD/pages/459014157/EEZ+Boundaries) |
| mparu | I think this is a subset of the WDPA MPA layer - restricted use? |
| vme | ??? |
| kkp | Indonesia |
| ocean | oceans from marine regions. Looks like this is just the 5 main oceans, but marine regions has some more detailed layers - World Seas IHO with 101 features and GOAS with 10 features. Does anyone even use this? |
| hsp | high seas pockets (marine regions?). I think this was a custom layer we created. |
| other | does not seem to be used |
| fao | [FAO Areas](file:////wiki/spaces/TD/pages/460292097/FAO+Areas) |
| mpant | I think this is a subset of the WDPA mpa layer. MPA No Take. |
| ames | ??? 207 values - ARC_1..11, ETTP_1..21, MED 1..17, NP_1..20, NWA_1..7, SEA_1..45, SIO_1..39, and so on… |
| rfmo | [RFMOs](file:////wiki/spaces/TD/pages/459472909/RFMOs) |
| major_fao | [FAO Areas](file:////wiki/spaces/TD/pages/460292097/FAO+Areas) |
|  | [WDPA Marine MPAs](file:////wiki/spaces/TD/pages/459112468/WDPA+Marine+MPAs) |

Checking to see what RFMOs are being used.

SELECT

```jsx
SELECT
distinct rfmo

FROM `world-fishing-827.pipe_static.spatial_measures_20201105`

CROSS JOIN UNNEST(regions.rfmo) as rfmo

LIMIT 1000
```

This gives a list of 42 RFMO labels. All of these have been incorporated into the new RFMO layer [RFMOs](file:////wiki/spaces/TD/pages/459472909/RFMOs)

**Legacy RFMO identifier**

---

ACAP

---

APFIC

---

BOBP-IGO

---

CACFISH

---

CCAMLR

---

CCBSP

---

CCSBT

---

COREP

---

CPPS

---

CRFM

---

CTMFM

---

EIFAAC

---

FCWC

---

FFA

---

GFCM

---

IATTC

---

ICCAT

---

ICES

---

IOTC

---

IPHC

---

IWC

---

LTA

---

LVFO

---

MRC

---

NACA

---

NAFO

---

NAMMCO

---

NASCO

---

NEAFC

---

NPAFC

---

NPFC

---

OSPESCA

---

PERSGA

---

PICES

---

RECOFI

---

SEAFDEC

---

SIOFA

---

SPC

---

SPRFMO

---

SRFC

---

SWIOFC

---

WCPFC

---

Design Issues

[https://github.com/GlobalFishingWatch/pipe-regions/blob/master/DESIGN.md](https://github.com/GlobalFishingWatch/pipe-regions/blob/master/DESIGN.md)

[EEZ Boundaries](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/EEZ%20Boundaries%20c9d7f29231374691b9647a4120231be5.md)

[WDPA Marine MPAs](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/WDPA%20Marine%20MPAs%20d03a66feed5649e4b15faa5c3f28a457.md)

[RFMOs](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/RFMOs%204516ced0f92b41a29da446c9593a1dfc.md)

[12 NM Boundary](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/12%20NM%20Boundary%2058fd490280ff4a20a8442d978eb40bb0.md)

[Protected Seas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/Protected%20Seas%2029ecb337972e4c91a65edcb7bd0fdfe9.md)

[FAO Areas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/FAO%20Areas%20d88199bc7cf048e497ca9450225370b2.md)

[High Seas](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/High%20Seas%204cfe42cc9f6f4dc1954613c1534fe737.md)

[                                                                                  High Seas Pockets](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/High%20Seas%20Pockets%20245bd906a21842a3af217ae7acca17ef.md)

[HOW TO - Update layer in pipe_regions](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/HOW%20TO%20-%20Update%20layer%20in%20pipe_regions%20bfcbdf4501ba415686253c04d8f0600b.md)

[HOW TO - Do point-in-polygon test in BigQuery (BQ) using regions](Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e/HOW%20TO%20-%20Do%20point-in-polygon%20test%20in%20BigQuery%20(BQ)%203ce81b2b246047719a9effdbd4bee72e.md)