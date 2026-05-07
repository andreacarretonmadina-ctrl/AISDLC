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
