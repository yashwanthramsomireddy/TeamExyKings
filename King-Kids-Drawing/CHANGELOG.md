# Changelog

All notable changes to King Kids Drawing are documented here.

## [Unreleased] - build-fix + feature round (latest)

### Fixed
- Fixed all 5 Gradle/Kotlin compile errors from the `:app:assembleRelease` build log: a
  `strokeWidth` parameter shadowing bug in `ColoringPageLibrary.kt`, a bad `weight` import in
  `ColorPicker.kt` and `ThemeOptionCard.kt`, a missing `FlowRow` experimental opt-in, a missing
  `animateFloat` import in `AboutScreen.kt`, and an `OptionCard` parameter-order bug that broke
  every trailing-lambda call site in `SettingsScreen.kt`
- Canvas screen's Save button now saves the drawing and keeps you on the canvas to keep
  drawing (shows a "Saved" toast), instead of exiting back to Home
- About screen's Website/Privacy/Terms links now point to `https://teamexykings.in/`,
  `https://teamexykings.in/King-Kids-Drawing/privacy-policy.html` and
  `https://teamexykings.in/King-Kids-Drawing/terms-of-service.html`

### Added
- "i" (About) and "?" (FAQ/Help) icons on the Home screen's top bar, next to Settings — About
  and FAQ remain reachable from inside Settings too
- Release signing now reads the upload keystore committed at
  `docs/keystore/KingKidsDrawing-upload.jks` via root `gradle.properties`
  (`KINGKIDSDRAWING_UPLOAD_*`), with `storeFile` resolved from the repo root

### Changed
- Project layout reorganized to match the flat structure: web pages
  (`index.html`/`privacy-policy.html`/`terms-of-service.html`) at the repo root, store assets
  under `docs/store-assets/`, and release notes (`v1.0.0.md`) at the repo root with no
  `docs/release-notes/` subfolder

## [Unreleased] - follow-up to v1.0.0

### Fixed
- About/Website/Privacy/Terms links now point to the GitHub Pages path
  `yashwanthramsomireddy.github.io/TeamExyKings/King-Kids-Drawing/` (previously pointed at a
  `teamexykings.in` path that was never actually deployed there)
- About screen's glowing icon now breathes as a soft halo around the whole rounded-square
  shape, instead of appearing to pulse only at the 4 corners

### Changed
- Unique launcher icon redesign: refined paintbrush glyph with a rainbow bristle trail and a
  small paint-drop accent, matching the About screen's icon
- Web pages moved from `web/kingkidsdrawing/` to `web/King-Kids-Drawing/` to match the actual
  GitHub Pages repo folder

### Added
- `store-assets/icon-512.png` and `store-assets/feature-graphic-1024x500.png` for the Play
  Console listing

## [1.0.0] - 2026-11-09

### Added
- Home/Gallery screen with saved drawings grid, rename, delete, and a "New Drawing" sheet
  (Blank Canvas or Pick a Coloring Page)
- Drawing canvas: round/soft brush, eraser, colour palette + adjustable brush size, flood
  Fill tool, Stamps (Shapes/Nature/Animals, code-drawn), Undo/Redo, Clear
- Coloring Page library: 8 code-drawn outlines across Shapes, Nature and Animals
- Settings: Pitch Black (default) / White / System theme, 5 font choices, accent colour
  picker with 12 presets + custom colour, Backup (single JSON file via system file picker)
  and Restore (validated import)
- About screen: glowing app icon, Visit Website, Privacy Policy, Terms of Use, Credits
  (by tech stack), Support/Contact (prefilled email), Share App
- FAQ screen with category sections and a search bar
- Web pages for teamexykings.in/kingkidsdrawing/ (index, privacy policy, terms of use)

### Known issues / not yet in v1.0.0
- The Canvas screen's Save action saves the drawing to the app's own gallery (Home) and
  updates its thumbnail; a separate "export finished drawing as an image to your phone's
  gallery" / "share as image" action is planned for v1.0.1 and not yet wired up.
- Stamps are placed at a fixed size/rotation on tap; drag-to-resize/rotate a placed stamp
  is not yet available.
- No drag-and-drop reordering or multi-select on the Home gallery yet.
