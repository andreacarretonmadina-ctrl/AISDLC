---
title: Validation & QA Rules
domain: Tracking & Analytics
last_updated: 2026-06-01
owner: Analytics Team
---

# Validation & QA Rules

This document describes how to verify a correct OneDataLayer + Adobe Analytics implementation.

---

## Tools

| Tool | Purpose |
|------|---------|
| **Browser Console** | Inspect `window.oneDataLayer` array directly |
| **Adobe Experience Platform Debugger** (Chrome extension) | Inspect Adobe Analytics server calls in real time |
| **Charles Proxy / Browser Network tab** | Filter requests to `*.2o7.net` or `*.omtrdc.net` |
| **Adobe Analytics Non-Production Script** | Use staging Launch library for pre-release testing |
| **ObservePoint / DataSlayer** | Automated tag auditing |

---

## Step-by-Step QA Process

### 1. Verify the Data Layer Array

Open the browser console and run:

```javascript
console.table(window.oneDataLayer);
```

**Check:**
- [ ] Array is initialized before any push
- [ ] Each push contains an `event` key
- [ ] Values are in English (except market-specific content)
- [ ] No `undefined` or `null` for required fields
- [ ] Timestamps are present and valid

### 2. Verify Adobe Launch is Loaded

In the Network tab, confirm a request to:
```
https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-*.min.js
```

**Check:**
- [ ] Only ONE script (prod OR non-prod) is present
- [ ] Script is in the `<head>` before any `oneDataLayer.push()`

### 3. Verify Adobe Analytics Server Calls

To debug a site with a clean browser cache and no leftover cookies, it is strongly recommended to close all open incognito mode windows (if they are in use) and open a new incognito window.

1. Visit the page where you want to test the EDDL implementation.
2. Open your browser's dev tools, either by pressing **F12** or from the browser menu.
3. In the Dev Tools, switch to the **Console** tab.
4. Type `oneDataLayer` in the console and press **Enter**. If EDDL is implemented with the correct name, the returned object will display the events and context data.

---

## Common Mistakes

| Issue | Symptom | Fix |
|-------|---------|-----|
| Missing `window.oneDataLayer = window.oneDataLayer \|\| []` | Events lost, console errors | Add initialization before any push |
| Duplicate events | Inflated metrics | Ensure event fires only once per trigger |
| `defaultPageView` missing on SPA navigation | Missing page views in reports | Push on every route change |
| Download tracked as `defaultLinkClick` | Duplicate tracking | Exclude download links from link click logic |
| Empty `pageName` | Traffic goes to "Unspecified" | Always set `page.pageName` |
| Wrong consent state | Hits blocked or sent without consent | Verify `consentUpdated` fires before other events |
| Events fire before `defaultPageView` | Broken pathing in Adobe | Ensure page view is always first |
| Non-English values | Inconsistent reporting | Use English except for market-specific content |
| Multiple Adobe Launch scripts | Double-counting | Only one script per environment |

---

## QA Sign-Off Template

| Item | Status | Notes |
|------|--------|-------|
| Adobe Launch script present | ⬜ | |
| `oneDataLayer` initialized | ⬜ | |
| `defaultPageView` fires on all pages | ⬜ | |
| All required dimensions populated | ⬜ | |
| Correct events fire per interaction | ⬜ | |
| No duplicate events | ⬜ | |
| E-commerce products string valid | ⬜ | |
| Consent handled correctly | ⬜ | |
| Non-prod script removed before go-live | ⬜ | |
| CSP directives configured | ⬜ | |