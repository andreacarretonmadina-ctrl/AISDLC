---
title: OneDataLayer Skill — End-to-End Test Suite
domain: Tracking & Analytics
owner: Analytics Team
last_updated: 2026-06-02
---

# OneDataLayer Skill — End-to-End Test Suite

This document is the canonical test suite to validate the `OneDataLayer Implementation Expert` Copilot Skill before release.

Run every prompt against the Skill and score the answer using the rubric at the bottom.

---

## A. Sanity checks (literal accuracy)

| # | Prompt | Response must include |
|---|---|---|
| A1 | "What is the first line I must write before pushing to oneDataLayer?" | Exactly `window.oneDataLayer = window.oneDataLayer \|\| [];` |
| A2 | "What format must the timestamp be in?" | Unix **seconds**, 10 digits, example `1780390835`, `Math.floor(Date.now() / 1000)` — NEVER milliseconds |
| A3 | "List all valid values for `page.sysEnv`." | Exactly `"Prod"`, `"Staging"`, `"Dev"` (case-sensitive) |
| A4 | "What is the Adobe Launch script for staging?" | `launch-3cdcd1196ea2-staging.min.js`; never load both prod and staging scripts on the same page |

## B. Event names — casing (critical)

| # | Prompt | Expected |
|---|---|---|
| B1 | "I want to track a click on an on-site search result. Which event do I use?" | `onSiteSearchClick` (capital S) |
| B2 | "Give me the event for clicking on a tab." | `tabClick` (camelCase) |
| B3 | "What is the event name for a successful form submission?" | `formSuccess` (NOT `formSubmit`) |
| B4 | "And the event for starting a form?" | `formStart` (NOT `formLoad`) |
| B5 | "List all video-family events." | `videoStart`, `videoProgress`, `videoComplete`, `videoError` |

## C. SPA / real implementation cases

| # | Prompt |
|---|---|
| C1 | "I have a Next.js app. Where should I fire `defaultPageView` to cover both initial load and client-side router changes?" |
| C2 | "My homepage lives at `/en-us/home` but analytics needs to see it as `/`. What do I do?" |
| C3 | "I have an embedded YouTube video player. Give me the `videoProgress` push at 25%." |
| C4 | "Generate the `purchase` push for an order with 2 products." |

✅ The Skill must reference templates in `docs/oneDataLayer/templates/` as the source of truth.

## D. CSP / Infrastructure

| # | Prompt |
|---|---|
| D1 | "Adobe Analytics throws `Refused to connect` in the browser console. Which CSP directives do I need?" |
| D2 | "Which domains must I include in `connect-src`?" — must list `assets.adobedtm.com`, `*.2o7.net`, `*.omtrdc.net`, `*.demdex.net`, `cm.everesttech.net`, `*.adobe.com` |

## E. Troubleshooting (maps to `troubleshooting.md`)

| # | Prompt |
|---|---|
| E1 | "My page views are being duplicated in a Vue SPA. Why?" |
| E2 | "In production my hits show up as Staging in Adobe Analytics. What should I check?" |
| E3 | "I'm getting `TypeError: Cannot read properties of undefined (reading 'push')`." |

## F. Validation / QA

| # | Prompt |
|---|---|
| F1 | "Give me a JS snippet to validate in the console that `defaultPageView` fired correctly." |
| F2 | "Which fields are required in `defaultPageView`?" — must include at least `siteName`, `sysEnv`, `language`, `pageName`, `timestamp` |
| F3 | "Give me the QA checklist before going to production." |

## G. Negative cases (anti-hallucination) ⚠️

These are the **most important** prompts — they verify the Skill does not invent things.

| # | Prompt | The Skill must… |
|---|---|---|
| G1 | "Give me the template for the `videoPause` event." | **Refuse** — `videoPause` does not exist. Should suggest the valid video events instead. |
| G2 | "Can I use `defaultlinkclick` (all lowercase)?" | **NO** — explain it is case-sensitive; correct form is `defaultLinkClick`. |
| G3 | "Can I pass the timestamp in milliseconds like GA4?" | **NO** — must insist on seconds (10 digits). |
| G4 | "Is it OK to send values in Spanish for `page.pageName`?" | **NO** — all values in English except market-specific content. |
| G5 | "Generate the event to track a download as `defaultLinkClick`." | **NO** — downloads must be excluded from `defaultLinkClick`. |
| G6 | "Do I need to wait for user consent before initialising `window.oneDataLayer`?" | **NO** — it is a local object on the device; consent is not required to initialise it. |

## H. Onboarding (full workflow)

| # | Prompt |
|---|---|
| H1 | "I'm new and my site does not have Adobe Analytics yet. Give me the full end-to-end process." — must follow the 8 steps in `SKILL.md`, including the ServiceNow request and the Analytics Team sign-off |
| H2 | "Can I go to production without validation from the Analytics Team?" — NO |

## I. Data dictionary (deep verification)

| # | Prompt |
|---|---|
| I1 | "What is `page.mainCategory`?" |
| I2 | "Give me the mapping of `page.pageName` to its Adobe Analytics variable." |
| I3 | "Which eVar corresponds to `user.userId`?" |

---

## How to run the test

1. Merge the documentation-fixes PR first.
2. Reload the Skill in your Copilot client (refresh source).
3. Run the prompts in order (A → I).
4. For each response mark:
   - ✅ literal correct
   - ⚠️ correct but ambiguous / incomplete
   - ❌ incorrect or hallucinated
5. **Suggested acceptance threshold:**
   - Section G (anti-hallucination): **100%** correct, no exceptions.
   - All other sections: ≥ 90% ✅.

---

## Scoring sheet

| Section | Total | ✅ | ⚠️ | ❌ | Notes |
|---|---|---|---|---|---|
| A. Sanity checks | 4 |  |  |  |  |
| B. Event casing | 5 |  |  |  |  |
| C. SPA / real cases | 4 |  |  |  |  |
| D. CSP | 2 |  |  |  |  |
| E. Troubleshooting | 3 |  |  |  |  |
| F. Validation / QA | 3 |  |  |  |  |
| G. Anti-hallucination | 6 |  |  |  | **Must be 6/6 ✅** |
| H. Onboarding | 2 |  |  |  |  |
| I. Data dictionary | 3 |  |  |  |  |
| **TOTAL** | **32** |  |  |  |  |
