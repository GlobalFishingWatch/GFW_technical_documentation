# VMS Data Sources

Select countries have agreed to share their proprietary Vessel Monitoring Systems (VMS) data with GFW. These data are distinct from the AIS data and their format and contents vary by country. 

> Each data sharing agreement between GFW and country partners has specific restrictions on data access and sharing. In general, GFW must request written permission to use the VMS data for use cases other than the public map and analyses for the country partner. For more information, see the [Overview of VMS Data and Use Policy](https://docs.google.com/document/d/1J6CFWGwILjlBwuuv33DqjG9iQ_0eOIy-5U-o_RkGKtg/edit?usp=sharing) doc.

## Unified VMS Pipeline

The GFW platform currently includes VMS data from 10+ countries. Each VMS dataset comes from a separate data feed and receives it's own BigQuery dataset. As of version 3 (`v3`) of the GFW vessel tracking pipeline, each VMS dataset is stored in two datasets labelled `pipe_vms_[iso3]_v3_[internal/published]`. Additionally, all VMS datasets are combined into a single "unified" VMS pipeline stored in the BQ datasets `pipe_vms_v3_internal` and `pipe_vms_v3_published`.

## Source VMS Datasets

### Indonesia

+ **Data description:** The Indonesian VMS data is provided by the KKP and includes data for all Indonesian flagged vessels and vessels authorized to fish in Indonesia's EEZ.
  + **Date range**: `2014-01-01` - `2020-07-03` NOTE: We will not be receiving new data in the near future
  + **Message frequency**: Messages are received in 5 - 60 minute intervals
  + **Vessel classes**: Anchored gillnet, Basic longline, Cast nets, Fish net/dragnet, Handline, Handline tuna, Lampara seine nets, Longline tuna, Oceanic gillnet, Pole and line, Pots, Purse seine small pelagics, Purse seine big pelagics with one boat, Purse seine group (2 boat) big pelagics, purse seine group (2 boat) small pelagics, Shrimp net, Squid hooking, Stick held lift net, Transporter
  + **Identity information**: transmitter_no (ssvid) and shipname
+ **BigQuery dataset:** `pipe_indonesia_production_v20200320`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Wildan (`@wildan`; wildan@globalfishingwatch.org) / Liva (`@liva`; liva@globalfishingwatch.org)
+ **Caveats & known issues:** Due to the data license agreement, the Indonesian VMS data cannot be used without the approval of the KKP as the data owner, at this time the data has been stopped and is in the process of extending the cooperation.


### Peru

+ **Data description:** The Peru VMS data is provided by API
  + **Date range**: `2012-01-01` - present
  + **Message frequency**: two ping rates, 20 minutes for artisanal vessels and 10 minutos for industrial vessels
  + **Vessel classes**: (fish_factory, tanker ) and (trawlers, purse_seines, set_gillnets, squid_jigger, set_longlines, pots_and_traps, bottom_longlines, longline_handline, drifting_longlines) 
  + **Identity information**: Shipname and PLATE
+ **BigQuery dataset:** `pipe_peru_production_v20211126`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**: vessel info (name,plate,length,gear, fishery type) is publish same in both private and public workspace
+ **GFW lead**:
+ **Caveats & known issues**: The fishing model is not working good for artisanal fleets (squid_jigger and longline_handline)

### Chile

+ **Data description:** The Chile VMS data is provided by NAF messages, it is a way they had to export it from CLS Themesis Systems.
  + **Date range**: `2019-02-01` - present
  + **Message frequency**: By law fishing, transport and aquaculture vessels every 15 min (BQ - Artisanal: 19min, Industrial: 10min, Aquaculture: 17min, Transport: 16min). Purse Seine fishing vessels every 8 min (BQ: 17min)
  + **Vessel classes**: Artisanal, Industrial, Transport, Aquaculture
  + **Identity information**: shipname, ssvid, callsign, vessel_id
+ **BigQuery dataset:** `pipe_chile_production_v20211126`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**: Public Map only shows industrial and artisanal vessels. Private one also includes aquaculture and transport vessels. Both spaces don’t have any identifying info for each vessel, only flag, first and last transmission, and fleet.
+ **GFW lead**:
+ **Caveats & known issues**: Slight inconsistencies between BQ and public map tracks for certain vessels. Some ssvids have multiple callsigns associated. Sometimes all are the same vessel and others they are different vessels (often times with the same ship name). No unique vessel identifier found to distinguish these cases. Noted pipe_chile_production_v20211110 is available (created at a newer date, but with no 2021 data. Have asked engineering about it).

### Panama 

+ **Data description:** The Panama VMS data is provided by NAF messages, it is a way they had to export it from CLS Themesis Systems.
  + **Date range**: `2012-01-01` - present
  + **Message frequency**: 1 hours
  + **Vessel classes**: drifting_longlines, set_longline, purse_seines, supply_vessel, tanker, reefer
  + **Identity information**: IMO and Callsign
+ **BigQuery dataset:** `pipe_panama_production_v20211126`
  + **Vessel identity table(s)**: `vessel_info`  
+ **Data redacted from GFW map (if applicable)**: vessel info is not displayed on the public workspace 
+ **GFW lead**:
+ **Caveats & known issues:** 

### Norway

+ **Data description:** 
  + **Date range**: 
  + **Message frequency**: 
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_norway_production_v20220112`
  + **Vessel identity table(s)**: 
+ **Data redacted from GFW map (if applicable)**:
+ **GFW lead**:
+ **Caveats & known issues:** 

### Brazil 

+ **Data description:** The Brazil VMS data is provided by two vms providers Onixsat and Ariasat companies...
  + **Date range**: `2016-01-01` - present
  + **Message frequency**: two ping rates, 20 minutes for tuna longliners and one hour for the others
  + **Vessel classes**: Trawlers (bottom, single net, doble rig net, pair trawl), gillnet (bottom and surface), line (surface longline, bottom longline, handline), purse seine, pots and traps (lobster, octopus and red snapper) 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_brazil_production_v20211126`
  + **Vessel identity table(s)**:  `world-fishing-827.VMS_Brazil.brazil_registry_2021_09_09`
  + **Identity information**:
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (`@Claudino`; claudino@globalfishingwatch.org)
+ **Caveats & known issues**: Ariasat data source has no speed and course information, need to calculate. and also ariasat has no historical information.

### Costa Rica 

+ **Data description:** The Costa Rica VMS data is providade by INCOPESCA
  + **Date range**: 2021-04-08 to present
  + **Message frequency**: 1 - 2 hours
  + **Vessel classes**: Sardineros, Avanzados, atuneros 
  + **Identity information**: NA
+ **BigQuery dataset:** `pipe_costarica_production_v20211126`
  + **Vessel identity table(s)**: NA
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (@Claudino; claudino@globalfishingwatch.org)
+ **Caveats & known issues:**

### Ecuador 

+ **Data description:** The Ecuador VMS data is provided by DIRNEA
  + **Date range**:  2020-08-14 to present
  + **Message frequency**: one hour according regulations
  + **Vessel classes**: Industrial
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_ecuador_production_v20211126`
  + **Vessel identity table(s)**: `world-fishing-827.VMS_Ecuador.manual_registry_list_q32020` static information from 2020
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (@Claudino; claudino@globalfishingwatch.org)
+ **Caveats & known issues:** 
  + data show some positions with wrong timestamp
  + after march 2021 part of wrong timestamps gone and start appears gaps
  + there are ~4000 artisanal vessels and only 400 regularly broadcasting. Due low ping rates artisanal are not show in map 3.0
  + vessel categories are created based on matricula pattern, there are 14 different categories including non-fishing vessels

### Belize 

+ **Data description:** The Belize VMS data is provided by PoleStar
  + **Date range**:  2021-08-01 to present
  + **Message frequency**: TBD
  + **Vessel classes**: TBD
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_belize_production_v20220304`
  + **Vessel identity table(s)**: TBD
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (@Claudino; claudino@globalfishingwatch.org)
+ **Caveats & known issues:** TBD

### Mexico - NOT STABLE. CONTACT ENGINEER TEAM BEFORE USE.

+ **Data description:** The Mexico VMS data provider is Orbcomm and Conapesca monitors data and alerts through a system called SISMEP. We currently don’t have an agreement signed and don’t receive data directly from Conapesca. However, we have two sets of data. One was requested by Oceana early 2021 and made public as a csv. The other is from downloading csv files directly from the Conapesca website. In march of 2021 Conapesca decided to publish and make all VMS data public through their open data portal.
  + **Date range**: Oceana data: `2013-01-01 - 2020-12-31` Conapesca data: `2011-01-01 - 2021-02-28`
  + **Message frequency**: By law pings should be every hour. BQ for Oceana data: 107min. BQ for Conapesca data: 76min
  + **Vessel classes**: None given. But primarily industrial fishing vessels. VMS required for vessels with > 80HP and length > 10.5 meters
  + **Identity information**: RNP/ssvid, NOMBRE/shipname,vessel_id
+ **BigQuery dataset:** Oceana: `pipe_mexico_oceana_production_v20210811` Conapesca: `VMS_Mexico.raw_mexican_open_data_partitioned`
  + **Vessel identity table(s)**: None yet. Messages scored or original data sets could be used
+ **Data redacted from GFW map (if applicable)**: Not currently on the public map. Oceana data is displayed in the private workspace
+ **GFW lead**:
+ **Caveats & known issues**: Previously identified and currently testing for speed discrepancies/unknown units. Conapesca data has several issues with timestamps being incomplete or in inconsistent format. However, it has a lot more positions than data given to Oceana. BQ tables and private workspace based on Oceana data, shows significant inaccuracies in non-fishing activity being scored as fishing

### Namibia NOT STABLE. CONTACT ENGINEER TEAM BEFORE USE.

+ **Data description:** The Namibian data is provided by Google Drive and it is not a good system because they do a lot of manual work on their side. 
  + **Date range**: `2014-01-01` - present
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **Caveats & known issues:**
