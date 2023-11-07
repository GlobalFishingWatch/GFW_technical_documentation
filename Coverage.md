The coverage metric was first released in Vessel Viewer (prototype) in 2023 as a contextual piece of information to help users identify how useful/complete the activity summary for the vessel being searched is. The coverage metric is a percentage representing the proportion of one hour blocks a vessel is in a voyage and has at least one AIS transmission. See Data Description section for more information. 


## Key Tables

+ There is no key table for coverage. Coverage is instead calculated in the API without an associated BigQuery table, because coverage is calculated every day (with a 3 day delay) for a vessels activity in the previous year. The coverage value therefore must be recalculated every day, and also must consider if you are vessel records being manually merged together, which requires individual calculations for every manual merge configuration. See Example Queries sections for information on how to manually calculate coverage. 

## Source Tables

+ `pipe_v20201001_api.indicators_coverage_blocks_v2` (v2 used in vv 2.0; should be used as default) - daily hourly coverage rate per vessel_id. Each vessel_id lists for every day the number of hours the vessel was active (in a voyage), and the number of hours were at least one AIS transmission was counted. In this way, this table provides a daily coverage value which gets aggregated together in the API to get an overall coverage value. See example query section for query which illustrates how to nearly replicate the coverage calculation seen in Vessel Viewer.
> + `pipe_v20201001_api.indicators_coverage_blocks` (original version currently used in vv (prototype)) - this table is generally reflective of what is in v2, but is maintained separately for vv (prototype).   
+ `pipe_production_v20201001.vessel_info` - used to set the date range for a vessels activity (vessel_id resolution)

## Data Description

+ **Coverage definition in Vessel Viewer:** The coverage metric is an estimate of how well a vessel's activities, i.e. where it traveled and what it did, can be captured by AIS data. To calculate it, all voyages linked to a vessel in the last year (i.e. 12 months) are segmented into one hour blocks and the total number of blocks with at least one AIS transmission are counted. The coverage metric is a percentage representing the proportion of one hour blocks a vessel is in a voyage and has at least one AIS transmission.


## Caveats and Known Issues

+ **Coverage only calculated during voyages:** The coverage calculation is limited to the period of time a vessel is detected in a voyage (eg the activity of a vessel out at sea, between port visits). This means that if a vessel is detected in port, this activity the vessel is active on AIS will not be factored into the coverage calculation. Some of the reasons for this are, (a) if a vessel is in port for a long period of time with its AIS on, that high coverage may be disproportionate and not reflective of the vessels coverage at sea, (b) vessels frequently turn off AIS once in port. If there is an issue detecting port visits for a vessel, this may affect the accuracy of the coverage calculation, given port visits are used to bound the period of time to calculate coverage for.  
+ **One hour intervals:** Coverage is evaluated by looking at the frequency a vessel transmits on AIS at least once every hour. Depending on the use case or the area where a vessel is active, evaluating coverage on one hour intervals may result in low coverage. For instance, in some areas satellite reception may be low, resulting in low coverage subsequently. Fishing vessels frequently transmit Class B AIS, which has a less frequent ping rate that could result in lower coverage. These factors and others are important to consider when trying to understand the value/meaning of the coverage metric. 
+ **One year intervals:** In Products, the coverage metric is calculated for the past year of activity with a three day delay. If a vessel of interest has primarily been active during at time not in the past year, then the coverage value is not very useful. However coverage can manually be calculated in BigQuery in these cases. See Example Queries for more.  

## Example Queries
** if trying to replicate coverage in Vessel Viewer, user should define time range for one year period considering 3 day delay. For example if user looks at coverage in Vessel Viewer on Oct 20th 2023, then date range in example query should be set for Oct 17th 2023 to Oct 17th 2022. 
+ [coverage_single_vessel_id.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/coverage_single_vessel_id.sql) - coverage for specified vessel_id and user defined time range
+ [coverage_multi_vessel_id.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/coverage_multi_vessel_id.sql) - coverage for list of vessel_ids with user defined time range
+ [coverage_aggregate.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/coverage_aggregate.sql) - aggregate coverage vessel for select group of vessel_ids and user defined time range 

## Links

+ [Vessel Viewer FAQs](https://drive.google.com/file/d/1Z3WAgFrsibAUjtrapMnvs8m9ZrDXp-8T/view?pli=1) - see bottom of page 6 for description of how coverage is defined. 

## Updates
Last updated on **October 20, 2023**
