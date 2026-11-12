# King Parking

**Never lose your car again.** Save your parking spot with its GPS position, an optional photo of the bay and a note — then walk straight back to it.

A TeamExyKings product, built by Yashwanth Ram Somireddy.

- Package: `com.teamexykings.kingparking`
- Version: 1.0.0 (versionCode 1)
- Free forever · No ads · No subscription · No sign-in · Fully offline

---

## What it does

| Feature | Notes |
|---|---|
| Save a spot | GPS latitude/longitude with the accuracy of the fix, saved on one tap |
| Bay photo | Optional camera shot of the pillar, bay number or row — stored privately |
| Note & bay text | Free text note plus a short "Level / bay" field (GPS can't tell you the floor) |
| Multiple spots | Car, bike, a friend's vehicle — as many active spots as you like |
| Walk back | Hands the position to Google Maps with walking directions, or drops a pin |
| History | Cleared spots move to a history list and can be restored with one tap |
| Share | Sends the spot as text + a maps link, with the photo attached if there is one |
| Backup | One JSON file written through the system file picker; restore merges or replaces |
| Themes | Pitch black (default), dark grey, light, follow system — with 7 accent colours |

## Tech stack

- Kotlin 2.0.21, Jetpack Compose (BOM 2024.12.01), Material 3
- MVVM with manual ViewModel factories (no Hilt)
- DataStore Preferences + kotlinx.serialization for storage
- Navigation Compose, AndroidX SplashScreen API
- Plain Android `LocationManager` — **no Google Play Services**, so the app ships without the INTERNET permission
- AGP 8.9.1, Gradle 8.11.1, compileSdk/targetSdk 36, minSdk 29, JVM target 17

## Project layout

```
.
├── index.html                  # Landing page   → teamexykings.in/King-Parking/
├── privacy-policy.html         # Privacy policy → teamexykings.in/King-Parking/privacy-policy.html
├── terms-of-service.html       # Terms of use   → teamexykings.in/King-Parking/terms-of-service.html
├── README.md
├── CHANGELOG.md
├── RELEASE_NOTES_1.0.0.md
├── STORE_LISTING.md            # Play Console copy, ready to paste
├── LICENSE
├── docs/
│   └── store-assets/
│       ├── icon-512.png        # Play Store icon
│       └── feature-graphic.png # 1024 x 500 feature graphic
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
├── gradle/
│   ├── libs.versions.toml
│   └── wrapper/gradle-wrapper.properties
└── app/
    ├── build.gradle.kts
    ├── proguard-rules.pro
    └── src/main/
        ├── AndroidManifest.xml
        ├── res/
        └── java/com/teamexykings/kingparking/
            ├── KingParkingApplication.kt   # AppContainer (manual DI)
            ├── MainActivity.kt
            ├── navigation/KingParkingNavHost.kt
            ├── data/                        # spots, settings, photos, backup
            ├── location/LocationProvider.kt
            ├── ui/
            │   ├── common/                  # shared components, app icon mark
            │   ├── home/  save/  detail/
            │   ├── settings/  faq/  about/
            │   └── theme/
            └── util/                        # formatters, intents
```

## Building

1. Open the folder in Android Studio (Ladybug or newer) and let it sync.
2. `Run ▶` for a debug build, or `./gradlew assembleRelease` / `bundleRelease` for release.
3. Android Studio regenerates `gradle/wrapper/gradle-wrapper.jar` and `local.properties` on first sync — neither is shipped in the zip.

### Release signing (optional)

Add these to your **user-level** `~/.gradle/gradle.properties` (never to the project):

```properties
KINGPARKING_UPLOAD_STORE_FILE=C:/path/to/king-parking.jks
KINGPARKING_UPLOAD_STORE_PASSWORD=...
KINGPARKING_UPLOAD_KEY_ALIAS=...
KINGPARKING_UPLOAD_KEY_PASSWORD=...
```

If they are absent the release build still compiles — it just comes out unsigned.

## Permissions

| Permission | When it is used |
|---|---|
| `ACCESS_FINE_LOCATION` / `ACCESS_COARSE_LOCATION` | Only while saving a spot or refreshing a distance. No background location. |
| `CAMERA` | Only when the user taps to take a bay photo. |

There is deliberately **no `INTERNET` permission**.

## Data on the device

- Spots and settings: DataStore Preferences (`king_parking_spots`, `king_parking_settings`)
- Photos: app-private `files/photos/`, shared out only through a `FileProvider` when the user shares a spot
- Backup: a single JSON file written to a location the user picks (photos are not included)

## Web pages

The three HTML files at the project root are the pages for `teamexykings.in/King-Parking/`. Copy them into the `King-Parking` folder of the TeamExyKings pages repo.

---

© 2026 TeamExyKings. All rights reserved. Proprietary — see `LICENSE`.
