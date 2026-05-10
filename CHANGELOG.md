# CHANGELOG

## 2026.05.10

### What Changed
Replaced the broken background rendering with a proper multi-monitor implementation borrowed from the KDE breeze SDDM theme. Root element changed from `Rectangle` to `Item`. Background now uses `Repeater { model: screenModel }` so each physical display gets its own background instance, matching the breeze pattern. Work done on branch `breeze-background`.

### Technical Details
- Copied `Background.qml` from `/usr/share/sddm/themes/breeze/` — pure QtQuick, no KDE/Plasma dependencies
- Renamed it to `SceneBackground.qml` to avoid name collision with `SddmComponents 2.0`'s own `Background` type (which would shadow the local file and cause "non-existent property sceneBackgroundImage" errors)
- Background image path passed via `Qt.resolvedUrl("images/background.jpg")` so the URL is resolved relative to `Main.qml`, not `SceneBackground.qml`
- `SddmComponents 2.0` import kept for `TextConstants` only
- PKGBUILD unchanged — `cp -r usr/` picks up `SceneBackground.qml` automatically

### Files Modified
- `usr/share/sddm/themes/edu-simplicity/Main.qml`
- `usr/share/sddm/themes/edu-simplicity/SceneBackground.qml` (new)

### Outstanding
- Real hardware "library import requires a version" error: still undiagnosed — need `ldd /usr/bin/sddm-greeter* | grep libQt` on that machine to confirm Qt version

## 2026.05.09

### What Changed
Ported the theme from Qt5 to Qt6. The original QML used versioned imports (`QtQuick 2.12`, `QtQuick.Controls 2.12`) which fail on Qt6 SDDM builds. Replaced with unversioned imports. Also fixed the background image loading — the `SddmComponents` `Background` component resolves relative paths against its own location (`/usr/lib/qt6/qml/SddmComponents/`), not the theme directory, causing it to always fail. Replaced with a plain `Image` element using a theme-relative path.

### Technical Details
- Qt6 QML modules use unversioned imports (`import QtQuick` with no version suffix); versioned imports are Qt5-only
- `SddmComponents.Background { source: config.background }` silently fails in Qt6 because the relative path `images/background.jpg` is resolved against Background.qml's own directory, not the theme root. Switching to `Image { source: "images/background.jpg" }` in Main.qml resolves correctly because QML resolves relative URLs against the file that references them
- `depends` in PKGBUILD updated from `qt5-quickcontrols2 qt5-quickcontrols` to `qt6-declarative`
- Removed duplicate `makedepends=('git')` line from PKGBUILD

### Files Modified
- `usr/share/sddm/themes/edu-simplicity/Main.qml`
- `usr/share/sddm/themes/edu-simplicity/SimpleControls/Button.qml`
- `usr/share/sddm/themes/edu-simplicity/SimpleControls/ComboBox.qml`

### Outstanding
- Black screen in VirtualBox: Qt6 RHI rendering doesn't work with VirtualBox virtual GPU — not a theme bug, VirtualBox limitation
- "Library import requires a version" on real hardware: real hardware may be running Qt5 SDDM; needs `ldd /usr/bin/sddm-greeter* | grep libQt` to confirm Qt version before diagnosing further
