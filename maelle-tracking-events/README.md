# Maelle – Tracking Events

This folder contains JSON samples for all tracked events on the Maelle website, organized by event type.

## Folder structure

### `defaultLinkClick events/`

Tracks user clicks on links, buttons, and interactive elements.

| File | Description |
|------|-------------|
| `accessMaelle_hero_defaultLinkClick.jsonc` | Click on "Access Maelle" button in hero section |
| `accessMaelle_navBar_defaultLinkClick.jsonc` | Click on "Access Maelle" in navigation bar |
| `BusinessModuleLink_defaultLinkClick.jsonc` | Click on a business module link |
| `capabilitiesButton_defaultLinkClick.json` | Click on capabilities button |
| `capabilitiesLink_defaultLinkClick.json` | Click on capabilities link |
| `contactSubmit_defaultLinkClick.jsonc` | Submit contact form |
| `howItWorksLink_defaultLinkClick.json` | Click on "How it works" link |
| `PillarExternalLink_defaultLinkClick.json` | Click on external pillar link |
| `readMoreLink_defaultLinkClick.jsonc` | Click on "Read more" link |
| `resourcesIcon_defaultLinkClick.json` | Click on resources icon |
| `resourcesLink_defaultLinkClick.json` | Click on resources link |
| `teamContact_body_defaultLinkClick.jsonc` | Click on team contact in body |
| `teamContact_hero_defaultLinkClick.jsonc` | Click on team contact in hero section |

### `defaultPageView events/`

Tracks page load events.

| File | Description |
|------|-------------|
| `aboutPage_defaultPageView.jsonc` | About page view |
| `capabilities_defaultPageView.jsonc` | Capabilities page view |
| `homepage_defaultPageView.jsonc` | Homepage view |
| `resourcesPage_defaultPageView.jsonc` | Resources page view |

### `onSiteSearch events/`

Tracks search queries performed by users.

| File | Description |
|------|-------------|
| `capabilities_onSiteSearch.json` | Search within capabilities |

### `onSiteSearchClick events/`

Tracks clicks on search results.

| File | Description |
|------|-------------|
| `capabilitiesBubble_onSiteSearchClick.json` | Click on capabilities search result bubble |

## Usage

Each JSON file represents a sample payload that should be pushed to the `oneDataLayer` array. See the [EDDL overview](../maelle-how-to-track-events/eddl-overview.md) for implementation details.
