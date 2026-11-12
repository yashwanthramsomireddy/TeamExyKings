# King PDF Reader

A lightweight, pitch-black PDF reader for Android, under the **TeamExyKings** brand.

Opens a document and gets out of the way: continuous scrolling, sharp zoom, night mode,
bookmarks and a recent-files list that remembers your place. Fully offline, no ads, no
sign-in, free forever.

| | |
|---|---|
| Package | `com.teamexykings.kingpdfreader` |
| Version | 1.0.0 (versionCode 1) |
| Min SDK | 26 (Android 8.0) |
| Target / Compile SDK | 35 |
| Language | Kotlin 2.1.0 |
| UI | Jetpack Compose + Material 3 |
| Architecture | MVVM with manual ViewModel factories |
| Persistence | DataStore (Preferences), JSON-encoded lists |
| PDF engine | `android.graphics.pdf.PdfRenderer` (platform) |
| Build | AGP 8.9.1 / Gradle 8.11.1 / JDK 17 |

---

## Features

**Reading**
- Continuous vertical scroll through the whole document
- Pinch zoom and double-tap zoom, 100%–500%
- Pages are **re-rendered** at each zoom step rather than stretched, so text stays sharp
- Night mode: inverts the page for dark-room reading on AMOLED screens
- Fit width or fit page layouts, with an optional gap between pages
- Tap the middle to hide the toolbars; optional tap-the-edges to scroll
- Page counter with a go-to-page dialog and a seek slider

**Organising**
- Bookmarks per document, with rename, jump and delete
- Recent files with saved reading position, progress bar, pinning and name search
- Opens PDFs shared or viewed from file managers, mail and other apps

**Settings**
- Night mode, resume at last page, keep screen on, tap-to-scroll
- Page layout and render quality (Balanced / Sharp / Sharpest)
- Clear recent files, clear all bookmarks
- Back up and restore settings, recent files and bookmarks as JSON

**By design**
- No `INTERNET` permission is declared — the app cannot make network requests
- No storage permission: files arrive through Android's document picker, one at a time
- No ads, no analytics, no crash reporting, no accounts
- Theme locked to pure black (`#000000`) with a white accent; no light theme, no accent picker

---

## Project layout

```
settings.gradle.kts
build.gradle.kts
gradle.properties
gradle/
  libs.versions.toml
  wrapper/gradle-wrapper.properties
app/
  build.gradle.kts
  proguard-rules.pro
  src/main/
    AndroidManifest.xml
    java/com/teamexykings/kingpdfreader/
      MainActivity.kt
      data/
        AppDataStore.kt          DataStore repository: settings, recents, bookmarks, backup
        Models.kt                RecentFile, Bookmark, AppSettings, PageFit
      pdf/
        PdfDocumentSource.kt     PdfRenderer wrapper: mutex, LruCache, page measuring
      ui/
        KingPdfReaderApp.kt      Navigation graph
        HomeViewModel.kt
        ReaderViewModel.kt
        SettingsViewModel.kt
        components/
          Common.kt              PillButton, SectionCard, SettingsRow, GlowingAppIcon…
          PinchZoom.kt           Pinch-only gesture modifier (Initial pass, 2+ pointers)
        screens/
          HomeScreen.kt
          ReaderScreen.kt
          AboutScreen.kt
          FaqScreen.kt
          SettingsScreen.kt
        theme/
          Color.kt  Theme.kt  Type.kt
    res/
      values/   strings.xml  colors.xml  themes.xml
      drawable/ ic_launcher_foreground.xml  ic_launcher_background.xml  ic_splash_logo.xml
      mipmap-anydpi-v26/ ic_launcher.xml  ic_launcher_round.xml
      xml/      file_paths.xml  backup_rules.xml  data_extraction_rules.xml
index.html
privacy-policy.html
terms-of-service.html
README.md
CHANGELOG.md
RELEASE_NOTES_1.0.0.md
STORE_LISTING.md
LICENSE
docs/store-assets/
  icon-512.png
  feature-graphic-1024x500.png
```

---

## Building

1. Open the project folder in Android Studio (Ladybug or newer) and let Gradle sync.
2. Run the `app` configuration on a device or emulator (API 26+).

### Debug

```
./gradlew :app:assembleDebug
```

The debug build installs alongside release as `com.teamexykings.kingpdfreader.debug`.

### Release

Signing reads four properties from your user-level `gradle.properties`
(`C:\Users\Yash\.gradle\gradle.properties`):

```properties
KINGPDFREADER_UPLOAD_STORE_FILE=C:/path/to/king-pdf-reader-upload.jks
KINGPDFREADER_UPLOAD_STORE_PASSWORD=…
KINGPDFREADER_UPLOAD_KEY_ALIAS=…
KINGPDFREADER_UPLOAD_KEY_PASSWORD=…
```

If those properties are absent the release build still compiles, just unsigned.

```
./gradlew :app:bundleRelease     # AAB for Play, at app/build/outputs/bundle/release/
./gradlew :app:assembleRelease   # APK for sideloading
```

R8 minification and resource shrinking are on for release.

---

## How the reader works

`PdfRenderer` allows only one page open at a time and is not thread-safe, so
`PdfDocumentSource` serialises every render behind a `Mutex` on `Dispatchers.IO`. Rendered
bitmaps go into an `LruCache` sized at a quarter of the heap, capped at 96 MB, with evicted
bitmaps recycled.

The reader asks for pages at a **quantised** width rather than the exact on-screen width, so
pinching does not trigger a render on every frame:

| Zoom | Render step |
|---|---|
| under 1.5× | 1× |
| 1.5× – 2.5× | 2× |
| over 2.5× | 3× |

That step is then multiplied by the render-quality setting (1.0 / 1.5 / 2.0) and capped at
2600 px wide.

Page heights come from `measurePage`, which opens a page just long enough to read its
dimensions. Until a page has been measured it is laid out at A4 proportions, so the scroll
range is close to correct from the first frame and settles as pages are reached.

Zooming is handled by `Modifier.pinchToZoom`, which listens on `PointerEventPass.Initial` but
consumes nothing while a single finger is down. One-finger drags therefore fall through to the
`LazyColumn` for normal scrolling, and only genuine two-finger pinches are claimed for zoom.

---

## Licence

Proprietary. All rights reserved — see `LICENSE`.

Built by Yashwanth Ram Somireddy · © TeamExyKings · [teamexykings.in](https://teamexykings.in/)
