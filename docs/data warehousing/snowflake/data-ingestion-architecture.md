Data Architecture: Immutable Ingestion Layer (PSA)
==================================================

This section defines the standards for the **Persistent Staging Area (PSA)**, the layer responsible for storing all ingested data from source systems.

Core Principles
------------------

-   **Insert-Only Layer:** This is a non-mutable layer. Data is stored as a continuous stream of ingested records without updates or deletions.

-   **Domain Isolation:** Each ingested data source is allocated exactly one dedicated schema with its own specific access roles.

-   **Ownership & Utilization:** A schema is owned by **one domain only**. However, its data may be utilized by other domains as needed.

Implementation Standards
----------------------------

### 1\. Naming Conventions

To ensure traceability, tables retain their original source names but include a source-specific prefix:

-   **Pattern:** `[SOURCE]_[ORIGINAL_OBJECT_NAME]`

-   **Example:** `SYS_USER` from ServiceNow is ingested as `SNOW_SYS_USER`.

### 2\. Ingestion Framework & Tooling

-   **Default Tool:** **Talend** is the standard ETL tool for data ingestion.

-   **Initial Setup:** For every new source, a dedicated schema must be created, followed by the initialization of **configuration and logging tables**.

-   **Data Structure:** A dedicated table is created for each ingested object. Data is stored in **JSON format** to maintain a standardized and flexible table structure.

* * * * *

Lifecycle Management
-----------------------

### Schema Evolution

The architecture is designed to be resilient to changes in the source systems:

-   **Dynamic Extraction:** Ingestion jobs automatically extract all columns from the source table or view according to the definition at the time of execution.

-   **JSON Resilience:** Because data is stored in JSON format, changes in the source structure (adding or removing columns) do not impact the physical structure of the PSA layer.

-   **Impact:** There is no need to manually handle schema changes; new data is simply appended.

### Data Retention

-   **Policy:** There is no data retention limit enabled.

-   **Default Behavior:** All ingested data is kept indefinitely by default.

* * * * *

### AI-Ready Summary

> **Layer Type:** Persistent Staging (Raw/Bronze).
>
> **Format:** Semi-structured (JSON) within Relational Tables.
>
> **Logic:** Append-only, source-prefixed naming, domain-driven ownership. No automated cleanup or purging is currently applied.


Ingested data structure ([*data source prefix*]_[object_name]) 
---------------------------------------------------------------

If you are planning to use **data vault automation** tool that design is given. Each data source object is ingested to own table, but each table has the same structure where:

|**Column** | **Description** |
| --- | --- | 
| **SYS_DATASOURCE** | Name of data source system |
| **SYS_LOADED_BY** | job which populated record 
| **SYS_LOADED_FORMAT**  |  format in which are stored ingested data. currently only json 
| **SYS_LOADED_ID** | Extraction timestamp , in UTC time formatted as YYYYMMDDHH24MISS 
| **JSON_STRING** | Extracted data in json format as name + value pairs. Columns are ordered alphabetically and have the same structure as in source. <br>Exception is "soft-delete" field which may be added artificially if source has none such field. 
| **SYS_SOURCE_ENV**  | environment from which were data taken → dev, test, prod 

###  Object Naming & Delta Logic: Direct Points

-   **Naming Convention:** Tables must follow the `[SOURCE_PREFIX]_[OBJECT_NAME]` pattern (e.g., `MDIN_ANALYSES`).

-   **Extraction Scope:** Ingestion is limited to **changed records only** (Inserts, Updates, and Deletes).

-   **State Capture:** Data is transferred "as-is." There are **no CRUD indicators**; updates are indistinguishable from inserts at this layer.

-   **Mandatory Soft-Deletes:** Deleted source records **must** include a `soft-delete` column within the `JSON_STRING`, even if the source system lacks this field.

-   **Universal Applicability:** This table structure is the mandatory standard for all ingestion flows, whether using Data Vault Automation (DVA) tools or not.