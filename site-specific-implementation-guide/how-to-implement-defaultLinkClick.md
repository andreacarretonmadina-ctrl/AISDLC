# How to implement `defaultLinkClick` on Maelle

## 1. Event schema

```javascript
window.oneDataLayer.push({
  event: "defaultLinkClick",
  timestamp: Math.floor(Date.now() / 1000),
  link: {
    url: "<destination URL>",
    domain: "<destination domain>",
    path: "<destination path>",
    innerText: "<visible text of the clicked element>",
    position: "<section of the page>",
    type: "<element type>"
  }
});
```

> **No `page` object.** Page context is captured by the preceding `defaultPageView`. The analytics platform correlates link clicks to the active page view automatically.

---

## 2. Field definitions — `link` object

| Field | Type | Dynamic? | Resolution Logic | Example |
|-------|------|----------|------------------|---------|
| `link.url` | `string` | ✅ Dynamic | `element.href` — full destination URL | `"https://…/how-it-works"` |
| `link.domain` | `string` | ✅ Dynamic | `new URL(element.href).origin` | `"https://maelle.dev.nax.navify.com"` |
| `link.path` | `string` | ✅ Dynamic | `new URL(element.href).pathname` | `"/how-it-works"` |
| `link.innerText` | `string` | ✅ Dynamic | `element.innerText.trim()` — visible label of the clicked element | `"Learn more"` |
| `link.position` | `string` | ❌ Static per element | Set via `data-tracking-position` attribute. Identifies where on the page the element lives. | `"hero"`, `"navBar/header"` |
| `link.type` | `string` | ❌ Static per element | Set via `data-tracking-type` attribute. Element type. | `"link"`, `"button"`, `"icon"` |

### Type classification

| Type | Use when… |
|------|-----------|
| `link` | A standard hyperlink (`<a>`) that navigates to another page or section |
| `button` | A call-to-action button (`<button>` or styled `<a>`) that triggers a primary action |
| `icon` | A clickable icon or icon-based element with minimal text |

### Position values

| Value | Where on page |
|-------|---------------|
| `hero` | Hero/banner section |
| `navBar/header` | Top navigation bar |
| `body/onboardingSection` | Body — onboarding area |
| `onboardingSection` | Onboarding CTA section |
| `capabilitiesSection` | Capabilities listing |
| `howItWorksSection` | How-it-works section |
| `bottomForm` | Contact form at bottom |
| `ResourcesFeed` | Resources feed/list |

---

## 3. HTML data attributes

The agent must add these attributes if you want to track these elements:

| Attribute | Maps to | Required |
|-----------|---------|----------|
| `data-tracking-position` | `link.position` | Optional |
| `data-tracking-type` | `link.type` | Optional |

Dynamic fields (`link.url`, `link.domain`, `link.path`, `link.innerText`) are resolved automatically from the element at click time.

### Example markup

```html
<a
  data-tracking-position="hero"
  data-tracking-type="button"
  href="https://maelle.dev.nax.navify.com"
>
  Access Maelle
</a>
```

```html
<a
  data-tracking-position="hero"
  data-tracking-type="link"
  href="/#contact"
>
  Talk to our team
</a>
```

---

## 4. Source-of-truth JSON payloads

The expected event payloads per page are defined in:

| Route | JSON file |
|-------|-----------|
| `/` | `maelle-tracking-events/defaultLinkClick events/homepage_defaultLinkClick.json` |
| `/how-it-works` | `maelle-tracking-events/defaultLinkClick events/aboutPage_defaultLinkClick.json` |
| `/capabilities` | `maelle-tracking-events/defaultLinkClick events/capabilities_defaultLinkClick.json` |
| `/resources` | `maelle-tracking-events/defaultLinkClick events/resources_defaultLinkClick.json` |

---

## 5. Rules and constraints

1. `defaultPageView` **must** have fired before any `defaultLinkClick` on the same page.
2. Fire exactly **once per user click** — debounce or guard against duplicates.
3. Do **not** include a `page` object in the push.
4. `link.url`, `link.domain`, `link.path`, and `link.innerText` are resolved from the DOM element at click time.
5. **Conflict resolution:** if this document and the JSON payload files differ, the JSON files win for structure and values.

---

## 6. Agent implementation guidance

1. Identify all interactive elements to track on each page.
2. Add `data-tracking-position` and `data-tracking-type` attributes to each tracked element - in case is needed.
3. Attach click handling using whatever pattern fits the app's framework.
4. On click, resolve from the clicked element:
   - `link.url` → `element.href`
   - `link.domain` → `new URL(element.href).origin`
   - `link.path` → `new URL(element.href).pathname`
   - `link.innerText` → `element.innerText.trim()`
   - `link.position` → `element.getAttribute("data-tracking-position")`
   - `link.type` → `element.getAttribute("data-tracking-type")`
5. Push the event with `timestamp` generated at runtime.
6. Do **not** add `page` object.