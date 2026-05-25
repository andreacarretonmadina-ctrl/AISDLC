---
title: Adobe Analytics Setup for New Websites
sidebar_position: 2
domain: Tracking & Analytics
last_updated: 2026-05-22
owner: Analytics Team
---

# Adobe Analytics Setup for New Websites

> **Audience:** Product owners, developers, or technical leads onboarding a new website into Roche's analytics ecosystem.

---

## Overview

Before tracking data can flow from your website to Adobe Analytics, two things must happen:

1. Your website must be **added to the Roche Adobe Report Suite**.
2. The **Adobe Launch script** must be installed and **validated**.

---

## Step 1 — Request Report Suite Inclusion

Submit a request to add your website to the Roche Adobe Report Suite via ServiceNow:

🔗 **[Request Report Suite Inclusion](https://roche.service-now.com/rose?id=sc_cat_item_guide&sys_id=edc527858790b918ceae87340cbb35c0)**

### Information to provide in the request:

| Field | Description |
|-------|-------------|
| Website URL(s) | Production and non-production URLs |
| Website name | The name of your application or property |
| Team / Owner | Who owns the website |
| Expected go-live date | When tracking should be active |
| Environment details | How many environments (Dev, Staging, Prod) |

> ⏳ Allow time for the request to be processed before proceeding to Step 2.

---

## Step 2 — Install the Adobe Launch Script

Once your request is approved, install the script on your website. See [Adobe Launch Script Installation](Adobe%20Script%20Implementation.md) for full instructions.

**Quick summary:**

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

---

## Step 3 — Implement Tracking Events

Push events to the data layer following the [OneDataLayer event reference](when%20to%20use%20each%20event.md):

```javascript
window.oneDataLayer.push({
  "event": "defaultPageView",
  "page": {
    "siteName": "Your Site Name",
    "sysEnv": "Prod",
    "language": "en",
    "url": window.location.href,
    "domain": window.location.origin,
    "path": window.location.pathname,
    "pageTitle": document.title,
    "mainCategory": "HomePage"
  }
});
```

---

## Step 4 — Request Validation

After the script is implemented and events are being pushed, **request validation from the Analytics Team** to confirm:

- ✅ Data is arriving in the Report Suite
- ✅ Event payloads are correctly structured
- ✅ No duplicate or missing events
- ✅ CSP directives are not blocking requests

### How to request validation:

1. Deploy your implementation to a **staging/QA environment**
2. Contact the Analytics Team (owner listed in your ServiceNow ticket) or reach out via your project's Slack/Teams channel
3. Provide:
   - URL of the staging environment
   - List of events implemented
   - Screenshots or console logs showing `oneDataLayer` pushes (optional but helpful)

> ⚠️ **Do not go live** with tracking until the Analytics Team confirms the implementation is correct.

---

## Summary Checklist

| # | Step | Status |
|---|------|--------|
| 1 | Submit ServiceNow request for Report Suite inclusion | ☐ |
| 2 | Receive approval | ☐ |
| 3 | Install Adobe Launch script (correct environment) | ☐ |
| 4 | Add CSP directives | ☐ |
| 5 | Initialize `window.oneDataLayer` | ☐ |
| 6 | Implement event pushes | ☐ |
| 7 | Request validation from Analytics Team | ☐ |
| 8 | Receive confirmation & go live | ☐ |