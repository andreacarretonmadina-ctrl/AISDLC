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