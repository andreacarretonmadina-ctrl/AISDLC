---
title: Adobe Analytics Metrics Mapping
domain: Tracking & Analytics
last_updated: 2026-06-01
owner: Analytics Team
---

# Adobe Analytics Metrics Mapping

This document maps OneDataLayer events to Adobe Analytics event variables (metrics).

> **Note:** Adobe Analytics also provides native metrics. Pages without ODL will only track native metrics.

---

## Scroll & Page Engagement

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Scroll Depth 75% | 75% scroll depth | User scrolled 75% of page | `event18` | Counter |
| Scroll Depth 90% | 90% scroll depth | User scrolled 90% of page | `event19` | Counter |
| Scroll Depth 100% | 100% scroll depth | User scrolled 100% of page | `event20` | Counter |
| 30s on Page | User spent 30s on page | Tag Management System trigger | `event70` | Counter |
| 60s on Page | User spent 60s on page | Tag Management System trigger | `event71` | Counter |
| 90s on Page | User spent 90s on page | Tag Management System trigger | `event72` | Counter |
| 120s on Page | User spent 120s on page | Tag Management System trigger | `event73` | Counter |

---

## Page Errors

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Page Errors | Amount of page errors | On every `pageError` event | eVar98 Instances | Counter |

---

## Content

| Metric | Description | When to Fire | Adobe Event | Type | Unit |
|--------|-------------|--------------|-------------|------|------|
| Content Duration | Anticipated read time in seconds | On `defaultPageView` | `event12` | Numeric | Seconds |
| Component Impressions | Number of component impressions | On `componentImpression` event | `event74` | Counter | — |

---

## Search

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Searches | All onsite searches | On `onsiteSearch` | `event13` | Counter |
| Nullsearches | Searches with 0 results | On `onsiteSearch` with results = 0 | `event14` | Counter |
| Search Results Amount | Number of results (numeric) | On `onsiteSearch` / `onsiteSearchClick` | `event15` | Numeric |
| Search Results Clicks | Clicks on search results | On `onsiteSearchClick` | `event89` | Counter |

---

## Forms

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Form Starts | Forms started | On `formStart` | `event10` | Counter |
| Form Submission successful | Successful submissions | On `formSuccess` | `event33` | Counter |
| Forms Error Count | Form errors | On `formError` | `event34` | Counter |
| Newsletter Subscriptions | Newsletter subscribed | On any event with `eventFlags.nlSub` | `event21` | Counter |

---

## E-Commerce

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Product View | Product viewed | On `prodView` | `prodView` | Counter |
| Cart Addition | Added to cart | On `scAdd` | `scAdd` | Counter |
| Cart Views | Cart viewed | On `scView` | `scView` | Counter |
| Cart Removals | Removed from cart | On `scRemove` | `scRemove` | Counter |
| Checkouts | Checkout initiated | On `scCheckout` | `scCheckout` | Counter |
| Orders | Purchase completed | On `purchase` | `purchase` | Counter |
| Revenue | Monetary amount | On `purchase` | `purchase` | Currency |

---

## Video

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Video Start | Videos started | On `videoStart` | `event28` | Counter |
| Video Complete | Videos completed | On `videoComplete` | `event29` | Counter |
| Video Milestone reached | Progress milestone | On `videoProgress` / `videoComplete` | `event30` | Counter |
| Video Total Duration | Duration in seconds | On any video event | `event27` | Numeric |
| Video Errors | Video player errors | On `videoError` | `event31` | Counter |

---

## Audio

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Audio Start | Audio started | On `audioStart` | `event58` | Counter |
| Audio Complete | Audio completed | On `audioComplete` | `event59` | Counter |
| Audio Milestone reached | Progress milestone | On `audioProgress` / `audioComplete` | `event60` | Counter |
| Audio Total Duration | Duration in seconds | On any audio event | `event57` | Numeric |
| Audio Errors | Audio player errors | On `audioError` | `event61` | Counter |

---

## Course / Learning

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Course Enroll | Courses enrolled | On `courseEnroll` | `event40` | Counter |
| Course Start | Courses started | On `courseStart` | `event41` | Counter |
| Course Complete | Courses completed | On `courseComplete` | `event42` | Counter |

---

## User Authentication

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Registrations | Successful registrations | On `userRegistration` | `event5` | Counter |
| Registrations Error Count | Registration errors | On `userRegistrationError` | `event8` | Counter |
| Logins | Successful logins | On `userLogin` | `event35` | Counter |
| Auto-Logins | Successful auto-logins | On `userAutoLogin` | `event36` | Counter |
| Logins Error Count | Login errors | On `userLoginError` | `event37` | Counter |

---

## Bookmarks

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Content bookmarked | Bookmarked page viewed | On `defaultPageView` if `bookmark.bookmarked: true` | `event84` | Counter |
| Add Bookmark | Bookmark added | On `defaultLinkClick` with `link.type: "Add Bookmark"` | `event85` | Counter |
| Remove Bookmark | Bookmark removed | On `defaultLinkClick` with `link.type: "Remove Bookmark"` | `event86` | Counter |
| Bookmarked Content Clicks | Click on bookmarked content | On `defaultLinkClick` with `link.type: "Click Bookmark"` | `event87` | Counter |

---

## Social Media

| Metric | Description | When to Fire | Adobe Event | Type |
|--------|-------------|--------------|-------------|------|
| Social Media Links | Social links clicked | On `defaultLinkClick` with `link.socialInteraction: "Link"` | `event46` | Counter |
| Social Media Shares | Share buttons clicked | On `defaultLinkClick` with `link.socialInteraction: "Share"` | `event47` | Counter |