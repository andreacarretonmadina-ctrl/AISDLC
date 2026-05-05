#### 1.Data Loading Sequence and Strategy [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/raw-dv-layer/rdv-data-load/#1data-loading-sequence-and-strategy)

The execution of the Raw Data Vault (RDV) load is regulated by a time-based sequence known as the LOAD_ID. This identifier is generated when the data is initially ingested into the preceding layer.

In traditional Data Vault methodology, the expected flow involves identifying all LOAD IDs that are chronologically greater than the latest sucessful load ID. Subsequently, the system is expected to run the loading flow for each of those identified LOAD IDs individually, following a cyclical pattern.

However, the specific recommendation for the Roche environment proposes an optimized approach for speed: instead of processing each LOAD ID separately, the system should load all remaining data at once. This is achieved by fething all data associated with not-yet-loaded LOAD IDs. This collective loading strategy still utilizes the principle of only fetching data greater than the latest successful load for each specific data source. This approach is designed to speed up the loading process particularly for sources where the delta (changes) between loads can be reliably identified.

For sources where data extracts are always full (meaning the delta cannot be identified and a full copy of data is stored in the staging area for every LOAD ID), a different method is used. In these scenarios, only the most recent LOAD ID should be fethed and all previous full data extracts are ignored.

#### 2\. Architectural Approaches for Loading [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/raw-dv-layer/rdv-data-load/#2-architectural-approaches-for-loading)

There are two primary architectural strategies for loading data into the Raw Data Vault layer.

One essential approach involves splitting the Raw Data Vault load into several distinct layer based on the entity type. This layering methodology, which was deemed necessary for historical reasons (Data Vault 1.0 sequences) ensures ordered dependencies. Within this structure:

1.  Parallel Execution: All objects belonging to the same entity layer are loaded simultaneously (in parallel).
2.  Sequential Progression: The load for objects in a subsequent layer only begins after the previous layer has been fully completed.

This sequential, layered structure provides two main benefits:

-   It enables necessary lookup calls to reference objects.
-   It ensures data consistency by preventing the creation of orphan records - it is designed so that links or satellites cannot be loaded without a correlating Hub record already existing.

Fourth Layer

LSAT

Third Layer

SAT

LINK

Second Layer

HUB

REF

First Layer

TMP

In other hand, previous approach adds dependency and it prolongs overall load time toward data warehouse.

The second primary strategy for loading data aims for maximum parallelism by loading all entities simultaneously, or "at once". This method intentionally bypasses the use of global synchronization points. Unlike the first approach, which requires a flow to halt and wait for previous objects or entity types (such as all Hubs) to fully complete, this parallel method has no mandatory waiting periods between the loading of different objects. The only exception to this complete parallel loading is related to temporary tables, where dependencies must sill be managed; however, even these dependencies are kept local to the specific one or few objects that utilize the temporary tables, and are not applied across all entities of a similar type (e.g. all satellites).

This focus on concurrent Loading suggests an emphasis on maximizing speed and minimizng latency during the data ingestion process.

check_circle

The below approach is recommended by Roche.

First Layer

TMP

HUB

REF

SAT

LSAT

LINK

The "load everything at ones" strategy, which aims to maximize concurrency is strongly recommended for populating the data because the resulting much shorter time to load new data compensates for it's potential disadvantages.

The primary drawbacks of this high-speed, concurrent loading method are related to data integrity and object reliance:

1.  Data Inconsistency Risk: There is a chance for data inconsistency if the loading flow fails during execution.
2.  Inability to use existing RDV objects: Load queries are restricted from using or referencing any other existing RDV objects. This restriction is necessary because due to the concurrent nature of the load, those other objects may not be up to date yet.

The raw data vault layer adheres to very specific, strict rules regarding data manipulation, as it is intended to store data "as is":

-   No Soft Business Rules: During the load, the system does not perform any soft business rules. This explicity excludes activites such as data quality checks, master data management or data standardization.
-   Data Transfer: Data is simply transferred as it is from the source.
-   Insert Only Mode: All tables, except for temporary (stage) tables, operate strictly in "insert only" mode. This guarantees that data already existing in the RDV layer is never updated or deleted. The only tables allowed to be truncated or have parts of their records deleted are the temporary (stage) tables.