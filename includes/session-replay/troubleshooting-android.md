## Troubleshooting

For more information about individual statuses and errors, see the [Session Replay Ingestion Monitor](/session-replay/ingestion-monitor).

### Styles, fonts, drawables, images don't appear in replay

(alpha) When Amplitude captures a replay, it doesn't transfer or store styles or other static assets that are part of your application. 

The following are not supported at this time:

- Images
- Drawables
- Fonts
- Custom styles

### View components don't appear in replay

(alpha) Session replay is in alpha and does not currently support all Android View components.

| View Feature              | Support Level | Notes                                                                                                        |
|---------------------------|---------------|--------------------------------------------------------------------------------------------------------------|
| Layout: position          | Full          |                                                                                                              |
| Layout: dimensions        | Full          |                                                                                                              |
| Layout: padding           | Partial       | Broken for some text views                                                                                   |
| Layout: insets            | Partial       | Buttons are supported                                                                                        |
| View: background color    | Full          |                                                                                                              |
| View: background drawable | In progress   |                                                                                                              |
| View: background radius   | Full          |                                                                                                              |
| View: scrolling           | Partial       |                                                                                                              |
| View: animations          | Partial       | Any animations adding/removing views are supported.                                                          |
| Images: placeholders      | Full          |                                                                                                              |
| Images: bitmaps           | In progress   |                                                                                                              |
| Text: alignment           | Partial       |                                                                                                              |
| Text: size                | Partial       |                                                                                                              |
| Text: color               | Full          |                                                                                                              |
| Text: font                | In progress   |                                                                                                              |
| Text: fontFamily          | Full          |                                                                                                              |
| Text: all caps            | None          | Issue in Android SDK. `isAllCaps` doesn't work.                                                              |
| Text: compound drawables  | Partial       | Left/right drawables added to text views have placeholders. Top/bottom drawables are not currently supported |
| Touch: up                 | Full          |                                                                                                              |
| Touch: down               | Full          |                                                                                                              |
| Touch: move               | In progress   |                                                                                                              |
| WebView                   | In progress   |                                                                                                              |
| MapView                   | In progress   |                                                                                                              |
| Canvas Views              | In progress   | Views that draw to a custom Canvas are not currently supported.                                              |
