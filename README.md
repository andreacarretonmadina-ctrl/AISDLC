# Roche Data Documentation Hub

Central documentation repository for Roche analytics, tracking, data governance, and data modeling guidance.

This repository brings together implementation guides, standards, and reference material used by product teams, developers, analysts, and technical leads.

## What you will find here

This repo contains documentation for:

- **OneDataLayer / EDDL**
  - event-driven tracking implementation
  - event naming and firing guidance
  - payload templates
  - data dictionary
- **Adobe Analytics**
  - setup guidance for new websites
  - Adobe script implementation references
  - analytics-related supporting documentation
- **Data Governance**
  - OneLake concepts and integration guidance
  - governance-related reference documentation
- **Data Modeling**
  - layer definitions
  - configuration and logging standards
  - data vault and loading approaches
- **Site-specific implementation guides**
  - practical examples for individual websites such as Maelle

## Who this repo is for

This repository is intended for:

- frontend developers implementing tracking
- technical leads onboarding websites into analytics
- analytics engineers and analysts
- data platform and governance teams
- project teams looking for Roche standards and implementation references

## Start here

### If you are implementing website tracking
Start with:

- [OneDataLayer Overview](docs/oneDataLayer/overview.md)
- [What is EDDL and how it works](docs/oneDataLayer/what-is-EDDL-and-how-it-works.md)
- [EDDL Implementation Steps](docs/oneDataLayer/EDDL-implementation-steps.md)
- [When to fire each event](docs/oneDataLayer/when-to-fire-each-event.md)
- [Data Dictionary](docs/oneDataLayer/data-dictionary.md)

### If you are onboarding a new website to Adobe Analytics
Start with:

- [Adobe Analytics Setup for New Websites](docs/oneDataLayer/adobe-analytics-set-up-for-new-websites.md)
- [Adobe Script Implementation](docs/oneDataLayer/adobe-script-implementation.md)

### If you need site-specific examples
See:

- [Site-Specific Implementation Guide](site-specific-implementation-guide/README.md)

### If you are looking for broader platform documentation
See:

- [Documentation Index](docs/README.md)
- [AISDLC Documentation Hub](docs/aisdl-documentation-hub.md)

## Repository structure

```text
AISDLC_data/
├── README.md
├── docs/
│   ├── README.md
│   ├── aisdl-documentation-hub.md
│   ├── adobe-analytics/
│   ├── oneDataLayer/
│   ├── data_governance/
│   ├── data_modeling/
│   ├── data warehousing/
│   └── agent/
├── site-specific-implementation-guide/
│   ├── README.md
│   ├── adobe-script-implementation.md
│   ├── how-to-implement-defaultPageView-on-maelle.md
│   └── maelle-tracking-events/
└── img/
```

## Recommended navigation

Use this reading order for tracking implementations:

1. `docs/oneDataLayer/overview.md`
2. `docs/oneDataLayer/what-is-EDDL-and-how-it-works.md`
3. `docs/oneDataLayer/EDDL-implementation-steps.md`
4. `docs/oneDataLayer/when-to-fire-each-event.md`
5. `docs/oneDataLayer/templates/`
6. `docs/oneDataLayer/data-dictionary.md`

For Adobe onboarding:

1. `docs/oneDataLayer/adobe-analytics-set-up-for-new-websites.md`
2. `docs/oneDataLayer/adobe-script-implementation.md`
3. Request validation from the Analytics team after implementation

## Contribution guidance

When adding or updating documentation:

- keep file and folder naming consistent
- add frontmatter where required
- prefer reusable documentation in `docs/`
- place website-specific examples in `site-specific-implementation-guide/`
- update this README when adding a new major documentation area

## Suggested documentation ownership model

- **Shared standards** go in `docs/`
- **Implementation examples** go in `site-specific-implementation-guide/`
- **Images and supporting assets** go in `img/`

---

If you are new to this repository, begin with the **Start here** section above and follow the path that matches your role or task.