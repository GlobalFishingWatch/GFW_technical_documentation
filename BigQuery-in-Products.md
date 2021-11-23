The purpose of GFW products broadly is to create and publicly share knowledge about human activity at sea to enable fair and sustainable use of our ocean.



## [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/)

The Carrier Vessel Portal (CVP) uses publicly available data from 2017 through the present to identify potential vessel encounters and loitering events. Updated with new data monthly, it synthesizes fishing registry information to create a picture of potential authorizations for both carrier and fishing vessels involved in transshipment activity. 

**Data in the Portal**
<br>V0 API | [Current CVP queries](https://github.com/GlobalFishingWatch/pipe-carrier-portal/tree/master/assets/bigquery)
+ **Encounters** are limited to those between carriers and fishing vessels and support and fishing vessels
+ **Loitering events** are limited to those by carrier vessels with a minimum duration of 1 hour. 
+ **Port visits** are limited to Confidence 4 visits. 
+ Information on **vessel authorization** is updated on two month lag 

The encounters, loitering, and port visit data included in the CVP can be found in BigQuery under the bin name `proj_carrier_portal_pew`

**Unique Features (compared to GFW Map):** Authorization Data, Port visits

**Resources**
+ [CVP FAQs](https://globalfishingwatch.org/help-faqs/)
+ [CVP Disclaimers](https://globalfishingwatch.org/carrier-vessel-portal-disclaimers/)
+ [Authorization Records](https://globalfishingwatch.org/authorization-records/)
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Upcoming/Future Plans**
+ Encounters code currently being updated to use published_events_encounters and carrier, support, and fishing by_year tables. CVP with updated encounters code likely in next ~month. 
+ If the CVP is maintained as a standalone product, then queries will be updated and V1 API will be used
+ Currently there is a 2 month lag on the CVP. If CVP transitions to V1 API, the end Portal will be on 3 day delay

**Updates**
+ Nov 2021: v0 API updated port visits and loitering events on CVP to pull from pipe_production_vYYYYMMDD published events ports and loitering tables. Port visits in the published events table are limited to confidence = 4. The loitering events logic has been changed to group points at seg_id level and not ssvid, and thus the number of loitering events in some areas increased noticeably due to some long loitering events now being grouped as many small loitering events. The CVP encounters query 
<br>

## [GFW Map](https://globalfishingwatch.org/map/) 
The Global Fishing Watch map is the first open-access online tool for visualization and analysis of vessel-based human activity at sea. Powered by satellite technology and machine learning, the map merges multiple types of vessel tracking data to provide a view of global human activity at sea, including fishing activity, encounters between vessels, night light vessel detection and vessel presence. Anyone with an internet connection can access the map to monitor global fishing activity from 2012 to the present from more than 65,000 commercial fishing vessels that are responsible for a significant part of global seafood catch. Users can search for vessels, filter activity by flag State or time period, identify port visits and view encounters between vessels. The map also allows anyone to upload and overlay their own data, download aggregated fishing reports of activity from custom areas, and save and share what is on the current view. We call these “workspaces”. Free and easy-to-use features offer unprecedented opportunities to increase transparency across the world’s ocean and support the fair and sustainable use of marine resources.

**Data in the Portal**
<br>V1 API

**Resources**
+ [Map 3.0 GFW Internal Slide Deck](https://docs.google.com/presentation/d/1aexIaw_Gw_LpBVJf2O1elapq-wZ5iAu5q8aJcg3yXrA/edit?usp=sharing) - highly recommend. Succinct but informative overview of target users, new functionalities, and examples of how to interact with map.
+ [GFW Map Launch Materials](https://docs.google.com/document/d/18Thsx0ebYzyvm8JZoKm6LtrrfOfI6WZHcnG3FyAdLrE/edit)
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Upcoming/Future Plans**
+ **Port visits** will be added to the GFW map (date TBD)

**Updates**
+ Oct 18 2021: GFW legacy map officially removed from GFW website
+ July 15 2021: GFW Map launched. Previous version of map will be available as a legacy map until Oct 18 2021 at https://globalfishingwatch.org/legacy-map
<br>

## [Marine Manager](https://globalfishingwatch.org/marine-manager-portal/)
Global Fishing Watch and Dona Bertarelli have partnered to develop Global Fishing Watch Marine Manager, a new technology portal that will help improve insight into marine protected areas. Dona Bertarelli’s vision in creating Global Fishing Watch Marine Manager was to ensure robust and science-based management to fully realize the vital contribution of protected areas, and with time, monitor the quality, efficiency and impact of those protections on the long term. The portal empowers managers to rapidly collate and analyze scientific data integral to the governance of marine protected areas and other area-based conservation measures.

**Data in the Portal**
<br>V1 API

**Unique Features (compared to GFW Map):** Environmental data layers

**Resources**
+ [GFW Product Tutorials](https://globalfishingwatch.org/tutorials/)

**Upcoming/Future Plans**
+ Dec 2021: Marine Manager "v1.5" [planned release](https://docs.google.com/document/d/1zndSgeY_kZN8yzRuLkquQ4b5CiJiohG8b6A1GpiFxVg/edit)

**Updates**
+ None yet :)
<br>

## [Vessel Viewer](https://globalfishingwatch.org/vessel-viewer)
The Vessel Viewer, formally the Port Inspector App, is geared towards Port Inspectors and is currently being prototyped in the field. The App's objective is to provide relevant information for port inspectors to assist them in identifying vessels which should be prioritized for inspection upon arrival in port.

At this time the Vessel Viewer is currently in development and available to GFW staff for internal use. 

**Data in the Portal**
<br>V1 API

**Resources**
+ TBD

**Upcoming/Future Plans**
+ TBD

**Updates**
+ TBD
<br> 

## APIs

GFW’s API’s provide a set of instructions for software to access select GFW data and information. An API retrieves and delivers data to both front-end and back-end applications. Currently GFW has two API’s used in products. The earliest version (version 0) used in the Carrier Vessel Portal (CVP), and then the current version (version 1) used in all other GFW products (GFW Map and Marine Manager).

At this time the use of GFW API’s is limited to GFW products, although GFW’s API’s make it feasible for our data to be hosted on the maps of collaborating organizations (like SeaVision). 

### Version 0
The Version 0 API is used exclusively in the CVP and has been updated to mimic the Version 1 API (using the same datasets as the Version 1 API and similar data restrictions). It is still to be decided if the CVP will remain a stand alone product, or be merged into the GFW Map or into another product. If the CVP remains a stand alone product, future efforts will be made to update the portal to support the Version 1 API. 

### Version 1 
The Version 1 API is used in the GFW Map and Marine Manager. In the future all GFW products will use this API.

The queries used to curate date for the V1 API can be found here: 
+ https://datasets.globalfishingwatch.org/  (Username: datasets | Password: gfw_doc). 

Within the queries are the BQ source datasets.

**APIs Resources**
+ [Intro APIs](https://docs.google.com/document/d/1CWVXqZpyutLOUO5YyACBzftUiEIxug7EPd8PXsadEE8/edit?usp=sharing) - slide deck for introduction to API’s <br>
+ [GFW Ocean-Engine: APIs and Blueprints, June 2021](https://docs.google.com/presentation/d/1E6h00EUEEr2HRAAXm3rJUwrF6NcM52t8S0n_yinJbqw/edit#slide=id.gc69f7383cc_0_1092) - introduction to GFW’s APIs 

**Upcoming/Future Plans**
+ A wrapper in R is being developed so that GFW API’s can be easily used in R in the future. When tools like these are created we will be able to help partners and interested individuals of the public be able to directly query data included on our public portals.

