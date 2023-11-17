The All Vessels table is the sole table used to identify which vessels (vessel_ids) to include in Product and what information to associate with the vessel. The additional fields on vessel activity, vessel type, and noise in the All Vessels table are primarily for GFW staff, and, in particular, GFW analysts, to understand why a vessel appears the way it does in Products, or why a vessel may not be visible to non-GFW staff. Owner: Willa 

**All Vessels v2 officially launched in Products October 26th 2023** as part of public Vessel Viewer (VV 2.0) release. See `Updates` section for more on the lastest changes to the dataset. 

## Key Tables

+ `pipe_production_v20201001.all_vessels_byyear_v2`
> + `pipe_production_v20201001.all_vessels_byyear` - the previous all vessels table, maintained in pipeline 2.5, but to be officially deprecated in pipeline 3.0. 

## Source Tables

All Vessels v2 uses the following source tables, below. For more information on how these source tables are utilized to populate the All Vessels table, please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1).
+ `pipe_production_v20201001.base_vessel_identity_info_merge` - internal table not to be used outside of products. This table was created to match our vessel_id (AIS transmitted data) to our registry matched table (identity core) so that Products automatically connect vessel_id (AIS records) to any existing registry records. 
> * `vessel_identity.identity_core`
> * `pipe_production_v20201001.vessel_info`
> * `pipe_production_v20201001.research_segs`
> * `pipe_production_v20201001.segment_info`
+ `gfw_research.vi_ssvid_byyear_vYYYYMMDD`
+ `vessel_database.purse_seine_support_vessels_byyear_vYYYYMMDD`

Links to the queries used to populate the base vessel identity merge and all vessels v2 tables to be added here in the near future.      

## Data Description

The All Vessels v2 table is a step in the Vessel Identity API, which links AIS records to registry records and is intended to act as a one stop shop for vessel identity for Products. At the product level we want to provide the cleanest information we have on vessel identity, which is why there is an extra layer of logic to lay out a hierarchy for how to represent a vessel in Products when there is conflicting information. 

We want to be transparent on this hierarchy and the logic behind it, internally, but also externally for all users. As a result, the table includes numerous summary fields on vessel activity, noise, and identity. While these extra details are not featured in Products, the information is intended to provide context for internal staff to understand why a vessel is represented the way it is. Further these fields allow analysts to use the All Vessels v2 table more flexibly than how it is included in Products, to meet broader uses cases. Being able to use All Vessels v2 for analysis that are separate from Products is valuable, as it is still easy to link analysis back to what is in Products and allows analysts to easily identify how the vessels in their reports reflect what is in Product, what is miss from Product, and what is represented differently from what is in Product, and why. 

How the Vessel Identity API is populated is highlighted in the diagram below; specifically how All Vessels v2 fits into the Vessel Identity API. 

![Vessel Identity API diagram](https://github.com/willabrooksGFW/gfw_photos/blob/main/vessel_api_3_steps.drawio_1.png) <br>
[Vessel Identity API diagram link](https://drive.google.com/file/d/1w-H8JawKDdjj0bikUXlXpZ-I8vNvPccm/view?usp=sharing)

Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) for field definitions of all fields in table. In the coming weeks, field definitions will be added to the schema of the All Vessel v2 table in BigQuery.

## Caveats and Known Issues

+ Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Caveats` section and `FAQs` section for more on the following topics.
> * Caveats
> > * Inaccuracies in inferred (neural net) class information
> > * Possibility of error between ssvid and vessel id merger
> * FAQs
> > * Why is a vessel I know to be a fishing vessel, not indicated as fishing on the Map?
> > * What if a vessel is considered multiple gear types?
> > * What should I do if I think a vessel type has been incorrectly assigned?
> > * Why is my vessel_id NOT included in All Vessels v2 (and thus in Products)?
> > * Multiple Vessel IDs for a single unique hul
> > * Why am I only seeing a short track segment when I search for a vessel that I know is transmitting on AIS across years?
> > * Making sense of vessels with shiptype ‘Discrepancy’
> > * What is the difference between core_geartype and registry_vessel_class fields?
+ **Noise filter:** Products only include non-noisy segments, and subsequently vessel_id's with at least one non-noisy seg_id (eg passes the `WHERE good_seg and NOT overlapping_and_short` filter) are excluded from Products. 

## Example Queries

+ Please see the [All Vessels Data Template](https://docs.google.com/document/d/1zhYOFaur-XNv5i1q3cE-IGn84bcJRNAJqTya0BIBmQo/edit?pli=1) `Use cases` section for example Queries expanding on the following questions:
> * How do I use the activity summary fields to understand vessel activity? 
> * How do I compare what the current product ship and gear type is (v2) to what was in the previous all vessel (unversioned) table?
> * Discrepancy vessel example
> * Why is a vessel I know to be a fishing vessel, not indicated as fishing on the Map?
> * How can the All Vessels v2 table be used to identify all possible fishing vessels? 
+ [vessel_id_connected_to_noisy_segment_check](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/vessel_id_connected_to_noisy_segment_check.sql) - if a vessel_id does not appear in All Vessels v2, the only reason this should happen is because the vessel_id is connected to only noisy segments. The example query allows users to input a vessel_id or ssvid and verify if the vessel_ids connected to the vessel have at least one non-noisy segment and should thus be included in the All Vessels v2 table.

## Links

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
> * all_vessels: `pipe_production_v20201001.all_vessels_byyear_v2`
> * coverage: `pipe_v20201001_api.indicators_coverage_blocks_v2`
> * encounters: `pipe_production_v20201001.published_events_encounters_v2` - carriers and fishing vessels are identified differently in  All Vessels v2. Thus which encounters are flagged as carrier-fishing and support-fishing and thus included publicly in map are different. See below for more details.
> > * Carrier vessels (and bunker vessels) require a match to a hull_id in the identity core table to be flagged. The process for matching carrier vessel records to AIS records is more precise than what was originally implemented. As a result there are fewer carriers in products. Based on initial analysis, this change is a good one and should make our identification of carriers more accurate. I am still investigating this to make sure we aren’t systematically losing legitimate carrier vessels, and from the analysis so far, things look good.
> > * Fishing vessels: The long standing bug which classified known fishing vessels to 'other' vessel type due to noise should now be fixed in the All Vessels v2 table. Further the list of vessels flagged as fishing is expanded. In addition to vessels that are flagged as on_fishing_list_best (the original filter for identifying fishing vessels) in the vi_ssvid_byyear table, vessels that are flagged as a fishing vessel in a registry on by the neural net are included as fishing vessels in Product if the vessel isn’t already flagged as a support, carrier, bunker, or discrepancy vessel (see whimsical in data template for visual flow diagram of what this looks like).
> * fishing events: `pipe_production_v20201001.published_events_fishing_v2`
> > * fishing events query has been re-run on an expanded list of fishing vessels using the All Vessels v2 table as the source, where if a vessel is potential_fishing or on_fishing_list_sr (see data template for field definitions), then the vessel has fishing events populated for it. In Products, a narrower set of vessels have fishing events displayed (eg only vessels where prod_shiptype = ‘fishing’). This provided, depending on the use case, analysts may need to add an extra filter when using the published events fishing table. If you have any questions on this, please reach out.
> * loitering: `pipe_production_v20201001.published_events_loitering_v2`
> * port visits: `pipe_production_v20201001.published_events_port_visits_v2`
+ **[July 2023]** The `all_vessels_byyear_v2` table is shared internally.  
