> ### [Data Vault Automation Tool](https://data-vault-automation.roche.com/ "https://data-vault-automation.roche.com/") is a web-based tool that simplifies the modelling of the data vault layer and generates code to create data objects and procedures to populate them.

Data Vault - Overview
=====================

* * * * *

Data Vault is a comprehensive architecture for business intelligence, formally known as the Common Foundational Warehouse Architecture. The architecture is specifically built to handle long-term historical storage and easily integrate data from many different source. Data Vault encompasses four integrated key components:

-   Data Vault Modeling - Includes changes for performance and scalability, allowing models to work seamlessly with NoSQL and Big Data systems.

-   Data Vault Methodology - Follows Scrum and Agile best practices, focusing on 2 to 3 week sprint cycles tailored for repeatable data warehousing tasks

-   Data Vault Architecture - Incorporates NoSQL systems, real-time feeds, and big data systems for handling unstructured data and big data integration.

-   Data Vault Implementation: Emphasizes pattern-based automation and generation to achieve time savings, error reduction, and rapid productivity.

Core Components:

The model is built on three main types of tables:

-   Hubs: These table store the core business keys (like a customer ID or product number) and are designed to be very stable.
-   Links: These tables define the relationships or transactions between the business keys (linking Hubs together).
-   Satellites: These tables store all the descriptive, contextual, and historical attributes for a Hub or a Link. Because satellites are separate, they allow you to track changes over time and add new descriptive data from new sources without changing the existing structure.

notifications

Please note that DataOne does not use a textbook approach to Data Vault modeling. This approach has been slightly modified to meet all business needs. All information provided below is tailored to the way data is modeled in the DataOne domain, not to the standard approach.

At DataOne, before the final version of data product is created, data flows and is transformed in the mean time through several dedicated layers. These layers are:

-   [Persistent Staging Area (PSA)](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/psa-layer/psa-layer/) - A Persistent Staging Area (PSA) is landing zone for data in a data warehouse environment that does not wipe out data between loads and, consequentyl, maintaints the full history from its data sources. The primary advantage of using a PSA is that it allow developers to reload the next layer in the data warehouse, such as a data vault, when requirements change without needing to access to origiinal data source again.

-   [Raw Data Vault (RDV)](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/raw-dv-layer/rdv-layer/) - The Raw Data Vault holds unmodified raw data that is loaded from the Landing Area. The loading process is purely technical, focusing on restructuring the physical format of the data rather than altering its content, meaning the source data is neither cleaned nor modified. Because the raw data remains unaltered, the original source data could be entirely reconstructed from the Raw Data Vault without issue.

-   [Business Data Vault (BDV)](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/business-data-vault-layer/) - The Business Data Vault extends the Raw Data Vault layer by incorporating additional tables and rows derived from specific business rules. This layer server as a central location to store the results of business logic, including deduplication, conformed results, and calcualtions. Storing these results separately from the raw data simplifies the management of business rule changes and prevents duplicate computation and potential incosistencies across multiple data marts.

-   [Data Product Layer (DP)](https://data-engineering-docs-52075d.pages.roche.com/docs/data-modeling/data-vault/data-product-layer/) - In the DataOne Data Vault approach, data marts are generally ephemeral and may be implemented as views directly over the Business Data Vault and Raw Data Vault, which helps ensure consistency and ease of modification. The Data Product Layer provides an alternative option if these views do not offer the necessary level of performance. In this scenario, the Data Product Layer involves storing resutlts in a materialized table format.

If you want to learn more about DataOne Data Vault layers, click on the name of the layer.

Below is a data flow diagram showing how the layers work together to achieve the final data product.
