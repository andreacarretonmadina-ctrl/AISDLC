# How to implement `consentUpdated` on Maelle

## 1. Event schema

```javascript
window.oneDataLayer.push({
  event: "consentUpdated",
  timestamp: Math.floor(Date.now() / 1000),
  user: {
    consent: {
      C0001: true,
      C0002: false,
      C0003: false,
      C0004: false,
      C0005: false
    }
  }
});
```

> **Dependency:** This event relies on [OneTrust / CookiePro](https://community.cookiepro.com/s/article/UUID-1e75fb0d-bb79-2af8-cb85-f905d16f1220?language=en_US) being integrated on the site. Without OneTrust, Adobe Analytics tracking will **not** work.

---

## 2. When to fire

| Trigger | Description |
|---------|-------------|
| Cookie banner closed | User clicks "Accept All", "Reject All", or closes the banner |
| Preferences saved | User opens the cookie preferences center and saves their choices |
| Returning visitor | OneTrust reads previously stored consent from cookies on page load |

The event must fire **before** any other tracking event on the page. The analytics platform uses `consentUpdated` to decide which hits are allowed to be processed.

---

## 3. Field definitions — `user.consent` object

All consent categories default to `false` except `C0001` (Strictly Necessary), which is always `true`.

| Field | Category | Default | Set `true` when… |
|-------|----------|---------|-------------------|
| `C0001` | Strictly Necessary | `true` (always) | Always — cannot be opted out |
| `C0002` | Performance | `false` | User consents to **Analytics** cookies |
| `C0003` | Functional | `false` | User consents to **Personalization** cookies |
| `C0004` | Targeting | `false` | User consents to **Advertising / Retargeting** cookies |
| `C0005` | Social Media | `false` | User consents to **Social Media Pixels / Cookies** |

### Value resolution

| Value | Meaning |
|-------|---------|
| `true` | User has given explicit permission for this category |
| `false` | User has not given permission (default state) |

> **Note:** `C0001` can only ever be `true`. It covers cookies essential for the site to function (session management, security tokens, etc.) and is exempt from user opt-in requirements under GDPR/ePrivacy.

---

## 4. OneTrust integration logic

OneTrust exposes consent state via the `OnetrustActiveGroups` global variable and fires a callback when the user updates preferences.

### Reading consent state

```javascript
// OneTrust stores active groups as a comma-separated string, e.g. ",C0001,C0002,"
function getConsentState() {
  const activeGroups = window.OnetrustActiveGroups || ",C0001,";
  return {
    C0001: true, // always true
    C0002: activeGroups.includes("C0002"),
    C0003: activeGroups.includes("C0003"),
    C0004: activeGroups.includes("C0004"),
    C0005: activeGroups.includes("C0005")
  };
}
```

### Listening for consent changes

```javascript
// OneTrust callback — fires when banner is closed or preferences are saved
function OptanonWrapper() {
  window.oneDataLayer = window.oneDataLayer || [];
  window.oneDataLayer.push({
    event: "consentUpdated",
    timestamp: Math.floor(Date.now() / 1000),
    user: {
      consent: getConsentState()
    }
  });
}
```

> **`OptanonWrapper`** is a global function name expected by OneTrust. It is called automatically whenever the consent state changes. Do not rename it.

---

## 5. Source-of-truth JSON payload

The canonical payload lives at:

```
maelle-tracking-events/consentUpdate event/consentUpdated.json
```

### Example (user accepted Analytics only)

```json
{
  "event": "consentUpdated",
  "timestamp": 1234567890,
  "user": {
    "consent": {
      "C0001": true,
      "C0002": true,
      "C0003": false,
      "C0004": false,
      "C0005": false
    }
  }
}
```

> **Conflict resolution:** if this document and the JSON payload file differ, the JSON file wins for structure and values.

---

## 6. Rules and constraints

1. `consentUpdated` **must** fire before `defaultPageView` and any other tracking event on the page.
2. Fire exactly **once** per consent state change — do not fire on every page load if consent has not changed.
3. On returning visits, fire the event with the stored consent state from OneTrust cookies before other events.
4. Do **not** include a `page` or `link` object in the push.
5. `C0001` must always be `true` — never push `false` for Strictly Necessary cookies.
6. If OneTrust is not loaded or unavailable, do **not** push the event — tracking should not proceed without consent resolution.

---

## 7. Event sequencing

```
┌─────────────────────────┐
│  Page load begins       │
└──────────┬──────────────┘
           ▼
┌─────────────────────────┐
│  OneTrust initializes   │
│  (reads stored consent) │
└──────────┬──────────────┘
           ▼
┌─────────────────────────┐
│  consentUpdated fires   │  ← FIRST event in oneDataLayer
└──────────┬──────────────┘
           ▼
┌─────────────────────────┐
│  defaultPageView fires  │
└──────────┬──────────────┘
           ▼
┌─────────────────────────┐
│  Other events           │
│  (defaultLinkClick,     │
│   onSiteSearch, etc.)   │
└─────────────────────────┘
```

---

## 8. Agent implementation guidance

1. Ensure OneTrust script is loaded in `<head>` **before** the Adobe Launch script.
2. Define the `OptanonWrapper` function globally — OneTrust will invoke it automatically.
3. Inside `OptanonWrapper`, read `OnetrustActiveGroups` and push `consentUpdated`.
4. Ensure `consentUpdated` is the **first** event pushed to `window.oneDataLayer`.
5. Do **not** push any other tracking events until `consentUpdated` has been pushed.
6. On SPA route changes, do **not** re-fire `consentUpdated` unless the user actively changes preferences mid-session.