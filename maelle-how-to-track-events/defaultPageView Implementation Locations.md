## How to implement `defaultPageView`on Maelle

## 1. Prerequisites — Adobe Launch Script in `<head>`

Every page must load the Adobe Launch script **before** any `oneDataLayer.push()` calls, and the array must be initialized right after it:

```html
<head>
    <!-- Adobe Launch (Production) -->
    <script src='https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-bb453ba32d52.min.js' async></script>

    <!-- Initialize oneDataLayer -->
    <script>
        window.oneDataLayer = window.oneDataLayer || [];
    </script>
</head>
```

> For staging/QA use `launch-3cdcd1196ea2-staging.min.js` instead. Never load both scripts on the same page.

---

## 2. The `defaultPageView` Push — Full Pattern

Fire this on **initial page load AND on every SPA route change**:

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({
  "event": "defaultPageView",
  "timestamp": Math.floor(Date.now() / 1000),   // ✅ always dynamic
  "page": {
    "siteName": "Maelle",                         // ❌ static — always "Maelle"
    "sysEnv": "Prod",                             // ⚠️ set from build config: "Prod" | "Staging" | "Dev"
    "language": "es",                             // ✅ from <html lang> or locale config (ISO 639-1)
    "url": window.location.href,                  // ✅ dynamic
    "domain": window.location.origin,             // ✅ dynamic
    "path": window.location.pathname,             // ✅ dynamic ("" for root /)
    "pageTitle": document.title,                  // ✅ dynamic — or use the map below
    "mainCategory": "<see page map below>"        // ⚠️ per-page static value
  }
});
```
---

## 3. Per-Page Static Values (Route → Category Map)

| Route | `mainCategory` | `pageTitle` |
|-------|---------------|-------------|
| `/` | `HomePage` | `Maelle \| Home` |
| `/how-it-works` | `AboutPage` | `Maelle \| How it Works` |
| `/capabilities` | `CapabilitiesPage` | `Maelle \| Capabilities` |
| `/resources` | `ResourcesPage` | `"Maelle \| Reources` |

---



## Event Mapping Per Page

### About Page (`/how-it-works`)

| Field | Value |
|-------|-------|
| **Route / URL path** | `/how-it-works` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/how-it-works` |
| **Component file** | `maelle-tracking-events/defaultPageView events/aboutPage_defaultPageView.json` |
| **Trigger** | When page loads |

### Home Page (`/`)

 Field | Value |
|-------|-------|
| **Route / URL path** | `` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io` |
| **Component file** | `maelle-tracking-events/defaultPageView events/homepage_defaultPageView.json` |
| **Trigger** | When page loads |

### Capabilities Page (`/capabilities`)

 Field | Value |
|-------|-------|
| **Route / URL path** | `/capabilities` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/capabilities` |
| **Component file** | `maelle-tracking-events/defaultPageView events/capabilities_defaultPageView.json` |
| **Trigger** | When page loads |

### Resources Page (`/resources`)

 Field | Value |
|-------|-------|
| **Route / URL path** | `/resources` |
| **Full URL example** | `https://probable-adventure-1qg1yjl.pages.github.io/resources` |
| **Component file** | `maelle-tracking-events/defaultPageView events/resources_defaultPageView.json` |
| **Trigger** | When page loads |

## Page-specific `defaultPageView` payloads

For Maelle websites, use the page-specific JSON payloads in `maelle-tracking-events/defaultPageView events/`.

Source-of-truth mappings:

- `/` → `homepage_defaultPageView.json`
- `/how-it-works` → `aboutPage_defaultPageView.json`
- `/capabilities` → `capabilities_defaultPageView.json`
- `/resources` → `resources_defaultPageView.json`

Implementation note:
- Reuse the JSON structure from those files exactly.
- Only substitute runtime-derived values where explicitly intended, such as `url`, `path`, `pageTitle`, environment, or timestamp.
- Fire the event on initial page load and on SPA route changes where applicable.