# Changelog

All notable changes to King Info are documented here.

## [1.4.1] — 2026-11-11

### Fixed
- Release build failure: `Cannot access 'weight': it is internal in 'androidx.compose.foundation.layout'` at `MenuScreen.kt`. `weight` is a member function of `ColumnScope`/`RowScope`, not a top-level import — the same mistake as the earlier `stickyHeader` bug. Removed the bad import; `Modifier.weight(1f)` resolves correctly via its scope receiver with no import needed.
- Sensors tab "Show live values for all" toggle row: the label text block had no weight, so on some screens (reported on a Galaxy S9) it pushed the Switch off the visible edge instead of auto-adjusting. The text block now takes a `weight(1f)` so it shrinks to make room for the Switch, matching how every other row in the app is already laid out.

### Changed
- About screen: Privacy Policy and Terms of Use now point to the website's new per-app folder path (`teamexykings.in/King-Info/...`); Visit Website now opens the site's home page.
- Project structure: `index.html`, `privacy-policy.html` and `terms-of-service.html` moved from `web/kinginfo/` to the project root, to match the local folder layout.

## [1.4.0] — 2026-11-09

### Changed
- Menu grid layout: category icons are now sized to fill their tile instead of sitting small in the corner (list layout was already correct and is unchanged).
- About screen's pulsing icon glow now traces the icon box's own rounded-square shape instead of a circular halo — it previously showed as a glow peeking out specifically at the 4 rounded corners, since a circular halo doesn't match a rounded-square box evenly.
- About screen now shows "Private by design: No ads, no tracking, works offline" below the version number.

### Verified (no change needed)
- Website/privacy/terms URLs already point to the correct `teamexykings.in/kinginfo/` path — nothing was redirecting to an old path.

## [1.3.2] — 2026-11-08

### Fixed
- Pre-emptive fix for a release build failure (`Unresolved reference: stickyHeader`) that King Color Picker hit on the same code pattern: `FaqScreen.kt` had the same incorrect `import androidx.compose.foundation.lazy.stickyHeader` line. `stickyHeader` is a member function of `LazyListScope`, not a top-level import, so the bad import has been removed — it's called directly inside the `LazyColumn` block.

## [1.3.1] — 2026-11-07

### Fixed
- About screen: content wasn't scrollable, so on shorter screens the lower buttons (Credits, Share, etc.) could be pushed off-screen with no way to reach them. The screen now scrolls.
- About screen buttons: long labels no longer wrap to a second line — they stay on one line and shrink/ellipsize gracefully instead.

## [1.3.0] — 2026-11-05

### Fixed
- Release build error (`Unresolved reference: rememberSaveable`) in the Sensors tab — wrong import package, now corrected.
- Devices tab: ultra-wide (and telephoto, where present) rear camera lenses were missing — Android hides individual physical lenses behind one combined camera entry by default, so King Info now also queries each phone's physical lens list directly and lists every lens separately, labeled by focal length (Ultra-Wide / Wide / Telephoto).

### Added
- FAQ is now split into clearly labeled categories with a search bar at the top, so any question can be found instantly instead of scrolling a flat list.
- About → Credits now groups every tool by category (Language, UI Framework, Architecture & Libraries) instead of one flat list.
- About screen's app icon now has a soft pulsing glow behind it.

## [1.2.3] — 2026-11-01

### Changed
- compileSdk/targetSdk raised to 36 (was 35) to match the Play Console upload requirement.
- Sensors tab: added a "Show live values for all" toggle at the top (off by default, same as before) — switching it on starts every sensor's live listener at once instead of tapping each one individually. Tapping an individual sensor still works as before when the toggle is off.

## [1.2.2] — 2026-11-01

### Changed
- FAQ expanded significantly — from 9 to 28 questions, now grouped by topic (privacy & permissions, system/Android, CPU & thermal, display/GPU, network, devices, sensors, codecs & system files, sharing & general) covering nearly every platform behavior and limit in the app.

## [1.2.1] — 2026-11-01

### Changed
- Menu defaults to a **list view** (icon, label, chevron) instead of the grid of boxes — the grid felt off, per feedback.
- Added a **view-switch icon** next to Share in the menu's top bar — toggles between list and grid (box) layouts, icon shows the view you'll switch *to*.
- Updated the launcher icon glyph (and the two Play Store assets) to white, matching the app's pure-black/white-accent theme.

## [1.2.0] — 2026-11-01

### Changed
- Menu grid is now 3 columns of compact square tiles instead of 2 larger ones — all 12 sections fit on one screen without scrolling.
- Section screens are now swipeable left/right between each other, in the same A–Z order as the menu, instead of only being reachable by returning to the menu each time (Sensors, Codecs and System Files are part of the same swipe sequence).
- About → Credits rewritten: no "built from scratch" framing, clear "Built with" list (Kotlin, Jetpack Compose, Material Design 3, specific AndroidX libraries), and a new "Data source" note explaining every reading comes from Android's own public system APIs, no third-party data source.

## [1.1.0] — 2026-11-01

### Changed
- Theme is now a single fixed **pure AMOLED black (#000000) with a white accent** — removed the Light ("White") theme, System theme, and the accent-color picker entirely. There's nothing left to configure, so the Settings screen and its DataStore-backed preferences were removed too.
- Menu top bar: removed the gear/Settings icon (nothing to configure now) in favor of a simple **"i" info icon** that opens About directly.
- Menu layout: switched from a vertical list to a **2-column grid of rectangular tiles**, in plain **A–Z order** rather than any reference app's section ordering.
- About screen: removed the **Donate** and **Support/Contact** buttons.
- About → Credits now properly lists the tools King Info is built with (Kotlin, Jetpack Compose, Material Design 3, AndroidX libraries) — built from scratch, no reference tool named or credited.
- Removed the **Copy** icon everywhere, and removed the per-section Share icon from every detail screen. There's now a single **Share** action, on the menu's top bar, which builds the full report across every tab and shares it as **one real .txt file** (via a FileProvider attachment, not pasted text) — organized with a clear header per section, in menu order, with the device name and a generated timestamp at the top.

## [1.0.0] — 2026-10-31

### Added
- Initial build: Menu + 12 sections (System, CPU, Display, Network, Battery, Android, Devices, Thermal, Sensors, Codecs, Directories, System Files).
- Per-screen and full-report copy/share as text.
- Live CPU core frequencies (sysfs, graceful fallback), live sensor values (tap to watch update), live thermal zone temperatures.
- GPU summary (vendor/renderer/version) via an off-screen GL context — no visible surface needed.
- Camera specs per lens via CameraCharacteristics (no CAMERA permission required just to read them).
- System Files preview: tap a friendly-named `/proc` path to see its content or a clear "not accessible" message.
- Optional, clearly-explained permissions: coarse location (real Wi-Fi SSID), phone state (network type), Bluetooth connect on Android 12+ (adapter name) — every reader degrades gracefully when declined.
- Pitch-black theme with accent-color picker (shared King Ecosystem theming, electric-blue default), Dark/White/System modes — built with `surfaceTint = Color.Transparent` from the start (the fix King Battery needed after the fact).
- About screen matching the King Ecosystem pill-button layout, rendered from the plain foreground vector rather than the adaptive-icon mipmap (the crash King Battery hit was avoided here from the start); Donate button; FAQ covering every platform limit (no IMEI, no Apps list, no raw Vulkan dump, Wi-Fi SSID restriction, sysfs access variability, no fingerprint sensor model, root-check reliability).
- Offline-first: no internet permission required for any core feature.

### Scoped out by design (see README "Known platform limits")
- Apps section (QUERY_ALL_PACKAGES Play Store risk)
- Full raw Vulkan limits/extensions dump (readable GPU summary instead)
