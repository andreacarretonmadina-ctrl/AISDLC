# Shared Documentation Index

This folder contains shared documentation, standards, and reference material used across Roche analytics, tracking, AI, and data platform work.

Use the `docs/` directory for documentation that applies across teams, websites, or solutions. For implementation details tied to a specific website or product, use the site-specific guides outside this folder.

## Purpose

The `docs/` folder is the home for reusable documentation such as:

- tracking standards and event guidance
- Adobe Analytics onboarding and reference material
- data governance documentation
- data modeling standards
- AI / agent-related guidance
- platform and warehousing reference documentation

## When to use this folder

Use this folder if you need:

- shared standards that apply across multiple implementations
- reusable event definitions and implementation guidance
- data governance or modeling reference material
- platform-level documentation
- central documentation intended for more than one team or website

If you need concrete examples for a specific website or property, go to:

- [`../site-specific-implementation-guide/`](../site-specific-implementation-guide/)

## Documentation areas

### OneDataLayer / EDDL
Guidance for implementing Roche’s Event-Driven Data Layer using `window.oneDataLayer`.

Start with:

- [Overview](./oneDataLayer/overview.md)
- [What is EDDL and how it works](./oneDataLayer/what-is-EDDL-and-how-it-works.md)
- [Implementation steps](./oneDataLayer/EDDL-implementation-steps.md)
- [When to fire each event](./oneDataLayer/when-to-fire-each-event.md)
- [Data dictionary](./oneDataLayer/data-dictionary.md)

### Adobe Analytics
Supporting documentation related to Adobe Analytics.

Current contents include:

- [Data Access Layer](./adobe-analytics/data-access-layer.md)
- [Dimensions and Metrics](./adobe-analytics/dimensions-and-metrics.md)
- [KPI Definition Document](./adobe-analytics/kpi-definition-document.md)
- [Session Identity Logic](./adobe-analytics/session-identity-logic.md)

### Data Governance
Documentation related to governance architecture and governed platform usage.

See:

- [`data_governance/`](./data_governance/)

### Data Modeling
Documentation for modeling standards, layers, and implementation patterns.

See:

- [`data_modeling/`](./data_modeling/)

### Data Warehousing
Documentation related to warehousing platforms and standards.

See:

- [`data warehousing/`](./data%20warehousing/)

### Agent
Guidance and supporting references for AI / agent-related work.

See:

- [`agent/`](./agent/)

## Suggested reading paths

### For tracking implementation
1. [OneDataLayer Overview](./oneDataLayer/overview.md)
2. [What is EDDL and how it works](./oneDataLayer/what-is-EDDL-and-how-it-works.md)
3. [Implementation steps](./oneDataLayer/EDDL-implementation-steps.md)
4. [When to fire each event](./oneDataLayer/when-to-fire-each-event.md)
5. [Data dictionary](./oneDataLayer/data-dictionary.md)

### For shared standards and architecture
1. Choose the relevant domain folder under `docs/`
2. Start with overview or introductory documents if available
3. Use deeper reference files for implementation details and standards

## Documentation organization rule

As a general rule:

- put **shared, reusable documentation** in `docs/`
- put **website- or product-specific implementation guidance** in `site-specific-implementation-guide/`

## Contribution guidance

When adding documentation under `docs/`:

- prefer content that is reusable across teams or implementations
- avoid putting website-specific exceptions here
- use descriptive, consistent file names
- update this index when adding a new major section
- add frontmatter when required by the documentation site

---

If you are not sure where to begin, start with the section that matches your domain or implementation task.