# Changelog

All notable changes to the oneDataLayer documentation are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [Unreleased]

### Added
- `templates/audioProgress.json` — valid JSON replacement for the legacy `templates/audioProgress.jsonc` (which contained inline comments, making it invalid JSON)
- `troubleshooting.md` — common issues guide covering SPAs, CSP, initialisation order, `sysEnv`, event name casing, and missing required fields
- `README.md` — folder index with descriptions of all files and the folder structure

### Changed
- `SKILL.md` — fixed event name inconsistencies: `formLoad` → `formStart`, `formSubmit` → `formSuccess`; added `formStep` to form events
- `SKILL.md` — added missing event families: `userLogin`, `userAutoLogin`, `userLoginError`, `userRegistration`, `userRegistrationError`, `consentUpdated`, `purchase`, `scAdd`, `scRemove`, `scCheckout`, `scView`, `prodView`, `pageError`, `tabClick`, `componentImpression`
- `SKILL.md` — removed site-specific Maelle reference to keep the skill fully generic

### Deprecated
- `templates/audioProgress.jsonc` — use `templates/audioProgress.json` instead

---

## [1.0.0] — Initial release

### Added
- `SKILL.md` — AI skill definition for oneDataLayer / EDDL implementation
- `overview.md` — high-level EDDL concept overview
- `what-is-EDDL-and-how-it-works.md` — detailed mechanics of the data layer
- `when-to-fire-each-event.md` — event timing and trigger rules
- `validation-and-qa-rules.md` — QA checklist and validation guidance
- `implementation/` — Adobe Launch setup, script implementation, and EDDL steps guides
- `properties/data-dictionary.md` — full data dictionary for all EDDL fields
- `properties/dimensions-metrics-mapping.md` — mapping of EDDL fields to Adobe Analytics dimensions and metrics
- `templates/` — 30+ canonical JSON event templates covering page, video, audio, forms, ecommerce, user, search, and more
- `templates/audioProgress.jsonc` — initial audio progress template (legacy; see [Unreleased])
