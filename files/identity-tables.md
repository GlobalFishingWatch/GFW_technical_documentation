# Core AIS vessel ID table: `vessel_info`

The table that shows all information for each `vessel id` is:

`global-fishing-watch.pipe_ais_v3_published.vessel_info`

> __Note:__ As of August 4 2025, all tables in `pipe_ais_v3_published` were migrated from `world-fishing-827` to `global-fishing-watch`.

This table has __ONE ROW PER `vesselID`__ and includes: 

- `ssvid` (MMSI) associated with the `vesselId`
- Timestamps (first and last messages in the segment)
- Message and position counts
- Shipname, callsign, IMO number, raw and normalized (prefixed with `n_`). These six columns have `value`, `counts` and `freq` fields to accomodate for multiple values per `vesselId`
- Length and width of the vessel, when available, also with `value`, `counts` and `freq` values. 


> Note: Importantly, this table has no noise filters applied.


