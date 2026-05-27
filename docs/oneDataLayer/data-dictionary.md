# oneDataLayer Data Dictionary

This document centralizes the field definitions used in the `docs/oneDataLayer/templates/` event payloads.

## Purpose

Use this file as the shared reference for:
- field meaning
- expected usage
- example values
- source mapping from the Universal Solution Design Reference

The JSON templates should continue to show payload shape and event-specific examples, while this document explains what each variable means.

## How to use this dictionary

- **ODL Path**: Canonical `oneDataLayer` field path.
- **Description**: Meaning of the field.
- **When used**: When the field should be populated.
- **Example**: Example value or pattern.
- **Notes**: Extra implementation guidance, fallbacks, or scope notes.

---

## Core event fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `event` | Name of the tracking event being pushed to `oneDataLayer`. | On every tracking event. | `defaultPageView`, `defaultLinkClick`, `videoStart` | Required top-level field. |
| `eventDetails` | Additional business or technical context for the event. | On every event, if applicable. | `registration-step-2`, `cta-variant-a` | Use when the event name alone is not enough. |
| `timestamp` | Unix timestamp in milliseconds for when the event occurred. | On every tracking event. | `1674842407984` | Should generally be generated at event time. |
| `campaign` | Campaign code from the landing URL. | On page tracking events when campaign parameters exist. | `spring_launch`, `cid=12345` | Usually sourced from URL query parameters such as UTM values or `cid`. |

---

## Page fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `page.siteName` | Name of the website or application. | On every tracking event. | `Portal` | Stable site identifier. |
| `page.market` | Market providing the site, in English or market code. | On every tracking event. | `CH`, `DE`, `ES`, `INT` | Keep values standardized across sites. |
| `page.language` | Displayed page language. | On every tracking event. | `en`, `de`, `es` | Prefer ISO language codes. |
| `page.url` | Full URL of the page. | On every tracking event. | `https://example.roche.com/en/home` | Usually mirrors `location.href`. |
| `page.domain` | Domain of the site. | On every tracking event. | `example.roche.com` | Usually mirrors `location.hostname`. |
| `page.path` | Path portion of the URL. | On every tracking event. | `/en/home` | Should start with `/`. |
| `page.pageTitle` | Current page title, often aligned with SEO/title tag needs. | On every tracking event. | `Roche Portal – Home` | May change over time; not the preferred stable reporting label. |
| `page.pageName` | Stable, human-friendly name of the page for reporting. | On every tracking event. | `home`, `dashboard`, `product overview` | Should not change over time if avoidable. |
| `page.uuid` | Adobe Experience Manager UUID of the page. | On every tracking event, if applicable. | `95561ad0-c364-408b-9cc3-f43dea4d4560` | Populate only when available. |
| `page.pageType` | Type of the page or view. | On every page load or SPA view. | `Home`, `Contact`, `Article` | Useful for reporting by page class. |
| `page.categoryTree` | Breadcrumb or hierarchical category trail for the page. | On every tracking event, if applicable. | `Inicio|Patologías|Atrofia Muscular Espinal` | Separator should stay consistent. |
| `page.mainCategory` | Main page section/category, in English where possible. | On every tracking event, if applicable. | `Products`, `Training` | Keep normalized across the site. |
| `page.errorType` | Type of page error. | On page error events. | `HTTP 404`, `HTTP 500`, `Application error` | Usually paired with the `pageError` event. |
| `page.error` | Error identifier or message for page errors. | On page error events. | `route-not-found` | Use concise, reportable values. |
| `page.scrolled` | Scroll milestone reached on the page. | On `scroll` events. | `75%` | Current guidance focuses on meaningful milestones only. |
| `page.protected` | Access level of the page. | On page tracking events. | `public`, `registrationwall`, `protected` | Use a controlled vocabulary. |
| `page.media[]` | Media items displayed on the page. | On page tracking events, if applicable. | `{ "type": "video", "name": "About cystic fibrosis" }` | Used to describe media present, not necessarily interacted with. |

### Previous page fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `page.previousPage.url` | Full URL of the previous page. | On tracking events when previous page context is known. | `https://example.roche.com/en/start` | Can fall back to `document.referrer` where appropriate. |
| `page.previousPage.path` | Path of the previous page. | On tracking events when previous page context is known. | `/en/start` | Optional when unavailable. |
| `page.previousPage.pageTitle` | Title of the previous page. | On tracking events when previous page context is known. | `Start Page` | Optional when unavailable. |
| `page.previousPage.pageName` | Stable name of the previous page. | On tracking events when previous page context is known. | `start page` | Optional when unavailable. |
| `page.previousPage.categoryTree` | Breadcrumb/category trail of the previous page. | On tracking events when previous page context is known. | `Section|Subsection` | Optional when unavailable. |
| `page.previousPage.mainCategory` | Main category of the previous page. | On tracking events when previous page context is known. | `Products` | Optional when unavailable. |

---

## Link fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `link.url` | Destination URL of the clicked link. | On link tracking events. | `https://external.site/resource.pdf` | Commonly used on `defaultLinkClick`. |
| `link.domain` | Destination domain of the clicked link. | On link tracking events. | `external.site` | Derive from `link.url` where possible. |
| `link.path` | Destination path of the clicked link. | On link tracking events. | `/resource.pdf` | Optional when not applicable. |
| `link.innerText` | Visible text inside the clicked element. | On link tracking events. | `Download brochure` | Useful when `link.name` is not enough. |
| `link.position` | Position of the clicked link in the page or component. | On link tracking events. | `header`, `footer`, `1`, `3` | Keep format consistent within an implementation. |
| `link.name` | Friendly name of the link clicked. | On link tracking events. | `download brochure` | Use a stable descriptive name. |
| `link.type` | Type of clicked element. | On link tracking events. | `Button`, `Link` | Prefer a controlled vocabulary. |
| `link.socialMedia` | Social media platform linked to or used for sharing. | On social link/share interactions. | `Facebook`, `LinkedIn`, `WhatsApp` | Use standard platform names. |
| `link.socialInteraction` | Whether the action was a direct social link click or share interaction. | On social link/share interactions. | `Link`, `Share` | Use a controlled vocabulary. |

---

## Content classification fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.vvpm` | Veeva PromoMats identifier of displayed or interacted content. | On tracking events where applicable. | `VVPM123456` | Populate when governed content is involved. |
| `content.indication[]` | Indication values related to the content. | On page or content-related events, if applicable. | `{ "id": "ROX1305277804343", "name": "Colorectal cancer" }` | Use arrays when multiple values apply. |
| `content.therapeuticArea[]` | Therapeutic area values related to the content. | On page or content-related events, if applicable. | `{ "id": "ROX1412243655827", "name": "Oncology" }` | Use arrays when multiple values apply. |
| `content.product[]` | Product values related to the content. | On page or content-related events, if applicable. | `{ "id": "ROX1302017050223", "name": "Avastin" }` | Content classification, not ecommerce product tracking. |
| `content.contentTags[]` | Named tags that apply to the content. | On page tracking events, if applicable. | `{ "id": "ROX38521440444119025", "name": "Product information" }` | Use arrays for multiple tags. |
| `content.contentHashtags[]` | Hashtags associated with the content. | On page or content events when applicable. | `Neurology`, `Prevention` | Prefer plain strings without `#` unless implementation requires it. |
| `content.contentDuration` | Anticipated time required to consume the content, in seconds. | On page or content events when available. | `60`, `180`, `450` | Numeric value preferred in the payload. |
| `content.contentTitle` | Title of the content being displayed. | On page or content events when available. | `addressing-unmet-needs-in-diagnosis` | Can differ from page title. |
| `content.contentSummary` | Short summary of the content. | On page tracking events when applicable. | `learn-how-in-vitro-diagnostics-support-decision-making` | Keep concise. |

---

## Search fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.onsiteSearch.term` | Search term entered by the user. | On `onSiteSearch` and `onSiteSearchClick`. | `lung cancer` | Populate with the submitted query. |
| `content.onsiteSearch.filter` | Search filters applied by the user. | On `onSiteSearch` and `onSiteSearchClick`, if applicable. | `{ "Region": ["Europe"] }` | Keep key/value representation consistent across implementations. |
| `content.onsiteSearch.searchComponent` | Name of the search component used. | On `onSiteSearch` and `onSiteSearchClick`. | `global search`, `header search` | Important when multiple search tools exist. |
| `content.onsiteSearch.results` | Number of search results returned. | After search results load/update. | `42` | Prefer numeric value in payload. |
| `content.onsiteSearch.resultPosition` | Position of the clicked search result. | On `onSiteSearchClick`. | `1`, `2`, `3` | Use one-based indexing unless implementation states otherwise. |

---

## Form fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.form.name` | Name of the form. | On form load and form interactions. | `newsletter signup` | Stable reporting-friendly label. |
| `content.form.type` | Form type/category. | On form load and form interactions. | `Newsletter`, `Registration` | Use controlled values when possible. |
| `content.form.step` | Current form step. | On multi-step form events. | `1`, `2`, `1/3` | Align format across templates. |
| `content.form.stepsTotal` | Total number of form steps. | On multi-step form events when known. | `3` | Optional when unknown. |
| `content.form.error` | Error encountered in the form. | On `formError`, `userRegistrationError`, `userLoginError`. | `email-invalid`, `required-field-missing` | Use concise, reportable values. |
| `content.form.previousStepFields` | Summary of fields filled in previous step(s). | On `formStep` and `userRegistration` where applicable. | `Mandatory: 3, Optional: 2/3 on Step 2` | Use only if this summary is available and meaningful. |
| `content.form.communicationsConsent.channels` | Communication channels selected during registration. | On successful user registration. | `Email: true, SMS: false` | Prefer structured representation if implementation supports it. |
| `content.form.communicationsConsent.topics` | Communication topics selected during registration. | On successful user registration. | `Medical Education: true` | Prefer structured representation if implementation supports it. |

---

## Media interaction fields

### Video

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.video.name` | Name/title of the video. | On video interaction events. | `Strength exercises (Part 1)` | Used on `videoStart`, `videoProgress`, `videoComplete`, `videoError`. |
| `content.video.url` | URL of the video. | On video interaction events. | `https://video.example.com/watch/123` | Optional when unavailable from the player. |
| `content.video.provider` | Video hosting/provider name. | On video interaction events. | `YouTube` | Use standard provider names. |
| `content.video.progress` | Playback milestone reached. | On `videoProgress` and `videoComplete`. | `10`, `25`, `50`, `75`, `100` | Usually percent milestone. |
| `content.video.durationTotal` | Total duration of the video in seconds. | On video interaction events. | `240` | Numeric value preferred. |
| `content.video.error` | Video player error identifier. | On `videoError`. | `network-error` | Keep values concise and stable. |
| `content.video.autoplay` | Indicates whether playback started automatically. | On initial video play. | `true`, `false` | Use boolean if supported by implementation. |

### Audio

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.audio.name` | Name/title of the audio asset. | On audio interaction events. | `The patient-centered approach` | Used on `audioStart`, `audioProgress`, `audioComplete`, `audioError`. |
| `content.audio.url` | URL of the audio asset. | On audio interaction events. | `https://audio.example.com/file.mp3` | Optional when unavailable from the player. |
| `content.audio.provider` | Audio hosting/provider name. | On audio interaction events. | `Self-hosted` | Use standard provider names. |
| `content.audio.progress` | Listening milestone reached. | On `audioProgress` and `audioComplete`. | `10`, `25`, `50`, `75`, `100` | Usually percent milestone. |
| `content.audio.error` | Audio player error identifier. | On `audioError`. | `decode-error` | Keep values concise and stable. |

---

## Course and congress fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `content.course.category` | Course category. | On course interaction events. | `Oncology courses` | Used on `courseEnroll`, `courseStart`, `courseComplete`. |
| `content.course.module` | Course module. | On course interaction events. | `Module 1` | Optional when unavailable. |
| `content.course.id` | Course identifier. | On course interaction events. | `123456` | Stable ID preferred. |
| `content.course.name` | Course name. | On course interaction events. | `Hospital pharmacy` | Human-friendly title. |
| `content.congress.id` | Congress identifier. | On congress-related events. | `123456` | Stable ID preferred. |
| `content.congress.name` | Congress name. | On congress-related events. | `WASH 2021` | Human-friendly title. |

---

## User fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `user.isLoggedIn` | Indicates login status of the user. | On every tracking event when available. | `true`, `false` | Keep representation consistent across templates. |
| `user.patientOrHcp` | User classification as patient/relative or healthcare professional. | On events where applicable. | `Patient/Relative`, `Health Care Professional` | Use controlled values. |
| `user.profession` | Logged-in user's profession. | On tracking events when applicable. | `Doctor` | Optional when unavailable. |
| `user.specialty` | Logged-in user's specialty. | On tracking events when applicable. | `Pneumology` | Optional when unavailable. |
| `user.persona` | User persona classification. | On events where applicable. | `technician`, `researcher`, `patient` | Use controlled values where possible. |
| `user.center` | Logged-in user's center or hospital. | On tracking events when applicable. | `University Hospital Basel` | Optional when unavailable. |
| `user.position` | Logged-in user's position/job title. | On tracking events when applicable. | `Head of department` | Optional when unavailable. |
| `user.areaOfInterest[]` | User areas of interest. | On tracking events when applicable. | `Chronic lymphocytic leukemia` | Use array when multiple values apply. |
| `user.internal` | Indicates internal Roche traffic. | On tracking events when applicable. | `true`, `false` | Often derived from rules, query params, IP lookup, or user agent. |
| `user.mdm` | MDM / EpiCX identifier of the user. | On tracking events when applicable. | `b65dec5025304c496d1cb4d8f4a362c5` | Sensitive identifier; ensure governance requirements are met. |
| `user.oneKey` | IQVIA OneKey identifier. | On tracking events when applicable. | `123456789` | Sensitive identifier; ensure governance requirements are met. |
| `user.sId` | Salesforce user identifier. | On tracking events when applicable. | `005xxxxxxxxxxxx` | Sensitive identifier; ensure governance requirements are met. |
| `user.uci` | UniteCRM user identifier. | On tracking events when applicable. | `UCI-12345` | Sensitive identifier; ensure governance requirements are met. |
| `user.sapcdcUID` | SAP CDC / Gigya / CIAM user identifier. | On tracking events when applicable. | `9a8b7c6d...` | Sensitive identifier; ensure governance requirements are met. |
| `user.tenantId` | Tenant identifier for multi-tenant systems. | On tracking events when applicable. | `account-123` | Optional when implementation is multi-tenant. |
| `user.identifiers[]` | Generic array of user identifiers. | On tracking events when applicable. | `{ "type": "mdm", "value": "b65dec..." }` | Preferred extensible structure when multiple IDs may exist. |
| `user.socialProviderSso` | Social provider used for login or registration. | On auth events with SSO provider. | `Facebook`, `Google`, `LinkedIn` | Use on registration/login events where applicable. |

---

## Component fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `components.contentSummary.items[]` | Content Summary component instances present on the page. | On page tracking events when applicable. | `{ "id": "summary-01" }` | Useful for component impression reporting. |
| `components.bookmark.items[]` | Bookmarkable content items rendered or interacted with. | On bookmark-related interactions. | `{ "id": "12345", "title": "About cystic fibrosis" }` | Use item arrays when multiple bookmarks are present. |
| `components.embeds.items[]` | Embedded widgets or components rendered in the experience. | On page or component events when applicable. | `[]` | Referenced in the overview as a recurring structure. |

---

## Ecommerce 

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `ecommerce.items[].item_id` | Product identifier in ecommerce events. | On `prodView`, `scAdd`, `scView`, `scRemove`, `scCheckout`, `purchase`. | `SKU-12345` | Use ecommerce structure where feasible. |
| `ecommerce.items[].item_name` | Product name in ecommerce events. | On ecommerce events. | `Diagnostics Kit A` | Human-friendly label. |

## Product fields

| Field | Description | When used | Example | Notes |
|---|---|---|---|---|
| `product.productId` | Product ID outside the ecommerce structure. | On product interactions when ecommerce tracking is not used. | `ROX12345` | Fallback for non-ecommerce implementations. |
| `product.productName` | Product name outside the ecommerce structure. | On product interactions when ecommerce tracking is not used. | `Avastin` | Fallback for non-ecommerce implementations. |
| `product.productCategory` | Product category outside the ecommerce structure. | On product interactions when ecommerce tracking is not used. | `Oncology` | Fallback for non-ecommerce implementations. |

---