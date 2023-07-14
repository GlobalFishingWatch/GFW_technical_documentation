The satellite timing offset table contains estimates of how far off the satellite clocks are for each Spire satellite. It's only Spire at present, because that is the only set of satellites we have IDs for. Exact Earth satellite's are reputed to have very stable clocks so it may not be necessary there, but might be useful for shore based AIS if we ever get IDs for individual stations.


## Key Tables

+ `pipe_ais_v3_alpha_published.satellite_timing_offsets` 

### Source Tables
 * `pipe_ais_sources_v20220628.normalized_spire_*`

## Data Description

For Spire's satellite clocks there are two effects that we've seen. One is a steady, slow drift over time. This is rarely an issue since the clocks are periodically reset. The second effect is a sudden, large jump in clock's time which I suspect is due to bit flips in the clocks due to cosmic rays or similar unfriendly particles. This can offset the clock anywhere from minutes to days (about two days is the largest I've seen) and results in some very confusing tracks.

In order to estimate the clocks offset we compare the times from clocks on all of the satellites we have IDs for by looking at signals from vessels that are broadcast closely in time and and received by different satellites. By using the vessels location, course and speed we can estimate the time between when each signal was broadcast. Comparing this to the difference in the message timestamps gives an estimate of how far off the timestamps are for a pair of satellites. By looking across many vessels and pairs of satellites we derive estimate for how far off the clock for each satellites is from the consensus timestamp, that is the median of all the satellite clocks.

This table is derived directly from the `normalized_spire` sources, e.g., `pipe_ais_sources_v20220628.normalized_spire_*`

## Use of Table

+ Automatically drop all the messages from satellites with clocks that are off by more than 30 s (I need to check that), this happens on an hour by hour basis. This only applies to Spire. Pretty sure we don’t try to fix the timestamps currently just drop the ones that are way off (in part because once the clocks are sufficiently far off, the estimate starts to become less trustworthy).  The table is available though for anyone who want to try to adjust the clocks by hand or do anything else with the time offsets.

## Example queries
+ [encounters_1_ssvid.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_1_ssvid.sql) - this query pulls information on both vessels in the encounter, including vessel attributes that are nested within the `event_vessels` field  
+ [encounters_2_carriers_fishing.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_2_carriers_fishing.sql) - this query identifies encounters between carrier and fishing vessels, pulling from the shiptype attribute used in Products (eg mirroring the encounters you would expect to see in Products as of June 2023 when this query was drafted).
+ [encounters_3_iccat.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_3_iccat.sql) - new example query that pulls out encounters that occurred inside ICCAT. Pulls using the ICCAT shapefile in the `pipe_regions_layers` bin. Technically you could also use the method in the below example query to identify encounters in ICCAT - both pulling region information from within the published events table, and using the regions shapefiles in the `pipe_regions_layers` should yield the same results. 
+ [encounters_3_no_take_mpa.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_3_no_take_mpa.sql) - new example query that pulls out encounters that occurred inside a no take MPA based on the region information in the Map.
+ [encounters_4_original_table.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/encounters_4_original_table.sql) - this query pull encounters with an average speed of greater than 2 knots from the original encounter table that is the base for the published event encounter table. Note, for the most part, the only encounters in the original table that are not in the `published_event_encounter` table, are those encounters with an average speed above 2 knots. 
+ [published_events_unnest_auth_info.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/published_events_unnest_auth_info.sql) - how to pull event authorization from published event table schema  
+ [loitering_overlap_encounters.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/loitering_overlap_encounters.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 


