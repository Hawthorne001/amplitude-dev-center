### Bitmap Capture

!!!info "Bitmap Capture is disabled by default"
    Enabling bitmap capture may increase the amount of CPU and memory used by the Session Replay SDK. This performance will be improved in future releases.

To enable bitmap capture, use one or more of the following

1. Set `internalOptions.bitmapViewFilter=ViewFilter()`. For example, `SessionReplay(internalOptions=InternalOptions(bitmapViewFilter = ViewFilters.ImageViewFilter()))` will capture all `ImageView` views and `Role.Image` elements in Compose. You can define your own capture rules by extending the `ViewFilter` interface and using `matches(mobileNode, view)` to filter what views to capture as bitmaps.
2. In Android XML, add `android:tag="amp-bitmap""` to your view. For example,  `<View android:tag="amp-bitmap"></View>`
3. In Compose layouts, add a `Modifier.ampBitmap()` to the view `modifiers`. For example, `Checkbox(checked = true, modifier = Modifier.ampBitmap())`
