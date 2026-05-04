# Agent Glossary

## PSA (Persistent Staging Area)
Insert-only, raw/bronze ingestion layer. Stores source data in a durable structure, typically with raw payload (`JSON_STRING`) and operational SYS columns.

## Soft delete
Represent deletions from the source system with a flag/marker (e.g., `IS_DELETED` + `DELETED_AT`) rather than physically deleting records.

## Delta load
Ingestion where only changes since a point-in-time are loaded. Requires a defined delta mechanism and tracked `DELTA_DATE` / watermark.

## Full load
Re-ingestion of the full dataset (snapshots). Must not violate append-only; implement via new snapshots/versions.

## Data Asset / Dataset / Data Product
- **Data Asset**: a governed collection concept in the catalog.
- **Dataset**: a concrete table/file collection representing a coherent dataset.
- **Data Product**: curated, consumer-ready deliverable composed of one or more datasets with defined ownership and SLAs.