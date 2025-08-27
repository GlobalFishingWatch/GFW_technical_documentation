---
date-modified: last-modified
---

# Updating Layers in pipe_regions


<!--Owner: Hannah Linder
Last edited time: 23 de mayo de 2024 2:14
Created time: 1 de marzo de 2024 13:10-->


Use Case: Replacing a region layer with a different version of the shapefile. See: [ENG-395](https://globalfishingwatch.atlassian.net/browse/ENG-395) - CCSBT region for events Done

We will be updating this layer: [CCSBT](https://www.notion.so/globalfishingwatch/CCSBT-3bcd1a10e9534556a02a0cfcf5503e5a)

## Steps

1. get the new shapefile. We want this one. [https://drive.google.com/drive/folders/17ZlvqBEaLDBsUmq-QjdPvD6ge3sNj-k1?usp=sharing](https://drive.google.com/drive/folders/17ZlvqBEaLDBsUmq-QjdPvD6ge3sNj-k1?usp=sharing) . Looks like there are two versions in there, let’s look at them in QGIS
2. Look at the shapefile in qgis. Check for edge cases - latitude values outside -90, 90. Lon values outside -180, 180, really big polygons that are more than 180 degrees wide, anything out of place. Might need to clip and/or split into multiple pieces. Looks like CCSBT_all_SA is one big polygon and CCSBT_SA_multi covers the same area but is split into multiple polygons. For pipe-regions, more small polygons is better than one big one, since we are going to be chopping them up into small pieces anyway.
3. Pack up the shapefile into a zip file with all the files at the root of the directory structure inside the zip (no subdirectories).

```python
$ ls
CCSBT_SA_Multi.cpg	CCSBT_SA_Multi.qpj	
CCSBT_SA_Multi.dbf	CCSBT_SA_Multi.shp
CCSBT_SA_Multi.prj	CCSBT_SA_Multi.shx
$ zip CCSBT_SA_Multi.shp.zip *
updating: CCSBT_SA_Multi.cpg (stored 0%)
updating: CCSBT_SA_Multi.dbf (deflated 97%)
updating: CCSBT_SA_Multi.prj (deflated 15%)
updating: CCSBT_SA_Multi.qpj (deflated 39%)
updating: CCSBT_SA_Multi.shp (deflated 82%)
updating: CCSBT_SA_Multi.shx (deflated 61%)
$ 
```

1. This will be the “normalized” version of the shapefile. In this case we don’t know where the shapefile came from originally, but if we had an original version we would store that in GCS also. Copy the zipped shapefile to GCS. Lets just use the same file as both the original and normalized version

```python
$ gsutil cp CCSBT_SA_Multi.shp.zip gs://pipe-regions-layers-us-central1/rfmo_ccsbt/original/
Copying file://CCSBT_SA_Multi.shp.zip [Content-Type=application/zip]...
/ [1 files][  3.7 KiB/  3.7 KiB]                                                
Operation completed over 1 objects/3.7 KiB.                                      
$ gsutil cp CCSBT_SA_Multi.shp.zip gs://pipe-regions-layers-us-central1/rfmo_ccsbt/
Copying file://CCSBT_SA_Multi.shp.zip [Content-Type=application/zip]...
/ [1 files][  3.7 KiB/  3.7 KiB]                                                
Operation completed over 1 objects/3.7 KiB.                                      
$
```

1. Now we need to download the layers.yaml file from GCS. We download the current file using the following command:

```python
gsutil cp gs://pipe-regions-layers-us-central1/layers.yaml .
```

1. We update the layer file to point to the new shapefile. In this case, let’s keep the old layer in case we want to use it later, so let’s just rename the ID value. Editing the file layers.yaml we change from this

```python
...
  -
    layer_name: rfmo
    id_value: CCSBT
    shp_file : rfmo_ccsbt/proposedArea_CCSBT_1.shp.zip
    doc: https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/459833345/CCSBT
    publish_api: True
    pg_table: rfmo-areas
    append_to_table: True

```

to this

...

```python
...
  -
    layer_name: rfmo
    id_value: CCSBT-proposed
    shp_file : rfmo_ccsbt/proposedArea_CCSBT_1.shp.zip
    doc: https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/459833345/CCSBT
    publish_api: True
    pg_table: rfmo-areas
    append_to_table: True
  -
    layer_name: rfmo
    id_value: CCSBT
    shp_file : rfmo_ccsbt/CCSBT_SA_Multi.shp.zip
    doc: https://globalfishingwatch.atlassian.net/wiki/spaces/TD/pages/459833345/CCSBT
    publish_api: True
    pg_table: rfmo-areas
    append_to_table: True
  -
```

Each key of the yaml means:

- layer_name: Name of the layer. It will be used to name the layer in BQ
- id_value: This param is optional. In the case of rfmo, we need to define it because we will generate a RFMO layer using different layers. In the rest of the layers (mpa, eez, etc) is not neccesary.
- id_field: This param is optional. For the rest of the layers, with this field we define the name of the field that we will use to obtain the id of each region.
- shp_file: Path in the bucket that the shapefile exists
- doc: Url of the doc
- publish_api: Boolean. True if you want that the process import the data in postgres
- pg_table: String. Required if publish_api is true. Name of the table in postgres
- append_to_table: Boolean. Required if publish_api is true. If false, the sql will replace the table, if not it will delete the rows with the same id of id_value and insert the new ones.
- jq_query: String. Optional. This field lets to filter the original geojson file generated from the shapefile with a jq query and only import the regions filtered. Example: select(.properties.category_n == "IUCN MPA"). We use this in protected seas to only import regions with category = "IUCN MPA"

7. Now we upload the file again to GCS

```python
gsutil cp layers.yaml gs://pipe-regions-layers-us-central1/layers.yaml
```

8. Now it's time to update the regions tables in bigquery. Since we only changed 2 shapefiles, we only need to update those. We could re-run all the layers, but that takes a while, so for this we will execute the command defining the --layer_names_to_ingest=rfmo . With this option we only update the rfmos layers but we will regenerate the view with all layers.

```python
docker compose run --rm pipeline \
  -v \
  ingest_layers \
  --layer_list_file=layers.yaml \
  --gcs_root=pipe-regions-layers-us-central1 \
  --bq_dataset=pipe_regions_layers \
  --pg_instance=dataset-data-production \
  --pg_database=postgres \
  --bq_view=event_regions \
  --layer_names_to_ingest=rfmo  
```

9. The last step is to edit the documentation page for this layer to describe what we did. Now there are 2 versions of this layer available, one with id

CCSBT which is the new one, and one with id CCSBT-proposed which is the old one.

Edit this page: [CCSBT](https://www.notion.so/globalfishingwatch/CCSBT-3bcd1a10e9534556a02a0cfcf5503e5a)

And then we’re done!
