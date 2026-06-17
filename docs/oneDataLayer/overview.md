------

sidebar_position: 1sidebar_position: 1

title: OneDataLayer Overview---

domain: Tracking & Analytics

last_updated: 2026-06-17# OneDataLayer Overview

owner: Analytics Team

---OneDataLayer is the shared analytics data model used by this repository. It defines the event structure that applications and widgets should use so telemetry remains consistent across the portal ecosystem.



# OneDataLayer Overview## Common Building Blocks



## What is OneDataLayer / EDDL?The templates in this repository show a recurring set of top-level objects:



An **Event-Driven Data Layer (EDDL)** is a client-side JavaScript object that provides information about the displayed page and user interactions. It aims to reduce site instrumentation efforts by providing a standardised method for exposing and accessing any type of data that must be available on all pages of a website.- `event`: the event family name, such as `defaultPageView` or `onSiteSearchClick`

- `timestamp`: when the event happened (Unix milliseconds)

Our EDDL implementation is the object `window.oneDataLayer` — a shared **array** that acts as a common language for Digital Analytics and MarTech tools to understand how visitors behave across Roche websites, markets, and products.- `page`: route and page metadata when the event is page-related

- `content`: content and search metadata when the event is content-related

It defines the event structure that applications and widgets should use so telemetry remains consistent across the portal ecosystem.- `link`: clicked link metadata for click-oriented events

- `user`: user identity, consent, and profile-related context when available

---- `components`: metadata about widgets, embeds, or other rendered units involved in the interaction



## How it works## Reference Shape



The `oneDataLayer` array serves as an **event queue** that:```json

{

- **Avoids racing conditions** between scripts — events can be pushed before the tag management script has loaded.  "event": "defaultPageView",

- **Retains the entire history** of events until the page is refreshed.  "timestamp": 0,

- **Enables asynchronous loading** — the page and other scripts can start triggering events and registering data by adding them to the array even before Adobe Launch (or any other tag manager) is loaded.  "page": {

    "siteName": "Portal",

Events are pushed when:    "sysEnv": "QA",

    "market": "ES",

- The page (and subsequent **SPA** views) loaded successfully.    "language": "es",

- A user interacted with website elements such as links, buttons, forms, videos, etc.    "url": "https://navifyportal-qa.roche.com/es/es-es/dashboard",

    "path": "es/es-es/dashboard",

This is done by calling `window.oneDataLayer.push()` with a structured JSON object each time an event occurs.    "pageTitle": "Portal Dashboard",

    "pageName": "dashboard",

---    "pageType": "landing"

  },

## How to push events  "user": {

    "isLoggedIn": 1

```javascript  },

window.oneDataLayer = window.oneDataLayer || [];  "components": {

window.oneDataLayer.push({    "embeds": {

    event: "eventName",      "items": []

    timestamp: Date.now(),    }

    // ...additional parameters  }

});}

``````



> **Important:**## What The Model Optimizes For

> - Always include `window.oneDataLayer = window.oneDataLayer || [];` before any `.push()` call to avoid breaking event tracking.

> - `oneDataLayer` can be initialised without considering the consent state (opt-in/opt-out) since it is a local object on the user's device. Data does not leave the device until a tag manager processes it.- Reusable event families across products

> - Do **not** use native array methods (`.pop()`, `.shift()`, `.splice()`, etc.) to modify `window.oneDataLayer`. Only use `.push()`.- Stable field names for downstream analytics processing

- Enough context to interpret behavior without product-specific reverse engineering

---- A clear boundary between required structure and scenario-specific values



## Top-Level Object Structure## Templates And Examples



Every event pushed to `oneDataLayer` is built from a recurring set of top-level objects:Use the repository assets together:



| Object | Purpose |- `templates/`

|--------|---------|

| `event` | The event family name (e.g., `defaultPageView`, `onSiteSearchClick`) |Templates define the shape. Examples show realistic combinations of values. Templates show the full standard structure and available fields for an event. Not every field shown in a template is mandatory in every implementation context unless explicitly required by documentation.

| `timestamp` | When the event happened — Unix milliseconds, 13 digits (e.g., `1674842407984`) |

| `page` | Route and page metadata |e.g. eventDetails appears in many templates but is optional unless a use case requires additional context.

| `content` | Content, search, form, video, audio, and course metadata |

| `link` | Clicked link metadata |

| `user` | User identity, consent, and profile context |## Guidance For Implementers

| `components` | Metadata about widgets, embeds, or other rendered units |

| `ecommerce` | E-commerce interaction data |Before adding a new event shape, check whether an existing event family already models the behavior. Reusing the standard is preferred over inventing a new payload family.

| `product` | Product context (non-ecommerce) |


---

## Reference Shape

```json
{
  "event": "defaultPageView",
  "timestamp": 1674842407984,
  "page": {
    "siteName": "Portal",
    "sysEnv": "QA",
    "market": "ES",
    "language": "es",
    "url": "https://navifyportal-qa.roche.com/es/es-es/dashboard",
    "path": "/es/es-es/dashboard",
    "pageTitle": "Portal Dashboard",
    "pageName": "dashboard",
    "pageType": "landing"
  },
  "user": {
    "isLoggedIn": 1
  },
  "components": {
    "embeds": {
      "items": []
    }
  }
}
```

---

## What the model optimises for

- Reusable event families across products
- Stable field names for downstream analytics processing
- Enough context to interpret behaviour without product-specific reverse engineering
- A clear boundary between required structure and scenario-specific values

---

## Templates

The folder `docs/oneDataLayer/templates/` contains one canonical JSON file per event type. These templates are the **structural source of truth** for every event push.

- Templates define the **shape** — which fields exist and how they nest.
- Not every field shown in a template is mandatory in every implementation context — only those explicitly marked as required in the documentation.
- `eventDetails` appears in many templates but is optional unless a use case requires additional context.

**Values in the templates must be in English**, except for market-specific content names.

---

## Guidance for implementers

1. Before adding a new event shape, check whether an existing event family already models the behaviour. Reusing the standard is preferred over inventing a new payload family.
2. Start from the matching template in `templates/` when implementing an event.
3. Populate only fields supported by the template and your implementation context.
4. Keep runtime-derived fields (`timestamp`, `url`, `domain`, `path`, `pageTitle`) dynamic.
5. Do not invent new field names when a standard field already exists in the [Data Dictionary](properties/data-dictionary.md).

---

## External references

| Resource | Link |
|----------|------|
| EDDL Changelog (GitLab) | https://code.roche.com/gps/onedatalayer/-/blob/main/oneDataLayer-changelog.md |
