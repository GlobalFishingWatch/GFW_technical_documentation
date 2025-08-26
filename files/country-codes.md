# Country codes

GFW maintains a table which links MID codes (the first three digits of the MMSI) to alternative country/flag designations and EEZ characteristics. This table is also used to link the numeric `eez_id` to an EEZ name/label.  
 
The for long flag State labels `country_name_gfw` is the GFW approved naming convention and should be used in most cases.

## Key Table

`gfw_research.country_codes`

## Data Description

This table provide a means of linking MID code values to ISO3 country codes and corresponding country names. It is also possible to ignore the MID code field and simply use this table to map ISO3 to country name, to produce easier to interpret country labels. The table also includes some specific characteristics of each country/flag including their continent, if they are member of the EU, if they are considered a flag of convenience by the ITF, and the area of their EEZ.   
This table also includes the `eez_id` a numeric value that you'll find in various GFW tables including the vessel info table. Using the country_codes table it is possible map this numeric EEZ_id to and ISO3 or country name.  
  
A critical field is the `country_name_gfw` field which represents the GFW approved naming convention. In all external GFW products it is important to use this naming convention.  

## Caveats and Known Issues
Just a note that naming is very important and care should be taken with labeling vessels or regions with flag State. In nearly all cases, use `country_name_gfw` when providing longer flag State names.


## Example Query

[adding_country_code_names.sql](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/blob/master/queries/adding_country_code_names.sql)


