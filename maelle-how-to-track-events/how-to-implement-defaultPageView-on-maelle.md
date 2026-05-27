## How to implement `defaultPageView` on Maelle

## 1. Prerequisites — Adobe Launch Script in `<head>`

Every page must load the Adobe Launch script **before** any `oneDataLayer.push()` calls, and the array must be initialized immediately after it:

```html
<head>
  <!-- Adobe Launch (Production) -->
  <script src="https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-bb453ba32d52.min.js" async></script>

  <!-- Initialize oneDataLayer -->
  <script>
    window.oneDataLayer = window.oneDataLayer || [];
  </script>
</head>
```

> For staging/QA, use `launch-3cdcd1196ea2-staging.min.js` instead. Never load both scripts on the same page.

---

## 2. The `defaultPageView` push — Full pattern

Fire this event on:
- **initial page load**
- **every SPA route change**

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({
  event: "defaultPageView",
  timestamp: Math.floor(Date.now() / 1000), // always dynamic
  page: {
    siteName: "Maelle",                     // static
    sysEnv: "Prod",                         // from build config: "Prod" | "Staging" | "Dev"
    language: "es",                         // from <html lang> or locale config (ISO 639-1)
    url: window.location.href,              // dynamic
    domain: window.location.origin,         // dynamic
    path: window.location.pathname,         // dynamic; homepage must resolve to "/"
    pageTitle: document.title,              // dynamic, or use the route map below if titles are controlled
    mainCategory: "<see route map below>"   // route-specific static value
  }
});
```

### Canonical path rule
For the homepage, use `"/"` as the canonical `path` value.

Even if the visible URL is shown as:

```text
https://probable-adventure-1qg1yjl.pages.github.io
```

the correct root path value for tracking is still:

```text
/
```

---

## 3. Per-page static values (Route → Category map)

| Route | `mainCategory` | `pageTitle` |
|-------|----------------|-------------|
| `/` | `HomePage` | `Maelle \| Home` |
| `/how-it-works` | `AboutPage` | `Maelle \| How it Works` |
| `/capabilities` | `CapabilitiesPage` | `Maelle \| Capabilities` |
| `/resources` | `ResourcesPage` | `Maelle \| Resources` |

---

## 4. Event mapping per page

### Home Page (`/`)

| Field | Value |
|-------|-------|
| **Route / URL path** | `/` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io` |
| **Component file** | `maelle-tracking-events/defaultPageView events/homepage_defaultPageView.json` |
| **Trigger** | When page loads |

### About Page (`/how-it-works`)

| Field | Value |
|-------|-------|
| **Route / URL path** | `/how-it-works` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/how-it-works` |
| **Component file** | `maelle-tracking-events/defaultPageView events/aboutPage_defaultPageView.json` |
| **Trigger** | When page loads |

### Capabilities Page (`/capabilities`)

| Field | Value |
|-------|-------|
| **Route / URL path** | `/capabilities` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/capabilities` |
| **Component file** | `maelle-tracking-events/defaultPageView events/capabilities_defaultPageView.json` |
| **Trigger** | When page loads |

### Resources Page (`/resources`)

| Field | Value |
|-------|-------|
| **Route / URL path** | `/resources` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/resources` |
| **Component file** | `maelle-tracking-events/defaultPageView events/resources_defaultPageView.json` |
| **Trigger** | When page loads |

---

## 5. Page-specific `defaultPageView` payloads

For Maelle websites, use the page-specific JSON payloads in:

`maelle-tracking-events/defaultPageView events/`

### Source-of-truth mappings

- `/` → `homepage_defaultPageView.json`
- `/how-it-works` → `aboutPage_defaultPageView.json`
- `/capabilities` → `capabilities_defaultPageView.json`
- `/resources` → `resources_defaultPageView.json`

### Implementation notes

- Reuse the JSON structure from those files exactly.
- Only substitute runtime-derived values where explicitly intended, such as:
  - `timestamp`
  - `page.sysEnv`
  - `page.language`
  - `page.url`
  - `page.domain`
  - `page.path`
  - `page.pageTitle`
- Fire the event on initial page load and on SPA route changes where applicable.

### Conflict resolution rule

If this document and the JSON payload files differ:
1. use the JSON payload files as the source of truth for structure
2. keep explicitly runtime-derived fields dynamic at runtime
3. use `"/"` as the canonical homepage path

---

## 6. Agent implementation guidance

An implementation agent should:

1. Identify the shared tracking integration point in the app.
2. Ensure Adobe Launch and `window.oneDataLayer` are initialized before any push.
3. Implement `defaultPageView` firing on:
   - first page load
   - SPA route transitions, if applicable
4. Use the route map above to assign:
   - `mainCategory`
   - `pageTitle` where needed
5. Use runtime values for:
   - current URL
   - origin/domain
   - pathname
   - timestamp
   - environment
   - language
6. Normalize the homepage path to `"/"`.

---

## 7. Expected result

For each tracked page, the site should emit one `defaultPageView` event with the correct:

- route
- category
- title
- environment
- language
- URL metadata
- timestamp

And if the app is an SPA, it should emit again on each client-side navigation to a tracked route.