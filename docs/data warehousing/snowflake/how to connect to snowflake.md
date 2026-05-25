# Snowflake Connection Guide (Roche / DataOne)

Purpose: concise, AI-friendly Roche reference for how to connect to Snowflake — covering account identifiers, supported clients, authentication methods, and Roche-specific rules per persona.

> For authentication policy details (SSO, key-pair, exceptions), see [`security-and-access-control.md`](./security-and-access-control.md).  
> For access provisioning (CIDM groups, role requests), see the same file under *CIDM group-based access model*.

---

## Account Identifiers

Each Roche Snowflake account has a unique account identifier used in all connection strings.

| Account | Region | Account Identifier (format) |
|---------|--------|-----------------------------|
| ROCHE_DIA | AWS EU-Central-1 (Frankfurt) | `<account_locator>.eu-central-1.aws` |
| ROCHE_PD | *(confirm with DataOne)* | `<account_locator>.<region>.aws` |
| DIA_CHLOCAL | AWS EU-Central-1 (Frankfurt) | `<account_locator>.eu-central-1.aws` |
| SANDBOX | *(confirm with DataOne)* | `<account_locator>.<region>.aws` |

> **Note:** Exact account locators are managed by the DataOne / OneSnowflake team. Request yours via [DataOne: How to Request Snowflake Access](https://rochewiki.roche.com/confluence/spaces/dataone/pages/797111537/Snowflake).

The full connection hostname pattern is:
```
<account_locator>.<region>.snowflakecomputing.com
```

---

## Authentication Rules (Summary)

| Persona | Required Method | Notes |
|---------|----------------|-------|
| Human / personal account | Federated SSO (SAML/OIDC) | Mandatory. No exceptions. MFA enforced via IdP. |
| Service account / automation | Encrypted RSA Key-Pair | Keys must be vaulted and rotated per Roche policy. |
| Legacy / no-SSO app | Password auth | Last resort. Requires InfoSec exception approval. |

---

## Connecting via the Snowflake Web UI

1. Navigate to your account URL: `https://<account_locator>.<region>.snowflakecomputing.com`
2. Click **Sign in with SSO** (do not use username/password unless you have an approved exception).
3. Authenticate through the Roche enterprise identity provider (SAML/OIDC).
4. Select the appropriate role upon login (e.g., your team's `DEVELOPER` or `REPORTING` functional role).

---

## Connecting via SnowSQL (CLI)

```bash
snowsql -a <account_locator>.<region> --authenticator externalbrowser
```

- `--authenticator externalbrowser` triggers SSO via your default browser.
- For service accounts using key-pair:

```bash
snowsql -a <account_locator>.<region> \
        -u <service_account_user> \
        --private-key-path /path/to/rsa_key.p8
```

---

## Connecting via Python (Snowflake Connector)

### Human user — SSO (recommended)
```python
import snowflake.connector

conn = snowflake.connector.connect(
    account="<account_locator>.<region>",
    user="<your_roche_email>",
    authenticator="externalbrowser",
    role="<YOUR_TEAM_FUNCTIONAL_ROLE>",
    warehouse="<WAREHOUSE_NAME>",
    database="<DATABASE_NAME>",
    schema="<SCHEMA_NAME>"
)
```

### Service account — RSA Key-Pair
```python
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import serialization
import snowflake.connector

with open("/path/to/rsa_key.p8", "rb") as key_file:
    private_key = serialization.load_pem_private_key(
        key_file.read(),
        password=b"<key_passphrase>",  # or None if unencrypted
        backend=default_backend()
    )

conn = snowflake.connector.connect(
    account="<account_locator>.<region>",
    user="<service_account_user>",
    private_key=private_key,
    role="<TEAM_ADMIN_OR_SCOPED_ROLE>",
    warehouse="<WAREHOUSE_NAME>",
    database="<DATABASE_NAME>",
    schema="<SCHEMA_NAME>"
)
```

> Keys must be stored in a vault (not hardcoded) and rotated per Roche key rotation policy.

---

## Connecting via JDBC / ODBC

Use the official Snowflake JDBC or ODBC driver.

**JDBC connection string:**
```
jdbc:snowflake://<account_locator>.<region>.snowflakecomputing.com/?authenticator=externalbrowser&role=<ROLE>&warehouse=<WAREHOUSE>
```

**ODBC DSN parameters:**
| Parameter | Value |
|-----------|-------|
| Server | `<account_locator>.<region>.snowflakecomputing.com` |
| Authenticator | `externalbrowser` (SSO) or `snowflake_jwt` (key-pair) |
| Role | Your assigned functional role |
| Warehouse | Assigned warehouse name |

---

## Connecting via dbt

In your `profiles.yml`:

```yaml
roche_snowflake:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: <account_locator>.<region>
      user: <your_roche_email>
      authenticator: externalbrowser
      role: <YOUR_TEAM_DEVELOPER_ROLE>
      database: DEV_<DOMAIN>
      warehouse: <WAREHOUSE_NAME>
      schema: <SCHEMA_NAME>
      threads: 4
```

For CI/CD pipelines using a service account, replace `authenticator: externalbrowser` with `private_key_path` and `private_key_passphrase`.

---

## Connecting via Talend

Talend is the **standard ETL tool** for data ingestion at Roche (see [`data-ingestion-architecture.md`](./data-ingestion-architecture.md)).

- Use the Snowflake Talend connector with key-pair authentication for service accounts.
- The service account must belong to the team's `ADMIN` functional role for write operations.
- Credentials must be stored in Talend Vault or the approved secrets manager — never hardcoded in job properties.

---

## Network & Connectivity Requirements

- **Production accounts:** Private connectivity (PrivateLink / private endpoints) is required. Ensure your client is running within the Roche network or VPN.
- **Sandbox accounts:** Standard internet connectivity may be used; confirm with the DataOne team.
- Network policies and IP allowlisting are managed exclusively by the Snowflake Team. Do not attempt to modify these.

---

## Role & Warehouse Selection

Always specify the **minimum required role** for your task:

| Persona | Recommended Role |
|---------|-----------------|
| Analyst / consumer | `<TEAM>_REPORTING` |
| Developer (DEV work) | `<TEAM>_DEVELOPER` |
| Automation / pipeline | `<TEAM>_ADMIN` (via service principal) |
| Schema provisioning | `SYSADMIN` (platform engineers only) |

Do **not** use `ACCOUNTADMIN` or `ORGADMIN` for queries or everyday operations.

---

## Troubleshooting

| Issue | Likely Cause | Action |
|-------|-------------|--------|
| SSO redirect fails | Not on Roche network / VPN | Connect to VPN and retry |
| `IP not allowed` error | Network policy violation | Confirm you're on the corporate network; contact Snowflake Team if persistent |
| Role not found | CIDM group not provisioned yet | Check SCIM sync status; allow up to 24h after approval |
| Key-pair auth rejected | Key not registered or expired | Re-register the public key via the DataOne team; check rotation schedule |
| Access denied to schema | Missing role grant | Request appropriate CIDM group membership via the [DataOne access request process](https://rochewiki.roche.com/confluence/spaces/dataone/pages/797111537/Snowflake) |

---

## Links & Runbooks

- [DataOne: How to Request Snowflake Access](https://rochewiki.roche.com/confluence/spaces/dataone/pages/797111537/Snowflake)
- [Snowflake Python Connector docs](https://docs.snowflake.com/en/developer-guide/python-connector/python-connector)
- [SnowSQL CLI docs](https://docs.snowflake.com/en/user-guide/snowsql)
- [Snowflake JDBC Driver docs](https://docs.snowflake.com/en/developer-guide/jdbc/jdbc)
- [Snowflake ODBC Driver docs](https://docs.snowflake.com/en/developer-guide/odbc/odbc)
- Security & Access Control: [`security-and-access-control.md`](./security-and-access-control.md)
- Data Ingestion Architecture: [`data-ingestion-architecture.md`](./data-ingestion-architecture.md)