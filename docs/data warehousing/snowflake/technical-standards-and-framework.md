PSA Technical Specification: Schema & Metadata
==============================================

This section details the standardized table structures and the configuration framework required for the Persistent Staging Area (PSA).

1\. Standardized Table Structure
--------------------------------

Regardless of the source, every table in the PSA follows a uniform schema to support **Data Vault Automation (DVA)** and consistent ingestion.

### A. Core Columns (The Envelope)

Every ingested table contains the following technical metadata columns alongside the payload:

| **Column** | **Description** | **Format** |
| --- | --- | --- |
| **SYS_LOADED_ID** | Extraction timestamp (Primary key for versioning) | `YYYYMMDDHH24MISS` (UTC) |
| **SYS_DATASOURCE** | Name of the source system | String |
| **SYS_SOURCE_ENV** | Origin environment | `dev`, `test`, `prod` |
| **SYS_LOADED_FORMAT** | Format of the payload | Currently: `json` |
| **SYS_LOADED_BY** | Name/ID of the job that populated the record | String |
| **JSON_STRING** | The actual data payload (Name/Value pairs) | JSON Object |

### B. Payload Logic (JSON_STRING)

-   **Ordering:** Columns are ordered alphabetically.

-   **Consistency:** Matches the source structure exactly.

-   **Soft Deletes:** If the source lacks a deletion indicator, a "soft-delete" field is added artificially to the JSON to track removed records.

-   **CRUD Limitations:** The current approach does not use CRUD indicators; it is an append-only log of states.

* * * * *

2\. Configuration & Logging Framework
-------------------------------------

The ingestion engine (Talend) and DVA tools rely on a mandatory set of metadata tables for every data source.

### A. Object Inventory (`CONFIG_[Source]_OBJECTS`)

Defines what to ingest and how.

-   **Load Types:** Supports `Full`, `Full2Delta`, and `Delta`.

-   **Delta Tracking:** Identifies the specific column (timestamp/sequence) used for incremental loads.

-   **Operational Keys:** Defines the `BUSINESS_KEY` and `CHUNKED_LOAD` parameters for performance tuning.

### B. Schema Tracking (`CONFIG_[Source]_OBJECT_SCHEMA`)

Used for **Observability** and DVA automation.

-   Stores a JSON representation of the source schema (Length, Precision, Scale, Type).

-   Enables tracking of schema changes over time.

### C. Execution State (`CONFIG_[Source]_LOAD_STATUS` & `DELTA_DATE`)

-   **Load Status:** Tracks successful job completions. It is a strict rule that **failed jobs must not insert data into the PSA**.

-   **Delta Date:** Stores the high-water mark (latest UTC extraction date) to ensure data continuity in the next run.

### D. Data Quality Logs (`SOURCE_COUNTS_[Source]`)

Facilitates mandatory consistency checks between the Source and the PSA:

-   Tracks `RECORD_COUNT` and `ACTIVE_RECORD_COUNT`.

-   Stores `PRIMARY_KEYS` arrays to validate uniqueness and completeness.

* * * * *

3\. Governance & Quality Standards
----------------------------------

### Data Quality (DQ) Requirements

Every ingestion flow must validate the following three pillars:

1.  **Uniqueness:** Ensure business keys behave as expected.

2.  **Completeness:** Source record counts must match PSA record counts.

3.  **Freshness:** Data must be delivered within the defined SLA window.

### Access Control & Retention

-   **Restricted Access:** This is a **purely technical layer**. Business users are strictly prohibited from accessing the PSA.

-   **Retention Policy:** No retention period is defined; all historical data is preserved indefinitely.

* * * * *

### AI-Ready Technical Summary

> **Architecture Pattern:** Metadata-Driven Ingestion.
>
> **Key Entities:** Config Objects (Strategy), Object Schema (Metadata), Source Counts (Quality).
>
> **Constraint:** Append-only logic with artificial soft-delete support in JSON.
>
> **Integration:** Optimized for Data Vault Automation (DVA) tools.