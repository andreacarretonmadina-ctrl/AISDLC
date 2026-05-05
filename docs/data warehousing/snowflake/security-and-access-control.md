# Security & Access Control (Snowflake)

Purpose: concise, AI-friendly Roche reference covering authentication, authorization, and security controls used in Snowflake at Roche.

Short Roche summary
- RBAC: Roche follows a strict role-based access model. Organization-level privileges are limited to `ORGADMIN` (OneSnowflake technical team). `ACCOUNTADMIN` is reserved for account-level actions and delegated only to trusted platform engineers. Day-to-day operations are performed with scoped domain roles (see "Role mapping").
- Authentication: Roche mandates SSO (SAML/OIDC) via the enterprise identity provider and SCIM provisioning for user lifecycle. Key-pair authentication and service principals are used for programmatic access where necessary.
- Data protection: Use column masking policies and row access policies for PII and regulated data. All sensitive objects must be tagged following the DiGi tag registry (see `data-governance-strategy.md`).
- Encryption & KMS: Snowflake-managed encryption is enabled by default. Customer-managed keys (BYOK/External Tokenization) may be used where required by legal or regulatory constraints—coordination with Roche Security is mandatory.
- Network controls: Private connectivity (private endpoints/PrivateLink) is preferred for production accounts. Network policies and account-level IP whitelisting are applied for administrative roles.

Role mapping (recommended Roche baseline)
- ORGADMIN: OneSnowflake technical team (very limited membership). Responsible for organization-level account creation and org settings.
- ACCOUNTADMIN: Platform engineers with elevated account administration privileges. Use sparingly; prefer role delegation.
- SECURITYADMIN: Security operations and data governance team — manages masking policies, tags, and grants related to security.
- SYSADMIN: Schema and object provisioning for platform engineers and ingestion teams (e.g., Talend service roles).
- DOMAIN_<NAME>_OWNER (example): Domain-level owner role for each business domain; owns schemas for that domain and is point of contact for access requests.
- READ_ONLY / ANALYST roles: Business/analyst roles with least privilege for consumption (granted via role hierarchy and data sharing where needed).

CIDM group-based access model (DataOne)
--------------------------------------
Roche uses CIDM (Corporate Identity Management) Active Directory groups to manage Snowflake access. The DataOne Platform Team creates AD groups in CIDM which are provisioned into Snowflake as Functional Roles. Data Teams manage membership by approving or rejecting access requests for their groups.

Key concepts
- CIDM AD groups -> provisioned into Snowflake as Functional Roles.
- Data Team owners approve membership requests (Owner group is non-requestable and managed by existing members via iCare).
- Group ownership and approval flow can follow the default (OWNER as approver) or an alternative (IT_RESPONSIBLE as approver for ADMIN/DEVELOPER).

Group prefixes
- Each Snowflake account uses a dedicated account prefix for AD group names. Common prefixes used by DataOne include (examples):
	- `GLOAZUDIABI` / `ROCHE_DIA`
	- `GLOAZUSFPD` / `ROCHE_PD`
	- `GLOAZUDIABICH` / `DIA_CHLOCAL`
	- `GLOAZUDIABINAM` / `DIA_NAMLOCAL`
	- `GLOAZUUSW` / `US_WEST`
	- `GLOAZUSBX` / `SANDBOX`

Standard group types (naming pattern: `GLOAZU<Acct_Prefix>_<Data_Team>_<ROLE>`) and behavior
- OWNER (example: `GLOAZUDIABI_ATEAM_OWNER`)
	- Purpose: primary approver group and accountable business owners (Business Process Owner must be member).
	- Membership: non-requestable. Managed via iCare Generic Support Request by existing OWNER members.
	- Notes: must have at least 2 users (one Roche employee minimum). Acts as a distribution list for team topics.

- IT_RESPONSIBLE (example: `GLOAZUDIABI_ATEAM_IT_RESPONSIBLE`)
	- Purpose: technical representatives accountable for security, FinOps (usage & cost), service account logs and technical operations.
	- Membership: requestable and must have at least 2 users.
	- Access: broad metadata and operational privileges; may act as approver in the alternative approach.

- ADMIN (example: `GLOAZUDIABI_ATEAM_ADMIN`)
	- Purpose: full access to team-owned databases (DEV, TEST, PROD). Often used with service accounts for automation (e.g., DO.R DataOps@Roche).
	- Membership: requestable; approver is OWNER by default (or IT_RESPONSIBLE if alternative configured).

- DEVELOPER (example: `GLOAZUDIABI_ATEAM_DEVELOPER`)
	- Purpose: full access to DEV, read-only to TEST, optional read-only to PROD if requested. Editor access in Monte Carlo, Immuta and DVA.
	- Membership: requestable; approver is OWNER by default.

- REPORTING (example: `GLOAZUDIABI_ATEAM_REPORTING`)
	- Purpose: read-only access to PROD DP_ schema (data product output schema) for analysts and consumers.
	- Membership: requestable; approved by OWNER (end-user role approvals are always OWNER-managed).

- CUSTOM (example: `GLOAZUDIABI_ATEAM_<CUSTOM_NAME>`)
	- Purpose: for special access requirements not covered by standard groups. Requestable via the Custom Groups process.

Ownership & approval patterns
- Default approach: OWNER group is approver for all other groups.
- Alternative approach: IT_RESPONSIBLE may act as approver for ADMIN and DEVELOPER groups while OWNER remains approver for REPORTING groups. This alternative must be explicitly requested and configured.

Managing CIDM group membership (process)
1. Request access: Users request membership via the CIDM access request workflow (link: How to Request Snowflake Access).
2. Approval: The approver group (OWNER by default) receives and approves or rejects the request per Data Team policy.
3. Provisioning: Approved AD group membership is automatically provisioned into Snowflake through SCIM/CIDM sync.
4. Notifications: OWNER and IT_RESPONSIBLE groups are distribution lists for operational and business notifications.

Managing the OWNER group
- The OWNER group is special: it is non-requestable and changes must be made by an existing OWNER member via an iCare Generic Support Request. The group must have at least 2 members (one Roche employee minimum).

Removing access
- Revocation follows CIDM group membership removal workflows. To revoke access, remove the user from the AD group in CIDM; provisioning will remove Snowflake access on the next sync. For OWNER group changes, use iCare as above.

Requesting custom groups
- If the standard set of groups does not meet access needs, Data Teams can request custom functional roles. Follow the "Requesting for Custom Groups / Functional Roles in Snowflake" process linked from DataOne.

Examples
- Data Team "ATEAM" in account `ROCHE_DIA` would have:
	- `GLOAZUDIABI_ATEAM_OWNER`
	- `GLOAZUDIABI_ATEAM_IT_RESPONSIBLE`
	- `GLOAZUDIABI_ATEAM_ADMIN`
	- `GLOAZUDIABI_ATEAM_DEVELOPER`
	- `GLOAZUDIABI_ATEAM_REPORTING`
	- Optionally: `GLOAZUDIABI_ATEAM_<CUSTOM_NAME_1>`

Sandbox accounts
- Sandboxes use a distinct AD group setup (prefix `GLOAZUSBX` / `SANDBOX`) and a simplified governance model. See the Snowflake Sandboxes wiki page for details.

Links & runbooks
- How to request Snowflake access (CIDM): [DataOne: How to Request Snowflake Access](https://rochewiki.roche.com/confluence/spaces/dataone/pages/797111537/Snowflake)
- How to manage access requests in CIDM: [DataOne: Managing Access Requests]
- How to revoke access: [DataOne: Revoke Access]

Mapping to Snowflake functional roles
- Each CIDM AD group maps to a Snowflake role (Functional Role). The provisioning process should ensure role names follow the naming pattern above and that grants are applied according to the Data Team's SRA (Solution Registration Agreement) and least-privilege principle.

Next actions for this doc
- Add exact CIDM group naming templates for each Roche account (from DataOne). 
- Paste links/instructions from the DataOne pages for the request/approval UI and sample iCare templates.

Account-level permissions & access restrictions
----------------------------------------------
Global account-level permissions allow operations across the entire Snowflake account. Many of these features, if not restricted, would pose a serious security or financial risk. The table below summarises the Roche access stance for high-impact permissions. If a permission is not listed, the default restriction is "Restricted to: Snowflake Team".

| Permission | Impact | Description | Roche restriction / availability |
| --- | --- | --- | --- |
| APPLY MASKING POLICY | Very High | Overrides RBAC. Allows unrestricted SET/UNSET MASKING POLICY operations on all applicable objects in Snowflake. | Restricted to: Snowflake Team, Immuta |
| APPLY ROW ACCESS POLICY | Very High | Overrides RBAC. Allows unrestricted ADD/DROP ROW ACCESS POLICY operations on all applicable objects in Snowflake. | Restricted to: Snowflake Team, Immuta |
| APPLY TAG | Very High | Overrides RBAC. Allows unrestricted SET TAG operations on all applicable objects in Snowflake. | Restricted to: Snowflake Team, Immuta |
| EXECUTE TASK | Low | Allows executing Snowflake Tasks that are in possession of the role or a role in the hierarchy for the session. | Freely available by request |
| CREATE DATABASE | Medium | Allows creating databases. | Limited availability. Delegated via service principal (SP) to Data Team ADMIN roles for DO.R pipelines (feature branches). |
| CREATE INTEGRATION | Very High | Allows creating security (SAML, OAuth) and storage integrations. | Restricted to: Snowflake Team |
| CREATE NETWORK POLICY | Very High | Allows creating network policies, including IP whitelisting. | Restricted to: Snowflake Team |
| CREATE ROLE | Medium | Allows creating Snowflake roles. | Limited availability. Restricted to: Snowflake Team, DataOne L1 Support, SCIM, Immuta. Delegated via SP to Data Team ADMIN roles for DO.R pipelines. |
| CREATE USER | Very High | Allows creating Snowflake users. | Restricted to: Snowflake Team, DataOne L1 Support, SCIM |
| CREATE WAREHOUSE | Very High | Allows creating Snowflake virtual warehouses (could lead to serious budget implications). | Restricted to: Snowflake Team, DataOne L1 Support |
| MANAGE GRANTS | Highest | Equivalent to SECURITYADMIN. Allows unrestricted permission control across the entire account, incl. self-granting ACCOUNTADMIN, ORGADMIN (if applicable). | Restricted to: Snowflake Team |
| READ SESSION | Low | Allows seeing session context of users. Required for Streamlit apps that use Masking/Row access policies. Considered safe — user that owns Streamlit can see their own session only. | Restricted to: Streamlit Application Owner by request |

Guidance and operational controls
- Default restriction: Any permission not explicitly listed above is considered restricted to the Snowflake Team unless otherwise noted. This is to protect account-wide configuration, security posture, and financial exposure.
- Delegation via service principals: Where automation or DO.R pipelines require higher privileges (CREATE DATABASE, CREATE ROLE), Roche delegates via service principals assigned to Data Team ADMIN functional roles. Such delegations are timeboxed and reviewed.
- Requesting elevated permissions: Follow CIDM/Snowflake access request process. Approval must include justification, risk assessment and an owner for auditability. Links: How to Request Snowflake Access (CIDM) and iCare templates.
- Auditing & monitoring: Any assignment of high-impact permissions (CREATE USER, MANAGE GRANTS, CREATE WAREHOUSE, APPLY MASKING/ROW policies) must be logged and reviewed periodically. Use `ACCOUNT_USAGE` views and `ACCESS_HISTORY` to verify activity.

Example observability queries

- Recent attempts to set masking policies:
	- Reference: Snowflake Masking Policies docs: https://docs.snowflake.com/en/user-guide/security-column-reduction-masking-policies.html
```
SELECT event_time, user_name, object_name, statement
FROM SNOWFLAKE.ACCOUNT_USAGE.ACCESS_HISTORY
WHERE statement ILIKE '%MASKING POLICY%'
ORDER BY event_time DESC
LIMIT 200;
```

- Who has MANAGE GRANTS-like capabilities (roles with SECURITYADMIN):
	- Reference: Snowflake ACCOUNT_USAGE GRANTS_TO_ROLES docs: https://docs.snowflake.com/en/sql-reference/account-usage/grants_to_roles.html
```
SELECT grantee_name, privilege
FROM SNOWFLAKE.ACCOUNT_USAGE.GRANTS_TO_ROLES
WHERE privilege = 'MANAGE GRANTS'
ORDER BY grantee_name;
```

Access revocation and emergency controls
- Emergency suspension: Resource monitors and guardrails can suspend warehouses or block operations in billing emergencies; for security incidents, remove users from CIDM groups and notify the Snowflake Team for rapid access revocation.
- Periodic reviews: Perform quarterly reviews of roles with Very High / Highest impact permissions and verify membership and justifications.


Authentication & provisioning

Overview
While Snowflake supports multiple authentication methods, DataOne mandates federated authentication (SAML/OIDC) as the first-choice method for personal user accounts. This provides centralized identity, MFA enforcement, and automated provisioning via SCIM.

Authentication method recommendations (DataOne)
- Federated Authentication (SAML/OIDC) — Mandatory for personal user accounts
	- Federated SSO is the required method for all human user accounts. Exceptions are not allowed.
	- SCIM provisioning must be used to map CIDM AD groups into Snowflake functional roles and to automate lifecycle events (joiners, movers, leavers).

- Encrypted RSA Key-Pair — Allowed for service accounts (second choice)
	- Service accounts (machine/service principals) that require programmatic access should use encrypted RSA key-pair authentication.
	- Key pairs must be stored securely (vaulted), rotated according to Roche key rotation policy, and scoped to the minimal roles required.

- Snowflake password authentication — Last resort (requires exception)
	- Password-based authentication for personal accounts is considered a much less secure alternative and must not be used unless the application does not support federated auth.
	- Any use of password authentication requires a documented, approved security exception from Roche InfoSec.

Service account rules
- Use key-pair auth for service principals and non-interactive logins.
- Avoid long-lived embedded credentials; prefer vault integration and short-lived tokens where supported.
- Document the owner, purpose, and rotation schedule for each service account in the team registry.

Provisioning & lifecycle
- SCIM is used to provision and synchronize CIDM groups into Snowflake functional roles. Ensure group naming follows the DataOne pattern and that OWNER/IT_RESPONSIBLE approvers are correctly configured.
- Onboarding/offboarding: tie CIDM lifecycle events to access reviews and automated deprovisioning workflows to ensure rapid revocation when users leave or change roles.

Exceptions & approval
- Any deviation from the mandatory federated authentication rule (for example, allowing password auth) must be accompanied by a formal exception request, risk assessment, and a documented mitigation plan approved by Roche InfoSec.


Masking & Row Access
- Column masking policies: Apply masking policies to columns carrying PII or sensitive values. Prefer dynamic data masking (Snowflake masking policies) tied to roles (e.g., hide for ANALYST, show for PHI_ADMIN).


Row access policies
- Use row access policies to restrict rows based on attributes (e.g., organisation, legal entity, or tag-driven access). Combine with session context for attribute-based access where required.

Encryption & Key Management
- Default: Snowflake-managed keys. For Roche accounts requiring customer-managed keys (CMK), follow Roche KMS onboarding steps and get approval from InfoSec. Document the account-level parameter changes and key rotation schedule.


Operational notes & Roche rules
- Principle of least privilege is mandatory—grant roles with minimum required privileges and use role chaining.
- Do not use `ACCOUNTADMIN` or `ORGADMIN` for operational queries or everyday tasks; create scoped roles instead.
- All PII/regulated data must be tagged using the DiGi tag registry and enforced via masking/row policies.
- Any deviation from these policies must be approved by Roche Security and documented in the change request.
