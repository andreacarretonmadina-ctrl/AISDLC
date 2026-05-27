---
sidebar_position: 3
title: Implementation steps
domain: Tracking & Analytics
---

# OneDataLayer / EDDL Implementation Steps

This guide explains how to implement Roche's Event-Driven Data Layer (EDDL) using the JavaScript object `window.oneDataLayer`.

## Purpose

`window.oneDataLayer` is the shared client-side event queue used to store analytics events and context before they are consumed by tag management and analytics tooling.

Applications should push structured event objects into `window.oneDataLayer` so tracking remains consistent across websites, SPAs, and widgets.

## 1. Initialize the data layer

Initialize the array as early as possible in the `<head>` and before any event pushes:

```html
<script>
  window.oneDataLayer = window.oneDataLayer || [];
</script>
```

Use `window.oneDataLayer` consistently. Do not use `window.digitalData` for OneDataLayer events.

## 2. Load the tag management / Adobe script

Load the appropriate Adobe Launch or tag management script according to the environment. The data layer must already exist before downstream tooling consumes queued events.

Example pattern:

```html
<head>
  <script>
    window.oneDataLayer = window.oneDataLayer || [];
  </script>

  <!-- Adobe Launch script for the target environment goes here -->
  <script src="YOUR_ADOBE_LAUNCH_SCRIPT" async></script>
</head>
```

## 3. Push a page-view event

Push `defaultPageView` once the page metadata needed for tracking is ready.

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({
  event: "defaultPageView",
  timestamp: Math.floor(Date.now() / 1000),
  page: {
    siteName: "Portal",
    sysEnv: "Prod",
    market: "ES",
    language: "es",
    url: window.location.href,
    domain: window.location.origin,
    path: window.location.pathname || "/",
    pageTitle: document.title,
    pageName: "dashboard",
    pageType: "landing",
    mainCategory: "Dashboard"
  }
});
```

## 4. Push interaction events

Push additional OneDataLayer events when a user interaction occurs, using the matching event template from `docs/oneDataLayer/templates/`.

Example:

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({
  event: "defaultLinkClick",
  timestamp: Math.floor(Date.now() / 1000),
  link: {
    url: "https://example.com",
    name: "add to cart"
  }
});
```

## 5. SPA implementation guidance

For SPAs, fire `defaultPageView`:

- on the initial page load
- on each client-side route change after the new route metadata is ready

Implementation should avoid duplicate page-view events caused by:

- initial render plus hydration
- multiple route listeners for the same navigation
- firing before title or route metadata has updated

## 6. Runtime and canonicalization rules

- Use runtime-derived values for fields such as `timestamp`, `url`, `domain`, `path`, and `pageTitle`.
- Normalize the homepage path to `/`.
- Prefer a canonical pathname value without query string or hash in `page.path`.
- Keep controlled values and enums in English unless market-specific naming requires otherwise.

## 7. Use templates as the structural source of truth

Use the payload files in `docs/oneDataLayer/templates/` as the source of truth for event structure.

When implementing an event:

1. start from the matching template
2. populate only fields supported by the template and the implementation context
3. keep runtime-derived fields dynamic at runtime
4. avoid inventing new field names when an existing standard field already exists

## 8. Minimum validation checklist

Before shipping an implementation, verify:

- `window.oneDataLayer` is initialized before any push
- the correct event name is used
- the event fires at the intended moment
- required page or interaction metadata is present
- page paths are canonicalized consistently
- no duplicate `defaultPageView` events are emitted for a single page view
- field names and casing match the template exactly
