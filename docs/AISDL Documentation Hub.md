---
title: AISDLC Documentation Hub
last_updated: 2026-05-08
owner: AI & Data Platform Team
---

# AISDLC Documentation Hub

Central repository for AI, Data Governance, and Tracking documentation at Roche.

---

## 📁 Project Structure

```
AISDLC/
├── docs/
│   ├── data_governance/
│   │   └── oneLake-for-awsGlue/
│   │       ├── oneLake-awsGlue.md        # Technical architecture overview
│   │       └── rdm-nomenclature.md        # RDM data hierarchy & naming
│   └── data_modeling/
│       ├── data-product-layer.md          # Data Product Layer specification
│       └── configuration-and-logging.md   # DVA tool config & logging tables
├── maelle-how-to-track/
│   └── eddl-overview.md                   # EDDL concepts & event pushing
├── maelle-tracking-events/
│   └── [event files]                      # Individual tracking event specs
├── GLOSSARY.md                            # Centralized term definitions
└── README.md                              # ← You are here
```

---

## 🗂️ Documentation Sections

| Section | Description | Start Here |
|---------|-------------|------------|
| **Data Governance** | OneLake architecture, RDM nomenclature, AWS Glue integration | [OneLake for AWS Glue](docs/data_governance/oneLake-for-awsGlue/oneLake-awsGlue.md) |
| **Data Modeling** | Data Product Layer, configuration & logging | [Data Product Layer](docs/data_modeling/data-product-layer.md) |
| **Maelle – How to Track** | EDDL concepts, how to push tracking events | [EDDL Overview](maelle-how-to-track/eddl-overview.md) |
| **Maelle – Tracking Events** | Catalog of all available tracking event specifications | [Tracking Events](maelle-tracking-events/README.md) |

---

## 📖 Quick Links

- [Glossary of Terms](GLOSSARY.md)
- [RDM Nomenclature](docs/data_governance/oneLake-for-awsGlue/rdm-nomenclature.md)
- [Which OneLake model should I choose?](docs/data_governance/oneLake-for-awsGlue/oneLake-awsGlue.md#4-which-model-should-i-choose)

---

## 🤝 Contributing

1. Use **kebab-case** for all new file and folder names.
2. Add YAML frontmatter to every new `.md` file.
3. Update this README and the relevant section README when adding new docs.
4. Add any new terms to the [Glossary](GLOSSARY.md).