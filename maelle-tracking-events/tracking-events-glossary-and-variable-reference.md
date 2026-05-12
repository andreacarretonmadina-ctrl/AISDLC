---
title: Tracking Events Glossary & Variable Reference
domain: Tracking & Analytics
last_updated: 2026-05-08
owner: Maelle / Analytics Team
related:
  - eddl-overview.md
  - adobe-script-implementation.md
  - ../maelle-tracking-events/README.md
---

# Tracking Events Glossary & Variable Reference

This document defines every event type, its required attributes, and which variables are **dynamic** (resolved at runtime from the page/user context).

---

## 1. Event Types Overview

| Event Name | Trigger | Sample Files |
|------------|---------|--------------|
| `defaultPageView` | Page or SPA view loads successfully | `defaultPageView events/` |
| `defaultLinkClick` | User clicks a link, button, or interactive element | `defaultLinkClick events/` |
| `onsiteSearch` | User submits a search query | `onSiteSearch events/` |
| `onsiteSearchClick` | User clicks on a search result | `onSiteSearchClick events/` |

---

## 2. Common Fields (All Events)

| Field | Type | Dynamic? | Description |
|-------|------|----------|-------------|
| `event` | `string` | ❌ Static | Event name identifier. Must match one of the four event types above. |
| `timestamp` | `number` | ✅ Dynamic | Unix epoch timestamp (seconds). Resolved at the moment the event fires via `Math.floor(Date.now() / 1000)`. |

---

## 3. `defaultPageView` — Attribute Reference

Fires once per page load (or SPA route change). All fields live under the `page` object.

| Field | Type | Dynamic? | Resolution Logic | Example |
|-------|------|----------|-----------------|---------|
| `page.siteName` | `string` | ❌ Static | Always `"Maelle"` | `"Maelle"` |
| `page.sysEnv` | `string` | ⚠️ Per-environment | Set from build/deploy config: `Prod`, `Staging`, `Dev` | `"Prod"` |
| `page.language` | `string` | ✅ Dynamic | Derived from `<html lang>` or locale config (ISO 639-1 lowercase) | `"es"` |
| `page.url` | `string` | ✅ Dynamic | `window.location.href` (full URL including query/hash) | `"https://…/capabilities"` |
| `page.domain` | `string` | ✅ Dynamic | `window.location.origin` | `"https://probable-adventure-1qg1yjl.pages.github.io"` |
| `page.path` | `string` | ✅ Dynamic | `window.location.pathname` (empty string `""` for root `/`) | `"/capabilities"` |
| `page.pageTitle` | `string` | ✅ Dynamic | `document.title` or a predefined page title map | `"Capabilities"` |
| `page.mainCategory` | `string` | ⚠️ Per-page | Mapped from route. See **Page Category Map** below. | `"HomePage"` |

### Page Category Map

| Route Pattern | `mainCategory` | `pageTitle` |
|---------------|---------------|-------------|
| `/` (root) | `HomePage` | `Maelle - Your Corporate Brain` |
| `/how-it-works` | `AboutPage` | `How it Works` |
| `/capabilities` | `CapabilitiesPage` | `Capabilities` |
| `/resources` | `ResourcesPage` | `Resources` |

---

## 4. `defaultLinkClick` — Attribute Reference

Fires when a user clicks a tracked link or button. All fields live under the `link` object. Some events include an optional `components` object.

| Field | Type | Dynamic? | Resolution Logic | Example |
|-------|------|----------|-----------------|---------|
| `link.url` | `string` | ✅ Dynamic | `element.href` — the full destination URL | `"https://…/how-it-works"` |
| `link.domain` | `string` | ✅ Dynamic | Extracted origin from `link.url` | `"https://probable-adventure-…"` |
| `link.path` | `string` | ✅ Dynamic | Pathname extracted from `link.url` | `"/how-it-works"` |
| `link.innerText` | `string` | ✅ Dynamic | `element.innerText.trim()` — visible label of the clicked element. The DOM Element innerText of the link clicked.| `"Learn more"` |
| `link.position` | `string` | ❌ Static per element | Identifies where on the page the element lives. The DOM Element position of the link clicked. Set via `data-tracking-position` attribute or hardcoded per component. | `"hero"`, `"navBar/header"`, `"body/onboardingSection"` |
| `link.name` | `string` | ❌ Static per element | Logical name for the action. Set via `data-tracking` attribute. | `"AccessMaelle"`, `"ContactSubmit"` |
| `link.type` | `string` | ❌ Static per element | Element type: `"link"`, `"button"`, or `"icon"` | `"button"` |

### Position Values Reference (not used at the moment as it would require manual config. Website contains one unique link/click for each content by page so we can differentiate where it happened. For Access Maelle button we could implement a data-tracking-position attribute)

| `link.position` | Where on page | Used by |
|-----------------|---------------|---------|
| `hero` | Hero/banner section | accessMaelle, teamContact, resourcesLink |
| `navBar/header` | Top navigation bar | accessMaelle (navbar) |
| `body/onboardingSection` | Body — onboarding area | teamContact (body) |
| `onboardingSection` | Onboarding CTA section | capabilitiesButton |
| `capabilitiesSection` | Capabilities listing | capabilitiesLink, BusinessModuleLink |
| `howItWorksSection` | How-it-works section | howItWorksLink |
| `bottomForm` | Contact form at bottom | contactSubmit |
| `ResourcesFeed` | Resources feed/list | readMoreLink, resourcesIcon |
| `hero\|resourcesFeed` | Hero or resources feed (context-dependent) | PillarExternalLink |

### Link Name Values Reference (ignoring for the moment as it requires manual config, instead we will capture element innerText)

| `link.name` | Action |
|-------------|--------|
| `AccessMaelle` | Navigate to Maelle app |
| `TeamContact` | Scroll to contact section (hero) |
| `Contact` | Scroll to contact section (body) |
| `ContactSubmit` | Submit contact form |
| `ExploreCapabilities` | Navigate to capabilities page |
| `ExploreBusiness/module` | Navigate to a specific capability module |
| `HowItWorks` | Navigate to methodology page |
| `ResourcesFeed` | Navigate to resources feed |
| `ResourcesReadMore` | Open a specific resource article |
| `Resources` | Click resource icon |
| `ExternalPage` | Navigate to an external site |

### Optional: `components.embeds` (for content-linked clicks)

Used when a click is associated with a specific content item (e.g., a resource article or capability module).

| Field | Type | Dynamic? | Description |
|-------|------|----------|-------------|
| `components.embeds.items[].type` | `string` | ✅ Dynamic | Content type: `"BusinessResource"`, `"business"`, etc. |
| `components.embeds.items[].id` | `string` | ✅ Dynamic | Content identifier, typically derived from the URL slug (e.g., `"maelle-is-here:one-roche"`) |

---

## 5. `onsiteSearch` — Attribute Reference

Fires when a user submits a search query. 
On search results page or search/filter update. Fields live under `content`.

| Field | Type | Dynamic? | Resolution Logic | Example |
|-------|------|----------|-----------------|---------|
| `content.onsiteSearch.term` | `string` | ✅ Dynamic | The search query string entered by the user | `"data"` |
| `content.onsiteSearch.results` | `string` | ✅ Dynamic | Number of results returned (as string) | `"5"` |
| `content.searchComponent` | `string` | ❌ Static per component | Identifier of the search UI component | `"capabilities-search-bar"` |

---

## 6. `onsiteSearchClick` — Attribute Reference

Fires when a user clicks a search result. Combines `content`, `components`, and `link` objects.

| Field | Type | Dynamic? | Resolution Logic | Example |
|-------|------|----------|-----------------|---------|
| `content.onsiteSearch.term` | `string` | ✅ Dynamic | The original search query | `"data"` |
| `content.onsiteSearch.results` | `string` | ✅ Dynamic | Total results count | `"5"` |
| `content.onsiteSearch.resultPosition` | `string` | ✅ Dynamic | 1-based index of the clicked result | `"2"` |
| `content.searchComponent` | `string` | ❌ Static | Search component ID | `"capabilities-search-bar"` |
| `components.embeds.items[].type` | `string` | ✅ Dynamic | Type of result clicked | `"business"` |
| `components.embeds.items[].id` | `string` | ✅ Dynamic | ID of the result clicked | `"Product Manager"` |
| `link.*` | | | Same schema as `defaultLinkClick.link` | |

---

## 7. HTML Data Attributes for the Agent

To enable automatic event binding, add these `data-*` attributes to tracked elements:

```html
<a
  data-tracking-position="navBar/header/hero"
  data-tracking-type="button/link/icon"
  href="/projects"
>
  Access Maelle
</a>
```

| Attribute | Maps to | Required |
|-----------|---------|----------|
| `data-tracking` | `link.name` |  No |
| `data-tracking-position` | `link.position` | ✅ Yes |
| `data-tracking-type` | `link.type` | ✅ Yes  |

Dynamic fields (`link.url`, `link.path`, `link.domain`, `link.innerText`) are resolved automatically from the element at click time.

---

## 8. Dynamic Variable Resolution Summary

| Variable | Source |
|----------|--------|
| `timestamp` | `Math.floor(Date.now() / 1000)` |
| `page.url` | `window.location.href` |
| `page.domain` | `window.location.origin` |
| `page.path` | `window.location.pathname` |
| `page.pageTitle` | `document.title` |
| `page.language` | Locale config or URL segment |
| `link.url` | `element.href` |
| `link.domain` | `new URL(element.href).origin` |
| `link.path` | `new URL(element.href).pathname` |
| `link.innerText` | `element.innerText.trim()` |
| `content.onsiteSearch.term` | Search input value |
| `content.onsiteSearch.results` | Result count from search response |
| `content.onsiteSearch.resultPosition` | Index of clicked result |