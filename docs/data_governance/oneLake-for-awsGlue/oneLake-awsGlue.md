# OneLake for AWS Glue Catalog: Technical Architecture

OneLake for **AWS Glue** (formerly **ECAS DataHub**) is the Data Mesh enabler. While the S3 model is about *storage*, the Glue model is about *metadata and sharing* at the table level.

---

## 1. The Architectural Shift

The fundamental difference between the two models lies in **Data Ownership & Storage**:

| Feature | OneLake for S3 | OneLake for AWS Glue |
|---------|---------------|---------------------|
| Storage Location | Centrally stored in OneLake AWS Accounts | Decentralized: Owned, stored, and managed by the Project's AWS Account |
| Asset Type | Files and Prefixes (Folders) | Glue Data Assets (Databases and Tables) |
| Management | Managed by OneLake storage policies | Managed by the Project Team (Data Mesh style) |

---

## 2. Streamlined Workflows (Self-Service)

To lower technical barriers, the process is automated for both ends of the pipeline:

### For Data Producers (Registration)

Technical registration of Glue Data Assets is simplified through **AWS Service Catalog Products**. This allows producers to register assets in a standardized, *push-button* manner.

### For Data Consumers (Provisioning)

Access provisioning is transparent. Once the request is approved in the **RDM Portal**, the technical setup happens automatically without requiring the consumer's manual involvement.

---

## 3. Integrated Governance Tools

The Glue Catalog flavor follows the same governance backbone but with a different focus:

- **EDMA** — Used by Producers to register and assemble Data Assets into Datasets and Data Products.
- **RDM Portal** — The central hub where Consumers search and *shop* for these Data Products.
- **OneLake for Glue** — Handles the technical *registration, searching, and sharing* of the Glue-specific assets.

---

> **Key Takeaway:**
>
> *"In the S3 model, we move your data to a central OneLake bucket. In the Glue model, the data stays in your project account, but we use OneLake and RDM to 'index' and 'share' it as a governed Data Product. This makes your data F.A.I.R. without losing control over the physical storage."*

---

## 4. Which Model Should I Choose?

Use the following decision tree to determine the right approach for your project:

| Question | If Yes → | If No → |
|----------|----------|---------|
| Do you need to retain full control of data storage in your own AWS account? | **Glue Model** | Consider S3 Model |
| Are you sharing data at the **table level** (structured/semi-structured)? | **Glue Model** | S3 Model |
| Are you sharing raw files or unstructured data? | **S3 Model** | Glue Model |
| Does your team already manage its own Glue Catalog? | **Glue Model** | S3 Model (simpler onboarding) |