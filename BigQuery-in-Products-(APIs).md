GFW’s API’s provide a set of instructions for software to access select GFW data and information. An API retrieves and delivers data to both front-end and back-end applications. Currently GFW has two API’s used in products. The earliest version (version 0) used in the Carrier Vessel Portal (CVP), and then the current version (version 1) used in all other GFW products (GFW Map and Marine Manager).

At this time the use of GFW API’s is limited to GFW products, although GFW’s API’s make it feasible for our data to be hosted on the maps of collaborating organizations (like SeaVision). 

A wrapper in R is being developed so that GFW API’s can be easily used in R in the future. When tools like these are created we will be able to help partners and interested individuals of the public be able to directly query data included on our public portals. 

## Key Tables 

### CVP (v0 API)
The version 0 API is used in the CVP and has been updated to mimic the version 1 API. It is still to be decided if the CVP will remain a stand alone product, or be merged into the GFW Map or into another product. If the CVP remains a stand alone product, future efforts will be made to update the portal to support the version 1 API. 

+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_encounters_vYYYYMMDD`: Encounter events included in the CVP
+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_loitering_vYYYYMMDD`: Loitering events included in the CVP
+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_ports_vYYYYMMDD`: Port visits included in the CVP

### GFW Map / Marine Manager (v1 API)
The queries used to curate date for the V1 API can be found here: 
+ https://datasets.globalfishingwatch.org/  (Username: datasets | Password: gfw_doc). 

Within the queries are the BQ source datasets.

## Links:
**Products**
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/)
+ [GFW Map](https://globalfishingwatch.org/map/)
+ [Marine Manager](https://globalfishingwatch.org/marine-manager-portal/)   

**APIs**
+ [Intro APIs](https://docs.google.com/document/d/1CWVXqZpyutLOUO5YyACBzftUiEIxug7EPd8PXsadEE8/edit?usp=sharing) - slide deck for introduction to API’s <br>
+ [GFW Ocean-Engine: APIs and Blueprints, June 2021](https://docs.google.com/presentation/d/1E6h00EUEEr2HRAAXm3rJUwrF6NcM52t8S0n_yinJbqw/edit#slide=id.gc69f7383cc_0_1092) - introduction to GFW’s APIs

## Updates:
**Version 0 API**
+ **Nov 2021:** v0 API updated port visits and loitering events on CVP to pull from pipe_production_vYYYYMMDD published events ports and loitering tables. Port visits in the published events table are limited to confidence = 4. The loitering events logic has been changed to group points at seg_id level and not ssvid, and thus the number of loitering events in some areas increased noticeably due to some long loitering events now being grouped as many small loitering events. The CVP encounters query 

**Version 1 API**
