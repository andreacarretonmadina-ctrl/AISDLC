---
sidebar_position: 1
---

# OneDataLayer Overview

OneDataLayer is the shared analytics data model used by this repository. It defines the event structure that applications and widgets should use so telemetry remains consistent across the portal ecosystem.

## Common Building Blocks

The templates in this repository show a recurring set of top-level objects:

- `event`: the event family name, such as `defaultPageView` or `onsiteSearchClick`
- `timestamp`: when the event happened (Unix milliseconds)
- `page`: route and page metadata when the event is page-related
- `content`: content and search metadata when the event is content-related
- `link`: clicked link metadata for click-oriented events
- `user`: user identity, consent, and profile-related context when available
- `components`: metadata about widgets, embeds, or other rendered units involved in the interaction

## Reference Shape

```json
{
  "event": "defaultPageView",
  "timestamp": 0,
  "page": {
    "siteName": "Portal",
    "sysEnv": "QA",
    "market": "ES",
    "language": "es",
    "url": "https://navifyportal-qa.roche.com/es/es-es/dashboard",
    "path": "es/es-es/dashboard",
    "pageTitle": "Portal Dashboard",
    "pageName": "dashboard",
    "pageType": "landing"
  },
  "user": {
    "isLoggedIn": 1
  },
  "components": {
    "embeds ": {
      "items": []
    }
  }
}
```

## What The Model Optimizes For

- Reusable event families across products
- Stable field names for downstream analytics processing
- Enough context to interpret behavior without product-specific reverse engineering
- A clear boundary between required structure and scenario-specific values

## Templates And Examples

Use the repository assets together:

- `templates/`

Templates define the shape. Examples show realistic combinations of values. Templates show the full standard structure and available fields for an event. Not every field shown in a template is mandatory in every implementation context unless explicitly required by documentation.

e.g. eventDetails appears in many templates but is optional unless a use case requires additional context.


## Guidance For Implementers

Before adding a new event shape, check whether an existing event family already models the behavior. Reusing the standard is preferred over inventing a new payload family.

