---
sidebar_position: 3
title: Implementation steps
domain: Tracking & Analytics
---

# Event-Driven Data Layer (EDDL) — How It Works

Developing an Event-Driven Data Layer (EDDL) object involves initializing a plain JavaScript array on your `window` object and using the `.push()` method to transmit event-specific data as it becomes available. It decouples your site's code from your tracking tools, allowing Tag Management Systems (TMS) like Adobe Launch or Google Tag Manager to respond dynamically to user actions.

---

## Step-by-Step Implementation

### 1. Initialize the Data Layer

Define the EDDL array in the `<head>` of your website, **before** your TMS script loads. Naming conventions vary, but `oneDataLayer` is used for ODL events in Roche.

```javascript
window.oneDataLayer = window.oneDataLayer || [];
```

### 2. Push Page and User Attributes

As soon as the necessary information (e.g., page metadata, user login status) is ready on the page, push the data with a descriptive event key.

```javascript
window.oneDataLayer.push({
  "event": "defaultPageView",
  "page": {
    "pageName": "Product Page | Shoes",
    "MainCategory": "E-commerce",
    "language": "en"
  },
  "user": {
    "isLoggedIn": "Logged In",
    "tier": "Premium"
  }
});
```

### 3. Push Event Data

When a user interacts with your website (e.g., clicks "add to cart" or submits a form), push only the relevant event data and properties.

```javascript
window.digitalData.push({
  "event": "defaultLinkClick",
  "link": {
    "url": "example.com",
    "name": "add to cart"
  }
});
```