# King Color Picker

A free, offline, ad-free live camera color picker for Android — part of the King Ecosystem, built under the TeamExyKings brand.

Point the camera at anything and read its color off the live feed as HEX, RGB, and HSL — no photo is ever taken or stored, only the colors you choose to save.

## Package

`com.teamexykings.kingcolorpicker`

## Features (v1.0.0)

- **Live camera color picker** — a crosshair over the live preview samples the exact center pixel, ~8 times a second, shown as HEX / RGB / HSL simultaneously.
- **Hold** — freeze the current reading to study or copy it without camera jitter changing the value.
- **Copy** — one tap copies the HEX code to the clipboard.
- **Save / Recent Colors** — tap Save to add the current color to a Recent Colors list (name, HEX/RGB/HSL, and timestamp), capped at the **20 most recent** saves so it stays a quick recheck list, not an unbounded archive. Rename or delete any entry.
- **Export palette** — shares the full Recent Colors list as one neatly organized `.txt` file via the system share sheet (a real file attachment, not pasted text).
- **Pure AMOLED black theme** with a white accent — fixed, no light/system mode, no theme picker.
- **About screen** — same King Ecosystem layout as King Info (Visit Website, Privacy Policy, Terms of Use, FAQ, Credits, Share). No Donate button on this app.
- **FAQ** — covers privacy, camera permission, picking accuracy, saving/history limits, export, and general app info.
- Free forever, no ads, no subscriptions, no login, fully offline (no INTERNET permission).

## Architecture

- Native Kotlin + Jetpack Compose (Material3), Compose Navigation.
- CameraX (`camera-core`, `camera-camera2`, `camera-lifecycle`, `camera-view`) for the live preview; color sampling reads the center pixel of `PreviewView.bitmap` on a polling loop (no `ImageAnalysis`/YUV conversion needed).
- Saved colors persisted as plain JSON (`org.json`, no Room/DataStore) in the app's private `filesDir` — `PaletteStore` does blocking file I/O from a background dispatcher via `PaletteViewModel`.
- Palette export reuses the King Info pattern: `FileProvider` + a real `.txt` file handed to `ACTION_SEND`.

## Known platform limits

- Camera-based color readings depend on the device's sensor, white balance, and ambient lighting — they are an approximation of the real-world color, not a calibrated colorimeter reading. Documented in the in-app FAQ and the Terms of Use.
- No photo-import color picking in this version — live camera only.

## Signing

Release signing reads `KINGCOLORPICKER_UPLOAD_STORE_FILE` / `_STORE_PASSWORD` / `_KEY_ALIAS` / `_KEY_PASSWORD` from your **user-level** `~/.gradle/gradle.properties` (same pattern as King Info / King Craft / King Six). Falls back to a debug-signed build if those keys aren't present.

## Web pages

`web/kingcolorpicker/index.html`, `privacy-policy.html`, `terms-of-service.html` — ready to upload to `teamexykings.in/kingcolorpicker/`. The Play Store link in `index.html` points to `com.teamexykings.kingcolorpicker` and will resolve once the listing is live.

## Files to delete

None yet — this is the first delivered version (v1.0.0).
