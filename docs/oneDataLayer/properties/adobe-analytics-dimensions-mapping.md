---
title: Adobe Analytics Dimensions Mapping
domain: Tracking & Analytics
last_updated: 2026-06-01
owner: Analytics Team
---

# Adobe Analytics Dimensions Mapping

This document maps OneDataLayer properties to Adobe Analytics variables (props, eVars, hier, lists).

> **Note:** Adobe Analytics also provides native dimensions not listed here.

---

## Campaign & Event Identification

| Dimension | Description | When & Where | Data Source | Adobe Variable | Allocation | Expiration |
|-----------|-------------|--------------|-------------|----------------|------------|------------|
| Campaign | Campaign Code | On every page tracking event | URL query string parameter | `campaign` | Most Recent (Last) | 30 days |
| Event Name | The name of the Tracking Event | On every tracking event | `oneDataLayer[].event` | `prop1` | — | — |
| Event Details | Captures information about the event that provides business or technical context about the interaction beyond the event name and type itself. | On every event, if applicable | `oneDataLayer[].eventDetails` | `eVar66` | Most Recent (Last) | Hit |

---

## Timestamps

| Dimension | Description | Data Source | Adobe Variable | Example |
|-----------|-------------|-------------|----------------|---------|
| Timestamp Unix | Unix Timestamp | `oneDataLayer[].timestamp` | `prop2` | `1674842407984` |
| Date ISO 8601 | Simplified extended ISO 8601 (UTC) | `oneDataLayer[].timestamp` | `prop4` | `2023-01-27T18:00:07.984Z` |
| UTC Offset | User's local UTC offset | `oneDataLayer[].timestamp` | `prop3` | `UTC+01:00` |

---

## Page & Site Information

| Dimension | Description | Data Source | Fallback | Adobe Variable | Allocation | Expiration |
|-----------|-------------|-------------|----------|----------------|------------|------------|
| Website | Name of the website | `oneDataLayer[].page.siteName` | `document.location.hostname` | `eVar68` | Most Recent (Last) | Visit |
| Market | Market providing the site | `oneDataLayer[].page.market` | — | `eVar70` | Most Recent (Last) | Hit |
| Language | Displayed language | `oneDataLayer[].page.language` | `document.documentElement.lang` | `eVar69` | Most Recent (Last) | Hit |
| URL | URL of the page | `oneDataLayer[].page.url` | `document.location.href` | `eVar3` | Most Recent (Last) | Hit |
| Domain | Domain of the site | `oneDataLayer[].page.domain` | `document.location.hostname` | `server` | — | — |
| URL Path | Path of the URL | `oneDataLayer[].page.path` | `document.location.pathname` | `eVar4`, `hier1` | Most Recent (Last) | Hit |
| Page Title | SEO page title | `oneDataLayer[].page.pageTitle` | `document.title` | `eVar21` | Most Recent (Last) | Hit |
| Page Name | Reporting-friendly page name (stable over time) | `oneDataLayer[].page.pageName` | `document.location.pathname` | `pageName` | — | — |
| AEM Page UUID | Adobe Experience Manager UUID | `oneDataLayer[].page.uuid` | — | `eVar16` | Most Recent (Last) | Hit |
| Page Type | Type of the page | `oneDataLayer[].page.pageType` | — | `eVar15` | Most Recent (Last) | Hit |
| Page (incl. hierarchy) | Categories/Breadcrumbs + Page Name | `oneDataLayer[].page.categoryTree` + `pageName` | — | `eVar19` | Most Recent (Last) | Hit |
| Main Category | Category/site section | `oneDataLayer[].page.mainCategory` | `document.location.pathname` | `channel` | — | — |
| Categories or Breadcrumbs | Category tree of the page | `oneDataLayer[].page.categoryTree` | — | `eVar20`, `hier2` | Most Recent (Last) | Hit |
| Page Error | Error type and identifier | `oneDataLayer[].page.errorType` / `.error` | — | `eVar98` | Most Recent (Last) | Hit |
| Protected Page | Public, registrationwall, or protected | `oneDataLayer[].page.protected` | — | `eVar75` | Most Recent (Last) | Hit |
| Scroll Depth | Scroll milestone reached | `oneDataLayer[].page.scrolled` | `75%` | `eVar22` | Most Recent (Last) | Hit |

---

## Previous Page

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| URL previous page | `oneDataLayer[].page.previousPage.url` | `eVar103` |
| URL Path previous page | `oneDataLayer[].page.previousPage.path` | `eVar104` |
| Page Title previous page | `oneDataLayer[].page.previousPage.pageTitle` | `eVar121` |
| Page Name previous page | `oneDataLayer[].page.previousPage.pageName` | `eVar122` |
| Page (incl. hierarchy) previous page | `oneDataLayer[].page.previousPage.categoryTree` + `pageName` | `eVar119` |
| Main Category previous page | `oneDataLayer[].page.previousPage.mainCategory` | `eVar100` |
| Categories or Breadcrumbs previous page | `oneDataLayer[].page.previousPage.categoryTree` | `eVar120` |

---

## Geolocation (Web SDK)

| Dimension | Description | Data Source | Adobe Variable |
|-----------|-------------|-------------|----------------|
| xdm.placeContext.geo.countryCode | ISO 3166-1 alpha-2 country code | Datastreams | `prop20` |
| events.payload.geoCountry | ISO 3166-1 numeric codes | Datastreams | `prop21` |

---

## Link Tracking

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| Link URL | `oneDataLayer[].link.url` | `eVar40` |
| Link Domain | `oneDataLayer[].link.domain` | `eVar41` |
| Link Path | `oneDataLayer[].link.path` | `eVar42` |
| Link Innertext | `oneDataLayer[].link.innerText` | `eVar43` |
| Link Position | `oneDataLayer[].link.position` | `eVar47` |
| Link Name | `oneDataLayer[].link.name` | `eVar44` |
| Link Type | `oneDataLayer[].link.type` | `eVar45` |
| Exit Link URL | `href` attribute | `eVar48` |
| Social Media | `oneDataLayer[].link.socialMedia` | `eVar51` |
| Social Interaction | `oneDataLayer[].link.socialInteraction` | `eVar50` |

---

## Content Attributes (list1 — "Page Attributes")

These dimensions use **list1** with `,` delimiter, Hit expiration, Full allocation, max 250 values.

| Dimension | Data Source | Format in list1 | Also in |
|-----------|-------------|-----------------|---------|
| VVPM | `oneDataLayer[].content.vvpm` | `vvp\|\|M-XX-01234567,...` | — |
| Disease Area | `oneDataLayer[].content.diseaseArea[].id/.name` | `dis\|12345678\|name,...` | — |
| Indication | `oneDataLayer[].content.indication[].id/.name` | `ind\|12345678\|name,...` | — |
| Therapeutic Area | `oneDataLayer[].content.therapeuticArea[].id/.name` | `the\|12345678\|name,...` | — |
| Product (Content) | `oneDataLayer[].content.product[].id/.name` | `pro\|12345678\|name,...` | — |
| Content Tag | `oneDataLayer[].content.contentTags[].id/.name` | `tag\|\|name,...` | — |
| Content Hashtag | `oneDataLayer[].content.contentHashtags[]` | `has\|\|name,...` | — |
| Search Filter | `oneDataLayer[].content.onsiteSearch.filter[].key/.value` | `osf\|key\|value,...` | `eVar18` (JSON) |

---

## Content Details

| Dimension | Data Source | Adobe Variable | Expiration |
|-----------|-------------|----------------|------------|
| Content Duration | `oneDataLayer[].content.contentDuration` | `eVar30` | Hit |
| Content Title | `oneDataLayer[].content.contentTitle` | `eVar80` | Hit |
| Content Summary | `oneDataLayer[].content.contentSummary` | `eVar32` | Hit |

---

## Search

| Dimension | Data Source | Adobe Variable | Events |
|-----------|-------------|----------------|--------|
| Search Term | `oneDataLayer[].content.onsiteSearch.term` | `eVar7` | `onSiteSearch`, `onSiteSearchClick` |
| Search Component | `oneDataLayer[].content.onsiteSearch.searchComponent` | `eVar29` | `onSiteSearch`, `onSiteSearchClick` |
| Search Results Amount | `oneDataLayer[].content.onsiteSearch.results` | `eVar6` | `onSiteSearch`, `onSiteSearchClick` |
| Search Result Position | `content.onsiteSearch.resultPosition` | `eVar5` | `onSiteSearchClick` |

---

## Forms

| Dimension | Data Source | Adobe Variable | Events |
|-----------|-------------|----------------|--------|
| Form Name | `oneDataLayer[].content.form.name` | `eVar17` | `formStart`, `formStep`, `formSuccess`, `formError`, `userRegistration` |
| Form Type | `oneDataLayer[].content.form.type` | `eVar94` | Same as above |
| Form Step | `oneDataLayer[].content.form.step` / `.stepsTotal` | `eVar95` | Same as above |
| Form Error | `oneDataLayer[].content.form.error` | `eVar96` | `formError`, `userRegistrationError`, `userLoginError` |
| Form Fields filled out | `oneDataLayer[].content.form.previousStepFields` | `eVar97` | `formStep`, `userRegistration` |
| Communication channels consent | `oneDataLayer[].content.form.communicationsConsent.channels` | `eVar101`, `list2` | `userRegistration` |
| Communication topics consent | `oneDataLayer[].content.form.communicationsConsent.topics` | `eVar102`, `list2` | `userRegistration` |

---

## Video

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| Video Name | `oneDataLayer[].content.video.name` | `eVar24` |
| Video URL | `oneDataLayer[].content.video.url` | `eVar23` |
| Video Provider | `oneDataLayer[].content.video.provider` | `eVar26` |
| Video Progress | `oneDataLayer[].content.video.progress` | `eVar25` |
| Video Total Duration | `oneDataLayer[].content.video.durationTotal` | `eVar28` |
| Video Error | `oneDataLayer[].content.video.error` | `eVar27` |
| Video Autoplay | `oneDataLayer[].content.video.autoplay` | `eVar58` |

---

## Audio

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| Audio Name | `oneDataLayer[].content.audio.name` | `eVar54` |
| Audio URL | `oneDataLayer[].content.audio.url` | `eVar53` |
| Audio Provider | `oneDataLayer[].content.audio.provider` | `eVar56` |
| Audio Progress | `oneDataLayer[].content.audio.progress` | `eVar55` |
| Audio Error | `oneDataLayer[].content.audio.error` | `eVar57` |

---

## Course & Congress

| Dimension | Data Source | Adobe Variable | Format |
|-----------|-------------|----------------|--------|
| Course | `oneDataLayer[].content.course.*` | `eVar92` | `category\|module\|12345678\|name` |
| Congress | `oneDataLayer[].content.congress.id/.name` | `eVar93` | `12345678\|name` |

---

## User Attributes

| Dimension | Data Source | Adobe Variable | Expiration |
|-----------|-------------|----------------|------------|
| Login Status | `oneDataLayer[].user.isLoggedIn` | `eVar52` | Hit |
| Patient/Relative or HCP | `oneDataLayer[].user.patientOrHcp` | `eVar10` | Never |
| User Profession | `oneDataLayer[].user.profession` | `eVar11` | Never |
| User Specialty | `oneDataLayer[].user.specialty` | `eVar12` | Never |
| User Persona | `oneDataLayer[].user.persona` | `eVar13` | Never |
| User Center or Hospital | `oneDataLayer[].user.center` | `eVar62` | Never |
| User Position | `oneDataLayer[].user.position` | `eVar63` | Never |
| User Interests | `oneDataLayer[].user.areaOfInterest[]` | `eVar99`, `list2` | Never |
| Internal Traffic | Datastream / Processing Rules | `eVar110` | Never |
| Sign-On Social Provider | `oneDataLayer[].user.socialProviderSso` | `eVar46` | Visit |

---

## User Identifiers

| Dimension | Data Source | Adobe Variable | Expiration |
|-----------|-------------|----------------|------------|
| User Marketo Munchkin ID | Adobe Launch Core Code | `eVar67` | Visit |
| User Analytics ID | Adobe Analytics Extension | `eVar33`, `list2` | Hit |
| User MDM/EpiCX ID | `oneDataLayer[].user.mdm` | `eVar34`, `list2` | Never |
| User OneKey ID | `oneDataLayer[].user.oneKey` | `eVar35`, `list2` | Never |
| User Salesforce ID | `oneDataLayer[].user.sId` | `eVar36`, `list2` | Never |
| User UniteCRM ID | `oneDataLayer[].user.uci` | `eVar37`, `list2` | Never |
| User SAP CDC ID | `oneDataLayer[].user.sapcdcUID` | `eVar38`, `list2` | Never |
| Tenant ID | `oneDataLayer[].user.tenantId` | `eVar39` | Hit |
| User ID Type | `oneDataLayer[].user.identifiers[].type` | `list2` | Hit |
| User ID Value | `oneDataLayer[].user.identifiers[].value` | `list2` | Hit |

---

## Marketing & Campaign Parameters

| Dimension | Data Source | Adobe Variable | Expiration |
|-----------|-------------|----------------|------------|
| Google UTM Parameters | URL query string (utm_*) | `eVar59` | Visit |
| Google GCLID | URL query string `gclid` | `eVar71` | Visit |
| Google GBRAID | URL query string `gbraid` | `eVar73` | Visit |
| Google WBRAID | URL query string `wbraid` | `eVar74` | Visit |
| BannerID | URL query string `bid` | `eVar72` | Visit |

---

## E-Commerce

| Dimension | Data Source | Adobe Variable | Events |
|-----------|-------------|----------------|--------|
| Product | `oneDataLayer[].ecommerce.items.item_id` | `products`, `list2` | `prodView`, `scAdd`, `scView`, `scRemove`, `scCheckout`, `purchase` |
| Listing Name | `oneDataLayer[].ecommerce.items.item_name` | `eVar81` (Merchandising, Product Syntax) | Same as above |
| Product ID (non-ecommerce) | `oneDataLayer[].product.productId` | `eVar130` | All events |
| Product Name (non-ecommerce) | `oneDataLayer[].product.productName` | `eVar131` | All events |
| Product Category (non-ecommerce) | `oneDataLayer[].product.productCategory` | `eVar132` | All events |

---

## Downloads & Exit Links

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| Download Links | `href` attribute | `eVar49`, `list2` |
| Exit Link URL | `href` attribute | `eVar48` |

---

## Site Components (list3)

| Dimension | Data Source | Format |
|-----------|-------------|--------|
| Container Summary Component ID | `oneDataLayer[].components.contentSummary.items[].id` | `sum\|id,sum\|id,…` |
| Media on Page | `oneDataLayer[].page.media[].type/.name` | `med\|pdf\|\|name,med\|video\|\|name,...` |
| Bookmark | `oneDataLayer[].components.bookmark.items[].id/.title` | `eVar31` |

---

## Tag Manager & Technical Metadata

| Dimension | Data Source | Adobe Variable |
|-----------|-------------|----------------|
| Analytics Database | Adobe Analytics Extension | `prop5` |
| Analytics Tracking Server | Adobe Analytics Extension | `prop6` |
| Tag Manager ID | Adobe Launch Core Code | `prop7` |
| Tag Manager Environment | Adobe Launch Core Code | `prop8` |
| Tag Manager Version | Adobe Launch Core Code | `prop9` |
| Data Scope | Adobe Launch Core Code | `prop10` |
| Implementation Method | Adobe Launch Core Code | `prop11` |
| Tracking Mode | AEP Datastream Mapping | `prop12` |