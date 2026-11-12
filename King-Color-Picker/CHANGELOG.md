# Changelog

All notable changes to King Color Picker are documented here.

## [1.2.1] — 2026-11-11

### Changed
- About screen: Privacy Policy and Terms of Use now point to the website's new per-app folder path (`teamexykings.in/King-Color-Picker/...`); Visit Website now opens the site's home page.
- Project structure: `index.html`, `privacy-policy.html` and `terms-of-service.html` moved from `web/kingcolorpicker/` to the project root, to match the local folder layout.

## [1.2.0] — 2026-11-09

### Changed
- Removed the 20-save cap on Recent Colors — you can now save as many colors as you want; the oldest entries are no longer dropped automatically. FAQ, empty-state text, and the website updated to match.
- About screen's pulsing icon glow now traces the icon box's own rounded-square shape instead of a circular halo — it previously showed as a glow peeking out specifically at the 4 rounded corners, since a circular halo doesn't match a rounded-square box evenly.
- About screen now shows "Private by design: No ads, no tracking, works offline" below the version number.

### Verified (no change needed)
- Website/privacy/terms URLs already point to the correct `teamexykings.in/kingcolorpicker/` path — nothing was redirecting to an old path.

## [1.1.2] — 2026-11-08

### Fixed
- Release build failure: `Unresolved reference: stickyHeader` in `FaqScreen.kt`. `stickyHeader` is a member function of `LazyListScope`, not a top-level import — an incorrect `import androidx.compose.foundation.lazy.stickyHeader` line was removed; it's called directly inside the `LazyColumn` block, no import needed.

## [1.1.1] — 2026-11-07

### Fixed
- About screen: content wasn't scrollable, so on shorter screens the lower buttons (Credits, Share, etc.) could be pushed off-screen with no way to reach them. The screen now scrolls.
- About screen buttons: long labels (e.g. "Share King Color Picker") were wrapping to a second line. They now stay on one line and shrink/ellipsize gracefully instead.

## [1.1.0] — 2026-11-05

### Added
- Live stability percentage shown next to the HEX code while scanning — a rolling-variance read on how steady the last few camera samples have been (shown as "Reading…" until enough samples are collected, then as "NN% stable"). Not a measure of color accuracy against any reference — camera-based color picking has no ground truth to compare against — and the FAQ now explains this plainly.
- FAQ is now split into clearly labeled categories with a search bar at the top.
- About → Credits now groups every tool by category (Language, UI Framework, Camera, Architecture & Libraries) instead of one flat list.
- About screen's app icon now has a soft pulsing glow behind it.

### Changed
- Website updated to describe the new stability readout.

## [1.0.1] — 2026-11-01

### Fixed
- Release build failure: wrong `LocalClipboardManager` import (`androidx.compose.foundation` instead of `androidx.compose.ui.platform`) in `CameraScreen.kt` and `PaletteScreen.kt`.
- Release build failure: `Color(0xFF000000 or (r shl 16) or ...)` caused an Int/Long overload-resolution mismatch against the Compose `Color(Long)` constructor. Replaced with the explicit `Color(red = r, green = g, blue = b)` constructor in both files — no bit-masking, no ambiguity.

### Changed
- compileSdk/targetSdk raised to 36 (was 35) to match the Play Console upload requirement.

## [1.0.0] — 2026-11-01

### Added
- Initial build: live camera color picker with a center-crosshair sampling loop showing HEX, RGB, and HSL simultaneously.
- **Hold** button to freeze the current reading; **Copy** to clipboard.
- **Save** action and a **Recent Colors** screen (rename, delete), capped at the 20 most recent saves.
- **Export palette** — shares all saved colors as one organized `.txt` file via a real file attachment (FileProvider + `ACTION_SEND`).
- Pure AMOLED black theme with a white accent (fixed, no theme picker).
- About screen matching the King Ecosystem layout (Visit Website, Privacy Policy, Terms of Use, FAQ, Credits, Share) — no Donate button.
- FAQ covering privacy, camera permission, picking accuracy/limitations, saving & history behavior, export, and general app info.
- Unique launcher icon and Play Store assets: a white eyedropper glyph with a single rainbow-gradient color drop.
- Web pages (index, privacy policy, terms of service) for `teamexykings.in/kingcolorpicker/`.
