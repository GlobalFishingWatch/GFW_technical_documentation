The All Vessels table is used in Products to select what vessels to show in various Product layers, and how to represent the data, depending on shiptype.  

## Key Tables

Current version
+ `world-fishing-827.pipe_production_v20201001.all_vessels_byyear_v2`


Previous versions 
+ `world-fishing-827.pipe_production_v20201001.all_vessels_byyear`

## Source Tables

All Vessels
+ base vessel identity info (step 1 of creating Vessel API)
+ `world-fishing-827.gfw_research.vi_ssvid_byyear_vYYYYMMDD`
+ `world-fishing-827.vessel_database.purse_seine_support_vessels_byyear_vYYYYMMDD`


Base Vessel Identity Info (step 1)
+ `world-fishing-827.vessel_identity.identity_core`
+ `world-fishing-827.pipe_production_v20201001.vessel_info`
+ `world-fishing-827.pipe_production_v20201001.research_segs`
+ `world-fishing-827.pipe_production_v20201001.segment_info`

## Data Description

The All Vessels table is the sole table used to identify which vessels (vessel_ids) to include in Product and what information to associate with the vessel. The additional fields on vessel activity, vessel type, and noise are primarily for GFW staff, and, in particular, GFW analysts, to understand why a vessel appears the way it does in Products, or why a vessel may not be visible to non-GFW staff. 

All the data attributes required to understand why a vessel is classified the way it is in Products or why a vessel is excluded or missing from a Product are included in this table. 


## Caveats and Known Issues

TBD

## Example Queries

+ TBD

## Links

+ All vessels v1 confluence [HERE](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/445284357/How+to+generate+the+all+vessels+byyear+v+table)
+ Vessel API link to All vessels [HERE](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/507084801/Vessel+Identity+API+flow)
+ All Vessels documentation [HERE](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1)

## Updates
Last updated on **July 3rd, 2023**

+ **[July 2023]** The `all_vessels_byyear_v2` table is shared internally.  
