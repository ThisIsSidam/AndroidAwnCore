# AndroidAwnCore

Awesome Notifications Android Core

## Fork notice

This is a fork of [rafaelsetragni/AndroidAwnCore](https://github.com/rafaelsetragni/AndroidAwnCore),
maintained for the [rem-reminds-you](https://github.com/ThisIsSidam/rem-reminds-you) app, forked at
tag `0.12.1`.

**What's different from upstream:** a new `NotificationLayout.QuickTimeGrid` value, which renders a
notification's `actionButtons` as a static 4x2 grid of clickable cells in the expanded view instead
of the OS-limited (~3 visible) native action row. Used to let a reminder notification's postpone
options ("in 10 min", "at 9:30 AM", etc.) be tapped directly from the notification shade, without
opening the app.

Changed/added files (all under `core/src/main/`):
- `java/.../enumerators/NotificationLayout.java` — added the `QuickTimeGrid` enum value.
- `java/.../builders/NotificationBuilder.java` — added `setQuickTimeGridLayout()`, extracted the
  shared `buildActionPendingIntent()` helper (also used by the existing native action-button path,
  `createActionButtons()`, so tap dispatch is identical for both), and a guard so
  `createActionButtons()` skips its native action row for this layout.
- `res/layout/awn_quick_time_grid.xml` — new; the grid's 8 static cells (`awn_quick_time_button_0..7`).

Everything else (channels, permissions, scheduling, other layouts, dismiss tracking) is unmodified
from upstream.

### Keeping this fork in sync with upstream

```
git remote add upstream https://github.com/rafaelsetragni/AndroidAwnCore.git
git fetch upstream
git merge upstream/main   # or: git rebase upstream/main
```

The patch above is intentionally small and localized to reduce merge-conflict surface.

### Consuming this fork locally

The `awesome_notifications` fork's `android/build.gradle` expects this repo to be included as a
Gradle subproject named `:core` by the consuming app, e.g. in the app's `android/settings.gradle`:

```groovy
include ':core'
project(':core').projectDir = new File(rootDir, '../../AndroidAwnCore/core')
```
