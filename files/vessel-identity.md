# Vessel Identity

Vessel identity is one of Global Fishing Watch's most important research areas.
Vessel identity workflows and datasets are used in nearly every analysis and 
product.

Global Fishing Watch gathers identity information from mainly two types of source:

   1. the Automatic Identification System (AIS) 
   2. more than 40 public registries  from countries and entities like RFMOs.
   
Other vessel characteristics are _predicted_ by our fishing models and join the identity tables along the pipeline.


## Identity in AIS data

The Automatic Identification System (AIS) is an automatic tracking system originally developed to help preventing collisions between vessels at sea. 

Vessels broadcast AIS to alert other vessels of their presence, but terrestrial and satellite receivers can also receive these messages and this data can be used to monitor vessel movements.
AIS data is at the core of Global Fishing Watch analysis pipelines, including the [AIS-based fishing effort](#fishing-effort-and-vessel-presence) displayed on our map. 

To know more about AIS, check our [AIS explainer](https://docs.google.com/presentation/d/1scUh94eyJ6rvFvCBWT3G4S-luoYLbkEcH54PYn3i3os/edit?usp=sharing) 

### Identity markers transmitted in AIS messages

AIS messages include identity information about the vessel, such as ship name, call sign, and International Maritime Organization (IMO) number, as well as an identifier known as the __Maritime Mobile Service Identity (MMSI)__.

- __MMSI__ are nine-digit numbers broadcast in AIS messages.
The first three digits of an MMSI are known as the __Maritime Identification Digits (MDIs)__ and they refer to the flag state of the vessel. 

> Note: MMSI is referred to as `ssvid` in our tables.
`ssvid` stands for “source-specific vessel identity”. In this case, the source is AIS, and `ssvid = MMSI`.

- __Shipname__ and __callsign__ can be also transmitted in AIS messages but they are optional, not every AIS-broadcasting vessel transmits them, and their transmission can be inconsistent.
Shipnames can also vary a lot in their spelling, requiring some fuzzy matching to detect if they refer to the same vessel.

- __IMO numbers__ are permanent unique identifiers that follow a vessel from construction to scrapping.
Assigned by the International Maritime Organization, IMO numbers are required for only a subset of industrial fishing vessels. 
IMO number can be transmitted along with MMSI in AIS messages but they are frequently missing.

Other variables in AIS messages can be `shiptype`, `length`, but these are often missing or wrong.

### AIS segments and `seg_id` 

A key to understanding the intricacies of AIS identity is that __spatial information and identity message are broadcast separately__, meaning that one of the first tasks of Global Fishing Watch [pipelines](#ais-pipeline) is matching the messages with identity and the messages with position from any given vessel.

This first task organizes AIS tracks into __segments__. Each segment has a `seg_id` .

The main identity marker to organize AIS messages into segments is __MMSI__: 

- Segments are created by grouping/separating __AIS time series for each MMSI__ into logical chunks of positions considering location, course, speed. 
During this process, single points are filtered out, spoofing vessels are split into separate tracks, and grouped noises such as satellite timing offsets are partially corrected. 
A new segment is created after a 24-hour gap in AIS positions. 
Most valid AIS positions are assigned a corresponding `seg_id`.

See more documentation about the "segmenter" in the [AIS pipeline](#ais-pipeline) section
These segments are the building blocks for `vessel_id`. 


### Processing identity in AIS messages and `vessel ID`

MMSIs are supposed to be unique for each vessel, but  

1. __a vessel can change MMSIs throughout its lifecycle__–for example when it’s reflagged, because the first three digits refer to the flag country (MDIs change)

2. __several vessels can broadcast the same MMSI at the same time (MMSI duplication/spoofing) or at different points in time (MMSI recycling)__. This happens for many reasons, including the fact that data entry in AIS messages is manual and error-prone. A vessel can also transmit a duplicated MMSI intentionally (spoofing). 

Because some vessel identifiers can be missing, shared by different vessels, and they can change in time, Global Fishing Watch developed __`vessel ID`__, a vessel identity variable that combines vessel information and is __specific to a time interval__.

A __`vessel ID`__ is formed by a combination of:

- the MMSI and the IMO number, when available, or
- the MMSI, callsign and shipname transmitted in AIS messages

A single MMSI can have many associated `vessel_ids`, but a `vessel_id` will only be associated with a single MMSI.

`vesselId` is used in products when searching for a vessel and to investigate any [AIS event](#ais-events) (encounters, loitering, port visits, AIS disabling events ("gaps") and voyages). 

## Uniting `vessel_ids` into single "vessel units": `vessel_record_ID` and `hull_id`

When vessel identity stories are simple, vessels have a single `vesselId`.

When vessel identity stories are more complex, a single vessel can have multiple `vesselIds`, and this is why simple calls to a single vessel in our products can return tables that have many `vesselIds` and different identity markers.

Ideally, `vesselIds` would align in time. In reality, incomplete identity fields broadcast can cause `vesselIds` to overlap in time in complex ways.

FIGURE ABOUT THEORY AND PRACTICE OF VESSELID


<!-- 
 
### Vessel record ID 

The variable that unifies different `vesselIDs` in time is the __Vessel Record ID__.

Some vessels have a single `vesselID` along their history, and therefore they lack this unifying variable. 


### Hull ID 

Hull ID is in development as a single identity marker for each vessel. 
Hull ID is not connected to registry and remains stable in time along identity changes of the same vessel.
Unlike vessel record ID, all vessels--and not only those that appear in the registries--would have a Hull ID. 



## Registry data 

Vessel registries carry important vessel identity information, like vessel characteristics, registration history, licenses to fish in certain areas, and vessel ownership data. 
Global Fishing Watch compiles vessel information from over 40 public vessel registries and matches this information with the AIS-transmitted identity fields to provide a better overview of a vessel's identity. 

More about vessel registry data __here__
-->
