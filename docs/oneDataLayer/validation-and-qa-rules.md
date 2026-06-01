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

In Adobe Debugger or Network tab, filter for requests to `*.2o7.net` or `*.omtrdc.net`.

**For each server call, validate:**

| Field | Network Parameter | What to Check |
|-------|-------------------|---------------|
| Page Name | `pageName` | Matches `oneDataLayer[].page.pageName` |
| Channel | `ch` | Matches `oneDataLayer[].page.mainCategory` |
| Server | `server` | Matches `oneDataLayer[].page.domain` |
| Campaign | `v0` | Present only with URL campaign parameters |
| Events | `events` | Correct event codes fire (see metrics mapping) |
| Props | `c1`, `c2`, etc. | Match expected prop values |
| eVars | `v3`, `v4`, etc. | Match expected eVar values |
| Products | `products` | Correct syntax for e-commerce hits |
| Hier | `h1`, `h2` | Path/category hierarchy correct |
| List vars | `l1`, `l2`, `l3` | Correct delimiter and format |

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

## Automated Checks (CI/QA)

If integrating into automated testing:

```javascript
// Example: Validate oneDataLayer structure after page load
function validateODL() {
    const odl = window.oneDataLayer || [];
    const pageView = odl.find(e => e.event === 'defaultPageView');
    
    const errors = [];
    
    if (!pageView) errors.push('Missing defaultPageView event');
    if (!pageView?.page?.pageName) errors.push('Missing pageName');
    if (!pageView?.page?.market) errors.push('Missing market');
    if (!pageView?.page?.language) errors.push('Missing language');
    if (!pageView?.page?.siteName) errors.push('Missing siteName');
    if (!pageView?.timestamp) errors.push('Missing timestamp');
    
    // Page view should be first event
    if (odl[0]?.event !== 'defaultPageView') {
        errors.push('defaultPageView is not the first event');
    }
    
    return errors;
}
```

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