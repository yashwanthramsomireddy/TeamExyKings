# King Travel

Packing checklists that reset and reuse — a free, offline Android app under the **TeamExyKings** brand.

- **Package:** `com.teamexykings.kingtravelcheck`
- **Version:** 1.0.0 (versionCode 1)
- **Min SDK:** 26 · **Target/Compile SDK:** 35
- Free forever · no ads · no subscription · no sign-in · no tracking · no internet permission

---

## What it does

| Area | Details |
|---|---|
| Trips | Any number of trips, each with its own checklist, destination, start date and notes |
| Templates | Blank, Weekend getaway, Beach holiday, Business trip, International trip, Family trip, Trek & outdoors, Pilgrimage |
| Items | Grouped by category (Essentials, Documents, Clothing, Toiletries, Electronics, Health, Extras), with quantity |
| Packing | Tap to tick, live progress bar, per-category counts, hide packed items |
| Reuse | Reset ticks on a trip, duplicate a trip unticked, add extra items from another template |
| Housekeeping | Archive / restore trips, remove packed items, share a checklist as plain text |
| Backup | Export all trips to JSON, restore by merge or replace |
| Theme | Pitch black (default), Dark, Light, Follow system — white accent |

## Stack

Kotlin · Jetpack Compose · Material 3 · MVVM with manual ViewModel factories (no Hilt) · DataStore Preferences · Navigation Compose · kotlinx.serialization · SplashScreen API
AGP 8.9.1 · Gradle 8.11.1 · Kotlin 2.1.0 · Compose BOM 2024.12.01

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
  src/main/java/com/teamexykings/kingtravelcheck/
    MainActivity.kt
    data/        Models.kt · TripTemplates.kt · TripRepository.kt · SettingsRepository.kt
    ui/          KingTravelApp.kt (nav)
      theme/     Color.kt · Theme.kt · Type.kt
      components/Common.kt
      screens/   TripsScreen · NewTripScreen · TripDetailScreen · SettingsScreen · AboutScreen · FaqScreen
      vm/        TripsViewModel · TripDetailViewModel · SettingsViewModel
    util/        Utils.kt
  src/main/res/  drawable · mipmap-anydpi-v26 · values · xml
index.html
privacy-policy.html
terms-of-service.html
STORE_LISTING.md
CHANGELOG.md
RELEASE_NOTES_1.0.0.md
LICENSE
docs/store-assets/   app-icon-512.png · feature-graphic-1024x500.png
```

## Build

1. Open the project folder in Android Studio and let it sync.
   The Gradle wrapper JAR is not in this zip — Android Studio supplies its own Gradle, or run `gradle wrapper` once to regenerate `gradle/wrapper/gradle-wrapper.jar` and `gradlew`.
2. Debug build: `Run ▶` (installs as `com.teamexykings.kingtravelcheck.debug`, so it can sit beside a release build).
3. Release build: `Build → Generate Signed App Bundle`.

### Release signing

Add to your **user-level** `~/.gradle/gradle.properties` (not to the repo):

```properties
KINGTRAVEL_UPLOAD_STORE_FILE=C:/path/to/king-travel-upload.jks
KINGTRAVEL_UPLOAD_STORE_PASSWORD=********
KINGTRAVEL_UPLOAD_KEY_ALIAS=king-travel-upload
KINGTRAVEL_UPLOAD_KEY_PASSWORD=********
```

The release build type only attaches the signing config when `KINGTRAVEL_UPLOAD_STORE_FILE` is present, so the project still builds for anyone without the keystore.

## Web pages

`index.html`, `privacy-policy.html` and `terms-of-service.html` are flat at the project root and are meant to be published at **https://teamexykings.in/King-Travel/**. The in-app links point there:

- Visit website → `https://teamexykings.in/`
- Privacy policy → `https://teamexykings.in/King-Travel/privacy-policy.html`
- Terms of use → `https://teamexykings.in/King-Travel/terms-of-service.html`

## Data & privacy

Everything lives in two DataStore Preferences files inside the app sandbox (`king_travel_trips`, `king_travel_settings`). No `INTERNET` permission is declared and no runtime permissions are requested. Backup files go only where the user picks them through the Storage Access Framework.

---

© 2026 TeamExyKings · Built by Yashwanth Ram Somireddy. All rights reserved.
