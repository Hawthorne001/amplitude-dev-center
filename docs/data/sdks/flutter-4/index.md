---
title: Flutter SDK 4.0 (Beta)
description: The Amplitude Flutter SDK Installation & Quick Start guide.
icon: simple/flutter
---

This is the official documentation for the Amplitude Analytics Flutter SDK. The Flutter SDK lets you send events from your Flutter application to Amplitude.

!!!beta "Flutter SDK Resources (Beta)"
    [:material-github: GitHub](https://github.com/amplitude/Amplitude-Flutter/tree/beta) · [:material-code-tags-check: Releases](https://github.com/amplitude/Amplitude-Flutter/releases) · [:material-book: Example](https://github.com/amplitude/Amplitude-Flutter/tree/beta/example)

--8<-- "includes/no-ampli.md"

--8<-- "includes/size/flutter.md"

## Getting started

### Install the dependency

1. Go to the `pubspec.yaml` file and add Amplitude SDK as a dependency.

    ```yml
    dependencies:
        amplitude_flutter: ^4.0.0-beta.1
    ```

2. Run `flutter pub get` in the terminal to install the SDK.

#### iOS installation

Add `platform :ios, '10.0'` to your Podfile.

To enable Bitcode, follow Flutter's [documentation](https://github.com/flutter/flutter/wiki/Creating-an-iOS-Bitcode-enabled-app).

## Use

### Initialization

Before you instrument your application, initialize the SDK with your Amplitude project's API key.

```dart
import 'package:amplitude_flutter/amplitude.dart';
import 'package:amplitude_flutter/configuration.dart';
import 'package:amplitude_flutter/events/base_event.dart';

class YourClass {
  Future<void> exampleForAmplitude() async {
    // Create and initailize the instance
    final Amplitude analytics = Amplitude(Configuration(
        apiKey: 'YOUR-API-KEY',
    ));

    // Wait until the SDK is initialized
    await amplitude.isBuilt;

    // Track an event   
    amplitude.track(BaseEvent(
        eventType: 'BUTTON_CLICKED', 
        eventProperties: {'Hover Time': '100ms'},
    ));
    
    // Send events to the server
    amplitude.flush()
  }
}
```

### Configuration

???config "Configuration Options for all platforms"
    | <div class="big-column">Name</div>  | Description | Default Value |
    | --- | --- | --- |
    | `apiKey` | `String`. The apiKey of your project. | `null` |
    | `flushQueueSize` | `int`. SDK attempts to upload once unsent event count exceeds the event upload threshold or reaches `flushIntervalMillis` interval.  | `30` |
    | `flushIntervalMillis` | `int`. The amount of time SDK attempts to upload the unsent events to the server or reaches the `flushQueueSize` threshold. The value is in milliseconds. | `30000` |
    | `instanceName` | `String`. The name of the instance. Instances with the same name shares storage and identity. For isolated storage and identity use a unique `instanceName` for each instance.  | `$default_instance`|
    | `optOut` | `bool`. Opt the user out of tracking. | `false` |
    | `logLevel` | `LogLevel` The log level. `LogLevel.off`, `LogLevel.error`, `LogLevel.warn`, `LogLevel.log`, `LogLevel.debug` | `LogLevel.warn` | 
    | `minIdLength` | `int`. The minimum length for user id or device id. | `5` |
    | `partnerId` | `int`. The partner id for partner integration. | `null` |
    | `flushMaxRetries` | `int`. Maximum retry times.  | `5` |
    | `useBatch` | `bool`. Whether to use batch API. | `false` |
    | `serverZone` | `ServerZone`. `ServerZone.us` or `ServerZone.eu`. The server zone to send to. Adjust server URL based on this config. | `ServerZone.us` |
    | `serverUrl` | `String`. The server URL events upload to. | `https://api2.amplitude.com/2/httpapi` |
    | `minTimeBetweenSessionsMillis` | `int`. The amount of time for session timeout. The value is in milliseconds. | `300000` |
    | `defaultTracking` | `DefaultTrackingOptions`. Options to control the default events tracking. | Check [Tracking default events](#tracking-default-events). |
    | `trackingOptions` | `TrackingOptions`. Options to control the values tracked in SDK. | `enable` |

???config "Configuration Options for all Android and iOS only"
    | <div class="big-column">Name</div>  | Description | Default Value |
    | --- | --- | --- |
    | `enableCoppaControl` | `bool`. Whether to enable COPPA control for tracking options. | `false` |
    | `flushEventsOnClose` | `bool`. Flush unsent events on app close. | `true` |
    | `identifyBatchIntervalMillis` | `int`. The amount of time SDK attempts to batch intercepted identify events. The value is in milliseconds| `30000` |
    | `migrateLegacyData` | `bool`. Whether to migrate maintenance Android SDK and maintenance iOS SDK data (events, user/device ID). Learn more at the configuration section of the underlying [Kotlin SDK](/data/sdks/android-kotlin/#configuration) and [Swift SDK](/data/sdks/ios-swift/#configuration). | `true`|

???config "Configuration Options for Android only"
    | <div class="big-column">Name</div>  | Description | Default Value |
    | --- | --- | --- |
    | `locationListening` | `bool`. Whether to enable Android location service. Learn more [here](/data/sdks/android-kotlin/#location-tracking).| `true` |
    | `useAdvertisingIdForDeviceId` | `bool`. Whether to use advertising id as device id. Check [here](/data/sdks/android-kotlin/#advertiser-id) for required module and permission. | `false` |
    | `useAppSetIdForDeviceId` | `bool`.  Whether to use app set id as device id. Check [here](/data/sdks/android-kotlin/#app-set-id) for required module and permission. | `false` |

<!-- ???config "Configuration Options for Web only"
    | <div class="big-column">Name</div>  | Description | Default Value |
    | --- | --- | --- |
    |`appVersion` | `String`. Sets an app version for events tracked. This can be the version of your application. For example: "1.0.0" | `undefined` | -->
    
--8<-- "includes/sdk-ts/shared-batch-configuration.md"

```dart
final Amplitude analytics = Amplitude(Configuration(
    apiKey: 'YOUR-API-KEY',
    flushIntervalMillis: 50000,
    flushQueueSize: 20,
));
```

--8<-- "includes/sdk-quickstart/quickstart-eu-data-residency.md"

```dart
final Amplitude analytics = Amplitude(Configuration(
    apiKey: 'YOUR-API-KEY',
    serverZone: ServerZone.eu,
));
```

### Track

Events represent how users interact with your application. For example, "Song Played" may be an action you want to track.

```dart
amplitude.track(BaseEvent('Song Played'));
```

You can also optionally include event properties.

```dart
amplitude.track(BaseEvent('Song Played', eventProperties: {'title': 'Happy Birthday'}));
```

Refer to the [BaseEvent](https://github.com/amplitude/Amplitude-Flutter/blob/beta/lib/events/base_event.dart) interface for all available fields.

### Identify

--8<-- "includes/sdk-identify-reduction.md"

```dart
final Identify identify = Identify()
    ..set('color', 'green')
amplitude.identify(identify)
```

### Tracking default events

The SDK can track more default events. You can configure it to track the following events by default:

- Sessions
- App lifecycles
- Deep links (now only available on Android)

???config "Tracking default events options"
    | <div class="big-column">Name</div> | Type | Default Value | Description |
    |-|-|-|-|
    `config.defaultTracking.sessions` | Optional. `bool` | `true` | Enables session tracking. If value is `true`, Amplitude tracks session start and session end events otherwise, Amplitude doesn't track session events. When this setting is `false`, Amplitude tracks `sessionId` only.<br /><br />See [Tracking sessions](#tracking-sessions) for more information.|
    `config.defaultTracking.appLifecycles` | Optional. `bool` | `false` | Enables application lifecycle events tracking. If value is `true`, Amplitude tracks application installed, application updated, application opened, and application backgrounded events.<br /><br />Event properties tracked includes: `[Amplitude] Version`,<br /> `[Amplitude] Build`,<br /> `[Amplitude] Previous Version`, `[Amplitude] Previous Build`, `[Amplitude] From Background`<br /><br />See [Tracking application lifecycles](#tracking-application-lifecycles) for more information.|
    `config.defaultTracking.deepLinks` | Optional. `bool` | `false` | Only available on Android. It enables deep link tracking. If value is `true`, Amplitude tracks deep link opened events.<br /><br />Event properties tracked includes: `[Amplitude] Link URL`, `[Amplitude] Link Referrer`<br /><br />See [Tracking deep links](#tracking-deep-links) for more information.|

You can enable Amplitude to start tracking all events mentioned above, use the code sample below. Otherwise, you can omit the configuration to keep only session tracking enabled.

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions.all()
  )
);
```

!!!warn
    Amplitude may add more events in a future version, and this configuration enables tracking for those events as well.

Similarly, you can prevent Amplitude from track default events with the following code sample:

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions.none()
  )
);
```

You can also customize the tracking with `DefaultTrackingOptions`.

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions(
      sessions: false,
      appLifecycles: true,
      deepLinks: true,
    )
  )
);
```

#### Tracking sessions

When you set `configuration.defaultTracking.sessions: true`, you instruct Amplitude to track session events.

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions(
      sessions: true,
    )
  )
);
```

#### Tracking application lifecycles

When you set `configuration.defaultTracking.appLifecycles` to `true`, Amplitude tracks application lifecycle events.

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions(
      appLifecycles: true,
    )
  )
);
```

After enabling this setting, Amplitude tracks the following events:

- `[Amplitude] Application Installed`, this event fires when a user opens the application for the first time right after installation.
- `[Amplitude] Application Updated`, this event fires when a user opens the application after updating the application.
- `[Amplitude] Application Opened`, this event fires when a user launches or foregrounds the application after the first open.
- `[Amplitude] Application Backgrounded`, this event fires when a user backgrounds the application.

#### Tracking deep links

!!!note
    Deep link tracking is available on Android.

When you set `configuration.defaultTracking.deepLinks` to `true`, Amplitude tracks events related to deep links in your application.

```dart
Amplitude(
  Configuration(
    apiKey: 'YOUR-API-KEY',
    defaultTracking: DefaultTrackingOptions(
      deepLinks: true,
    )
  )
);
```

After enabling this setting, Amplitude tracks the `[Amplitude] Deep Link Opened` event with the URL and referrer information.

### User groups

--8<-- "includes/editions-growth-enterprise-with-accounts.md"

--8<-- "includes/groups-intro-paragraph.md"

!!! example
    If Joe is in 'orgId' '15', then the `groupName` would be '15'.

    ```dart
    // set group with a single group name
    amplitude.setGroup('orgId', '15');
    ```

    If Joe is in 'sport' 'tennis' and 'soccer', then the `groupName` would be '["tennis", "soccer"]'.

    ```dart
    // set group with multiple group names
    amplitude.setGroup('sport', ['tennis', 'soccer']);
    ```

--8<-- "includes/event-level-groups-intro.md"

```dart
amplitude.track(BaseEvent('event type',
    eventProperties: {'event property': 'event property value'},
    groups: {'ordId': '15'}));
```

### Group identify

--8<-- "includes/editions-growth-enterprise-with-accounts.md"

--8<-- "includes/group-identify-considerations.md"

```dart
final groupType = 'plan';
final groupName = 'enterprise';

final identify = Identify().set('key', 'value');
amplitude.groupIdentify(groupType, groupName, identify);
```

### Track revenue

--8<-- "includes/revenue-intro-paragraph.md"

```dart
final revenue = Revenue()
  ..productId = 'com.company.productId'
  ..price = 3.99
  ..quantity = 3;
amplitude.revenue(revenue);
```

--8<-- "includes/revenue-properties.md"

### Custom user ID

If your app has its login system that you want to track users with, call `setUserId` at any time.

```dart
amplitude.setUserId('user@amplitude.com');
```

### Custom device ID

You can assign a new device ID using `deviceId`. When setting a custom device ID, make sure the value is sufficiently unique. Amplitude recommends using a UUID.

```dart
amplitude.setDeviceId('your-unique-device-id');
```

### Reset when user logs out

--8<-- "includes/sdk-reset-when-user-logs-out-intro.md"

```dart
amplitude.reset();
```
