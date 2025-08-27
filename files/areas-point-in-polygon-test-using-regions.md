## Point-in-polygon Test Using Regions

Owner: Hannah Linder
Last edited time: 23 de mayo de 2024 2:14
Created time: 1 de marzo de 2024 13:10


[WORK IN PROGRESS]

This guide describes how to use the regions tables that are present in bigquery to do ad-hoc analysis to determine which of a set of lat/lon values are inside the set of regions.

### Architecture

We use sets of polygons in the data pipeline to determine when events (fishing events, encounter events, etc) are located inside pre-determined polygons, such as MPAs, EEZs and the like. These sets of polygons are called “regions” and all the code that is used to create and use them is in the pipe-regions [github repo](https://github.com/GlobalFishingWatch/pipe-regions).

Each layer is loaded into bigquery in a separate layer. The production dataset is `world-fishing-827.pipe_regions_layers`. For example, the WDPA layer containing all the marine MPAs is in `world-fishing-827.pipe_regions_layers.WDPA_Mar_2022_Marine`. All the associated fields from the original source are present in that table, so if you want to filter to a subset you could do something like `WHERE status="Proposed"`.

BTW documentation for all these layers is  [right here in notion](../Spatial%20Region%20Sources%20f8a8713c76e541bf93859581a271d10e.md).

### Adding Regions

Let’s say you have a big query table full of records with positions (lat, lon) and for each position record you want to get all of the region polygons that contain the position and put that in a new big query table

The following query will give you a new table with all the same rows as the source_table, with one additional field called regions which is a JSON string with the ids of the containing regions. 

```sql
CREATE TEMP FUNCTION s2_level() AS (10);
WITH
positions as (
  SELECT
    *,
  FROM `{{ source_table }}`
),
regions as (
  SELECT
    *,
  FROM `{{ regions_table }}`
),
gridded_positions as (
  SELECT
    *,
    S2_CELLIDFROMPOINT(geo, s2_level()) as s2_cell
  FROM positions
),
gridded_regions as (
  SELECT
    * except(s2_cells),
  FROM regions
  CROSS JOIN UNNEST (s2_cells) as s2_cell
),
create_position_region_matches as (
  SELECT
    msgid, id, layer,
  FROM gridded_positions p
  JOIN gridded_regions r
  ON p.s2_cell = r.s2_cell
  WHERE ST_INTERSECTS(p.geo, r.geo)
),
position_by_layer_by_id as (
  SELECT
    msgid, layer, id
  FROM create_position_region_matches
  GROUP BY msgid, layer, id
),
position_by_layer as (
  SELECT
    msgid, 
    CONCAT('"', layer, '":' ,TO_JSON_STRING(array_agg(id))) as json_fragment
  FROM position_by_layer_by_id
  GROUP BY msgid, layer
),
position_with_region as (
  SELECRT
    msgid,
    PARSE_JSON(CONCAT( "{", STRING_AGG(json_fragment, ","), "}")) as regions
  FROM position_by_layer
  GROUP BY msgid
),
full_position_with_region as (
  SELECT
    *
  FROM positions
  LEFT JOIN position_with_region
  USING (msgid)
)

SELECT * FROM full_position_with_region
```
