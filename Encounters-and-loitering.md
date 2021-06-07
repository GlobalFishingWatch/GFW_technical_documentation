Vessel meet up at sea for a variety of reasons, including transshipment (transferring catch, crew, fuel, etc.), and these encounters between vessels are of interest for monitoring illegal behavior. 

## Key Tables

+ `pipe_production_vYYYYMMDD.published_events_encounters` - 
+ `pipe_production_vYYYYMMDD.encounter_events` -
+ `pipe_production_vYYYYMMDD.encounters` -
+ `gfw_research.loitering_events_vYYYYMMDD` -

## Data Description

When two vessels are both observed moving very slowly in close proximity for an extended period of time it is considered and **encounter**. If only one vessel is observed operating in this fashion, it is considered a **loitering event**. Loitering events are important because they may indicate that the vessel is encountering another vessel that is not broadcasting AIS. For more information, see data training slides on encounter and loitering events [HERE](https://docs.google.com/presentation/d/17ZSpH0F5sW0R7sTiNoDAm_pyUhHJeSd4fyyBFDHiAtw/edit?usp=sharing).

## Caveats & Known Issues

### Overlapping detection

Due to the definition of encounter and loitering events, loitering events can overlap with encounter events.

## Example queries
+ [encounters_1_ssvid_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_1_ssvid_v20200921.sql)  
+ [encounters_2_carriers_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_2_carriers_v20200921.sql) 
+ [encounters_3_carriers_vessel_info_v20200921.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/encounters_3_carriers_vessel_info_v20200921.sql) 
+ [loitering_v20200831.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/examples/current/loitering_v20200831.sql) 
+ [analysis-pew-ts-reports/rfmo/rfmo-yyyy](https://github.com/GlobalFishingWatch/analysis-pew-ts-reports): see `queries` folder for BQ data pull and `analysis` folder for data cleaning and analysis 