# Site-Specific Implementation Guide

This section contains practical implementation guidance for individual Roche websites and digital properties.

Use this folder when you need examples that translate the shared OneDataLayer / EDDL standards into a concrete website implementation.

## Purpose

The documentation in this folder is intended to help teams:

- apply Roche tracking standards to a specific website
- understand how shared event models are implemented in a real project
- reuse working examples for page views, link clicks, and other tracked interactions
- align site-level implementations with the central documentation in `docs/oneDataLayer/`

## When to use this folder

Use this folder if you are:

- implementing tracking for a specific website or product
- looking for practical examples beyond the shared standards
- validating how common tracking patterns are applied in a real implementation
- onboarding a team that needs site-level event examples

Use the central documentation in `docs/oneDataLayer/` if you need:

- the shared standard
- event structure rules
- the data dictionary
- implementation steps applicable across websites
- onboarding guidance for Adobe Analytics

## Current implementation example

This folder currently includes guidance for the **Maelle** website implementation.

## Contents

- [Adobe Script Implementation](./adobe-script-implementation.md)  
  How to install and validate the Adobe Launch script for the site.

- [How to Implement `defaultPageView` on Maelle](./how-to-implement-defaultPageView-on-maelle.md)  
  Site-specific guidance for page-view implementation.

- [Tracking Events Glossary and Variable Reference](./tracking-events-glossary-and-variable-reference.md)  
  Field definitions and implementation reference for tracked values.

- [Maelle Tracking Events](./maelle-tracking-events/)  
  Event payload examples grouped by event type.

## Suggested reading order

If you are new to this section, read in this order:

1. [Adobe Script Implementation](./adobe-script-implementation.md)
2. [How to Implement `defaultPageView` on Maelle](./how-to-implement-defaultPageView-on-maelle.md)
3. [Tracking Events Glossary and Variable Reference](./tracking-events-glossary-and-variable-reference.md)
4. [Maelle Tracking Events](./maelle-tracking-events/)

## Relationship to the shared standard

This folder complements, but does not replace, the central tracking documentation.

Before using site-specific examples, review the shared standards in:

- [`docs/oneDataLayer/overview.md`](../docs/oneDataLayer/overview.md)
- [`docs/oneDataLayer/EDDL-implementation-steps.md`](../docs/oneDataLayer/EDDL-implementation-steps.md)
- [`docs/oneDataLayer/when-to-fire-each-event.md`](../docs/oneDataLayer/when-to-fire-each-event.md)
- [`docs/oneDataLayer/data-dictionary.md`](../docs/oneDataLayer/data-dictionary.md)

## Recommended usage pattern

1. Start with the shared OneDataLayer / EDDL documentation
2. Use this folder for site-specific implementation examples
3. Reuse patterns where appropriate without breaking the shared event structure
4. Keep website-specific exceptions documented here rather than in the central standard

## Contribution guidance

When adding a new site-specific guide:

- create documents that are clearly tied to one website or property
- keep shared standards in `docs/oneDataLayer/`
- use descriptive filenames
- add links from this README to all new guides
- group event examples in a dedicated subfolder when needed

---

If you are implementing tracking for a specific Roche website, this folder should be your practical companion to the shared standards.