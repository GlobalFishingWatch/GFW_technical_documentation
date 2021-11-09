# Standardizing Documentation

In an effort to ensure standard documentation and ownership requirements, this page was developed to detail what is required when creating a wiki page, along with dataset ownership.

# Wiki Requirements
Each unique wiki page must include the following sections:

* Owner name - so people know who they can contact

* Short paragraph overviewing the data

* Key Tables

  * Current output table
  * Previous output table version (at least one version previous with any important notes on pipeline version or other table versioning that fed into the table)
  * Source tables - These are tables that are needed to develop the output table 

* Data description (should be written to an analyst or advanced user but that is totally unfamiliar with the dataset. However in-depth explanation should be expanded upon in training slides)
  * Definition of output table- what is it, and what is it used for
  * Explanation of any restrictions on the data (e.g., if there are confidence values involved, specific vessel types, etc.)
  * Key fields 

* Caveats and Known Issues

* Updates (dated)

* Example Queries (linked to git wiki) 

* Link to training slides / loom videos / other training material on the subject (for which the owner is responsible to maintain)

* Link to repo for data source code (if pertinent)

* Links to wiki pages on related tables

# Data Ownership Requirements

* Any changes made to the table need to be reflected in the github wiki. Note: A new version of the dataset is required when a change is large enough that it affects how the data is queried or how the engineers ingest it. 

* The owner must ensure the BQ schema and data description is filled out accurately (or discuss with engineers the automation of this process), along with including the source tables in the data description. See _pipe_production_v20201001.published_events_port_visits_ for an example

* Owner must update current version in the wiki page that lists current versions of primary datasets

* Owner is responsible for up-to-date BQ examples and training slides of the dataset

* In order for a dataset to move from a prototype to published, the owner must work with the engineers to develop and document automated QA metrics for their dataset. This should be within one quarter

