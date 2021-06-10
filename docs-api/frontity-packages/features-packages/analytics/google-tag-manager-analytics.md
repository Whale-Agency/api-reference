# @frontity/google-tag-manager-analytics

Analytics package to use [Google Tag Manager](https://tagmanager.google.com/) with Frontity

## Table of Contents

- [Install](google-tag-manager-analytics.md#install)
- [Settings](google-tag-manager-analytics.md#settings)
- [Usage](google-tag-manager-analytics.md#usage)
  - [`actions.analytics.pageview`](google-tag-manager-analytics.md#actions-analytics-pageview)
  - [`actions.analytics.event`](google-tag-manager-analytics.md#actions-analytics-event)

## Install

```bash
npm i @frontity/google-tag-manager-analytics
```

## Settings

The [namespace](https://docs.frontity.org/learning-frontity/namespaces) for this package is **`googleTagManagerAnalytics`**

Every Google Tag Manager account has a [Container ID](https://support.google.com/tagmanager/answer/6103696?hl=en).  
To connect the package with a specific account \(or accounts\) we can set the following properties in the `frontity.settings.js`:

- `state.googleTagManagerAnalytics.containerId`: to specify just one _container ID_
- `state.googleTagManagerAnalytics.containerIds`: to specify a list of _container ID's_

```javascript
export default {
  packages: [
    {
      name: "@frontity/google-tag-manager-analytics",
      state: {
        googleTagManagerAnalytics: {
          containerId: "GTM-BCDFGHJ",
        },
      },
    },
  ],
};
```

```javascript
export default {
  packages: [
    {
      name: "@frontity/google-tag-manager-analytics",
      state: {
        googleTagManagerAnalytics: {
          containerIds: ["GTM-BCDFGHJ", "GTM-HJSFDUF"],
        },
      },
    },
  ],
};
```

## Usage

This `@frontity/google-tag-manager-analytics` package can co-exist with some other `analytics` packages. Once we have properly installed and configured these `analytics` packages, their actions will be centralized by the `analytics` namespace

- `actions.analytics.pageview()` will take into account settings in `state.analytics.pageviews`
- `actions.analytics.event()` will take into account settings in `state.analytics.events`

> Read more [here](./#how-to-use) about how to use Analytic packages

### `actions.analytics.pageview`

If `@frontity/google-tag-manager-analytics` is configured configured and enabled for _pageviews_ in `state.analytics.pageviews`, every time a link changes \(or every time `action.router.set(link)` is launched\) a tracking for that page will be sent to Google Tag Manager.

#### Sending `pageviews` to Google Analytics via Google Tag Manager

In order to send pageviews to Google Analytics through Google Tag Manager in a Frontity project you have to create a Trigger listening to a **custom event named `pageview`**, instead of the _default `pageview`_ \([the default Event Type `pageview`, doesn't work properly in Isomorphic Apps](https://github.com/frontity/docs/issues/262#issuecomment-822417769)\). This custom event is automatically send by `@frontity/google-tag-manager-analytics package`

**Trigger**

![](https://frontity.org/wp-content/uploads/2021/04/google-tag-manager-custom-event.png)

**Tag**

![](https://frontity.org/wp-content/uploads/2021/04/google-tag-manager-tag.png)

{% hint style="info" %}
In the [Feature Discussion of this package](https://community.frontity.org/t/google-tag-manager-package/1400) you have [the format sent internally](https://community.frontity.org/t/google-tag-manager-package/1400/18) just in case you need it for your [own customizations](https://github.com/frontity/docs/issues/262#issuecomment-822426997)
{% endhint %}

### `actions.analytics.event`

If `@frontity/google-tag-manager-analytics` is configured and enabled for _events_ in `state.analytics.events`, every time you call the method `actions.analytics.event()` from any of your React components, the proper tracking info will be sent to Google Tag Manager.

The `actions.analytics.event()` must receive an event object with the following properties.

| Name          | Type   | Required | Description                                                                         |
| :------------ | :----- | :------- | :---------------------------------------------------------------------------------- |
| **`name`**    | string | yes      | The value of this property is mapped to the `event` field of the object sent to GTM |
| **`payload`** | object | yes      | Event payload.                                                                      |

You can add any info you want in the `payload` object.

These values will be transfomed \(by this package\) into the proper format before sending the data to Google Tag Manager
