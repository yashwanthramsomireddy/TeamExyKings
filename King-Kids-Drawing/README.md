# King Kids Drawing

A free, offline drawing and coloring app for kids, built by Yashwanth Ram Somireddy under the TeamExyKings brand.

- Package: `com.teamexykings.kingkidsdrawing`
- Free forever, no ads, no subscription, no login/account, no analytics, no tracking
- Fully offline - everything is built into the app, nothing is downloaded
- Private repository (not publicly distributed as source)

## Stack

- Kotlin + Jetpack Compose + Material 3
- MVVM with manual ViewModel factories (no Hilt/DI framework)
- Jetpack DataStore (Preferences) for Settings
- Navigation Compose
- AndroidX SplashScreen API
- kotlinx.serialization for the backup JSON format
- Min SDK 29, Target SDK 36, AGP 8.9.1, Gradle 8.11.1

## Project layout

```
app/src/main/java/com/teamexykings/kingkidsdrawing/
  MainActivity.kt, KingKidsDrawingApp.kt
  navigation/              Screen routes + NavHost
  ui/theme/                Colors, fonts, Material3 theme (Pitch Black / White / System)
  ui/components/           Shared pill buttons, colour pickers, option cards
  ui/screens/home/         Gallery of saved drawings
  ui/screens/canvas/       The drawing canvas, its ViewModel and the rendering/flood-fill engine
  ui/screens/coloringpages/ Coloring-page template picker
  ui/screens/settings/     Theme/font/accent + Backup/Restore
  ui/screens/about/        About screen + Credits dialog
  ui/screens/faq/          FAQ with category sections + search
  data/model/              Drawing + DrawAction (stroke/fill/stamp) models
  data/repository/         File-based drawing storage (JSON + thumbnail PNG per drawing)
  data/settings/           DataStore-backed settings repository
  data/backup/             Backup/restore JSON (single-file export of settings + all drawings)
  data/stamps/             Code-drawn (no image assets) stamp library
  data/coloringpages/      Code-drawn coloring-page outline library

docs/keystore/             Upload keystore (KingKidsDrawing-upload.jks)
docs/store-assets/         Play Console store listing assets
index.html, privacy-policy.html, terms-of-service.html   Public web pages (repo root)
v1.0.0.md                  Latest release notes (older versions are not kept in the zip)
```

## How a drawing is stored

Each drawing is saved as an ordered list of small **actions** (a brush stroke, a fill, or a
stamp placement) rather than as a bitmap. Re-playing the action list onto a blank canvas
reproduces the drawing exactly - this keeps the backup JSON small and makes every drawing
fully re-editable (undo = drop the last action and re-render). A cached PNG thumbnail is
kept separately for the gallery grid and is regenerated automatically after a restore.

## Signing

The project (root) `gradle.properties` already points at the upload keystore committed in this
repo at `docs/keystore/KingKidsDrawing-upload.jks`:

```properties
KINGKIDSDRAWING_UPLOAD_STORE_FILE=docs/keystore/KingKidsDrawing-upload.jks
KINGKIDSDRAWING_UPLOAD_STORE_PASSWORD=********
KINGKIDSDRAWING_UPLOAD_KEY_ALIAS=key0
KINGKIDSDRAWING_UPLOAD_KEY_PASSWORD=********
```

Replace the `********` placeholders with the real store/key passwords (and drop the real
`.jks` file into `docs/keystore/`) before building a signed release. If the keystore file or
properties aren't present, release builds simply build unsigned (debug builds are unaffected).

## Web pages

`index.html`, `privacy-policy.html` and `terms-of-service.html` live at the repo root. They're
meant to be published at:

- `https://teamexykings.in/`
- `https://teamexykings.in/King-Kids-Drawing/privacy-policy.html`
- `https://teamexykings.in/King-Kids-Drawing/terms-of-service.html`

The About screen's Website/Privacy/Terms buttons already point at these URLs.

## Store assets

`docs/store-assets/icon-512.png` and `docs/store-assets/feature-graphic-1024x500.png` are
ready for the Play Console listing (Store presence > Main store listing).

## Building

Open the project root in Android Studio and let it sync. If Android Studio doesn't
regenerate the Gradle wrapper jar automatically, run `gradle wrapper` once from a terminal
with Gradle 8.11.1 installed, or just use the "Sync Project with Gradle Files" action.

## Known assumptions (please correct me if any of these are wrong)

- **Fonts**: the 5 font choices use Android/Compose's built-in generic font families
  (Default, Sans-serif, Serif, Monospace, Cursive) rather than bundled Google Fonts files
  like King Calc's Inter/Nunito/Poppins/etc., since I don't have a way to fetch and bundle
  font binaries in this environment. If you'd rather I use the same bundled Google Fonts as
  King Calc, send me the five `.ttf`/`.otf` files and I'll wire them in.
- **Canvas resolution**: fixed at 1080x1440px for all drawings in v1.0.0.
- **Coloring pages / stamps**: a starter set (8 outlines, 12 stamps) across Shapes/Nature/
  Animals - let me know if you want more before the Play listing, or if this is enough for v1.
- Export/share of a finished drawing as an image is wired into the data layer
  (`DrawingRepository` + thumbnail bitmap) but the Canvas screen's save button currently
  saves the drawing to the gallery (Home); a dedicated "export to gallery/share image" button
  can be added as a quick follow-up if you want it separate from Save.
