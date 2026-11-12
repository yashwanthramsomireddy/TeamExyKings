# King QR

A fast, private QR code and barcode scanner and generator for Android, built natively in Kotlin and Jetpack Compose under the **TeamExyKings** brand.

Free forever · No ads · No subscription · No sign-in · No internet permission

---

## What it does

**Scan** — QR codes plus Aztec, Data Matrix, PDF417, EAN-13, EAN-8, UPC-A, UPC-E, Code 39, Code 93, Code 128, ITF and Codabar, read live through the camera with Google ML Kit's on-device model.

**Understand** — scanned payloads are classified and parsed into fields: links, Wi-Fi credentials, UPI payment requests, MECARD/vCard contacts, email, phone, SMS, geo and plain text.

**Generate** — QR codes for text, links, Wi-Fi networks, UPI requests and contact cards, rendered with ZXing and saved to the gallery or shared as PNG.

**Remember** — every scan and generated code is kept on the device, with search, type filters and starred entries. No cap on how many are stored.

**Look right** — pitch-black AMOLED theme by default, plus Dark, Light and System, and seven accent colours.

---

## Stack

| Area | Choice |
|---|---|
| Language | Kotlin 2.1.0 |
| UI | Jetpack Compose, Material 3 |
| Architecture | MVVM with manual ViewModel factories (no DI framework) |
| Navigation | Navigation Compose |
| Persistence | DataStore Preferences (settings + history as JSON) |
| Camera | CameraX 1.4.1 |
| Detection | Google ML Kit Barcode Scanning (bundled, on-device) |
| Encoding | ZXing core |
| Splash | AndroidX SplashScreen API |
| Build | AGP 8.9.1, Gradle 8.11.1, JDK 17 |
| SDK | minSdk 29, targetSdk 36, compileSdk 36 |

Package: `com.teamexykings.kingqr`

---

## Project layout

```
.
├── app/
│   ├── build.gradle.kts
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── res/
│       └── java/com/teamexykings/kingqr/
│           ├── KingQrApplication.kt
│           ├── MainActivity.kt
│           ├── data/
│           │   ├── model/        ScanRecord, ContentKind, payload types
│           │   └── repo/         DataStore, SettingsRepository, HistoryRepository
│           ├── qr/               BarcodeAnalyzer, QrGenerator, PayloadParser, PayloadBuilder
│           ├── ui/
│           │   ├── about/        About, Credits, Changelog, AppLinks
│           │   ├── components/   Shared buttons, cards, fields, top bars
│           │   ├── faq/          FAQ screen + content
│           │   ├── generate/     Generate screen + ViewModel
│           │   ├── history/      History screen + ViewModel
│           │   ├── navigation/   Routes
│           │   ├── scan/         Scan screen, camera preview, overlay, result sheet
│           │   ├── settings/     Settings screen + ViewModel
│           │   └── theme/        Colour, type, theme
│           └── util/             Share, MediaStore, date helpers
├── docs/store-assets/            Play Console icon + feature graphic
├── index.html                    Landing page
├── privacy-policy.html
├── terms-of-service.html
├── CHANGELOG.md
├── RELEASE_NOTES_1.0.0.md
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── gradle/libs.versions.toml
```

---

## Building

1. Open the project folder in Android Studio (Ladybug or newer) and let Gradle sync.
2. Run the `app` configuration on a device with a camera. The emulator works for generation but its virtual camera is poor for scanning.
3. `./gradlew assembleDebug` for an APK, `./gradlew bundleRelease` for a Play bundle.

### Release signing

The release build reads four properties from `C:\Users\Yash\.gradle\gradle.properties`:

```properties
KQ_STORE_FILE=C:/path/to/kingqr-release.jks
KQ_STORE_PASSWORD=********
KQ_KEY_ALIAS=kingqr
KQ_KEY_PASSWORD=********
```

If they are absent, the release build falls back to the debug signing config so the project still assembles. A build intended for the Play Store **must** have them set.

---

## Permissions

| Permission | Why |
|---|---|
| `CAMERA` | Reading codes on the Scan screen. Declared `required="false"` so the app still installs on devices without a camera. |

There is deliberately no `INTERNET` permission. Saving generated images uses MediaStore, which needs no permission on API 29+.

---

## Privacy design

- Camera frames are analysed in memory and discarded; nothing is recorded or written to storage.
- History and settings live in the app's private DataStore.
- No analytics, no crash reporting, no advertising SDKs, no network calls.
- Backup and restore is a plain JSON file the user picks a location for.

---

## Web pages

`privacy-policy.html` and `terms-of-service.html` are hosted on GitHub Pages under the public **TeamExyKings** repository, in a `King-QR/` subfolder. The URLs in `ui/about/AppLinks.kt` point there:

```
https://yashwanthramsomireddy.github.io/TeamExyKings/King-QR/privacy-policy.html
https://yashwanthramsomireddy.github.io/TeamExyKings/King-QR/terms-of-service.html
```

---

## Licence

© 2026 TeamExyKings. All rights reserved. This source is proprietary and is not licensed for redistribution or reuse.
