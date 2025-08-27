# Data Updates

### Running list of updates to note in regard to Global Fishing Watch data and data changes {.unnumbered}

> **Note**
> Do not forget to update the version of the tables in the [BigQuery Table Reference](bigquery-pipe3-table-reference.md).

## December 15, 2023

Due to limits on data partitions last year we removed 2012 data from the `pipe_production_v20201001` tables. We have now had to do that again for 2013. 

Note: this does not  apply to pipe 3 where this issue is resolved. Therefore the following applies:

If using pipe 2.5 (`pipe_production_v20201001`) currently note that it will now have data from 2014- present

If you want older data please join with the archive version of the table of interest which will include both 2012 and 2013 (you do no longer need to use the _2012 version of the dataset). Additional clarification:
* the tables with prefix _2012 will be kept
* For each cases, we’ll have tables with suffix archive_ which will contain the archive years (in this case 2012 and 2013 data). Making anyone decision if they want to join the archive_ or the _2012 ones => this implies a breaking change to who are using only _2012 and wants the 2012 and 2013 data, in that case it should be use archive_.


## August 19, 2022 Updates

This update contains a handful of updates that have been released since the last update message below on March 7, 2022. 

+ **The research tables are now located in the pipeline dataset and new analyses should no longer use the `gfw_research.pipe_vYYYYMMDD` tables**
  + All research tables are now prefixed with `research_` instead of `pipe_vYYYYMMDD` as they are now stored in the pipeline dataset they correspond to rather than `gfw_research`
  + The primary table of AIS positions (formerly `gfw_research.pipe_vYYYYMMDD`) is now called `research_messages` 
  + There is no longer a separate table for just fishing vessels (e.g. pipe_vYYYYMMDD_fishing`). Instead, the `research_messages` table includes a boolean `is_fishing_vessel` field and queries for just fishing vessels can be made ~10x cheaper by filtering with `WHERE is_fishing_vessel`.
  + All other research tables (e.g. `_ids`, `_segs`, etc.) are unchanged aside from their name
  + See the [Fishing effort and vessel presence](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/Fishing-effort-and-vessel-presence) Wiki page for more details.

+ **Vessel info tables are now automated in sync with the vessel database and will be updated every month**

+ **Offsetting table has been updated and is now at the `seg_id` level instead of the yearly level.**
  + Table: `gfw_research_precursors.offsetting_year_seg_v20220609`

## March 7, 2022 Updates

New vessel info tables added to gfw_research:  
+ `vi_ssvid_v20220101`
+ `vi_ssvid_byyear_v20220101`
+ `vi_stats_v20220101`
+ `fishing_vessels_ssvid_v20220101`

These versions use the corresponding version of the vessel database (`v20220101`) and therefore contain data through 2021-12-31. There are otherwise no improvements/updates to the table other than new data.

## December 14, 2021 Updates

VMS datasets, including Peru, Panama, Chile, Brazil, Costa Rica, and Ecuador have all been updated. See the VMS page and the Big Query Table Reference page for the new table versions. 


## October 26, 2021 Updates

- The VIIRS-AIS matching table is now automatically updated on a daily basis. The data itself is identical.
  - previous table (manually updated): `gfw_research.matches_raw_vbd_global_3top_v20210514`
  - new table (automatically updated): `pipe_production_v20201001.proto_matches_raw_vbd_global_3top_v20210514`
 
## October 13, 2021 Updates

Flags of convenience have been moved to a versioned system. The original table is `gfw_research.flags_of_convenience`. This remains unchanged, reflecting the ITF list of FOCs from 2019, and will be kept for the next 30 days to give time for people to switch over their code. If you need to use this historical list of FOCs, you can reference `gfw_research.flags_of_convenience_v20190110`. For all other cases, you should now use `gfw_research.flags_of_convenience_v20211013`. FOC flags as reported by ITF are pulled from https://www.itfseafarers.org/en/focs/current-registries-listed-as-focs. We continue to remove the French and German International Ship Registries as they do not have their own unique flag State and ISO3 code. The changes between the `20190110` and `20211013` versions are as follows:

* Added Cameroon (CMR), Cook Islands (COK), Palau (PLW), Sierra Leone (SLE), St Kitts and Nevis (KNA), Tanzania (TZA), and Togo (TGO
* Removed the Netherlands Antilles (ANT)
* Overall increase from 34 to 40 flag states in the dataset

## October 4, 2021 Updates

* Updated wiki page on encounters dataset with BQ example code. Note the internal logic has been adjusted (using segment id). See page for details.
* The `pipe_production_v20201001.published_events_loitering` table has been adjusted to include the restrictions: `avg_distance_from_shore_nm>=20`, and segments must be `good_seg` and NOT `overlapping_and_short`
* Latest vessel_info table is `gfw_research.vi_ssvid_v20210913`

## August 3, 2021 Updates

* Automated loitering data ready for use. Previously we only had occasionally updating loitering data, now it is fully automated to update everyday (with a 72 hour delay). The detailed documentation on these new tables, and what else was updated in the logic can be found in Loitering wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/

## July 30, 2021 Updates

* The [Big Query datasets](BigQuery-datasets) page has been updated to reflect the new data staging process, which you will notice based on the series of new datasets that will be shared soon. 
* New port visit and voyage tables are ready for use as PROTOTYPES (see _Big Query datasets_ for what it means to be a prototype dataset). The detailed documentation on these new tables can be found in _Ports and Voyages_ wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current
