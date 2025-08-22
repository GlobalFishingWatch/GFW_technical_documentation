# Vessel Identity

Vessel identity is one of Global Fishing Watch's most important research areas.
Vessel identity workflows and datasets are used in nearly every analysis and 
product.

Global Fishing Watch gathers identity information from mainly two types of source, (i) the Automatic Identification System (AIS) and (ii) more than 40 public registries. 
Other vessel characteristics are _predicted_ by our fishing models.
Before talking about our tables and workflows, here's a brief description about __identity markers__ in all tables and pipelines


## Automatic Identification System (AIS)

The Automatic Identification System (AIS) is an automatic tracking system originally developed to help preventing collisions between vessels at sea. 
AIS is at the core of Global Fishing Watch analysis pipelines, including the AIS-based fishing effort calculation displayed on our map. 
Vessels broadcast AIS to alert other vessels of their presence, but terrestrial and satellite receivers can also receive these messages and monitor vessel movements.

<!--link to more about AIS?-->

AIS messages include identity information about the vessel, such as ship name, call sign, and International Maritime Organization (IMO) number, as well as an identifier known as the __Maritime Mobile Service Identity (MMSI)__.

- __MMSI__ are nine-digit numbers broadcast in AIS messages. MMSIs are supposed to be unique for each vessel, but 
  1. a vessel can change MMSIs throughout its lifecycle–for example when it’s reflagged, because the first three digits refer to the flag country 
  2. several vessels can broadcast the same MMSI at the same time. 
  This happens for many reasons, including the fact that data entry in AIS messages is manual.

- __Shipname__ and __callsign__ can be also transmitted in AIS messages but they are optional, not every AIS-broadcasting vessel transmits them, and their transmission can be inconsistent.
Shipnames can also vary a lot in their spelling, requiring some fuzzy matching to detect if they refer to the same vessel.

- __IMO numbers__ are permanent unique identifiers that follow a vessel from construction to scrapping.
Assigned by the International Maritime Organization, IMO numbers are required for only a subset of industrial fishing vessels. 
IMO number can be transmitted along with MMSI in AIS messages but they are frequently missing.

These identity markers are often the starting point of any inquiry around vessel identity. 
However, due to their characteristics, none of these identifiers should be interpreted as the sole identity source for a vessel. 
Global Fishing Watch does extensive work to analyze and gather all the information available for a given vessel into cohesive sets. 

> Note: MMSI is referred to as `ssvid` in our tables.
`ssvid` stands for “source-specific vessel identity”. In this case, the source is AIS, and `ssvid = MMSI`.

## Registry data 

Vessel registries carry important vessel identity information, like vessel characteristics, registration history, licenses to fish in certain areas, and vessel ownership data. 
Global Fishing Watch compiles vessel information from over 40 public vessel registries and matches this information with the AIS-transmitted identity fields to provide a better overview of a vessel's identity. 

More about vessel registry data __here__

## Internal identity markers

### __`vesselId`__

Because some vessel identifiers can be missing, or shared by different vessels, and they can change in time, Global Fishing Watch developed __`vesselId`__, a vessel identity variable that combines vessel information and is specific to a specific time interval.

A __`vesselId`__ is formed by a combination of the MMSI and the IMO number, when available, or by the MMSI, callsign and shipname transmitted in AIS messages. 

> Each __`vesselId`__ is associated to a single vessel __at a specific period of time__.
A single vessel can have several __`vesselId`__ in time, and this is why simple calls to a single vessel can return tables that have many `vesselIds` and different identity markers in time.


### Vessel record ID 

The variable that unifies different `vesselIDs` in time is the __Vessel Record ID__.

Some vessels have a single vesselID along their history, and therefore they lack this unifying variable. 


### Hull ID 

Hull ID is in development as a single identity marker for each vessel. 
Hull ID is not connected to registry and remains stable in time along identity changes of the same vessel.
Unlike vessel record ID, all vessels--and not only those that appear in the registries--would have a Hull ID. 

<!--this needs revision and more information, but maybe not too much-->

