# (PART) Training materials {-} 
# GFW Data Training Materials

https://docs.google.com/document/d/1mvpQa01gRTvEKKCcrqCwinapJ_NKB66MlLmxa1RGQrM/edit?tab=t.0

**Purpose:** The [**GFW Data Training**](https://drive.google.com/drive/u/0/folders/0AIfysivHdyTgUk9PVA) Shared Drive contains training materials for using GFW data. 

**Uses:** 

* Reference for current GFW staff and partners  
* Onboarding for new GFW staff and partners  
* Periodic training meetings and workshops

**Contents:**

* [**Training Slide Decks**](https://drive.google.com/drive/u/0/folders/117s4rmWngMdB3BqySBkrpdYA04MZk5lY): This folder contains numerous slide decks focused on different GFW datasets. The most up-to-date training slide decks are in [**training\_slides\_current**](https://drive.google.com/drive/u/0/folders/1X_bzgvN4WzNRIWl_slUsXVpjkmgvJprg). Quarterly versions of the complete training slide deck set are saved in [**training\_slides\_archive**](https://drive.google.com/drive/u/0/folders/1v8euqxFUTVM6PIoPEEG_ey7Fe3UPpnoK) following the naming convention **training\_slides\_YYYY\_Q**  
    
* [**Example Queries**](https://drive.google.com/drive/u/0/folders/1hLvvUpCRVTQa-ZPqqY3DJfGIyijwc9zg): This folder contains a Google Doc with descriptions and links to the most current example queries for working with GFW data.   
  * All queries are saved as SQL files on GitHub in the queries/examples/current folder in the **bigquery-documentation-wf827** repo.  
  * Old versions of example queries are stored in the queries/examples/archive folder  
* [**BigQuery Table Reference**](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827/wiki/BigQuery-Table-Reference): Table containing current best list of datasets and tables to use for analysis  
    
* [**Training Workshops**](https://drive.google.com/drive/u/0/folders/1cZMaSkVSJcAlEm3qKg5Gj-JvEn6_xmbj): This folder contains various admin materials for planning GFW training workshops

**Updating**: As new versions of datasets and tables are produced, GFW staff should periodically update the GFW data training materials for their assigned datasets (see **Assignments**) accordingly. This process includes the following:

1. GFW staff add/update example queries.   
   1. The old versions of the queries should be moved to the queries/examples/archive folder in the [**bigquery-documentation-wf827**](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827) repo.  
   2. The new versions of the queries should be added to the queries/examples/current folder using the options below. Queries should be clearly named according to what they do and be date-versioned with the **vYYYYMMDD** format (e.g. **vessel\_track\_with\_fishing\_hours\_vYYYYMMDD.sql**).

* Option A: Clone the **bigquery-documentation-wf827** repo and push new queries to the correct folder (make sure to pull first if not cloning for the first time\!)  
* Option B: Upload files to the **bigquery-documentation-wf827** repo  
  * Step 1: Save example query locally as a .sql file  
  * Step 2: Navigate to the current example query folder **bigquery-documentation-wf827/queries/examples/current**  
  * Step 3: Click **Upload files**  
  * Step 4: Drag/drop file, add a commit message, and press **Commit**  
* Option C: Create a new file directly in the **bigquery-documentation-wf827** repo on github.com  
  * Step 1: Navigate to the example query folder for the current quarter in **bigquery-documentation-wf827/queries/examples/current**  
  * Step 2: Click **Create new file**  
  * Step 3: Name query in the **Name your file…** box (make sure to use .sql suffix)   
  * Step 4: Copy/paste query into the **\<\>Edit new file** window  
  * Step 5: Add a commit message and press **Commit**

2. GFW staff update their assigned slide decks in [**training\_slides\_current**](https://drive.google.com/drive/u/0/folders/1X_bzgvN4WzNRIWl_slUsXVpjkmgvJprg). Links to example queries should be updated to reference the correct version of the query stored in [**bigquery-documentation-wf827**](https://github.com/GlobalFishingWatch/bigquery-documentation-wf827).  
     
3. All slide decks are combined into a single, versioned slide deck and saved in [**training\_slides\_archive**](https://drive.google.com/drive/u/0/folders/1v8euqxFUTVM6PIoPEEG_ey7Fe3UPpnoK) (Tyler).

**Assignments**: GFW staff are responsible for updating the training materials for certain datasets. These assignments are generally based on that staff member’s expertise in the subject/dataset.

* **BigQuery**: Tyler, David  
* **Intro to AIS**: David  
* **Pipeline**: Enrique, Andres, Matias, Tim  
* **Research tables**: Tyler  
* **Fishing effort**: Tyler  
* **Vessel database**: Jaeyoon  
* **Vessel info tables**: Tyler  
* **Ports and voyages**: Nate, Tim  
* **Transshipment**: Hannah, Nate  
* **VIIRS**: Masaki  
* **SAR**: Fernando, David  
* **VMS**: Bjorn, Tim W

