Terminology
===========

-   Dataset - represents logically and technically data that are stored in AWS S3 bucket located in OneLake AWS account. Logical representation contains metadata of the data (available in OneLake UI and RDM tools: EDMA, Collibra, RDM Portal) and technically AWS S3 bucket and its configuration.

-   Storage Location - AWS S3 prefix (folder) under S3 bucket for a S3 Dataset. Storage Location is used while data shopping process to define the granularity of access for a S3 Dataset

-   Integration (Dataset Loader) - represents AWS IAM entities that has read/write access to the S3 Dataset bucket

-   Access Request - represents all the access activities for a S3 Dataset

-   Metadata - represents information gathered about the data first in EDMA, then published to Collibra and RDM Portal. Additionally part of the metadata are published to OneLake Dataset and S3 bucket as S3 tags

-   OneLake User Interface - provides information about Inventory of AWS accounts and S3 Datasets registered in OneLake

-   RDM (Roche Data Marketplace) - Informatics product that provides data governance tools used in OneLake processes including [EDMA](https://go.roche.com/edma) (Enterprise Data Management Application), [Collibra](https://rochewiki.roche.com/confluence/https:/roche.collibra.com), [RDM Portal](https://rdmportal.roche.com/)

-   Data Producer Team - represents all the roles for the S3 Dataset owning team

-   Data Consumer Team - represents all the consumers of the S3 Dataset

Dataset roles
=============

Creator - the person that requested creation of the Dataset

Data Owner - the person that is the owner of the data. In OneLake primarly responsible for the approval of the metadata describing data and approving data access requests. More information can be found in DataOne OneLake - Data Governance: Roles & Responsibilities  document

Data Steward - the person that is supporting Data Owner 

Information Architect - the person that is supporting Data Producer team from IA perspective

IT Responsible - the person that is supporitng Data Owner from technical perspective and is responsible for technical aspects of the data. More information can be found in DataOne OneLake - Data Governance: Roles & Responsibilities  document

Technical member - the person that is a member of the Data Producer team