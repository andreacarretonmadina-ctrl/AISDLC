Ingested data structure ([*data source prefix*]_[object_name]) 
---------------------------------------------------------------

If you are planning to use **data vault automation** tool that design is given. Each data source object is ingested to own table, but each table has the same structure where:

| SYS_DATASOURCE | Name of data source system |
| SYS_LOADED_BY | job which populated record |
| SYS_LOADED_FORMAT | format in which are stored ingested data. currently only json |
| SYS_LOADED_ID | Extraction timestamp , in UTC time formatted as YYYYMMDDHH24MISS |
| JSON_STRING | Extracted data in json format as name + value pairs. Columns are ordered alphabetically and have the same structure as in source. <br>Exception is "soft-delete" field which may be added artificially if source has none such field. 

{<br>"column_name" : "value",<br>"column_name2" : "value2",<br>"column_name3" : "value3",<br>} |
| SYS_SOURCE_ENV | environment from which were data taken → dev, test, prod |

###  Object Naming & Delta Logic: Direct Points

-   **Naming Convention:** Tables must follow the `[SOURCE_PREFIX]_[OBJECT_NAME]` pattern (e.g., `MDIN_ANALYSES`).

-   **Extraction Scope:** Ingestion is limited to **changed records only** (Inserts, Updates, and Deletes).

-   **State Capture:** Data is transferred "as-is." There are **no CRUD indicators**; updates are indistinguishable from inserts at this layer.

-   **Mandatory Soft-Deletes:** Deleted source records **must** include a `soft-delete` column within the `JSON_STRING`, even if the source system lacks this field.

-   **Universal Applicability:** This table structure is the mandatory standard for all ingestion flows, whether using Data Vault Automation (DVA) tools or not.