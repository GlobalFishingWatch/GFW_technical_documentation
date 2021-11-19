# VMS Data Sources

As part of our Transparency Initiative, select countries have agreed to share their proprietary Vessel Monitoring Systems (VMS) data with GFW. These data are distinct from the AIS data and their format and contents vary by country. Below is an overview of the data available in BigQuery for each VMS partner.

> Each data sharing agreement between GFW and country partners has specific restrictions on data access and sharing. In general, GFW  must request written permission to use the VMS data for use cases other than the public map and analyses for the country partner. For more information, see the [Overview of VMS Data and Use Policy](https://docs.google.com/document/d/1J6CFWGwILjlBwuuv33DqjG9iQ_0eOIy-5U-o_RkGKtg/edit?usp=sharing) doc.

## Indonesia

+ **Data description:** The Indonesian VMS data is provided by the KKP and includes data for all Indonesian flagged vessels and vessels authorized to fish in Indonesia's EEZ.
  + **Date range**: `2014-01-01` - `2020-07-03`
  + **Message frequency**: Messages are received in 5 - 60 minute intervals
  + **Vessel classes**: Anchored gillnet, Basic longline, Cast nets, Fish net/dragnet, Handline, Handline tuna, Lampara seine nets, Longline tuna, Oceanic gillnet, Pole and line, Pots, Purse seine small pelagics, Purse seine big pelagics with one boat, Purse seine group (2 boat) big pelagics, purse seine group (2 boat) small pelagics, Shrimp net, Squid hooking, Stick held lift net, Transporter
  + **Identity information**: transmitter_no (ssvid) and shipname
+ **BigQuery dataset:** `pipe_indonesia_production_v20200320`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Wildan (`@wildan`; wildan@globalfishingwatch.org) / Liva (`@liva`; liva@globalfishingwatch.org)
+ **Caveats & known issues:** Due to the data license agreement, the Indonesian VMS data cannot be used without the approval of the KKP as the data owner, at this time the data has been stopped and is in the process of extending the cooperation.


## Peru

+ **Data description:** The Peru VMS data is provided by...
  + **Date range**: `2012-01-01` - present
  + **Message frequency**: two ping rates, 20 minutes for artisanal vessels and 10 minutos for industrial vessels
  + **Vessel classes**: (fish_factory, tanker ) and (trawlers, purse_seines, set_gillnets, squid_jigger, set_longlines, pots_and_traps, bottom_longlines, longline_handline, drifting_longlines) 
  + **Identity information**: Shipname and PLATE
+ **BigQuery dataset:** `pipe_peru_production_v20200324`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Eloy (`@Eloy Aroni`; eloy@globalfishingwatch.org)
+ **Caveats & known issues**: The fishing model is not working good for artisanal fleets (squid_jigger and longline_handline)

## Chile

+ **Data description:** The Chile VMS data is provided by NAF messages, it is a way they had to export it from CLS Themesis Systems.
  + **Date range**: `2019-02-01` - present
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_chile_production_v20190618`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: 
+ **Caveats & known issues:**

## Panama 

+ **Data description:** The Panama VMS data is provided by NAF messages, it is a way they had to export it from CLS Themesis Systems.
  + **Date range**: `2012-01-01` - present
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_panama_production_v20190517`
  + **Vessel identity table(s)**: `vessel_info`  
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: 
+ **Caveats & known issues:**

## Brazil 

+ **Data description:** The Brazil VMS data is provided by two vms providers Onixsat and Ariasat companies...
  + **Date range**: `2016-01-01` - present
  + **Message frequency**: two ping rates, 20 minutes for tuna longliners and one hour for the others
  + **Vessel classes**: Trawlers (bottom, single net, doble rig net, pair trawl), gillnet (bottom and surface), line (surface longline, bottom longline, handline), purse seine, pots and traps (lobster, octopus and red snapper) 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_brazil_production_v20210910`
  + **Vessel identity table(s)**:  `world-fishing-827.VMS_Brazil.brazil_registry_2021_09_09`
  + **Identity information**:
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (`@Claudino`; claudino@globalfishingwatch.org)
+ **Caveats & known issues**: Ariasat data source has no speed and course information, need to calculate. and also ariasat has no historical information.

## Costa Rica 

+ **Data description:** The Costa Rica VMS data is providade by INCOPESCA
  + **Date range**:
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_costarica_production_v20210830`
  + **Vessel identity table(s)**: 
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (@Claudino; claudino@globalfishingwatch.org)
+ **Caveats & known issues:**

## Ecuador 

+ **Data description:** The Ecuador VMS data is provided by DIRNEA
  + **Date range**:  2020-08-14 to present
  + **Message frequency**: one hour according regulations
  + **Vessel classes**: Industrial
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_ecuador_production_v20210612`
  + **Vessel identity table(s)**: `world-fishing-827.VMS_Ecuador.manual_registry_list_q32020` static information from 2020
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Claudino (@Claudino; claudino@globalfishingwatch.org)
+ **Caveats & known issues:** 
  + data show some positions with wrong timestamp
  + after march 2021 part of wrong timestamps gone and start appears gaps
  + there are ~4000 artisanal vessels and only 400 regularly broadcasting. Due low ping rates artisanal are not show in map 3.0
  + vessel categories are created based on matricula pattern, there are 14 different categories including non-fishing vessels

## Mexico 

+ **Data description:** The Mexico VMS data is...
  + **Date range**: 
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_mexico_oceana_production_v20210830`
  + **Vessel identity table(s)**: 
+ **Data redacted from GFW map (if applicable)**:  
+ **GFW lead**: Esteban (@Esteban; esteban@globalfishingwatch.org)
+ **Caveats & known issues:**

## Namibia

+ **Data description:** The Namibian data is provided by Google Drive and it is not a good system because they do a lot of manual work on their side.
  + **Date range**: `2014-01-01` - present
  + **Message frequency**:
  + **Vessel classes**: 
  + **Identity information**: 
+ **BigQuery dataset:** `pipe_namibia_production_v20190207`
  + **Vessel identity table(s)**: `vessel_info`
+ **Data redacted from GFW map (if applicable)**:  
+ **Caveats & known issues:**