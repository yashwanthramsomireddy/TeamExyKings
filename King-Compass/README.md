# King Compass

A compass, bubble level and inclinometer in one pitch-black Android app.
Free forever, no ads, no tracking, no login, fully offline.

**Package:** `com.teamexykings.kingcompass`
**Current version:** 1.0.0 (versionCode 1)
**Brand:** TeamExyKings — built by Yashwanth Ram Somireddy

---

## What it does

| Tool | What it measures | Source |
|---|---|---|
| **Compass** | Heading 0–360° with cardinal point, magnetic field strength, sensor accuracy | Rotation vector, falling back to magnetometer + accelerometer |
| **Level** | Two-axis tilt (pitch and roll) to 0.1°, with user calibration | Gravity sensor, falling back to a filtered accelerometer |
| **Incline** | Slope from horizontal 0–90°, grade %, roof pitch x:12, hold-to-freeze | Gravity sensor |
| **Calibration** | Step-by-step guide with animated figure-8 and live accuracy readout | — |

---

## Stack

- Kotlin 2.1.0, Jetpack Compose (BOM 2025.01.00), Material 3
- MVVM with manual factories — no DI framework
- DataStore Preferences for settings
- Navigation Compose
- AndroidX SplashScreen API
- AGP 8.9.1, Gradle 8.11.1, JDK 17
- `compileSdk` 35 · `targetSdk` 35 · `minSdk` 24

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
    java/com/teamexykings/kingcompass/
      KingCompassApplication.kt      App class + manual AppContainer
      MainActivity.kt
      data/                          Settings model + DataStore repository
      sensor/                        SensorRepository, reading model, cardinals
      ui/
        KingCompassApp.kt            NavHost + bottom bar
        KingCompassViewModel.kt      One VM shared by all tool screens
        components/                  Shared cards, buttons, top bar, haptics
        compass/ level/ incline/     The three tools
        calibration/ settings/ faq/ about/
        theme/                       Pitch-black colour scheme + type
    res/                             Icons, themes, strings
docs/
  store-assets/                      icon-512.png, feature-graphic-1024x500.png
  PLAY_STORE_LISTING.md
index.html                           Landing page
privacy-policy.html
terms-of-service.html
CHANGELOG.md
RELEASE_NOTES_1.0.0.md
```

---

## Building

Debug:

```
gradlew assembleDebug
```

The debug build installs alongside release as `com.teamexykings.kingcompass.debug`.

Release AAB (what Play Console wants):

```
gradlew bundleRelease
```

Release APK for sideloading:

```
gradlew assembleRelease
```

### Signing

Release signing reads four Gradle properties. Put them in your **global**
`C:\Users\Yash\.gradle\gradle.properties`, never in the project:

```
KCMP_STORE_FILE=C:/path/to/king-compass.jks
KCMP_STORE_PASSWORD=...
KCMP_KEY_ALIAS=...
KCMP_KEY_PASSWORD=...
```

If `KCMP_STORE_FILE` is absent the release build still compiles — it just comes
out unsigned, so the properties are only needed on the machine that ships builds.

---

## Design rules

- Pitch black `#000000` with a white accent. No light theme, no accent picker,
  no theme setting — deliberately, same as King Info.
- Top bar on every tool screen carries three icons on the right: **i** (About),
  **?** (FAQ) and the gear (Settings).
- Portrait locked. The sensor maths still remaps for display rotation, so large
  screens that ignore the orientation request on Android 15+ stay correct.

---

## Permissions

`VIBRATE` only, for the haptic tick, and it is switchable in Settings.
No internet, no location, no storage, no camera. Motion sensors need no
runtime permission on Android.

---

## Web pages

`index.html`, `privacy-policy.html` and `terms-of-service.html` sit at the
project root. They are published through GitHub Pages on the public
TeamExyKings repo, under a `King-Compass/` subfolder, which is what the
in-app Privacy policy and Terms of use buttons point at.

---

## Licence

Proprietary. © 2026 TeamExyKings. All rights reserved.
