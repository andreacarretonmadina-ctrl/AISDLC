---
title: Adobe Analytics Setup for New Websites
sidebar_position: 2
domain: Tracking & Analytics
last_updated: 2026-06-02
owner: Analytics Team
---

# Adobe Analytics Setup for New Websites

> **Audience:** Product owners, developers, or technical leads onboarding a new website into Roche's analytics ecosystem.

---

## Overview

Before tracking data can flow from your website to Adobe Analytics, two things must happen:

1. Your website must be **added to the Roche Adobe Report Suite** (via ServiceNow).
2. The **Adobe Launch script** must be installed and **validated**.

> ✅ **These two tracks run in parallel.** You do **not** need to wait for the ServiceNow request to be approved before installing the script. In fact, the ServiceNow ticket team needs the script already deployed in staging/QA in order to validate it.

---

## Step 1 — Request Report Suite Inclusion *(in parallel with Steps 2–3)*

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

> 🚀 You can start Steps 2 and 3 immediately — there is no need to wait for the ServiceNow request to be approved.

---

## Step 2 — Install the Adobe Launch Script *(in parallel with Step 1)*

Install the **non-production** script on your staging/QA environment as soon as possible. See [Adobe Launch Script Installation](Adobe%20Script%20Implementation.md) for full instructions.

**Quick summary:**

```html
<head>
    <!-- Adobe Launch (Non-Production / Staging) -->
    <script src='https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-3cdcd1196ea2-staging.min.js' async></script>

    <!-- Initialize oneDataLayer -->
    <script>
        window.oneDataLayer = window.oneDataLayer || [];
    </script>
</head>
```

Also add the required **CSP directives** at this stage — without them the browser will block Adobe and tracking will not work.

---

## Step 3 — Implement Tracking Events *(in parallel)*

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

## Step 4 — Validations (two separate teams)

There are **two distinct validations**, performed by **two different teams**. Both must be passed before going live in Production.

### Step 4a — Script implementation validation *(ServiceNow ticket team)*

**Trigger:** as soon as the non-production Adobe Launch script is live in staging/QA.
**Does NOT require events to be implemented.**

The ServiceNow ticket team confirms:

- ✅ The Adobe Launch script is correctly installed
- ✅ The script is correctly linked to the Report Suite from your ticket
- ✅ The script loads on the staging/QA environment

#### How to request this validation:

1. Deploy the **non-prod Launch script** to your staging/QA environment.
2. Update your ServiceNow ticket with the **URL of the staging environment** where the script is live.
3. The ticket team will validate the script implementation and update the ticket.

---

### Step 4b — Data validation *(Analytics Team)*

**Trigger:** once `oneDataLayer` events are implemented in staging/QA.
**Requires events to be in place.**

The Analytics Team confirms in the Adobe Analytics console:

- ✅ Data is arriving in the Report Suite
- ✅ Event payloads are correctly structured
- ✅ No duplicate or missing events
- ✅ CSP directives are not blocking requests

#### How to request this validation:

1. Deploy your event implementation to **staging/QA**.
2. Contact the Analytics Team (owner listed in your ServiceNow ticket) or reach out via your project's Slack/Teams channel.
3. Provide:
   - URL of the staging environment
   - **List of events implemented**
   - Screenshots or console logs showing `oneDataLayer` pushes (optional but helpful)

> ⚠️ **Do not go live** with tracking until **both** the ServiceNow ticket team and the Analytics Team have confirmed their respective validations.

---

## Summary Checklist

| # | Step | Owner | Status |
|---|------|-------|--------|
| 1 | Submit ServiceNow request for Report Suite inclusion | You | ☐ |
| 2 | Install Adobe Launch **non-prod** script in staging/QA *(in parallel with #1)* | You | ☐ |
| 3 | Add CSP directives | You | ☐ |
| 4 | Initialize `window.oneDataLayer` | You | ☐ |
| 5 | **ServiceNow team validates script implementation** | 🎫 ServiceNow ticket team | ☐ |
| 6 | Implement event pushes | You | ☐ |
| 7 | **Analytics Team validates events in Adobe Analytics console** | 📊 Analytics Team | ☐ |
| 8 | Switch to **production** Launch script & go live | You | ☐ |

> ⚠️ Steps 5 and 7 are **both hard gates** before going live in Prod.