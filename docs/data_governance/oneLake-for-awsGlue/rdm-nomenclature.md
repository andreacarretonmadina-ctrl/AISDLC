#### RDM (Roche Data Marketplace) nomenclature:

**Data Product**- logical entity that contains 1 or more Datasets

**Dataset**- logical entity that contains 1 or more Data Assets

**Data Asset** - technical asset in the physical System

#### Data Asset in OneLake for AWS Glue Catalog

is represented by AWS Glue Databases, Tables, Columns and it`s metadata. However the main entity is represented by AWS Glue Tables meaning Dataset contains Data Assets on the Glue Table level. 

Dataset can have Data Assets from different AWS Accounts and Glue databases however Data Assets must be from the same AWS Region.

Currently OneLake for AWS Glue Catalog supports Glue tables based on AWS S3.
