The All Vessels table is the sole table used to identify which vessels (vessel_ids) to include in Product and what information to associate with the vessel. The additional fields on vessel activity, vessel type, and noise are primarily for GFW staff, and, in particular, GFW analysts, to understand why a vessel appears the way it does in Products, or why a vessel may not be visible to non-GFW staff. 

All the data attributes required to understand why a vessel is classified the way it is in Products or why a vessel is excluded or missing from a Product are included in this table.  

Please **provide feedback on All Vessels v2 table by August 31st 2023.** See `LINKS` section for feedback form links.

## Key Tables

NEW version - collecting feedback
+ `world-fishing-827.pipe_production_v20201001.all_vessels_byyear_v2` **COMING SOON**


Current version 
+ `world-fishing-827.pipe_production_v20201001.all_vessels_byyear`

## Source Tables

All Vessels v2
+ `world-fishing-827.pipe_production_v20201001.base_vessel_identity_info` **COMING SOON** engineer and data team table which is an intermediate step in creating the Vessel Identity API. This table uses the following source tables
> * `world-fishing-827.vessel_identity.identity_core`
> * `world-fishing-827.pipe_production_v20201001.vessel_info`
> * `world-fishing-827.pipe_production_v20201001.research_segs`
> * `world-fishing-827.pipe_production_v20201001.segment_info`
+ `world-fishing-827.gfw_research.vi_ssvid_byyear_vYYYYMMDD`
+ `world-fishing-827.vessel_database.purse_seine_support_vessels_byyear_vYYYYMMDD`

## Data Description

+ **UNDER CONSTRUCTION** For now, please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) for more.


## Caveats and Known Issues

+ **Noise filter:** Products only include non-noisy segments, and subsequently vessel_id's with at least one non-noisy seg_id (eg passes the `WHERE good_seg and NOT overlapping_and_short` filter) are excluded from Products.
+ **UNDER CONSTRUCTION** For now, please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Caveats` section for more. 

## Example Queries

+ **UNDER CONSTRUCTION** For now, please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Use cases` section for more.

## Links

Feedback **(Priority to feedback submitted before August 31st 2023)**
+ [All Vessels v2 Vessel/Fleet Specific Feedback Form](https://forms.gle/FfzsY9mtCcbg3M6AA)
+ [All Vessels v2 Feedback Form](https://forms.gle/ErASvnmezcQGLQNu8)

All Vessels
+ [All Vessels data template documentation](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1): redundant in many ways to the GitWiki. Intended to flesh out field definitions and other data tables in a format accessible by non-github using staff.
+ [Vessel API link to All vessels](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/507084801/Vessel+Identity+API+flow): Diagram showing how All Vessels v2 fits within the work flow of the Vessel Identity API
+ [All Vessels v2 Confluence](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/509345793/How+to+generate+the+all+vessels+byyear+v2+v+table+prototype): Includes source tables and code used to populated All Vessels v2 table. 

## Updates
Last updated on **July 19th, 2023**

+ **[July 2023]** The `all_vessels_byyear_v2` table is shared internally.  
