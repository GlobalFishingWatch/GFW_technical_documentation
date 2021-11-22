## Products
The purpose of GFW products broadly is to create and publicly share knowledge about human activity at sea to enable fair and sustainable use of our ocean.

### [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/)

The Carrier Vessel Portal (CVP) uses publicly available data from 2017 through the present to identify potential vessel encounters and loitering events. Updated with new data monthly, it synthesizes fishing registry information to create a picture of potential authorizations for both carrier and fishing vessels involved in transshipment activity. 

**Data in the Portal**

+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_encounters_vYYYYMMDD`: Encounter events included in the CVP
+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_loitering_vYYYYMMDD`: Loitering events included in the CVP
+ `world-fishing-827.proj_carrier_portal_pew.carrier_portal_published_events_ports_vYYYYMMDD`: Port visits included in the CVP

**Updates**
+ **Nov 2021:** v0 API updated port visits and loitering events on CVP to pull from pipe_production_vYYYYMMDD published events ports and loitering tables. Port visits in the published events table are limited to confidence = 4. The loitering events logic has been changed to group points at seg_id level and not ssvid, and thus the number of loitering events in some areas increased noticeably due to some long loitering events now being grouped as many small loitering events. The CVP encounters query 

**Upcoming/Future Plans**
+ Encounters code currently being updated to use published_events_encounters and carrier, support, and fishing by_year tables. CVP with updated encounters code likely in next ~month. 
+ The CVP will shift to using the V1 API?
+ The CVP will show data on 3 day lag?

**Resources**

### [GFW Map](https://globalfishingwatch.org/map/) 
The Global Fishing Watch Map...

**Data in the Portal**

**Updates**

**Upcoming/Future Plans**

**Resources**

### [Marine Manager](https://globalfishingwatch.org/marine-manager-portal/)
The Marine Manager Portal....

**Data in the Portal**

**Updates**

**Upcoming/Future Plans**

**Resources**

### Vessel Viewer (Coming soon...)
The Vessel Viewer is currently in development and accessible for prototype testing
 

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

**Upcoming/Future Plans**
+ A wrapper in R is being developed so that GFW API’s can be easily used in R in the future. When tools like these are created we will be able to help partners and interested individuals of the public be able to directly query data included on our public portals.

**APIs Resources**
+ [Intro APIs](https://docs.google.com/document/d/1CWVXqZpyutLOUO5YyACBzftUiEIxug7EPd8PXsadEE8/edit?usp=sharing) - slide deck for introduction to API’s <br>
+ [GFW Ocean-Engine: APIs and Blueprints, June 2021](https://docs.google.com/presentation/d/1E6h00EUEEr2HRAAXm3rJUwrF6NcM52t8S0n_yinJbqw/edit#slide=id.gc69f7383cc_0_1092) - introduction to GFW’s APIs 
