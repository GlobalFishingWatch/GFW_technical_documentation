The All Vessels table is the sole table used to identify which vessels (vessel_ids) to include in Product and what information to associate with the vessel. The additional fields on vessel activity, vessel type, and noise are primarily for GFW staff, and, in particular, GFW analysts, to understand why a vessel appears the way it does in Products, or why a vessel may not be visible to non-GFW staff. Owner: Willa 

**All Vessels v2 officially launched in Products October 26th 2023** as part of public Vessel Viewer (VV 2.0) release. See `Updates` section for more on the lastest changes to the dataset. 

## Key Tables

+ `pipe_production_v20201001.all_vessels_byyear_v2`
> + `pipe_production_v20201001.all_vessels_byyear` - the previous all vessels table, maintained in pipeline 2.5, but to be officially deprecated in pipeline 3.0. 

## Source Tables

All Vessels v2 uses the following source tables, below. For more information on how these source tables are referenced, please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1).
+ `pipe_production_v20201001.base_vessel_identity_info_merge` - engineer and data team table which is an intermediate step in creating the Vessel Identity API. `vessel_id`'s can be searched in this table to find associated `vessel_record_id`'s. 
> * `vessel_identity.identity_core`
> * `pipe_production_v20201001.vessel_info`
> * `pipe_production_v20201001.research_segs`
> * `pipe_production_v20201001.segment_info`
+ `gfw_research.vi_ssvid_byyear_vYYYYMMDD`
+ `vessel_database.purse_seine_support_vessels_byyear_vYYYYMMDD`

## Data Description

+ Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) for field definitions of all fields in table.

## Caveats and Known Issues

+ Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Caveats` section and `FAQs` section for more.
+ **Noise filter:** Products only include non-noisy segments, and subsequently vessel_id's with at least one non-noisy seg_id (eg passes the `WHERE good_seg and NOT overlapping_and_short` filter) are excluded from Products. 

## Example Queries

+ Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Use cases` section for more.
+ [vessel_id_connected_to_noisy_segment_check](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/vessel_id_connected_to_noisy_segment_check.sql) - if a vessel_id does not appear in All Vessels v2, the only reason this should happen is because the vessel_id is connected to only noisy segments. The example query allows users to input a vessel_id or ssvid and verify if the vessel_ids connected to the vessel have at least one non-noisy segment and should thus be included in the All Vessels v2 table.

## Links

All Vessels
+ [All Vessels data template documentation](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1): redundant in many ways to the GitWiki. Intended to flesh out field definitions and other data tables in a format accessible by non-github using staff.
+ [Vessel API link to All vessels](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/507084801/Vessel+Identity+API+flow): Diagram showing how All Vessels v2 fits within the work flow of the Vessel Identity API
+ [All Vessels v2 Confluence](https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/509345793/How+to+generate+the+all+vessels+byyear+v2+v+table+prototype): Includes source tables and code used to populated All Vessels v2 table. 
+ [All Vessels v2 Tech Call slides](https://docs.google.com/presentation/d/1JqegdW8X4jjrkCVIF2ehukuI894BAQQgQ0Z7D0tOK-s/edit?usp=sharing): from bi weekly tech call presentation on July 27th

## Updates
Last updated on **November 6th, 2023**

+ **[Oct 2023]** `all_vessels_byyear_v2` released in Products, with updates from internal feedback survey. 
> **Updates to table:**
> * added most frequent imo transmitted on AIS
> * added most frequent callsign transmitted on AIS
> * renamed flag field to mmsi_flag to make it clear that this flag is populated based on the first 3 digits of the MMSI transmitted on AIS
> * added gfw_best_flag field, which is pulled from vi_ssvid_byyear table and considers AIS and registry information
> * added core_flag, which is the flag information in the identity core table if an AIS records is high confidence matched (eg based on MMSI and another identity value) to a hull id.
> * Within prod_shiptype field, added 'gear' as a shiptype.
> * Within prod_shiptype field, eliminated the 'potential_fishing' shiptype option, and reclassified those vessels to ‘fishing’ to reduce confusion. We maintained the potential_fishing boolean field (see data template for field definition). See whimsical in data template on how shiptype and geartype are designated for clarity on what this means for how shiptype is determined. (shout out to Matt for spearheading the whimsical)
> * changed how prod_geartype field is populated, to reflect exactly what is expected to be seen in Products.
> * added potential_fishing_source field. This indicates the highest confidence field used to inform the potential_fishing boolean field.

> **Impacts on other datasets:** 
For engineering reasons (mainly the maintenance of Vessel Viewer (prototype)), we need to maintain All Vessels v1 and the AIS Published Event datasets along side All Vessels v2. That said, for the time being there are two sets of published event tables. **Please use the following tables moving forward for analysis.** If you have any questions about this, please reach out.
> * all_vessels: pipe_production_v20201001.all_vessels_byyear_v2
> * coverage block: pipe_v20201001_api.indicators_coverage_blocks_v2
> * encounters: pipe_production_v20201001.published_events_encounters_v2
> * fishing: pipe_production_v20201001.published_events_fishing_v2
> * loitering: pipe_production_v20201001.published_events_loitering_v2
> * port visits: pipe_production_v20201001.published_events_port_visits_v2
+ **[July 2023]** The `all_vessels_byyear_v2` table is shared internally.  
