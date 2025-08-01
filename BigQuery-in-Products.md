# BigQuery to APIs to Products
<br>
The flow of data from **BigQuery** to [**GFW Products**](https://docs.google.com/document/d/1JY0umwJ5j8qFpOb90OuaUOo43y45yExaenNWzUlbkCo/edit?tab=t.0)   is enabled through a set of **Application Programming Interfaces (APIs)**. This page documents the key APIs, the underlying queries powering them, and how they support user-facing products.

## GFW APIs

### [Current GFW APIs](https://globalfishingwatch.org/our-apis/documentation#introduction)

| API Name                     | Used In Products                                                                 |
|-----------------------------|-----------------------------------------------------------------------------------|
| **Map Visualization (4Wings API)** | API Portal, Map, Marine Manager                                          |
| **Vessels API**             | API Portal, Map, Vessel Profile, Marine Manager, `gfwR`, Python SDK              |
| **Events API**              | API Portal, Map, Vessel Profile, Marine Manager, `gfwR`, Python SDK              |
| **Insights API**            | API Portal, Map, Vessel Profile, Marine Manager                                 |
| **Datasets API**            | API Portal, Map                                                                 |
| **Bulk Download API**       | API Portal                                                                      |

### Products Using Legacy APIs

Some older products are still using legacy APIs:

- **Legacy CVP**
- **Vessel Profile Prototype**

## Accessing API Logic

API query logic is maintained in [this central hub](#).  

To view underlying queries and code for BigQuery tables:

- Open the relevant table in BigQuery.
- Click the **"Details"** tab to find:
  - Source tables
  - (When available) links to the GitHub code that generates each table

> ⚠️ If the logic in the API hub seems outdated or unclear, contact **Gisela**.

## Caveats and Considerations

**Permissions**

Access to API data may be restricted depending on your user role. Some datasets may only be available to:

- Internal GFW staff
- JAC members
- Specific partners

As a result, data visible through the API can vary based on your credentials.

**Data Filters**

We’ve made our best effort to document all filters and permission scopes on each API. However, gaps may still exist.

If you're uncertain about:

- Why data may be missing or inconsistent
- What filters are applied

Please contact **Gisela** for clarification.

<br>
<br>
<br>
<br>

## OLD MATERIALS - TO BE DELETED

## [Product Overview](https://docs.google.com/document/d/1JY0umwJ5j8qFpOb90OuaUOo43y45yExaenNWzUlbkCo/edit)

This page highlights the data included in products; bigquery source tables, API code, and data restrictions applied to the data. Additionally, updates to data in products, such as changes in source data tables or changes in restrictions to the data are outlined. Resources on how to use and interact with products are linked. 

| API | Product | BQ Queries | Notes |
| --- | --- | --- | --- | 
| V0 | [CVP](https://globalfishingwatch.org/carrier-vessel-portal/) | [V0 API Queries](https://github.com/GlobalFishingWatch/pipe-carrier-portal/tree/master/assets/bigquery) | The plan is to update the CVP to using the <br> same APIs as other the other products in the future. | 
| V1 | [GFW Map](https://globalfishingwatch.org/map/), <br> [Marine Manager](https://globalfishingwatch.org/marine-manager-portal/), <br> [Vessel Viewer](https://globalfishingwatch.org/vessel-viewer), <br> [API Portal](https://globalfishingwatch.org/ocean-engine/tokens/signup) | [V1 API Queries](https://datasets.globalfishingwatch.org/) <br> Username: datasets <br> Password: gfw_doc | There are certain filters and manipulations done to data <br> in the API which can occur outside of the BigQuery code. <br> Those nuances may eventually be updated and included <br> in the Wiki but are not currently, so be aware. | 

**Resources**
+ [Product Hour recordings](https://drive.google.com/drive/folders/1OZhFcIeSvGnCE0W6CiNBCDKxxgR8fgVT)
+ [Published Events with unnested authorization information example query](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_events_unnest_auth_info.sql)

## [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) | v0 API

The Carrier Vessel Portal (CVP) uses publicly available data from 2017 through the present to identify potential vessel encounters and loitering events. It synthesizes fishing registry information to create a picture of potential authorizations for both carrier and fishing vessels involved in transshipment activity. The CVP is the only Product using the v0 API, although, in the future, the Product and Engineering team will look to transition the CVP to using the same API as the other Products.

**Data in the Portal**
+ V0 API | [Current CVP queries](https://github.com/GlobalFishingWatch/pipe-carrier-portal/tree/master/assets/bigquery)
+ The output of the current CVP queries can be found in BigQuery under the bin name `pipe_carrier_portal_production_v20220124`

**Data Restrictions in Portal**
+ **Encounters** are limited to those between carriers and fishing vessels and support and fishing vessels
+ **Loitering events** are limited to those by carrier vessels with a minimum duration of 1 hour. 
+ **Port visits** are limited to Confidence 4 visits. For more information see [Anchorages and Voyages](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Anchorages-and-voyages) wiki 
+ Information on **vessel authorization** is updated on one month lag 

**Unique Features (compared to GFW Map):** Authorization Data, Port visits

**Resources**
+ [CVP Product Hour notes, resources, and demo recording](https://docs.google.com/document/d/1DbUyeKvZfE_wEsBNcJE61HBLixLPqm3t5fFn5FxJv0A/edit#)
+ [CVP FAQs](https://globalfishingwatch.org/help-faqs/)
+ [CVP Disclaimers](https://globalfishingwatch.org/carrier-vessel-portal-disclaimers/)
+ [Authorization Records](https://globalfishingwatch.org/authorization-records/)
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Upcoming/Future Plans**
+ If the CVP is maintained as a standalone product, then queries will be updated and V1 API will be used

**Updates**
+ Feb 15, 2022: CVP data updated on a 3 day lag rather than monthly update. CVP encounters code now pulls encounters from published_events_encounters and identified carrier, support, and fishing vessel  by_year tables.
+ Nov 2021: v0 API updated port visits and loitering events on CVP to pull from pipe_production_vYYYYMMDD published events ports and loitering tables. Port visits in the published events table are limited to confidence = 4. The loitering events logic has been changed to group points at seg_id level and not ssvid, and thus the number of loitering events in some areas increased noticeably due to some long loitering events now being grouped as many small loitering events. The CVP encounters query 
<br>

## [API Portal](https://globalfishingwatch.org/ocean-engine/tokens/signup) | v1 API 

GFW’s API’s provide a set of instructions for software to access select GFW data and information. An API retrieves and delivers data to both front-end and back-end applications. 

The GFW API Portal makes it feasible for our data to be hosted on the maps of collaborating organizations (like SeaVision). The API Portal launched on July 19th, 2022. A wrapper package in R, named `gfwr`, also launched alongside the API Portal, and allows individuals to call data from the API Portal directly into R.   

**APIs Resources**
+ [Intro APIs](https://docs.google.com/document/d/1CWVXqZpyutLOUO5YyACBzftUiEIxug7EPd8PXsadEE8/edit?usp=sharing) - slide deck for introduction to API’s <br>
+ [GFW Ocean-Engine: APIs and Blueprints, June 2021](https://docs.google.com/presentation/d/1E6h00EUEEr2HRAAXm3rJUwrF6NcM52t8S0n_yinJbqw/edit#slide=id.gc69f7383cc_0_1092) - introduction to GFW’s APIs 
+ R package to access API Portal: A [wrapper in R](https://github.com/GlobalFishingWatch/gfwr) has been developed so that GFW's API Portal data can be easily used in R. The package named `gfwr` can now be accessed. [Slack thread.](https://globalfishingwatch.slack.com/archives/C026FKDJGMT/p1654885678794689)

**Upcoming/Future Plans**
+ TBD
<br>

**Updates**
+ July 19, 2022: API Portal Launch! gfwr R package release
<br>


## [GFW Map](https://globalfishingwatch.org/map/) | v1 API
The Global Fishing Watch map is the "main" GFW product. The map merges multiple types of vessel tracking data to provide a view of global human activity at sea, including fishing activity, encounters between vessels, night light vessel detection and vessel presence. The map includes global fishing activity from 2012 to the present from more than 65,000 commercial fishing vessels that are responsible for a significant part of global seafood catch. Users can search for vessels, filter activity by flag State or time period, identify port visits and view encounters between vessels. The map also allows anyone to upload and overlay their own data, download aggregated fishing reports of activity from custom areas, and save and share what is on the current view.

**Data in the Portal**
+ V1 API | [V1 API Queries](https://datasets.globalfishingwatch.org/)  (Username: datasets | Password: gfw_doc). 
+ Within the V1 API Queries are the BQ source datasets.


**Resources**
+ [LATEST demo of map - July 2022](https://www.loom.com/share/dd1daa446295450aa5bdc0bba9e124ba)
+ [Map 3.0 GFW Internal Slide Deck](https://docs.google.com/presentation/d/1aexIaw_Gw_LpBVJf2O1elapq-wZ5iAu5q8aJcg3yXrA/edit?usp=sharing) - highly recommend. Succinct but informative overview of target users, new functionalities, and examples of how to interact with map.
+ [GFW Map Launch Materials](https://docs.google.com/document/d/18Thsx0ebYzyvm8JZoKm6LtrrfOfI6WZHcnG3FyAdLrE/edit)
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Internal Features (GFW Staff Only)**
+ [Load BigQuery data directly into GFW Map](https://www.loom.com/share/99594dec8de24887917ce02cae95629c) - when logged into GFW Map with GFW email press 'b' 7 times to get to interface to run BigQuery
+ [Vessel Groups](https://globalfishingwatch.slack.com/archives/G8V3UUFK8/p1660133391345519)

**Upcoming/Future Plans**
+ Sept 2022: Vessel groups (public access); vessel gaps
+ TBD: [layer on map with all Ports](https://docs.google.com/presentation/d/1NFLXiET5w6QYrPiQfAviJEMt0rqI8TR-_fRPDYyIDNo/edit?usp=sharing)

**Updates**
+ [internal only] Aug 10 2022: Vessel groups. [Slack thread.](https://globalfishingwatch.slack.com/archives/G8V3UUFK8/p1660133391345519)
+ June 8th 2022: SAR layer launched on Map. [Slack thread.](https://globalfishingwatch.slack.com/archives/G8V3UUFK8/p1654781549803069)
+ [internal only] Dec 23 2021: Functionality to load bigquery data directly into GFW map added
+ Dec 2021: Upload point files as reference layer functionality added
+ Oct 18 2021: GFW legacy map officially removed from GFW website
+ July 15 2021: GFW Map launched. Previous version of map will be available as a legacy map until Oct 18 2021 at https://globalfishingwatch.org/legacy-map
<br>

## [Marine Manager](https://globalfishingwatch.org/marine-manager-portal/) | v1 API
Global Fishing Watch and Dona Bertarelli have partnered to develop Global Fishing Watch Marine Manager. The portal empowers managers to rapidly collate and analyze scientific data integral to the governance of marine protected areas and other area-based conservation measures.

**Data in the Portal**
+ V1 API | [V1 API Queries](https://datasets.globalfishingwatch.org/)  (Username: datasets | Password: gfw_doc). 
+ Within the V1 API Queries are the BQ source datasets.

**Unique Features (compared to GFW Map):** Environmental data layers

**Resources**
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Upcoming/Future Plans**
+ N/A

**Updates**
+ Dec 9 2021: Marine Manager "v1.5" [release summary](https://docs.google.com/document/d/1zndSgeY_kZN8yzRuLkquQ4b5CiJiohG8b6A1GpiFxVg/edit)
<br>

## [Vessel Viewer](https://globalfishingwatch.org/vessel-viewer) | v1 API
The Vessel Viewer, formally the Port Inspector App, is geared towards Port Inspectors and is currently being prototyped in the field. The App's objective is to provide relevant information for port inspectors to assist them in identifying vessels which should be prioritized for inspection upon arrival in port.

At this time the Vessel Viewer is currently in development and available to GFW staff for internal use. 

Vessel Viewer is part of GFWs broader effort to develop a suite of vessel intelligence tools to support (1) port inspection, (2) patrol planning, (3) insurance underwriting and (4) supply chain management to mitigate IUU risk.

**Data in the Portal**
+ V1 API | [V1 API Queries](https://datasets.globalfishingwatch.org/)  (Username: datasets | Password: gfw_doc). 
+ Within the V1 API Queries are the BQ source datasets.

**Resources**
+ Vessel Viewer FAQ in [English](https://drive.google.com/file/d/1N4YRJ_yxAObEIWbaYM8l-YDPHS0bKe2N/view) and [French](https://drive.google.com/file/d/195mbtLoHaY7iltFUbogiEX1cZNBpFg4E/view)
+ Vessel Viewer How-To Guide in [English](https://drive.google.com/file/d/1q2veC-EEUHWPg_eCplERJzlo8PL3OFZS/view) and [French](https://drive.google.com/file/d/1QaO3XKaXSnerAtcgw_a9EJpUEtM3WHN2/view)
+ [Vessel Viewer basic video overview](https://drive.google.com/file/d/18yenS9pva3kF2YGKZcSqdHy8avzRinBP/view) (audio in English with French subtitles)

**Upcoming/Future Plans**
+ Add gaps to vessel viewer

**Updates**
+ TBD
<br>
