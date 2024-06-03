---
title: Web Experimentation Overview
description: Overview of Amplitude's web experimentation platform.
toplevel: true
---

Amplitude's web experimentation provides a no-code solution running experiments on the web. [Copy the script snippet](#implement-the-script-snippet) for your Amplitude project into your site and begin running experiments immediately.

!!!beta "Web Experimentation is in Beta"
    - **URL Redirect** tests are in open *Beta*.

## Implement the script snippet

To implement Amplitude's web experimentation, copy and paste the standalone Amplitude experiment script into your website. Paste the script into the `<head>` element of your site as high up as possible to avoid flickering.

This script snippet tracks [experiment events](../general/experiment-event-tracking.md) through the Amplitude Analytics SDK installed on your site. Replace `API_KEY` with your project's API key.

=== "US Data Center"

    ```html
    <script src="https://cdn.lab.amplitude.com/web/v1/script/API_KEY.js"><script>
    ```

=== "EU Data Center"

    ```html
    <script src="https://cdn.lab.eu.amplitude.com/web/v1/script/API_KEY.js"></script>
    ```

!!!info "Install Amplitude Analytics"
    You must install Amplitude Analytics on your website to enable [Experiment event tracking](../general/experiment-event-tracking.md) to Amplitude for analysis. Install the analytics SDK using a [script tag](https://github.com/amplitude/Amplitude-TypeScript/tree/main/packages/analytics-browser#installing-via-script-loader) or using your preferred [package manager](https://www.docs.developers.amplitude.com/data/sdks/sdk-quickstart/#install-the-dependency) (for example npm, yarn etc.)

## Web vs feature experimentation

Web experimentation builds off of Amplitude's end-to-end [feature experimentation platform](../index.md) to enable no-code experimentation on the web, but differs in a few key ways:

| Web Experimentation | Feature Experimentation |
| --- | --- |
| Requires implementation using Experiment script snippet | Generally implemented using a SDK or an API. |
| Enables no-code experimentation. | Requires engineering work to implement features and flags. |
| Only supported on Web platforms. | Supports on any platform or system. |

**Use web experimentation if...**

* You want to drive growth or other outcomes on a web based platform.
* You have limited engineering resources available implement and support feature flags.

**Use feature experimentation if...**

* You want to experiment or release arbitrary features on web or non-web (mobile, server, etc.) platforms.
* You work on a product team with engineering resources available.
