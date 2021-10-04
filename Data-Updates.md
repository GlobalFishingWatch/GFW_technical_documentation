**Running list of updates to note in regard to Global Fishing Watch data and data changes**

Do not forget to update the version of the tables in the [BigQuery Table Reference](https://docs.google.com/spreadsheets/d/1B8Q04rzWRdffty2gVBlHiBDS9DYZLdW8J-4B3xja5ws/edit#gid=0).

**October 4, 2021 Updates**:
* Updated wiki page on encounters dataset with BQ example code. Note the internal logic has been adjusted (using segment id). See page for details.
* The pipe_production_v20201001.published_events_loitering table has been adjusted to include the restrictions: avg_distance_from_shore_nm>=20, and segments must be good_seg and NOT Overlapping_and_short
* Latest vessel_info table is gfw_research.vi_ssvid_v20210913

**August 3, 2021 Updates**:
* Automated loitering data ready for use. Previously we only had occasionally updating loitering data, now it is fully automated to update everyday (with a 72 hour delay). The detailed documentation on these new tables, and what else was updated in the logic can be found in _Loitering_ wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current

**July 30, 2021 Updates**:
* The [Big Query datasets](BigQuery-datasets) page has been updated to reflect the new data staging process, which you will notice based on the series of new datasets that will be shared soon. 
* New port visit and voyage tables are ready for use as PROTOTYPES (see _Big Query datasets_ for what it means to be a prototype dataset). The detailed documentation on these new tables can be found in _Ports and Voyages_ wiki page. Along with new examples in the examples folder: https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current