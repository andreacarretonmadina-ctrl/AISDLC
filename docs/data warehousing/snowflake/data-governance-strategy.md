Introduction
------------

### Data Governance & Compliance (DiGi Strategy)

The **DiGi Strategy** is a mandatory framework for all teams operating on Snowflake. It ensures that every data asset is categorized for security, legal compliance, and lifecycle management.

#### 1\. Tagging Requirements

Every data team is responsible for labeling their assets using Snowflake Tags. These markers identify the "DNA" of the data and dictate how it should be handled.

-   **Scope:** Tags must be applied at the **Table level** (default) and the **Column level** (for specific PII or sensitive fields).

-   **Tag Categories:**

    -   **Sensitivity:** Confidentiality levels.

    -   **Data Type:** Identification of personal data (PII).

    -   **Compliance/Regulations:** Labeling for ICFR, HIPAA, Health, or Privacy regulations.

    -   **Lifecycle:** Data retention requirements.

#### 2\. Implementation & Standards

To ensure consistency across the organization, teams must strictly adhere to the central registry:

-   **Predefined Values:** Use only the values specified in the [Snowflake Predefined Governance Tag Values](https://docs.google.com/spreadsheets/d/1m5Mkzpfgjc3Zy5uWvSKcgooqd2lzt3wwPwEC2w2u5E0/edit?gid=1991169802#gid=1991169802) sheet.

-   **Implementation Guidelines:** Step-by-step technical instructions are located on the [DataOne Wiki](https://rochewiki.roche.com/confluence/display/dataone/Data+Governance+Tags "https://rochewiki.roche.com/confluence/display/dataone/Data+Governance+Tags").

-   **Roles:** For accountability and ownership details, refer to the [Data Governance: Roles and Responsibilities](https://phoenixportal.roche.com/glossary/glossary/GD-0303269/2.0?code=Wlu9zpcDKSRfMVOX6XqMXNCq7j_xnI3aOXoAAAzo "https://phoenixportal.roche.com/glossary/glossary/GD-0303269/2.0?code=Wlu9zpcDKSRfMVOX6XqMXNCq7j_xnI3aOXoAAAzo") document.

**DiGi: Data Governance Strategy**, requires each team on Snowflake to tag their data with relevant tags. 

For a detailed understanding of these guidelines and additional resources, please visit the [DataOne site](https://sites.google.com/roche.com/dataone/compliance "https://sites.google.com/roche.com/dataone/compliance") document.


TAGS
----

Check the below Gsheet for the exact definition of TAGS that need to be added. Note that tags are added on table level and some need to be added on column level.


Except from the List of TAGS (05-May-2025):
| Tag Number | List of predefined values for tags on Snowflake account level                                                                                 |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| T01        | [ORGANIZATION_DOMAIN](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")                |
| T03        | [CONFIDENTIALITY_LEVEL](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")              |
| T07        | [HIPAA_REGULATION](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")                   |
| T08        | [ICFR_REGULATION](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")                    |
| T09        | [PERSONAL_DATA](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")                      |
| T10        | [DATA_SUBJECT](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#")                       |
| T12        | [SNOWFLAKE_LOCALIZATION_RESTRICTION](https://navify.atlassian.net/wiki/spaces/navifymarketplace/pages/4059005090/Snowflake+Data+Tagging# "#") |

### AI-Ready Technical Summary

> **Governance Framework:** DiGi (Data Governance Strategy).
>
> **Platform:** Snowflake.
>
> **Mechanism:** Object-based Tagging (Table & Column level).
>
> **Key Dimensions:** Data Classification (Personal/Confidential), Regulations (ICFR, HIPAA, Privacy), and Retention.
>
> **Constraint:** Tags must follow predefined values from the central "Snowflake Predefined Governance Tag Values" registry.