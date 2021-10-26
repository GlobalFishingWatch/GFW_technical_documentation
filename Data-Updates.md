**Running list of updates to note in regard to Global Fishing Watch data and data changes**

Do not forget to update the version of the tables in the [BigQuery Table Reference](https://docs.google.com/spreadsheets/d/1B8Q04rzWRdffty2gVBlHiBDS9DYZLdW8J-4B3xja5ws/edit#gid=0).

**October 26, 2021 Updates**

- The VIIRS-AIS matching table is now automatically updated on a daily basis. The data itself is identical.
  - previous table (manually updated): `gfw_research.matches_raw_vbd_global_3top_v20210514`
  - new table (automatically updated): `pipe_production_v20201001.proto_matches_raw_vbd_global_3top_v20210514`
 
**October 13, 2021 Updates**

Flags of convenience have been moved to a versioned system. The original table is `gfw_research.flags_of_convenience`. This remains unchanged, reflecting the ITF list of FOCs from 2019, and will be kept for the next 30 days to give time for people to switch over their code. If you need to use this historical list of FOCs, you can reference `gfw_research.flags_of_convenience_v20190110`. For all other cases, you should now use `gfw_research.flags_of_convenience_v20211013`. FOC flags as reported by ITF are pulled from https://www.itfseafarers.org/en/focs/current-registries-listed-as-focs. We continue to remove the French and German International Ship Registries as they do not have their own unique flag State and ISO3 code. The changes between the `20190110` and `20211013` versions are as follows:
* Added Cameroon (CMR), Cook Islands (COK), Palau (PLW), Sierra Leone (SLE), St Kitts and Nevis (KNA), Tanzania (TZA), and Togo (TGO
* Removed the Netherlands Antilles (ANT)
* Overall increase from 34 to 40 flag states in the dataset

**October 4, 2021 Updates**:
* Updated wiki page on encounters dataset with BQ example code. Note the internal logic has been adjusted (using segment id). See page for details.
* The pipe_production_v20201001.published_events_loitering table has been adjusted to include the restrictions: avg_distance_from_shore_nm>=20, and segments must be good_seg and NOT Overlapping_and_short
* Latest vessel_info table is gfw_research.vi_ssvid_v20210913

**August 3, 2021 Updates**:
* Automated loitering data ready for use. Previously we only had occasionally updating loitering data, now it is fully automated to update everyday (with a 72 hour delay). The detailed documentation on these new tables, and what else was updated in the logic can be found in _Loitering_ wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current

**July 30, 2021 Updates**:
* The [Big Query datasets](BigQuery-datasets) page has been updated to reflect the new data staging process, which you will notice based on the series of new datasets that will be shared soon. 
* New port visit and voyage tables are ready for use as PROTOTYPES (see _Big Query datasets_ for what it means to be a prototype dataset). The detailed documentation on these new tables can be found in _Ports and Voyages_ wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current