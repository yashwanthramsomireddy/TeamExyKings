# King Grid

Split any photo into a perfect grid.

King Grid cuts one photo into equal tiles — 3×3, 2×2, 2×3 and more — and saves every tile to the
gallery, numbered in the order it should be posted. Built for Instagram multi-posts and profile
grids. Free forever, no ads, no subscription, no login, fully offline.

**Package:** `com.teamexykings.kinggrid`
**Current version:** 1.0.0 (versionCode 1)
**Brand:** TeamExyKings

---

## Features

- **Ten layouts** — 1×2, 2×1, 1×3, 3×1, 2×2, 2×3, 3×2, 3×3, 4×3, 4×4.
- **Live numbered preview** — the grid and the posting number are drawn over the photo.
- **Carousel or feed-grid numbering** — reading order for multi-image posts, reversed order so a
  profile grid assembles correctly as you post.
- **Square tiles** — centre-crops the photo so every tile is a perfect square, or off to use the
  whole photo.
- **Full-resolution tiles** — cut straight from the original file with `BitmapRegionDecoder`, so a
  48 MP photo never has to be held in memory in one piece.
- **EXIF-aware** — photos shot in portrait are turned upright automatically, per tile.
- **Saved in order** — each tile is written a second apart and numbered in its filename, so sorting
  by date or by name both give posting order.
- **Share all tiles** at once, or open the set in the gallery.
- **No permissions on Android 10+** — the system photo picker and MediaStore do the work.

## Tech stack

| Layer | Choice |
|---|---|
| Language | Kotlin 2.1.0 |
| UI | Jetpack Compose + Material 3 |
| Architecture | MVVM with manual ViewModel factories |
| Navigation | Navigation Compose |
| Persistence | DataStore (Preferences) |
| Imaging | `BitmapRegionDecoder`, AndroidX `ExifInterface` |
| Splash | AndroidX Core SplashScreen API |
| Build | AGP 8.9.1 · Gradle 8.11.1 · JDK 17 |
| SDK | compileSdk 35 · targetSdk 35 · minSdk 24 |

## Project layout

```
settings.gradle.kts
build.gradle.kts
gradle.properties
gradle/libs.versions.toml
gradle/wrapper/gradle-wrapper.properties
app/
  build.gradle.kts
  proguard-rules.pro
  src/main/AndroidManifest.xml
  src/main/java/com/teamexykings/kinggrid/
    MainActivity.kt
    core/       ImageSplitter.kt · MediaSaver.kt · AppLinks.kt
    data/       GridPreset.kt · SettingsRepository.kt
    vm/         EditorViewModel.kt · SettingsViewModel.kt
    ui/         KingGridApp.kt
      components/ Common.kt · GridPreview.kt
      screens/    HomeScreen.kt · EditorScreen.kt · SettingsScreen.kt · AboutScreen.kt · FaqScreen.kt
      theme/      Color.kt · Theme.kt · Type.kt
  src/main/res/  drawable · mipmap-* · values · xml
index.html
privacy-policy.html
terms-of-service.html
README.md
CHANGELOG.md
RELEASE_NOTES_1.0.0.md
STORE_LISTING.md
LICENSE
docs/store-assets/  play_icon_512.png · play_feature_graphic_1024x500.png
```

## Building

1. Open the project folder in Android Studio (Ladybug or newer) and let Gradle sync.
2. If the Gradle wrapper JAR is missing, Android Studio regenerates it on first sync, or run
   `gradle wrapper --gradle-version 8.11.1` once with a local Gradle install.
3. Run the `app` configuration on a device or emulator (Android 7.0 / API 24 or newer).

### Release signing

Signing reads from your **user-level** `gradle.properties`
(`~/.gradle/gradle.properties`), so no keystore details live in the project:

```properties
KINGGRID_UPLOAD_STORE_FILE=C:/keys/kinggrid-upload.jks
KINGGRID_UPLOAD_STORE_PASSWORD=********
KINGGRID_UPLOAD_KEY_ALIAS=kinggrid
KINGGRID_UPLOAD_KEY_PASSWORD=********
```

Then build the bundle:

```
gradlew bundleRelease
```

The signing config is applied only when `KINGGRID_UPLOAD_STORE_FILE` is present, so debug builds and
clean checkouts still build without it.

## Web pages

`index.html`, `privacy-policy.html` and `terms-of-service.html` are published at
`https://teamexykings.in/King-Grid/`. The app's About screen links to the site root for
"Visit website" and to those two pages for the policy links.

## Play Console assets

`docs/store-assets/` holds the two images the Play Console listing needs:

- `play_icon_512.png` — 512 × 512 app icon, no transparency.
- `play_feature_graphic_1024x500.png` — 1024 × 500 feature graphic.

Screenshots still need to be captured from a device.

---

© 2026 Yashwanth Ram Somireddy · TeamExyKings. All rights reserved.
