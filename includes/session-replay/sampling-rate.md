### Sampling rate

!!!info "Initial sample rate"
    As a new organization in Amplitude, when you instrument the Amplitude Browser SDK and Session Replay using the code snippet, Session Replay defaults to a 100% sample rate to help with validation. After you validate that Session Replay works, decrease the rate as recommended in the following section.

Use the `sampleRate` configuration option to set the percentage of total sessions that Session Replay captures. For example:

```js
// This configuration samples 1% of all sessions
await sessionReplay.init(AMPLITUDE_API_KEY, {
  sampleRate: 0.01
}).promise;
```

!!!tip
    Update sample rate from the Session Replay settings screen in Amplitude. See [The Settings page](https://help.amplitude.com/hc/en-us/articles/235649848-The-Settings-page#h_01HWX78NJWG0ZEYZ2D8Z6CW10P) for more information.

To set the `sampleRate` consider the monthly quota on your Session Replay plan. For example, if your monthly quota is 2,500,000 sessions, and you average 3,000,000 monthly sessions, your quota is 83% of your average sessions. In this case, to ensure sampling lasts through the month, set `sampleRate` to `.83` or lower.

Keep the following in mind as you consider your sample rate:

- When you reach your monthly session quota, Amplitude stops capturing sessions for replay.
- Session quotas reset on the first of every month.
- Use sample rate to distribute your session quota over the course of a month, rather than using your full quota at the beginning of the month.
- To find the best sample rate, Amplitude recommends that you start low, for example `.01`. If this value doesn't capture enough replays, raise the rate over the course of a few days. For ways to monitor the number of session replays captured, see [View the number of captured sessions](https://help.amplitude.com/hc/en-us/articles/20464659456667-See-how-your-customers-use-your-product-with-Session-Replay#h_01HFDAKA1125YYJ59RPK36WAE2).