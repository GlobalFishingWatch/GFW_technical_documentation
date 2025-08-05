# BigQuery to APIs to Products
<br>
The flow of data into [**GFW Products**](https://docs.google.com/document/d/1JY0umwJ5j8qFpOb90OuaUOo43y45yExaenNWzUlbkCo/edit?tab=t.0)   is enabled through a set of **Application Programming Interfaces (APIs)**. This page documents the key APIs, the underlying queries powering them, and how they support user-facing products.

## GFW APIs 

### Current GFW APIs

- [GFW API Portal](https://globalfishingwatch.org/our-apis/documentation#introduction)
- A mapping of [Global Fishing Watch Data Availability](https://globalfishingwatch.org/global-fishing-watch-data-availability)                                                                     

### Products Using Legacy APIs

Some older products are still using legacy APIs:

- **Legacy CVP**
- **Vessel Profile Prototype**

## Accessing API Logic

API query logic is maintained in [this central hub](#).  

To view underlying queries and code for BigQuery tables:

- Open the relevant table in BigQuery.
- Click the **"Details"** tab to find:
  - Source tables
  - (When available) links to the GitHub code that generates each table

> ⚠️ If the logic in the API hub seems outdated or unclear, contact **Gisela**.

## Caveats and Considerations

**Permissions**

Access to API data may be restricted depending on your user role. Some datasets may only be available to:

- Internal GFW staff
- JAC members
- Specific partners

As a result, data visible through the API can vary based on your credentials.

**Data Filters**

We’ve made our best effort to document all filters and permission scopes on each API. However, gaps may still exist.

If you're uncertain about:

- Why data may be missing or inconsistent
- What filters are applied

Please contact **Gisela** for clarification.
