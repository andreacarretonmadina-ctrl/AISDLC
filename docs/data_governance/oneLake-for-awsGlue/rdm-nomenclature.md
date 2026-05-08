# RDM (Roche Data Marketplace) Nomenclature

## Core Concepts

RDM uses a **hierarchical structure** to organize data. Think of it as nested containers:

```
Data Product
 └── Dataset
      └── Data Asset
```

| Term | Definition | Analogy |
|------|-----------|---------|
| **Data Product** | Logical entity that contains 1 or more Datasets | A "catalog item" in a store |
| **Dataset** | Logical entity that contains 1 or more Data Assets | A "shelf" grouping related items |
| **Data Asset** | Technical asset in the physical system | The actual "product on the shelf" |

---

## Data Assets in OneLake for AWS Glue Catalog

In the Glue context, a **Data Asset** maps to the following AWS resources:

| AWS Glue Resource | Role in RDM |
|-------------------|-------------|
| Glue Database | Container for tables |
| Glue Table | **Primary entity** — this is the level at which Data Assets are registered |
| Glue Columns + Metadata | Descriptive attributes of the Data Asset |

> 💡 **Key point:** A Dataset contains Data Assets at the **Glue Table level**.

---

## Rules & Constraints

- ✅ A Dataset **can** contain Data Assets from **different AWS Accounts** and **different Glue Databases**.
- ⛔ All Data Assets within a Dataset **must** be from the **same AWS Region**.
- 📦 Currently, only Glue Tables backed by **AWS S3** are supported.