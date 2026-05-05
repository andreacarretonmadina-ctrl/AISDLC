Account Creation
================

Our standard policy assumes maintaining only one account in each cloud region, a derivative from preference single account architecture. Snowflake Account creation within Roche is strictly controlled and is only available under legal requirements, mainly regarding data residency restrictions.

On-demand account creation requests will not be accepted under any but legal circumstances.

Roche Affiliate Account Policy
==============================

In our organization, we facilitate the creation of Snowflake accounts for Roche affiliates when justified by legal requirements. This ensures compliance and operational independence while maintaining a cohesive data management strategy across the organization.

ORGADMIN Role
=============

The ORGADMIN is a highly privileged role with extensive control over the entire [Roche Snowflake ORGANIZATION](https://docs.snowflake.com/en/user-guide/organizations).

Due to the significant risks associated with this level of access (ability to create and remove accounts), the ORGADMIN role is **exclusively reserved for OneSnowflake technical team**.

Exceptions 
===========

While we generally do not create additional Snowflake accounts on demand, exceptions are made exclusively if there is a legal requirement necessitating the creation of an additional account. 

We maintain a strict policy of having a maximum of one Snowflake account per region, however there are exceptions. AWS EU-Central-1 (Frankfurt) Cloud Region is an exception to our one-account-per-region policy, where we maintain [two accounts](https://rochewiki.roche.com/confluence/x/FfWqLw) due to specific operational needs.
