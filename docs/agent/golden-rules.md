# Agent Golden Rules (Non‑Negotiables)

These are invariant requirements. If any rule is violated, the agent must block the change (or request remediation) unless an explicit exception is documented and approved by the Data Owner + Platform Owner.

## PSA / Ingestion invariants (Snowflake PSA / Bronze)
1. **Append-only**: PSA tables are insert-only. No UPDATE/DELETE.
2. **Raw payload retention**: Raw record must be preserved (e.g., `JSON_STRING` column).
3. **Standard SYS columns**: Every ingested row must include the standard operational metadata columns (see checklist).
4. **Soft delete required**: Deletions from source must be represented as soft-delete flags/markers in PSA; do not physically delete.
5. **Failed loads must not insert**: If a job fails validation, do not insert partial/invalid data.
6. **Keep data indefinitely**: PSA retains history; avoid destructive retention policies unless explicitly approved.

## Naming invariants
1. PSA objects are prefixed with the **source system name**.
2. Column naming is consistent and documented (avoid ambiguous abbreviations; follow agreed casing convention).
3. Domains/ownership are explicit (owner, steward, business context).

## Governance invariants (tags, metadata, access)
1. Tables/columns must be **tagged/classified** per governance strategy.
2. Ownership metadata must be present and accurate (Data Owner, Steward, Technical contact).
3. Access is granted via defined workflow (no ad-hoc grants); restricted data must follow the restricted access path.

## Data Quality (DQ) invariants
At minimum, each ingestion must define checks for:
- **Uniqueness** (where keys exist)
- **Completeness** (critical fields not null)
- **Freshness** (data arrives within expected SLA)

DQ checks must produce auditable results (logs/metrics) and be tied to load status.

## Exceptions
Exceptions must include:
- Rule(s) being violated
- Business justification
- Compensating controls
- Approval evidence (Data Owner + Platform Owner)
- Expiration date and review plan
