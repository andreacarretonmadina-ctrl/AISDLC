---
sidebar_position: 3
title: When to Use Each Event
domain: Tracking & Analytics
---

# When to Use Each Event

This table defines every event type supported by OneDataLayer and the exact moment it should fire.

---

## Page & Navigation Events

| Event | When to Fire |
|-------|-------------|
| `defaultPageView` | On every page load, SPA view, modal view, or app view. |
| `pageError` | On every page or app error. |
| `scroll` | After scrolling, when 75% of the content is displayed. On all pages except 404 error pages. |

## Consent

| Event | When to Fire |
|-------|-------------|
| `consentUpdated` | When the user's consent is updated or the cookie consent layer is closed. Reference: [CookiePro documentation](https://community.cookiepro.com/s/article/UUID-1e75fb0d-bb79-2af8-cb85-f905d16f1220?language=en_US) |

## Interaction Events

| Event | When to Fire |
|-------|-------------|
| `defaultLinkClick` | On every link element clicked on the web or on every user action in an app. ⚠️ **Important:** Do not push on download links to prevent downloads from being tracked as link clicks (and introducing duplicate tracking events). Refer to the "Download Tracking" sheet. |
| `componentImpression` | When a UI component (banner, popup, or side sheet) becomes visible to the user. Only include the component type keys relevant to a given push. |
| `tabClick` | On every click on a tab in a tabs component. |

## Search Events

| Event | When to Fire |
|-------|-------------|
| `onsiteSearch` | On search results page or search/filter update. To indicate which search component was used, set `content.onsiteSearch.searchComponent` (e.g., `"search-bar-header"` or `"search-bar-header-search-suggestion"`). |
| `onsiteSearchClick` | On click on a search result. To indicate which search component was used, set `content.onsiteSearch.searchComponent` (e.g., `"search-bar-header"` or `"search-bar-header-result-suggestion"`). |

## Form Events

| Event | When to Fire |
|-------|-------------|
| `formStart` | Once for each form when a user begins filling in a field (form pristine → form dirty). `pristine=false` & `dirty=true`. |
| `formStep` | On every form step after the first step. |
| `formSuccess` | On every successful form submission. |
| `formError` | On every form error. |

## Video Events

| Event | When to Fire |
|-------|-------------|
| `videoStart` | On every initial video play or on replay after the video finished. An initial play is the first play action after the content loads. If a video is played after being paused, that is **not** an initial play. If a video is played after a page refresh, that **is** an initial play. |
| `videoProgress` | On video watch duration 10%, 25%, 50%, or 75% of total runtime. |
| `videoComplete` | On video watch duration 100% of total runtime. |
| `videoError` | On every video player error. |

## Audio Events

| Event | When to Fire |
|-------|-------------|
| `audioStart` | On every initial audio play. |
| `audioProgress` | On audio listen duration 10%, 25%, 50%, or 75% of total runtime. |
| `audioComplete` | On audio listen duration 100% of total runtime. |
| `audioError` | On every audio player error. |

## Course/Learning Events

| Event | When to Fire |
|-------|-------------|
| `courseEnroll` | On click on "enroll". |
| `courseStart` | On course/module start. |
| `courseComplete` | On course/module completion. |

## User Authentication Events

| Event | When to Fire |
|-------|-------------|
| `userRegistration` | On successful user account registration. |
| `userRegistrationError` | On registration error. |
| `userLogin` | On successful user login (any type). |
| `userAutoLogin` | On successful auto-login ([reference](https://rochewiki.roche.com/confluence/x/krf2GQ)). |
| `userLoginError` | On login error. |

## E-Commerce Events

| Event | When to Fire |
|-------|-------------|
| `prodView` | On product view (e.g., on a product detail page). |
| `scAdd` | When a product is added to the shopping cart. |
| `scView` | On shopping cart view. |
| `scRemove` | When a product is removed from the shopping cart. |
| `scCheckout` | When a user initiates the checkout process. |
| `purchase` | On successful order/purchase. |