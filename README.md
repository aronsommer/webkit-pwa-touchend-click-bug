# iOS Home Screen web app: click after a cancelled touchend

On iOS 26.2, a web app opened from the Home Screen sends a `click` after a long press, although the page cancelled the `touchend` with `preventDefault()`. Safari sends no `click` for the same page. On iOS 27.0 the `click` no longer comes.

The page has `user-select: none` and a `click` listener on the pressed element. With `user-select: auto` the `click` does not come.

Demo: https://aronsommer.github.io/webkit-pwa-touchend-click-bug/

WebKit bug: https://bugs.webkit.org/show_bug.cgi?id=326218

## Steps

1. Open the demo in Safari on an iPhone. Long press the grey area for one second, then lift your finger. Result: **PASS**.
2. Share, then Add to Home Screen. Open it from the Home Screen and repeat. Result: **FAIL** on iOS 26.2, **PASS** on iOS 27.0.
3. Tap the button to switch to `user-select: auto` and repeat. Result: **PASS**.

## Expected

No `click`, as in Safari. The Touch Events specification says: "If `touchstart`, `touchmove`, or `touchend` are canceled, the user agent should not dispatch any mouse event that would be a consequential result of the prevented touch event." ([Interaction with Mouse Events and click](https://w3c.github.io/touch-events/#mouse-events))

## What the page does

`index.html` has no dependencies. Once a touch on the grey element has lasted 600 ms, the page adds a `touchend` listener that calls `preventDefault()`. It shows FAIL if a `click` still reaches the element.

## Seen on

iOS and iPadOS 26.2, iPhone and iPad Simulator.

It does not reproduce on iOS and iPadOS 27.0, iPhone and iPad Simulator.
