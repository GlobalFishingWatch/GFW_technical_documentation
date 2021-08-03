Vessel meet up at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and these encounters between vessels are of interest for monitoring illegal behavior. 

## Key Tables

### current version

+ `pipe_production_v20201001.published_events_encounters` - 
+ `pipe_production_v20201001.encounters` - prior to creating published events encounters table, doesn’t have speed constraint
+ `world-fishing-827.pipe_production_v20201001.loitering` - loitering table is for all vessels types with no constraints on distance from shore and duration - analyst needs to add restrictions

## Data Description

When two vessels are both observed moving very slowly in close proximity for an extended period of time it is considered and **encounter**. If only one vessel is observed operating in this fashion, it is considered a **loitering event**. Loitering events are important because they may indicate that the vessel is encountering another vessel that is not broadcasting AIS. For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

#### definitions used in RFMO Transshipment Reports 

GFW, in partnership with The Pew Charitable Trusts, has produced annual reports covering the years of 2017-2019 on transshipment activity in the the five major tuna RFMOs. These reports compare reported activity to potential transshipment, fishing, and port activity detected on AIS. The definitions of encounter and loitering events used for these reports are as follows.

+ `Encounters`: when two vessels are within 500 meters of each other for at least 2 hours and traveling at < 2 knots, while at least 10 kilometers from a coastal anchorage ([Miller et al. 2018](https://www.frontiersin.org/articles/10.3389/fmars.2018.00240/full)). 
+ `Loitering events`: when a carrier vessel travelled at speeds of < 2 knots for at least 4 hours, while at least 20 nautical miles from shore (see Miller et al. 2018 for original methodology, however the original minimum of 8 hours has been changed to 4 hours for the purposes of this study).

## Caveats & Known Issues

+ Possible spoofing can cause a false encounter event
+ Point nature of events (if poor ais transmission average location can be inconsistent with full location of tracks during time period)
+ Maintaining up to date identity changes of carrier and fishing vessels of interest 
+ Loitering table is for all vessels types with no constraints on distance from shore and duration - analyst needs to add restrictions
+ Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.

## Example queries
+ [encounters_1_ssvid_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_1_ssvid_v20200921.sql)  
+ [encounters_2_carriers_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_2_carriers_v20200921.sql) 
+ [encounters_3_carriers_vessel_info_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_3_carriers_vessel_info_v20200921.sql) 
+ [loitering_v20200831.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_v20200831.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 

## Related Content
+ [Carrier Vessel Portal (CVP)](https://globalfishingwatch.org/carrier-vessel-portal/) 