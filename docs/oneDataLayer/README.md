# oneDataLayer Documentation

## What is oneDataLayer / EDDL?

`window.oneDataLayer` is Roche's **Event-Driven Data Layer (EDDL)** — a client-side JavaScript array used as a standardised event queue for analytics. Developers push structured JSON objects into the array; Adobe Launch listens and forwards data to Adobe Analytics and other downstream tools.

This folder contains everything needed to implement, validate, and maintain oneDataLayer tracking across any Roche web property.

---

## Folder Structure

```
docs/oneDataLayer/
├── SKILL.md                         # AI skill definition for Copilot
├── README.md                        # This file — navigation index
├── overview.md                      # High-level EDDL concept overview
├── what-is-EDDL-and-how-it-works.md # Deep-dive into EDDL mechanics
├── when-to-fire-each-event.md       # Event timing and trigger rules
├── validation-and-qa-rules.md       # QA checklist and validation guidance
├── troubleshooting.md               # Common issues and fixes
├── CHANGELOG.md                     # Version history
├── implementation/                  # Step-by-step implementation guides
│   ├── adobe-launch-setup.md
│   ├── script-implementation.md
│   └── eddl-steps.md
├── properties/                      # Data dictionary and dimensions/metrics
│   ├── data-dictionary.md
│   └── dimensions-metrics-mapping.md
└── templates/                       # Canonical JSON event templates
    ├── audioProgress.json           # ✅ Use this (valid JSON)
    ├── audioProgress.jsonc          # ⚠️ Legacy — use audioProgress.json instead
    └── ... (30+ event templates)
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
| `troubleshooting.md` | Solutions to common implementation issues |
| `CHANGELOG.md` | History of changes to this documentation set |
| `implementation/adobe-launch-setup.md` | How to set up Adobe Launch for a new property |
| `implementation/script-implementation.md` | Script installation and initialisation patterns |
| `implementation/eddl-steps.md` | Step-by-step EDDL implementation walkthrough |
| `properties/data-dictionary.md` | Full data dictionary for all EDDL fields |
| `properties/dimensions-metrics-mapping.md` | Mapping of EDDL fields to Adobe Analytics dimensions/metrics |
| `templates/` | 30+ canonical JSON templates for every supported event type |

---

## Notes

- **`templates/audioProgress.json`** is the canonical template. `templates/audioProgress.jsonc` is the legacy version (contains comments) and should not be used in new implementations.
- All documentation is **fully generic** and applies to any Roche web property.
- For AI-assisted implementation, see `SKILL.md` — it is the skill definition consumed by Copilot.
