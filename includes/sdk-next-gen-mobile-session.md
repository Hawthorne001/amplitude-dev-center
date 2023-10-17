Amplitude starts a session when the app is brought into the foreground or when an event is tracked in the background. A session ends when the app remains in the background for more than the time set by `setMinTimeBetweenSessionsMillis()` without any event being tracked. Note that a session will continue for the entire time the app is in the foreground no matter whether session tracking is enabled by `configuration.trackingSessionEvents` or `configuration.defaultTracking.sessions` or not. 

When the app enters the foreground, Amplitude tracks a session start, and starts a countdown based on `setMinTimeBetweenSessionsMillis()`. Amplitude extends the session and restarts the countdown any time it tracks a new event. If the countdown expires, Amplitude waits until the next event to track a session end event.

Amplitude doesn't set user properties on session events by default. To add these properties, use `identify()` and `setUserId()`. Amplitude aggregates the user property state and associates the user with events based on `device_id` or `user_id`.

Due to the way in which Amplitude manages sessions, there are scenarios where the SDK works expected but it may appear as if events are missing or session tracking is inaccurate:

* If a user doesn't return to the app, Amplitude does not track a session end event to correspond with a session start event.
* If you track an event in the background, it's possible that Amplitude perceives the session length to be longer than the user spends on the app in the foreground.
* If you modify user properties between the last event and the session end event, the session end event reflects the updated user properties, which may differ from other properties associated with events in the same session. To address this, use an enrichment plugin to set `event['$skip_user_properties_sync']` to `true` on the session end event, which prevents Amplitude from synchronizing properties for that specific event. See [$skip_user_properties_sync](/data/converter-configuration-reference/#skip_user_properties_sync) in the Converter Configuration Reference article to learn more.
