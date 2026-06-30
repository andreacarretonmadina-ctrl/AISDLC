---
title: OneTrust Setup for Adobe Analytics
sidebar_position: 1
domain: Tracking & Analytics
last_updated: 2026-06-30
owner: Analytics Team
---

# OneTrust Setup for Adobe Analytics

> **Audience:** Product owners, developers, or technical leads onboarding a new website into Roche's analytics ecosystem.
> **Prerequisites:** A website where you plan to install Adobe Analytics. See [Adobe Analytics Setup for New Websites](adobe-analytics-set-up-for-new-websites.md).

---

## Why OneTrust is Mandatory

Due to Roche's strict privacy compliance policies and data protection regulations, **Adobe Analytics tracking scripts must be strictly conditional**.

Until the **OneTrust Cookie Banner** is properly integrated and the user gives **explicit consent**, the Adobe Analytics tags are **blocked from firing**. This is the most common reason `defaultPageView` events do not appear in the Adobe Analytics platform during initial implementation.

> 🚨 **No OneTrust = no data.** Adobe Analytics is configured so that without OneTrust consent, no tracking calls are sent. Implementing OneTrust is a hard prerequisite, not an optional step.

---

## Prerequisites Checklist

Before starting the implementation steps below, confirm:

- [ ] Your project has a **OneTrust tenant** assigned
- [ ] You or your team members have **access to the OneTrust platform** for this project
- [ ] Your **staging domain** (e.g. `staging.example.navify.com`) has been **added and scanned** in the OneTrust tenant
- [ ] You know that Adobe Analytics cookies must be mapped to the **Performance / Analytical cookies** category (`C0002`) in your OneTrust tenant

> 🔑 **Need OneTrust access or domain setup?**
> If you or your developers do not have access yet, or if your domain has not been configured in the tenant, open a ServiceNow ticket at 🔗 **[https://roche.service-now.com/](https://roche.service-now.com/)**.
>
> In the ticket, include:
> - The **email addresses** of the people who should be granted access
> - The **staging and production domains** that need to be added to the tenant
> - The **project / website name**
>
> The OneTrust Team will assign the necessary roles and permissions so your team can manage scripts and consent banners.

---

## Implementation Workflow

The integration is standard, but it requires a few specific steps regarding event listening and wrapper configurations.

### Step 1 — Scan your domain in OneTrust

OneTrust will scan your staging URL to identify the Adobe Analytics cookies already present (or expected) on the site.

1. Log in to the OneTrust platform.
2. Add your staging domain (e.g. `staging.example.navify.com`) to the tenant if it isn't already there.
3. Run a domain scan.
4. Wait for the scan to complete and review the detected cookies.

### Step 2 — Categorize Adobe cookies as `C0002` (Performance)

Ensure the Adobe Analytics cookies detected by the scan are categorized under **Performance / Analytical Cookies**, which in Roche's OneTrust setup is category **`C0002`**.

Common Adobe cookies to verify in this category:

- `s_cc`, `s_sq`, `s_vi`, `s_fid` (legacy Adobe Analytics cookies)
- `AMCV_*`, `AMCVS_*` (Adobe Experience Cloud ID)
- `demdex` (Adobe Audience Manager)

> ⚠️ If Adobe cookies end up in the wrong category (e.g. Strictly Necessary or Targeting), tags will either fire without consent or never fire at all. Always confirm they are mapped to `C0002`.

### Step 3 — Wrap the Adobe script in a consent check

Modify your existing Adobe Launch implementation so it **only fires once the user has consented to `C0002`**.

You have **two options**:

#### Option A — Use the Adobe Launch ↔ OneTrust extension *(recommended)*

If you are using a Tag Management System (Adobe Launch / Adobe Experience Platform Tags), install the **OneTrust extension** inside your Launch property. It handles the consent wiring for you and is the lowest-maintenance option.

#### Option B — Manually wrap Adobe with `OnetrustActiveGroups`

If you cannot use the extension, gate the Adobe Launch script on the `OnetrustActiveGroups` global variable that OneTrust exposes after consent.

```html
<head>
    <!-- OneTrust loads first -->
    <script src="https://cdn.cookielaw.org/scripttemplates/otSDKStub.js"
            type="text/javascript"
            data-domain-script="YOUR-ONETRUST-DOMAIN-ID"></script>

    <!-- Initialize oneDataLayer -->
    <script>
        window.oneDataLayer = window.oneDataLayer || [];
    </script>

    <!-- Adobe Launch — loaded conditionally after consent to C0002 -->
    <script>
      function loadAdobeIfConsented() {
        if (window.OnetrustActiveGroups && window.OnetrustActiveGroups.indexOf("C0002") !== -1) {
          var s = document.createElement("script");
          s.src = "https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-3cdcd1196ea2-staging.min.js";
          s.async = true;
          document.head.appendChild(s);
        }
      }

      // Run once OneTrust has resolved consent state
      window.addEventListener("OneTrustGroupsUpdated", loadAdobeIfConsented);
      // Also run on initial page load in case consent was already given
      document.addEventListener("DOMContentLoaded", loadAdobeIfConsented);
    </script>
</head>
```

> ℹ️ Replace `YOUR-ONETRUST-DOMAIN-ID` with the domain ID issued by your OneTrust tenant for the environment (staging vs. production typically have different IDs).
> Replace the Adobe Launch URL with the production script when going live — see [Adobe Launch Script Installation](adobe-script-implementation.md).

### Step 4 — Publish the OneTrust script

1. Publish the **test/staging** OneTrust script to your staging environment.
2. Validate (see below).
3. Publish the **production** OneTrust script to production at go-live.

---

## Validation

After deploying to staging:

1. Open the site in an **incognito window**.
2. Open DevTools → Console.
3. Decline all cookies in the banner.
   - ✅ Confirm `window.OnetrustActiveGroups` does **not** include `C0002`.
   - ✅ Confirm **no** request to `assets.adobedtm.com` is made (Network tab).
4. Reload, accept all cookies.
   - ✅ Confirm `window.OnetrustActiveGroups` includes `C0002`.
   - ✅ Confirm the Adobe Launch script loads.
   - ✅ Confirm `oneDataLayer` pushes generate Adobe Analytics server calls (`*.2o7.net` / `*.omtrdc.net`).

If page views still do not appear in Adobe Analytics after consent is granted, see [Troubleshooting](../troubleshooting.md).

---

## Reference Documentation

The official OneTrust guide for integrating Adobe Analytics is here:

🔗 [OneTrust — Adobe Analytics integration guide](https://my.onetrust.com/s/article/UUID-bdeeeab3-4aa9-2b39-aab6-cf837bb9c1f9?language=en_US&topicId=0TO1Q000000ItVuWAK)

> Accessing this guide requires a free OneTrust account. If you or your developers have trouble registering or accessing the page, contact the Analytics Team — a PDF copy of the guide can be shared directly.

---

## Summary Checklist

| # | Step | Status |
|---|------|--------|
| 1 | Request OneTrust tenant access via [ServiceNow](https://roche.service-now.com/) | ☐ |
| 2 | Add and scan staging domain in OneTrust | ☐ |
| 3 | Categorize Adobe cookies under Performance (`C0002`) | ☐ |
| 4 | Wrap Adobe Launch in consent check (extension **or** `OnetrustActiveGroups`) | ☐ |
| 5 | Publish OneTrust test script to staging | ☐ |
| 6 | Validate consent gating in staging | ☐ |
| 7 | Publish OneTrust production script at go-live | ☐ |

> ⚠️ **Do not proceed** with the Adobe Analytics setup steps (Report Suite request, event implementation) expecting data to flow until OneTrust is fully integrated and consent-gating is verified in staging.

---

## Related Documentation

- [Adobe Analytics Setup for New Websites](adobe-analytics-set-up-for-new-websites.md)
- [Adobe Launch Script Installation](adobe-script-implementation.md)
- [Validation and QA Rules](../validation-and-qa-rules.md)
- [Troubleshooting](../troubleshooting.md)