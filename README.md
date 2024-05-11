## FENLES PLAUSIBLE ANALYTICS FORK

This is a custom version for [FENLES](https://fenles.com) that removes the localhost header to allow having geographic information on Plausible.

`request.headers.set('X-Forwarded-For', '127.0.0.1');`
so that geomaps works as otherwise the IP sent was 127...

## Features

Send pageviews and custom events to Plausible Analytics so you have privacy friendly
analytics.

This will log following information:

- A pageview or event
- Current page in the app (e.g. Homescreen)
- Operating System
- OS Version
- Referrer
- Screen width

Following information is generated by the Plausible server:

- Country
- Current time

## Plausible setup

Add a new site in plausible with your app name:

![Plausible Screenshot](https://github.com/bostrot/flutter_plausible_analytics/blob/main/plausible_screenshot.png?raw=true)

### Android

Using plausible on Android in release mode required internet permission in manifest

`<uses-permission android:name="android.permission.INTERNET" />`

## Basic Usage

For a simple pageview:

```dart
const String serverUrl = "https://plausible.io";
const String domain = "yourapp.com";

final plausible = Plausible(serverUrl, domain);
final event = plausible.event(); // click event
```

Or for a custom event (e.g. a conversion):

```dart
const String serverUrl = "https://plausible.io";
const String domain = "yourapp.com";

final plausible = Plausible(serverUrl, domain);
final event = plausible.event(
        name: 'conversion',
        page: 'homescreen',
        referrer: 'referrerPage',
        props: {
                'app_version': 'v1.0.0',
                'app_platform': 'windows',
                'app_locale': 'de-DE',
                'app_theme': 'darkmode',
});
```

Disable analytics (might be useful if a user opts out):

```dart
plausible.enabled = false;
```

You can also use a custom user agent but that is not recommended as
the default one already puts in the current Operation System & Version.

Also check [Plausible API docs Events API](https://plausible.io/docs/events-api) which this package uses.

## Usage with Navigator

```dart
const String serverUrl = "https://plausible.io";
const String domain = "yourapp.com";

final plausible = Plausible(serverUrl, domain);

MaterialApp(
  navigatorObservers: [
    PlausibleNavigatorObserver(plausible),
  ],
  home: HomeScreen(),
);
```
