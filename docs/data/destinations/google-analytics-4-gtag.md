---
title: Google Analytics 4 (Web) Event Streaming
description: Amplitude CDP's Google Analytics 4 (Web) streaming integration enables you to forward your Amplitude events and users straight to Google Analytics 4 (Web) with just a few clicks.
---

Amplitude CDP's Google Analytics 4 (Web) streaming integration enables you to forward your Amplitude events and users straight to Google Analytics 4 (Web) with just a few clicks.

!!!note "Choose the correct Google Analytics 4 destination"

      Google Analytics 4 (Web) destination works with a web application instrumented with Google Tag (gtag.js). If you are working with an iOS or Android mobile application using Firebase, set up a [Google Analytics 4 (iOS/Android)](../google-analytics-4-firebase) destination instead.

!!!note "BigQuery Import for GA4 (Google Analytics 4) Beta"

    Amplitude is working on a beta version of BigQuery Import for GA4. Contact [dwh+GA4beta@amplitude.com](mailto:dwh+GA4beta@amplitude.com) to learn more. For more information about importing BiqQuery data in to Amplitude, see the [BigQuery Source documentation](/data/sources/bigquery).

## Use cases

When you send events from Amplitude to Google Analytics 4, you enrich Google Analytics 4's data collection capabilities, deepen understanding of user journeys, and integrate product and marketing insights. This approach optimizes user acquisition and retention, and enhances the overall user experience. For more information, see Amplitude's blog post [GA4 as an Amplitude CDP Destination: Hybrid Tracking Method for Full User Journey Analysis](https://amplitude.com/blog/GA4-amplitude-hybrid-tracking) that walks through the end-to-end use case for this Google Analytics 4 streaming integration.

## Setup

### Prerequisites

To configure streaming from Amplitude to Google Analytics 4 (Web), you need the following information from Google Analytics 4 (Web).

- **Google Analytics 4 Measurement ID**: The measurement ID associated with a Google Analytics stream. See the [Google documentation](https://developers.google.com/analytics/devguides/collection/protocol/ga4/sending-events?client_type=gtag#required_parameters) for help locating your measurement ID.
- **Google Analytics 4 Measurement Protocol API Secret**: The measurement protocol API secret used for authentication. See the [Google documentation](https://developers.google.com/analytics/devguides/collection/protocol/ga4/sending-events?client_type=gtag#required_parameters) for help generating an API secret.

### Create a new sync

1. In Amplitude Data, click **Catalog** and select the **Destinations** tab.
2. In the Event Streaming section, click **Google Analytics 4 (Web)**.
3. Enter a sync name, then click **Create Sync**.

### Enter credentials

1. Enter your **Google Analytics 4 Measurement ID**.
2. Enter your **Google Analytics 4 Measurement Protocol API Secret**.

### Configure mappings

_This applies to both event and user forwarding. Transformed user properties aren't supported._

1. Select an Amplitude user property that corresponds to your [Google Analytics 4 **Client ID**](https://developers.google.com/analytics/devguides/collection/protocol/ga4/reference?client_type=gtag#payload_post_body), from the left dropdown.
2. (optional) Map an Amplitude user property to [Google Analytics 4 **User ID**](https://support.google.com/analytics/answer/9213390).
      1. Select an Amplitude user property that corresponds to your Google Analytics 4 **User ID**, from the left dropdown.
      2. Select **User ID**, from the corresponding right dropdown.

### Configure event forwarding

Under **Send Events**, make sure the toggle is enabled ("Events are sent to Google Analytics 4") if you want to stream events to Google Analytics 4. When enabled, events are automatically forwarded to Google Analytics 4 when they're ingested in Amplitude. Events aren't sent on a schedule or on-demand using this integration.

1. In **Select and filter events** choose which events you want to send. Choose only the events you need in Google Analytics 4. _Amplitude sets `non_personalized_ads` to `true` for all events events. Transformed events aren't supported._

    !!!warning "Events for non-Google Analytics 4 users cannot be streamed"

        Google Analytics 4 requires that all events have a Google Analytics 4 **Client ID** present. If you have selected any events to send to Google Analytics 4 that may not have a **Client ID**, add a filter to send only events where the **Client ID** is present. Otherwise, your delivery metrics may be affected.

        ![Setting up a filter for anonymous users on events](/../assets/images/streaming-anonymous-users-filter.png)

2. (optional) In **Select additional properties**, select any more event and user properties you want to send to Google Analytics 4. If you don't select any properties here, Amplitude doesn't send any. These properties are sent to Google Analytics 4 as [Google Analytics 4 parameters](https://developers.google.com/analytics/devguides/collection/protocol/ga4/reference?client_type=gtag#payload_post_body). _Transformed event properties and transformed user properties aren't supported._

### Configure user forwarding

To stream user and property updates to Google Analytics 4, enable **Send Users**. This setting creates or updates users in Google Analytics 4 when you update them in Amplitude with the [HTTP V2 API](/analytics/apis/http-v2-api/) or [Identify API](/analytics/apis/identify-api/). This integration doesn't support scheduled or on-demand updates.

You can optionally select user properties to send to Google Analytics 4 in the **Select additional properties** field. Amplitude sends only the properties you select and only when one of them is updated. Amplitude sends these properties as [Google Analytics 4 User Properties](https://developers.google.com/analytics/devguides/collection/protocol/ga4/user-properties?client_type=gtag). _This integration doesn't support transformed user properties_.

### Enable sync

When satisfied with your configuration, at the top of the page toggle the **Status** to "Enabled" and click **Save**.

--8<-- "includes/debug-delivery-metrics.md"
