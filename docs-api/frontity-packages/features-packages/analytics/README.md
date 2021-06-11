# Analytics packages

There is a set of official _Analytics Frontity packages_ that you can use to easily add web analytics services to your project.

These packages are:

- [`@frontity/google-analytics`](google-analytics.md) for data tracking using [Google Analytics](https://analytics.google.com/)
- [`@frontity/google-tag-manager-analytics`](google-tag-manager-analytics.md) for data tracking using [Google Tag Manager](https://tagmanager.google.com/)
- [`@frontity/comscore-analytics`](comscore-analytics.md) for data tracking using [Comscore](https://www.comscore.com/)

## Table of Contents

- [Installation](./#installation)
- [Settings](./#settings)
  - [`state.analytics.pageviews`](./#state-analytics-pageviews)
  - [`state.analytics.events`](./#state-analytics-events)
- [How to use](./#how-to-use)
  - [`actions.analytics.pageview`](./#actions-analytics-pageview)
  - [`actions.analytics.event`](./#actions-analytics-event)

## Installation

Install the analytics package you need for your project:

- [Installation instructions for `@frontity/google-analytics`](google-analytics.md#install)
- [Installation instructions for `@frontity/google-tag-manager-analytics`](google-tag-manager-analytics.md#install)
- [Installation instructions for `@frontity/comscore-analytics`](comscore-analytics.md#install)

## Settings

Each package will require some custom configuration to add things such as the tracking IDs for the services behind. In the description of each package you'll find the details of each configuration:

- [Settings for `@frontity/google-analytics`](google-analytics.md#settings)
- [Settings for `@frontity/google-tag-manager-analytics`](google-tag-manager-analytics.md#settings)
- [Settings for `@frontity/comscore-analytics`](comscore-analytics.md#settings)

Once we have properly installed and configured these `analytics` packages, their actions will be centralized by the `analytics` namespace.

In `frontity.settings.js` we can enable/disable specific analytics packages for pageviews or events through the following properties in the `state` \(under the `analytics` namespace\):

- `state.analytics.pageviews`
- `state.analytics.events`

These properties can be set directly in `frontity.settings.js` from `state.analytics`...

```javascript
const settings = {
  name: ...,
  state: {
    frontity: {...},
    analytics: {
      pageviews: {
        googleAnalytics: false,
        comscoreAnalytics: true,
      },
      events: {
        googleAnalytics: true,
        comscoreAnalytics: false,
      }
    },
  },
  packages: [{...}, {...}]
};

export default settings;
```

Or from each Analytics package setting...

```javascript
const settings = {
  ...,
  packages: [
    {
      name: "@frontity/google-analytics",
      state: {
        analytics: {
          pageviews: { googleAnalytics: true },
          events: { googleAnalytics: true }
        },
      },
    },
    {
      name: "@frontity/comscoreAnalytics",
      state: {
        analytics: {
          pageviews: { comscoreAnalytics: false },
          events: { comscoreAnalytics: false }
        },
      },
    },
    ...
  ],
};
export default settings;
```

### `state.analytics.pageviews`

Map of Analytics packages namespaces with boolean values.

This object is used by `actions.analytics.pageview` to know which analytics packages should send the pageview to their respective services.

If you want to disable sending pageviews for a specific analytics package, the respective namespace of that package should be set here to `false`.

{% hint style="info" %}
All analytics namespaces will be `true` by default in this setting.
{% endhint %}

### `state.analytics.events`

Map of Analytics packages namespaces with boolean values.

This object is used by `actions.analytics.event` to know which analytics packages should send the event to their respective services.

If you want to disable sending events for a specific analytics package, the respective namespace of that package should be set here to `false`.

{% hint style="info" %}
All analytics namespaces will be `true` by default in this setting.
{% endhint %}

## How to use

Once everything is properly configured, the following `actions` under the namespace `analytics` will be ready to be used:

- `actions.analytics.pageview`
- `actions.analytics.event`

### `actions.analytics.pageview`

Send a pageview to all active analytics packages.

This action takes all namespaces defined in `state.analytics.pageviews` that are `true` and calls the `pageview` action of each one with the specified `Pageview` object.

`actions.analytics.pageview` is automatically launched every time link changes \(or every time `action.router.set(link)` is launched\).

{% hint style="warning" %}
This action is is not meant to be called directly but in case you still want to do this it would be something like this:

```javascript
actions.analytics.pageview({
  link: "/2016/the-beauties-of-gullfoss",
  title: "The Beauties Of Gullfoss",
});
```

{% endhint %}

### `actions.analytics.event`

Send an event to all enabled analytics packages.

This action takes all namespaces defined in `state.analytics.events` that are `true` and calls the `event` action of each one with the specified `Event` object.

This is the method you can call from any component of your React app to track specific events.

_Example:_

```javascript
actions.analytics.event({
  name: "click",
  payload: {
    category: "video",
    label: "featured-media",
  },
});
```

The `actions.analytics.event()` must receive an event object with the following properties.

| Name          | Type   | Required | Description                                                                             |
| :------------ | :----- | :------- | :-------------------------------------------------------------------------------------- |
| **`name`**    | string | yes      | The value of this property is mapped to the proper name event of each analytics package |
| **`payload`** | object | yes      | Event payload.                                                                          |

This method will send the event tracking information to all the packages enabled in `state.analytics.events`.

Each package will handle the information sent through this `actions.analytics.event()` in a different way:

- [How `@frontity/google-analytics` handles this event object](google-analytics.md#actions-analytics-event)
- [How `@frontity/google-tag-manager-analytics` handles this event object](google-tag-manager-analytics.md#actions-analytics-event)
- [How `@frontity/comscore-analytics` handles this event object](comscore-analytics.md#actions-analytics-event)
