# King App Backup

Back up the apps installed on your Android phone as APK files, into a folder you choose.
Free forever, fully offline, no ads, no subscription, no sign-in.

Part of the King Ecosystem by **TeamExyKings**.

---

## Features

- **Installed tab** — every app on the device with icon, version, size and last-updated date.
- **Archived tab** — everything already saved in your backup folder, with the same detail.
- **Multi-select backup** — tick as many apps as you like, tap BACKUP, watch live progress.
- **Split APK support** — apps delivered as App Bundles are archived complete as one `.apks` file.
- **Restore** — reinstall any archived package through Android's own installer.
- **Share** — send an APK to another device or app through the system share sheet.
- **Uninstall** — remove selected apps from the Installed tab (Android confirms each one).
- **Search and sort** — by name, size or date, on both tabs.
- **Pitch black AMOLED theme** by default, plus Dark and Light.
- **No internet permission at all.** Nothing is collected, nothing is sent.

## The one real limit

Without root access, Android does not allow one app to read another app's data. King App Backup
copies the **installer package (APK) only** — app data, logins, settings and game saves are not
included, so a restored app starts out empty. That is an Android restriction, not a missing feature.

---

## Tech stack

| Area | Choice |
|---|---|
| Language | Kotlin 2.1.0 |
| UI | Jetpack Compose + Material 3 |
| Architecture | MVVM with manual ViewModel factories (no DI framework) |
| Navigation | Navigation Compose |
| Preferences | AndroidX DataStore |
| Storage | Storage Access Framework (`DocumentsContract`) |
| Install / query | `PackageManager`, `PackageInstaller` |
| Splash | AndroidX SplashScreen API |
| Build | AGP 8.9.1, Gradle 8.11.1, JDK 17 |
| SDK | compileSdk 35, targetSdk 35, minSdk 26 |

---

## Project layout

```
.
├─ app/
│  ├─ build.gradle.kts
│  ├─ proguard-rules.pro
│  └─ src/main/
│     ├─ AndroidManifest.xml
│     ├─ java/com/teamexykings/kingappbackup/
│     │  ├─ AppLinks.kt            links, support email, in-app release history
│     │  ├─ KingApp.kt             Application + dependency container
│     │  ├─ MainActivity.kt        splash, theme, SAF folder picker
│     │  ├─ data/
│     │  │  ├─ AppsRepository.kt   reads installed packages
│     │  │  ├─ BackupRepository.kt SAF backup, archive listing, index file
│     │  │  ├─ SettingsStore.kt    DataStore-backed settings
│     │  │  └─ model/Models.kt
│     │  ├─ install/ApkInstaller.kt install + PackageInstaller status receiver
│     │  ├─ ui/
│     │  │  ├─ AppNav.kt
│     │  │  ├─ about/AboutScreen.kt
│     │  │  ├─ components/Common.kt
│     │  │  ├─ faq/FaqScreen.kt
│     │  │  ├─ home/HomeScreen.kt + HomeViewModel.kt
│     │  │  ├─ settings/SettingsScreen.kt
│     │  │  └─ theme/Theme.kt
│     │  └─ util/                  Format, IconLoader, ShareHelper
│     └─ res/
├─ docs/store-assets/              icon-512.png, feature-graphic-1024x500.png
├─ index.html
├─ privacy-policy.html
├─ terms-of-service.html
├─ CHANGELOG.md
├─ RELEASE_NOTES_1.0.0.md
└─ LICENSE
```

---

## Building

1. Open the project folder in Android Studio and let Gradle sync.
2. Run the `app` configuration on a device or emulator (API 26+).

> The Gradle wrapper **jar** (`gradle/wrapper/gradle-wrapper.jar`, `gradlew`, `gradlew.bat`) is not
> included — Android Studio regenerates it on first sync, or you can copy it in from any of your
> other King projects. `gradle/wrapper/gradle-wrapper.properties` is present and pins Gradle 8.11.1.

### Release signing

Signing properties are read from your **user-level** `gradle.properties`
(`C:\Users\<you>\.gradle\gradle.properties`), so no secret ever lands in the project:

```properties
KINGAPPBACKUP_UPLOAD_STORE_FILE=C:/path/to/king-app-backup-upload.jks
KINGAPPBACKUP_UPLOAD_STORE_PASSWORD=********
KINGAPPBACKUP_UPLOAD_KEY_ALIAS=upload
KINGAPPBACKUP_UPLOAD_KEY_PASSWORD=********
```

If those properties are absent the release build simply stays unsigned, so a debug build still works
on a clean machine.

Build the bundle with:

```
gradlew bundleRelease
```

---

## Google Play notes

Two permissions need the sensitive-permission declaration in Play Console. Both map to the
**"Backup and restore"** permitted use, which is exactly what this app does:

| Permission | Why it is needed |
|---|---|
| `QUERY_ALL_PACKAGES` | Listing the apps you can back up is the app's core function. |
| `REQUEST_INSTALL_PACKAGES` | Needed to reinstall an archived APK from the Archived tab. |

Expect the review to look closely at both. The store listing and in-app FAQ both state the
root limitation plainly, which is the kind of thing reviewers check for.

No other permissions are requested — no internet, no storage, no location.

---

## Web pages

`index.html`, `privacy-policy.html` and `terms-of-service.html` are deployed to
`teamexykings.in/King-App-Backup/`. The Play listing's privacy policy URL points at
`https://teamexykings.in/King-App-Backup/privacy-policy.html`.

---

## License

Proprietary. All rights reserved — see `LICENSE`.

© 2026 TeamExyKings · Built by Yashwanth Ram Somireddy
