---
title: Adobe Launch Script Installation
domain: Tracking & Analytics
last_updated: 2026-05-08
owner: Maelle / Analytics Team
related:
  - eddl-overview.md
  - ../maelle-tracking-events/README.md
  - ../GLOSSARY.md
---

# Adobe Launch Script Installation

> **Audience:** Front-end developers implementing Maelle tracking on Roche web properties.
> **Prerequisites:** Access to the website's HTML `<head>`. See [EDDL Overview](eddl-overview.md) for context.

To enable the **oneDataLayer (ODL)** on your website, you must include the **Adobe Launch** script on every page.

---

## Installation Steps

1. Copy the appropriate script below (Production **or** Non-Production).
2. Paste it as **high in the `<head>`** of every page as possible.
3. Ensure the script loads **before** any `oneDataLayer.push()` calls.

---

## Production Script

```html
<!-- Adobe Launch -->
<script src='https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-bb453ba32d52.min.js' async></script>
<!-- End of Adobe Launch -->
```

## Non-Production Script (Staging / QA)

```html
<!-- Adobe Launch Non Prod -->
<script src='https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-3cdcd1196ea2-staging.min.js' async></script>
<!-- End of Adobe Launch Non Prod -->
```

---

## ⚠️ Important Notes

| Rule | Details |
|------|---------|
| **One script per environment** | Never include both Production and Non-Production scripts on the same page |
| **Placement** | Must be as high in `<head>` as possible to avoid race conditions |
| **`async` attribute** | The script loads asynchronously — see [EDDL Overview](eddl-overview.md#asynchronous-loading) for how this works with `oneDataLayer` |
| **Every page** | The script must be present on **all** pages, including SPA route changes if using a traditional `<head>` injection |

---

## Full Example

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <!-- Adobe Launch (Production) -->
    <script src='https://assets.adobedtm.com/9215900255c9/17f1b4dd3439/launch-bb453ba32d52.min.js' async></script>
    <!-- End of Adobe Launch -->

    <!-- Initialize oneDataLayer -->
    <script>
        window.oneDataLayer = window.oneDataLayer || [];
    </script>

    <!-- Other head elements -->
</head>
<body>
    <!-- Page content -->
</body>
</html>
```

---

## Related Documentation

- [EDDL Overview – What is oneDataLayer?](eddl-overview.md)
- [Tracking Event Catalog](../maelle-tracking-events/README.md)
- [Glossary of Terms](../GLOSSARY.md)

## Content Security Policy (CSP)

To ensure that Adobe Analytics works on your website, please include these directives in your Content Security Policy (CSP):

```
connect-src 'self' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
img-src 'self' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
script-src 'self' 'unsafe-inline' https://assets.adobedtm.com https://*.2o7.net https://*.omtrdc.net https://*.demdex.net https://cm.everesttech.net https://*.adobe.com;
frame-src 'self' https://*.demdex.net;
```

> ⚠️ **Without these CSP directives**, Adobe Launch and its associated services will be blocked by the browser, and tracking data will not be collected.