---
date-modified: last-modified
---

# RFMOs

Owner: Hannah Linder
Last edited time: 11 de febrero de 2025 12:35
Created time: 1 de marzo de 2024 13:14


Regional Fisheries Management Organizations (RFMOs)

## Key Attributes

| **Layer name** | rfmo |
| --- | --- |
| **Unique ID Field** | RFB |
| **Short description** | Regional Fisheries Management Organizations (RFMOs) |
| **Source** | [https://geonetwork.d4science.org/geonetwork/srv/en/main.home](https://geonetwork.d4science.org/geonetwork/srv/en/main.home)
For individual RFMOs, use this URL template
[https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-iattc)-[rfmo-id] |
| **Citation** | © FAO, 2019. FAO Regional Fishery Bodies. [RFMO Name]. In: FAO Fisheries and Aquaculture Department (FI) [online]. Rome. Updated 2019-02-21 [Cited <DATE>] [http://www.fao.org/geonetwork/srv/eng/main.home?uuid=fao-rfb-map-iattc](http://www.fao.org/geonetwork/srv/eng/main.home?uuid=fao-rfb-map-iattc) |
| **Attribution** | © FAO, 2019. FAO Regional Fishery Bodies. |
| **Last updated** | 2019-02-21 |
| **License** | Public |

## Description

42 RFMOs are included in one table. See below for a complete list. All RFMOS stem from the FAO geonetwork. Some RFMO shapefiles go to land, and there is a possibility of coastline issues. However, not quite sure there is an easy solution for RFMOs (since it is region-specific) so currently (Summer 2022) have not conducted any adjustments.

There are several RFMOs where we do not use the shapefile published in geonetwork. These are

- [NPFC](RFMOs%204516ced0f92b41a29da446c9593a1dfc/NPFC%204b67c4ebf64e49429ed87730d956115e.md)
- [CCSBT](RFMOs%204516ced0f92b41a29da446c9593a1dfc/CCSBT%203bcd1a10e9534556a02a0cfcf5503e5a.md)
- [ICCAT](RFMOs%204516ced0f92b41a29da446c9593a1dfc/ICCAT%20dd2c4c550f1f4a39807a95fd32823f6e.md)

See the linked pages for more on these. For the rest, see below.

## Data access

**GCS**

- Original: gs://pipe-regions-layers/fao_rfb_rfmos/original/FAO_RFB_*.zip
- Normalized and merged: gs://pipe-regions-layers/fao_rfb_rfmos/fao_rfb_rfmos.shp.zip

**Bigquery**

- Normalized: world-fishing-827.pipe_regions_layers.fao_rfb_rfmos
- Processed for S2 lookup:: world-fishing-827.pipe_regions_layers.fao_rfb_rfmos_s2_10

## Updating

There is a script in pipe-regions that will download original shapefiles, merge them and store in gcs. To use this, see the section on FAO RFMOs in [https://github.com/GlobalFishingWatch/pipe-regions/blob/master/USAGE.md](https://github.com/GlobalFishingWatch/pipe-regions/blob/master/USAGE.md)

## List of RFMOs

| ACAP | [Agreement on the Conservation of Albatrosses and Petrels](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-acap) |
| --- | --- |
| APFIC | [Asia Pacific Fisheries Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-apfic) |
| BOBP-IGO | [Bay of Bengal Programme](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-bobp-igo) |
| CACFISH | [Central Asia and Caucasus Fisheries and Aquaculture Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-cacfish) |
| CCAMLR | [Commission for the Conservation of Antarctic Marine Living Resources](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ccamlr) |
| CCBSP | [Convention on the Conservation and Management of Pollock Resources in the Central Bering Sea](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ccbsp) |
| CCSBT | [Commission for the Conservation of Southern Bluefin Tuna](RFMOs%204516ced0f92b41a29da446c9593a1dfc/CCSBT%203bcd1a10e9534556a02a0cfcf5503e5a.md) |
| COREP | [Commission Régionale des Pêches du Golfe de Guinee](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-corep) |
| CPPS | [Comisión Permanente del Pacifico Sur](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-cpps) |
| CRFM | [Caribbean Regional Fisheries Mechanism](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-crfm) |
| CTMFM | [Joint Technical Commission of the Maritime Front](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ctmfm) |
| EIFAAC | [European Inland Fisheries and Aquaculture Advisory Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-eifaac) |
| FCWC | [Fishery Committee of the West Central Gulf of Guinea](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-fcwc) |
| FFA | [Forum Fisheries Agency](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ffa) |
| GFCM | [General Fisheries Commission for the Mediterranean](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-gfcm) |
| IATTC | [Inter-American Tropical Tuna Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-iattc) |
| ICCAT | [International Commission for the Conservation of Atlantic Tunas](RFMOs%204516ced0f92b41a29da446c9593a1dfc/ICCAT%20dd2c4c550f1f4a39807a95fd32823f6e.md) |
| ICES | [International Council for the Exploration of the Sea](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ices) |
| IOTC | [Indian Ocean Tuna Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-iotc) |
| IPHC | [International Pacific Halibut Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-iphc) |
| IWC | [International Whaling Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-iwc) |
| LTA | [Lake Tanganyika Authority](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-lta) |
| LVFO | [Lake Victoria Fisheries Organization](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-lvfo) |
| MRC | [Mekong River Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-mrc) |
| NACA | [Network of Aquaculture Centers in Asia-Pacific](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-naca) |
| NAFO | [Northwest Atlantic Fisheries Organization](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-nafo) |
| NAMMCO | [North Atlantic Marine Mammal Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-nammco) |
| NASCO | [North Atlantic Salmon Conservation Organization](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-nasco) |
| NEAFC | [North-East Atlantic Fisheries Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-neafc) |
| NPAFC | [North Pacific Anadromous Fish Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-npafc) |
| NPFC | [North Pacific Fisheries Commission](RFMOs%204516ced0f92b41a29da446c9593a1dfc/NPFC%204b67c4ebf64e49429ed87730d956115e.md) |
| OSPESCA | [Central America Fisheries and Aquaculture Organization](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-ospesca) |
| PERSGA | [Red Sea and Gulf of Aden Environment Programme](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-persga) |
| PICES | [North Pacific Marine Science Organization](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-pices) |
| RECOFI | [Regional Commission for Fisheries](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-recofi) |
| SEAFDEC | [Southeast Asian Fisheries Development Center](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-seafdec) |
| SIOFA | [South Indian Ocean Fisheries Agreement](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-siofa) |
| SPC | [Secretariat of the Pacific Community](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-spc) |
| SPRFMO | [South Pacific Regional Fisheries Management Organisation](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-sprfmo) |
| SRFC | [Subregional Fisheries Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-srfc) |
| SWIOFC | [Southwest Indian Ocean Fisheries Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-swiofc) |
| WCPFC | [Western and Central Pacific Fisheries Commission](https://geonetwork.d4science.org/geonetwork/srv/en/metadata.show?uuid=fao-rfb-map-wcpfc) |

<!--TODO: Add this as separate pages?-->



[NPFC](https://www.notion.so/globalfishingwatch/NPFC-4b67c4ebf64e49429ed87730d956115e)

[IATTC](https://www.notion.so/globalfishingwatch/IATTC-031392aed43642b2bc52c968d530e078)

[ICCAT](https://www.notion.so/globalfishingwatch/ICCAT-dd2c4c550f1f4a39807a95fd32823f6e)

[SPRFMO](https://www.notion.so/globalfishingwatch/SPRFMO-405e2e7f77f740e39943813b8e8f8d6e)

[CCSBT](https://www.notion.so/globalfishingwatch/CCSBT-3bcd1a10e9534556a02a0cfcf5503e5a)

[IOTC](https://www.notion.so/globalfishingwatch/IOTC-1d8c46f6fd644f1993f137b4f54a0bb4)
