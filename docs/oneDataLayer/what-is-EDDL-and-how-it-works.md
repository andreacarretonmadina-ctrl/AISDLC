# Event-Driven Data Layer (EDDL)

An Event-Driven Data Layer (**EDDL**) is an object that provides information about the displayed page and user interactions. It aims to reduce site instrumentation efforts by providing a standardized method for exposing and accessing any type of data that must be available on all pages of the website.

## What is `oneDataLayer`?

Our **EDDL**, the object `oneDataLayer`, provides a data structure you can think of as a common language for Digital Analytics and MarTech tools to understand how visitors behave on our websites and what they do across markets and products.

Events, alongside context parameters, are pushed to the **EDDL** array in **JSON** syntax, using an event key to initiate the sending of events.

## How it works

The **EDDL** array serves as a queue that avoids racing conditions between scripts and displays the entire history of events until the page is refreshed.

Events are to be emitted when:

- The page (and subsequent **SPA** views) loaded successfully.
- A user interacted with website elements such as links, buttons, forms, etc.

This is done by calling `oneDataLayer.push()` when an event occurs.

## Asynchronous loading

The page and other scripts can start triggering events and registering data by adding them to the array even before the tag management script is loaded. This enables asynchronous loading of scripts.

## How to push events

```javascript
window.oneDataLayer = window.oneDataLayer || [];
window.oneDataLayer.push({
    event: "eventName",
    // ...additional parameters
});
```

> **Important:**
> - `oneDataLayer` can be set without considering the consent state (opt-in/opt-out) since it is a local object on the user's device. Data is not leaving the user's device.
> - Always use the first line `window.oneDataLayer = window.oneDataLayer || [];` before pushing events to not break event tracking.

The folder `templates/` specifies all possible key-value pairs. **Avoid languages other than English for the values set, except for market-specific values or specific content names.**
The folder `templates/` contains all Events alongside code examples (key-value pairs to be included if applicable) to be pushed.

All changes are tracked in EDDL Changelog in our GitLab Repo	
https://code.roche.com/gps/onedatalayer/-/blob/main/oneDataLayer-changelog.md	
	
Instructions for developping EDDL oneDataLayer object:	
https://docs.google.com/document/d/1nes69O-n85l0ve04urbIL0WqqtQ6MXJZoavUoFh_EXc/edit#heading=h.vbuwf7ch6uaz	

Instructions for debugging the EDDL implementation:	
https://docs.google.com/document/d/1iXraSkKPao7IxUtsSvgogYRlCKfr2y6QoVhq6rgUlIY/	