# oneDataLayer Troubleshooting Guide

Common issues and how to fix them.

---

## 1. Duplicate `defaultPageView` events in SPAs

**Symptom:** Adobe Analytics reports double page views on route changes or initial load.

**Causes:**
- Multiple route change listeners attached (e.g., both a framework router hook and a manual `history` listener)
- React/Vue/Angular hydration triggers a second render that re-fires the push
- `window.oneDataLayer` push called in both a layout component and a page component

**Fix:**
- Use a single, centralised location for `defaultPageView` (e.g., a router `afterEach` hook)
- Guard with a flag or debounce to prevent double-firing during hydration:
  ```javascript
  let lastPath = null;
  router.afterEach((to) => {
    if (to.path === lastPath) return;
    lastPath = to.path;
    window.oneDataLayer.push({ event: 'defaultPageView', ... });
  });
  ```
- Confirm with the browser Network tab that only one Analytics beacon fires per navigation

---

## 2. CSP blocking Adobe Analytics requests

**Symptom:** Browser console shows `Refused to connect` or `Refused to load script` errors referencing `adobedtm.com`, `omtrdc.net`, or `demdex.net`.

**Fix:** Add the required CSP directives to your server headers or `<meta http-equiv="Content-Security-Policy">` tag:

```
connect-src 'self' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
img-src 'self' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
script-src 'self' 'unsafe-inline' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
frame-src 'self' https://*.demdex.net;
```

See `SKILL.md` for the full list of required domains.

---

## 3. Events firing before `window.oneDataLayer` is initialized

**Symptom:** `TypeError: Cannot read properties of undefined (reading 'push')` or events silently dropped.

**Cause:** The `oneDataLayer` push call runs before the initialization script.

**Fix:** Always initialize defensively immediately before any push — and ensure the initialization script appears in `<head>` before any other scripts that push events:

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({ event: 'defaultPageView', ... });
```

Never assume `window.oneDataLayer` is already initialized.

---

## 4. Wrong `sysEnv` value in production

**Symptom:** Production data appears under `Staging` or `Dev` in Adobe Analytics reports.

**Cause:** The `page.sysEnv` value is hardcoded to `"Staging"` or `"Dev"`, or the environment detection logic is incorrect.

**Fix:**
- Use a build-time environment variable to set `sysEnv`:
  ```javascript
  page: {
    sysEnv: process.env.NEXT_PUBLIC_ENV === 'production' ? 'Prod' : 'Staging',
    ...
  }
  ```
- Valid values are exactly: `"Prod"`, `"Staging"`, `"Dev"` (case-sensitive)
- Verify the deployed value in the browser console before go-live

---

## 5. Event names with wrong casing

**Symptom:** Events appear in Adobe Analytics with unexpected names, or Adobe Launch rules do not trigger.

**Cause:** Event names are case-sensitive. Common mistakes:
- `DefaultPageView` instead of `defaultPageView`
- `formload` instead of `formStart`
- `VideoStart` instead of `videoStart`

**Fix:** Always copy event names exactly from the templates in `docs/oneDataLayer/templates/` or from the Core Event Families list in `SKILL.md`. Do not guess or infer casing.

---

## 6. Missing required fields causing silent failures

**Symptom:** Events fire (visible in the browser console / Network tab) but data is incomplete or missing in Adobe Analytics.

**Cause:** Required fields such as `page.siteName`, `page.sysEnv`, `page.language`, or `timestamp` are missing or `undefined`.

**Fix:**
- Cross-reference each event push against the corresponding template in `docs/oneDataLayer/templates/`
- Check `validation-and-qa-rules.md` for the full required-fields checklist
- Use the browser console to inspect the pushed object before it is processed:
  ```javascript
  console.log(JSON.stringify(window.oneDataLayer[window.oneDataLayer.length - 1], null, 2));
  ```
- Ensure no field value is `undefined` — use `null` or an empty string `""` as a fallback where a value is not yet available
