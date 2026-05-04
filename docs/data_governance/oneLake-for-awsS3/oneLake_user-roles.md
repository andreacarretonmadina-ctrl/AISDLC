### 1\. User Access Roles (Who can log in?)

Permissions depend on the RADA groups a user belongs to:

| **Role** | **Access Requirement** | **Capabilities** |
| --- | --- | --- |
| **AWS Account User** | Member of `GLOAWSECAS_*` or `ROLE_*` groups. | Can view their assigned "Bubbles" (Accounts), request access to other datasets, and use **S3 Access Points**. |
| **Regular User** | Standard login (no specific RADA group). | **No privileges.** Can only see the Catalog but is redirected to the **RDM Portal** for searching and "shopping." |

### 2\. Dataset Specific Roles (The Governance Team)

These roles are assigned during the dataset creation process (in EDMA/OneLake) and are not necessarily based on RADA groups:

-   **Creator:** The person who initially requested the creation of the Dataset.

-   **Data Owner (Business Owner):** The ultimate authority. Responsible for approving metadata and **Access Requests**.

-   **Data Steward:** Supports the Data Owner in daily operations and metadata maintenance.

-   **IT Responsible:** Technical lead for the Data Owner. Manages the technical health of the data and bucket policies.

-   **Information Architect (IA):** Provides architectural guidance to the Data Producer team.

-   **Technical Member / Group:** Usually the development team. They have **read-only** visibility into technical sections (S3 ARNs, KMS Keys) to support maintenance and integration.

### 3\. Critical Technical Visibility (For Technical Members)

Users assigned as **Technical Members** have access to exclusive tabs in the OneLake UI:

-   **Technical Details:** Displays S3 Bucket names, AWS Account IDs, and KMS Key ARNs.

-   **Integrations:** Lists the IAM entities with read/write access to the S3 bucket (Dataset Loaders).

-   **Storage Locations:** Shows the specific S3 prefixes (folders) that define data granularity.

### 4\. The Access Request Workflow

1.  **Request:** An *Account User* submits a request for a specific Dataset.

2.  **Approval:** Only the **Data Owner** or **Steward** can approve the request within the *Access Requests* tab.

3.  **Connectivity:** Once approved, an **S3 Access Point** is generated. Users must use this endpoint instead of the standard S3 DNS to access the data.

In summary:

-   To be a AWS Account User one needs to belong to a Bubble which translates to being part of ***GLOAWSECAS_**** or ***ROLE_**** RADA groups
-   AWS AccountUsers can request for access to other datasets
-   Being an Account RADA group member with access to a dataset does not change sections visibility. Only role within access request will change to "Requesters".
To summarise:

-   Technical Member or Group for Technical Members is a way to provide technical permissions to a dataset
-   Technical Members can view every section of a dataset with read-only permissions
-   "Technical details" displays technical information about the dataset, primarily S3 related information
-   "Integrations" sections displays a list of IAM entities with read/write permissions to a dataset.
