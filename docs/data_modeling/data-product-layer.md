The Data Product Layer is where data products are stored and made accessible to consumers. A data product is defined as an autonomous, read-optimized, standardized data unit designed to meet user needs.

Each data product contains at least one dataset (Domain Dataset). These products exist within a bounded context (domain), and a single bounded context may contain multiple data products. Essentially, data products are curated collections of datasets and business-approved metada created to answer specific, targeted questions. In this layer, data products might be physically represented by database tables or views.

Data Products are generally categorized into three types:

1.  Source Oriented Data Products

2.  Aggregated/Combined Data Products

3.  Consumer Oriented Data Products

Since any data product can potentially serve as the source for other data products, it is essential that certain capabilites are maintained:

-   It must be easily possible to detect changed data (delta).

-   It must be possible to identify the time when data was extracted from the source.

-   There must be an ability to check data quality, such as freshness.

Therefore, it is expected that every published data object will include specific columns that allow the data product to be consumed in a predictable manner:

| Key | Column Name | Type | Column Description |
| --- | --- | --- | --- |
| PK | `<TABLE_NAME>_KEY` | VARCHAR | Hashed primary key of table. |
|  | `<TABLE_NAME>_BKEY` | VARCHAR | Not hashed primary key string allowing to see original values used for hashing. |
| PK | `SYS_SEQUENCER` | VARCHAR | Used to create primary key from several columns - additional attribute. |
|  | `SYS_LOADED_ID` | NUMBER | Extraction date formatted as number YYYYMMDDHH24MISS |
|  | `SYS_DATA_SOURCE` | VARCHAR | Original source of the business key using lowest possible level of source object (table, file). e.g. for table "SAP06.FINANCE.GL" |
|  | `SYS_LOADED_FROM` | VARCHAR | Source of the data in stage layer e.g. PSA_SAP06.SAP06_/BIC/PLCMATSAL |
|  | `SYS_DELETED` | BOOLEAN | Defines if the record should be considered as deleted. |
|  | `SYS_EFFECTIVITY_START` | TIMESTAMP_NTZ | (optional for SCD2) since when is the record valid, technical validity calculated using SYS_LOADED_ID |
|  | `SYS_EFFECTIVITY_END` | TIMESTAMP_NTZ | (optional for SCD2) till when is the record valid, technical validity calculated using SYS_LOADED_ID |
|  | `SYS_CURRENT_RECORD` | BOOLEAN | (optional for SCD2) defines if this is the latest version of the record (TRUE/FALSE) |
|  | `SYS_CREADET_AT` | Timestamp_LTZ | Defines when was record created in the layer |
|  | `SYS_LOADED_AT` | Timestamp_LTZ | Defines when was record last time updated in the layer. |
|  | `SYS_PIPELINE_ID` | NUMBER | The dataops.roche pipeline ID, Sequence value allowing to identify delta |
|  | `Other Attributes` |  |  |

For a database structure to be considered and published as a true data product, it must be aligned with the Data Product Characteristics:

-   Monitors, data quality, and consistency checks must be designed and aligned with the data structure.

-   These checks are expected to be published in a data observability tool (Monte Carlo) and synchonized to Collibra.

When updating data, it is expected that objects are not recreated each time. This non-recreation allows consumers to utilize streams or dynamic tables to fetch changed data. Recreation a table blocks these possibilites and may lead to access issues, as access policies often need to be recreated after an object is recreated.

##### 1\. Source Oriented Data Product [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/#1-source-oriented-data-product)

The source oriented data product is fundamentally a structure that provides data originating from a specific source or domain.

This type of data product is owned by a cross-functional team. This team is responsible for managing the end-to-end lifecycle of the data product, which includes ensuring its quality, availability and evoltuion. The responsibilities span from data ingestion (PSA layer) and processing (RDV layer) through to publishing the data for consumption in the Data Product Layer.

A source oriented data product typically acts as the source of data for other data product. Nevertheless, consumers may still use it directly (e.g for reporting). To maintain fidelity, the structure of the data and the data itself are usually kept as close as possible to the original source. This means that the table name and column names are kept exactly as they are in the source system.

Regarding naming conventions, it is considered good practice to add a prefix defining the source system. For example, if the table is named RECORDTYPE and is sourced from the salesforce Rexis system, it should be named RXS_RECORDTYPE. An example provided below indicates that a table may be published as a view within this data product.

Product Table Example
View SQL

```
CREATE OR REPLACE VIEW <SOURCE_SYSTEM>_<TABLE_NAME> AS
      SELECT
         SAT.<TABLE_NAME>_KEY,
         SAT.<TABLE_NAME>_BKEY,
         SAT.SYS_LOADED_ID,
         SAT.SYS_DATASOURCE,
         SAT.SYS_LOADED_FROM,
         SAT.SYS_DELETED,
         TO_TIMESTAMP_NTZ(SAT.SYS_LOADED_ID, 'YYYYMMDDHH24MISS')  SYS_EFFECTIVITY_START,
         COALESCE(LEAD(TO_TIMESTAMP_NTZ(SAT.SYS_LOADED_ID, 'YYYYMMDDHH24MISS')) OVER (PARTITION BY <TABLE_NAME>_KEY ORDER BY SAT.SYS_LOADED_ID),TO_TIMESTAMP_NTZ('40001231', 'YYYYMMDD')) SYS_EFFECTIVITY_END,
         CASE WHEN SYS_EFFECTIVITY_END = TO_TIMESTAMP_NTZ('40001231', 'YYYYMMDD') THEN TRUE ELSE FALSE END SYS_CURRENT_RECORD,
         SAT.SYS_CREATED_AT,
         SAT.SYS_LOADED_AT,
         543 SYS_PIPELINE_ID,
         ...
      FROM
        "RAW_DV_<DOMAIN>"."<SAT_TABLE_NAME>" SAT
```
```
CREATE OR REPLACE VIEW <SOURCE_SYSTEM>_<TABLE_NAME> AS
      SELECT
         SAT.<TABLE_NAME>_KEY,
         SAT.<TABLE_NAME>_BKEY,
         SAT.SYS_LOADED_ID,
         SAT.SYS_DATASOURCE,
         SAT.SYS_LOADED_FROM,
         SAT.SYS_DELETED,
         TO_TIMESTAMP_NTZ(SAT.SYS_LOADED_ID, 'YYYYMMDDHH24MISS')  SYS_EFFECTIVITY_START,
         COALESCE(LEAD(TO_TIMESTAMP_NTZ(SAT.SYS_LOADED_ID, 'YYYYMMDDHH24MISS')) OVER (PARTITION BY <TABLE_NAME>_KEY ORDER BY SAT.SYS_LOADED_ID),TO_TIMESTAMP_NTZ('40001231', 'YYYYMMDD')) SYS_EFFECTIVITY_END,
         CASE WHEN SYS_EFFECTIVITY_END = TO_TIMESTAMP_NTZ('40001231', 'YYYYMMDD') THEN TRUE ELSE FALSE END SYS_CURRENT_RECORD,
         SAT.SYS_CREATED_AT,
         SAT.SYS_LOADED_AT,
         543 SYS_PIPELINE_ID,
         ...
      FROM
        "RAW_DV_<DOMAIN>"."<SAT_TABLE_NAME>" SAT
```

##### 2\. Aggregated Data Product [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/#2-aggregated-data-product)

An Aggregated Data Product delivers data that has been combined from several different sources. The primary objective of this product type is to offer a higher-level, integrated view of data that remains consistent, reliable and trustworthy.

-   Example: A "customer details" data product that integrates data from systems like Salesforce and SAP serves as an example of an Aggregated Data Product.

-   Structure: These products utilize more business-like oriented entites and column names, although they must still adhere to established Naming Standards.

-   Sourcing: Aggregated Data Products are typically built from source oriented data products. Importantly, they do not possess their own dedicated stage, raw or business data vault layer.

##### 3\. Consumer Oriented Data Product [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/#3-consumer-oriented-data-product)

A Consumer Oriented Data Product is specifically aligned to the needs of the consumer. This products usually comprises a set of pre-calculated KPIs, dimensions or reporting tables designed to address questions related to a common topic.

-   Goal: The focus is on assisting consumers in making data-driven decisions and driving innovation by ensuring the data aligns with the organization's overall goals.

-   Modeling: The data structure of this product might follow dimensional modeling (using dimensions and facts), but this is not a mandatory option. Pure datasets or standard reporting tables are considered valid structures.

-   Sourcing: Similar to aggregated products, Consumer Oriented Data Products are typically created by transforming or enriching source-oriented and/or aggregated data products. Therefore, they do not read data firectly from PSA, RDV, or BDV layer.

##### 4\. Schema Evolution [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/#4-schema-evolution)

When dealing with Schema Evolutionm, only non-breakable changes are permitted, such as the adding of new column. If a change is breaking (a "breaking change"), it must result in the creation of a new version of the entire dataset.

Versioning Example: The source suggests renaming the dataset, for example, changing `FACT_PMQ_MEASURE` to `FACT_PMQ_MEASURE_V2`. This approach ensures the uninterrupted consumption of the previous version of the data product.

##### 5\. Data Retention [](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/#5-data-retention)

Currently, there is no expectation that data retention be enabled by default. However, the decision regarding the appropriate data period that holds value to the consumers is ultimately left to the data product teams to determine.