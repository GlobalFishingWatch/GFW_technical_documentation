# Wiki Documentation Standard

## Page Requirements

Each unique wiki page describing a GFW dataset must include the below sections. For long sections, the use of sub-headers within  is encouraged to improve organization.

* `Data Overview` - Short paragraph providing a high level description of the dataset
    * This section should include the `Owner name` - the GFW staff member responsible for the relevant dataset (see [Data Ownership Requirements](#data-ownership-requirements))

* `Key Tables`

  * Current output table(s) and version
  * To understand what table is related to this one in the previous pipeline version, please refer to the specific version mapping page
  * In order to undertand which are the Precursor tables (when appropriate), meaning the intermediate tables used in the production of the output tables, pleae refer to Lineage tab in BQ, more information [here](https://cloud.google.com/dataplex/docs/track-lineage#view_graph).

* `Data Description` - Detailed but concise description of the data and methods. Should be written to an analyst or advanced user but someone totally unfamiliar with the dataset. This documentation should be also in BQ, if you can't find that this needs to be reported to the Data team.   
  * Definition of output table - what is it, and what is it used for
  * Explanation of any restrictions on the data (e.g., if there are confidence values involved, specific vessel types, etc.)
  * Key fields - Specific data fields in the key tables that are particularly important (e.g. `on_fishing_list_best`, `nnet_score`, `good_seg`, etc.) 

* `Caveats and Known Issues` - Descriptions of any important caveats, limitations, or known issues (e.g. minor bugs) to be aware of when using the data.

* `Example Queries` - Bulleted list of links to `.sql` files stored in the [`queries/examples/current`](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/tree/master/queries/examples/current) folder of this same GitHub repo

* `Links` 
    * Training slides / loom videos / other training material on the subject (for which the owner is responsible to maintain)
    * GitHub repo for data source code (if pertinent)
    * Links to related wiki pages

* `Updates` - Dated list of updates to the key table(s). 

* `Difference with data in API, Data Download Portal or Map` - Add links or describe what is the difference with the data in any of the public products. For example for Sentinel 2, we can link to the [future DDP page](https://docs.google.com/document/d/1uBobfopK7c12GnDOhKzNvYM9kHPRYO4P9NCl_bGztvM/edit?tab=t.0#heading=h.vasy5zi4hwfb).

# Data Ownership Requirements

* Any changes to key tables must be reflected in the GitHub wiki.  

* The owner must ensure the that key tables all have informative schemas and descriptions
    * Include all source tables in the table description. See `pipe_production_v20201001.published_events_port_visits_` for an example
    * This process should be automated whenever possible 

* Owner must update current version in the wiki page that lists current versions of primary datasets

* Owner is responsible for maintaining up-to-date example queries for the dataset


