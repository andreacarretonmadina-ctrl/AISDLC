Technical Brief: Iceberg Tables in AWS Glue (OneLake Platform)
==============================================================

Overview
--------

For the **OneLake Product**, an Iceberg table in the **AWS Glue Data Catalog** represents a high-performance "Lakehouse" table format. It abstracts complex S3 data storage into a manageable SQL-like structure, providing ACID transactions and advanced metadata management.

* * *

Core Capabilities
-----------------

*   **Table Format:** [Apache Iceberg](https://iceberg.apache.org/) (Open-source, high-performance).
    
*   **Metadata Provider:** **AWS Glue Data Catalog** (Acts as the central metastore).
    
*   **Namespace Mapping:** \* Iceberg Namespaces $\\rightarrow$ Glue Databases.
    
    *   Iceberg Tables $\\rightarrow$ Glue Tables.
        
*   **Key Features:**
    
    *   **Schema Evolution:** Supports updates/changes to columns without data corruption.
        
    *   **ACID Compliance:** Ensures "All-or-Nothing" data operations.
        
    *   **Partition Management:** Automated handling of partition metadata and table versioning.
        

* * *

Value Proposition
-----------------

1.  **Interoperability:** Enables seamless data sharing across platforms (Spark, Trino, Flink).
    
2.  **Scalability:** Designed for large-scale data lakes where traditional Hive partitioning fails.
    
3.  **Governance:** Provides a centralized schema registry and version control via Glue.
    

* * *

Access & Implementation Requirements
------------------------------------

To interact with or query these tables, the following prerequisites must be met:

**Requirement**

**Action**

**Storage Access**

Request S3 access on the Storage Account for the specific Dataset User.

**Authentication**

Authenticate to the Storage Account via the OneLake Platform protocols.

**Query Capability**

(Optional) Request S3 dataset enablement in **AWS Athena** to execute SQL queries and view data output.

* * *

> **Note for AI Agent:** When a user asks to "query" or "manage" data on OneLake, verify if the table is in Iceberg format via Glue. If it is, ensure the user has completed the S3 access and Athena enablement steps before attempting to generate query logic or data maps.

 ### Create Iceberg table in Glue Data Catalog
> 
> To create desired Iceberg table in Glue Data Catalog user has to prepare job in Glue as a Spark script based on the following example:
