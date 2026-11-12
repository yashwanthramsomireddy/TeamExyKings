# King GPS Camera

An Android camera that burns the location, coordinates, altitude and date/time
straight into the photo. Built for site engineers, surveyors, property agents,
field inspectors and anyone who needs a picture to prove where and when it was
taken.

Part of the **King Ecosystem** by **TeamExyKings**.

Free forever. No ads, no subscription, no sign-in, no tracking.

---

## What it does

- **Stamps the photo itself.** The information is painted into the pixels at the
  moment of capture, so it survives WhatsApp, email, printing and pasting into a
  report. It cannot be stripped out later.
- **Four templates** — Classic, Detailed, Compact and Minimal — positionable in
  any of the four corners.
- **Every field is optional**: place headline, country flag, full street
  address, coordinates, altitude, accuracy, bearing, date, time, time zone, a
  custom note and an app credit mark.
- **Formats to suit the job**: decimal degrees or degrees/minutes/seconds,
  metric or imperial, five date layouts, 12- or 24-hour clock.
- **EXIF GPS tags** written into the file as well, so maps and photo managers
  can place the shot.
- **True preview.** The viewfinder overlay is drawn by the same renderer that
  writes the saved file, so what you line up is what you get.
- **Photos land in your gallery** under `Pictures/King GPS Camera`.
- **Settings backup and restore** as a plain local JSON file.
- **Pitch-black AMOLED theme** throughout, white accent only.

---

## Tech stack

| Layer | Choice |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose, Material 3 |
| Architecture | MVVM with manual ViewModel factories (no DI framework) |
| Navigation | Navigation Compose |
| Settings | AndroidX DataStore (Preferences) |
| Camera | AndroidX CameraX |
| Location | Google Play services location, falling back to `LocationManager` |
| Addresses | Android's built-in `Geocoder` |
| Metadata | AndroidX ExifInterface |
| Images | Coil |
| Splash | AndroidX SplashScreen API |
| Build | AGP 8.9.1, Gradle 8.11.1, Kotlin 2.1.0 |

- `applicationId` — `com.teamexykings.kinggpscamera`
- `minSdk` 26 · `targetSdk` 35 · `compileSdk` 35

---

## Permissions and why

| Permission | Reason |
|---|---|
| `CAMERA` | Taking the photo. Required. |
| `ACCESS_FINE_LOCATION` / `ACCESS_COARSE_LOCATION` | The coordinates printed on the photo. Optional — the app still works without it. |
| `INTERNET` / `ACCESS_NETWORK_STATE` | One job only: turning coordinates into a readable street address through Android's geocoder. Switch off both the place headline and the full address in Settings and the app never touches the network. |
| `WRITE_EXTERNAL_STORAGE` (maxSdk 28) | Saving into the gallery on Android 8 and 9. Not requested on Android 10+. |

No analytics, no crash reporting, no advertising SDK, no account system.

---

## Project layout

```
build.gradle.kts            root build file
settings.gradle.kts
gradle.properties
gradle/libs.versions.toml   version catalog
app/
  build.gradle.kts
  proguard-rules.pro
  src/main/AndroidManifest.xml
  src/main/res/              theme, icon, xml configs
  src/main/java/com/teamexykings/kinggpscamera/
    MainActivity.kt
    KingGpsCameraApp.kt
    camera/                  StampRenderer, PhotoProcessor
    data/                    SettingsRepository, PhotoStore, models
    location/                LocationProvider, GeocodeHelper, Formatters
    ui/                      theme, components, screens, nav
    util/                    BackupManager, ShareUtils
    vm/                      Camera / Gallery / Settings view models
docs/store-assets/           Play Console icon and feature graphic
index.html                   landing page
privacy-policy.html
terms-of-service.html
CHANGELOG.md
RELEASE_NOTES_1.0.0.md
```

### Where the stamping actually happens

`camera/StampRenderer.kt` is the single source of truth for the overlay. It
sizes every element as a fraction of the image width, which is why one piece of
code can draw both the live viewfinder overlay and the full-resolution stamp on
the saved JPEG and have them match.

`camera/PhotoProcessor.kt` decodes the JPEG from CameraX with `inMutable = true`
so the stamp is drawn in place rather than onto a second full-size copy — on a
high-megapixel sensor that single choice is what keeps the app inside its memory
budget.

---

## Building

1. Open the folder in Android Studio (Ladybug or newer) and let Gradle sync.
2. `Run` for a debug build, or `./gradlew assembleRelease` / `bundleRelease`.

### Release signing

Signing properties are read from `~/.gradle/gradle.properties` so nothing secret
lives in the project folder:

```properties
KGC_STORE_FILE=C:/keystores/king-gps-camera.jks
KGC_STORE_PASSWORD=...
KGC_KEY_ALIAS=...
KGC_KEY_PASSWORD=...
```

If those keys are absent the release build falls back to the debug signing
config, so a fresh clone still compiles.

---

## Play Console assets

`docs/store-assets/` holds the 512×512 icon and the 1024×500 feature graphic.
Phone screenshots still need to be captured from a device.

---

## Licence and credits

Copyright © TeamExyKings. All rights reserved. This source is private and not
licensed for redistribution.

Built by Yashwanth Ram Somireddy — [teamexykings.in](https://teamexykings.in)
Contact: Yashwanthramsomireddy@gmail.com
