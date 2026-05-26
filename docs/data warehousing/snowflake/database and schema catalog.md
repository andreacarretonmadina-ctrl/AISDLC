## Database & Schema Catalogue

To query a table in Snowflake, you must know its fully qualified name:

```sql
<DATABASE>.<SCHEMA>.<TABLE>

## Database Naming Pattern

Databases follow an environment + domain pattern:

| Environment | Pattern | Example |
|-------------|---------|---------|
| Development | `DEV_<DOMAIN>` | `DEV_DIA` |
| Test | `TEST_<DOMAIN>` | `TEST_DIA` |
| Production | `PROD_<DOMAIN>` | `PROD_DIA` |

Domains correspond to the Snowflake account they belong to (e.g., DIA, PD, CHLOCAL, NAMLOCAL).