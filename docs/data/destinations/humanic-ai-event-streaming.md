---
title: Humanic AI Event Streaming
description: Stream Amplitude events to Humanic AI
---

[Humanic AI](https://www.humanic.ai/) is an Agentic Marketing Platform that helps you unlock growth by using AI agents. 

!!!Tip

    This integration is maintained by Humanic AI. Email [care@humanic.ai](mailto:care@humanic.ai) for support with this integration. 

## Considerations

- You must enable this integration in each Amplitude project you want to use it in.
- You need an Humanic.ai account to enable this integration.
- Amplitude sends selected user, event, and group properties along with the event.

## Setup

### Humanic AI setup

1. Log in to your [Humanic AI account](https://www.google.com/url?q=https://dashboard.humanic.ai/&sa=D&source=docs&ust=1718221705159195&usg=AOvVaw35JyMQx4p7cxsiEOBh7gIX).
2. Navigate to the **Profile** section by clicking on the name icon located on top right.
3. Go to the **API Key** tab and click on the **Create** button.
4. Copy the API key as this will be required to setup the integration in Amplitude.

### Amplitude setup

1. In Amplitude, navigate to **Data Destinations**, then find **Humanic AI - Event Stream**.
2. Enter a sync name, then click **Create Sync**.
3. Toggle Status from **Disabled** to **Enabled**.
4. Paste your **Humanic.ai API key** that you copied from Humanic dashboard.
5. Toggle the **Send events filter** to select the events to send. Humanic AI recommends choosing the events that are most important to your use case.
6. Use the **Event Properties** filter to select which Event Properties you would like to send.
7. When finished, enable the destination and **Save**.
