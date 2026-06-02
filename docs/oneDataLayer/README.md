# oneDataLayer Documentation

## What is oneDataLayer / EDDL?

`window.oneDataLayer` is Roche's **Event-Driven Data Layer (EDDL)** — a client-side JavaScript array used as a standardised event queue for analytics. Developers push structured JSON event objects into the array, and Adobe Launch (or any other tag manager / analytics consumer) reads from the queue to forward the events to downstream tools such as Adobe Analytics.

This folder contains everything needed to implement, validate, and maintain oneDataLayer tracking across any Roche web property.

---

## Folder Structure

```
docs/oneDataLayer/
├── SKILL.md                                  # AI skill definition for Copilot
├── README.md                                 # This file — navigation index
├── overview.md                               # High-level EDDL concept overview
├── what-is-EDDL-and-how-it-works.md          # Deep-dive into EDDL mechanics
├── when-to-fire-each-event.md                # Event timing and trigger rules
├── validation-and-qa-rules.md                # QA checklist and validation guidance
├── test-suite.md                             # Test suite for EDDL implementations
├── troubleshooting.md                        # Common issues and fixes
├── implementation/                           # Step-by-step implementation guides
│   ├── adobe-analytics-set-up-for-new-websites.md
│   ├── adobe-script-implementation.md
│   └── eddl-implementation-steps.md
├── properties/                               # Data dictionary and dimensions/metrics
│   ├── data-dictionary.md
│   ├── adobe-analytics-dimensions-mapping.md
│   └── adobe-analytics-metrics-mapping.md
└── templates/                                # Canonical JSON event templates
    ├── audioProgress.json                    # ✅ Use this (valid JSON)
    ├── audioProgress.jsonc                   # ⚠️ Legacy — use audioProgress.json instead
    └── ... (35 .json templates + 1 .jsonc legacy file)
```

---

## Key Documentation Files

| File | Description |
|------|-------------|
| `SKILL.md` | AI skill definition — the authoritative source of truth for Copilot on oneDataLayer |
| `overview.md` | High-level introduction to EDDL and its purpose |
| `what-is-EDDL-and-how-it-works.md` | Detailed explanation of how the data layer works end-to-end |
| `when-to-fire-each-event.md` | Rules for when each event type should be triggered |
| `validation-and-qa-rules.md` | Validation checklist and QA process |
| `test-suite.md` | Test suite for verifying EDDL implementations |
| `troubleshooting.md` | Solutions to common implementation issues |
| `implementation/adobe-analytics-set-up-for-new-websites.md` | How to set up Adobe Analytics / Launch for a new property |
| `implementation/adobe-script-implementation.md` | Script installation and initialisation patterns |
| `implementation/eddl-implementation-steps.md` | Step-by-step EDDL implementation walkthrough |
| `properties/data-dictionary.md` | Full data dictionary for all EDDL fields |
| `properties/adobe-analytics-dimensions-mapping.md` | Mapping of EDDL fields to Adobe Analytics dimensions (eVars / props) |
| `properties/adobe-analytics-metrics-mapping.md` | Mapping of EDDL fields to Adobe Analytics metrics (events) |
| `templates/` | 35 canonical JSON templates (plus 1 legacy `.jsonc`) for every supported event type |

---

## Notes

- **`templates/audioProgress.json`** is the canonical template. `templates/audioProgress.jsonc` is the legacy version (contains comments) and should not be used in new implementations.
- All documentation is **fully generic** and applies to any Roche web property.
- For AI-assisted implementation, see `SKILL.md` — it is the skill definition consumed by Copilot.
