# King System App Remover

A clean, ad-free Android app manager under the **TeamExyKings** brand. It lists everything installed
on the phone, lets you batch-uninstall the apps you installed yourself, and shows the sizes, install
dates and last-used dates that Android's own Settings app makes you hunt for one app at a time.

- Package: `com.teamexykings.kingsystemappremover`
- Version: **1.0.0** (versionCode 1)
- Min SDK 26 · Target SDK 35 · Kotlin 2.0.21 · AGP 8.9.1 · Gradle 8.11.1
- Free forever · no ads · no subscription · no sign-in · no tracking

---

## What it does

| Area | Detail |
|---|---|
| Two tabs | **User apps** (removable) and **System apps** (reference only), each with a live count |
| Batch uninstall | Tick any number of user apps; the app queues them and walks you through Android's confirm dialogs one by one, with a Stop button |
| Sorting | Name, size, install date, last updated, last used, package name — ascending or descending |
| Filters | All · Unused 30d+ · Over 100 MB · Recently installed · Disabled |
| Search | Matches app name and package name |
| Storage freed | Running total of bytes reclaimed and apps removed, kept in DataStore |
| Details sheet | Long-press any app: version, size, install/update dates, last used, installer, APK path, plus Open app / App info / Play Store / Share |
| Share | Exports the current list as a real `.txt` file through the system share sheet |
| Settings backup | Export and import preferences as JSON |
| Theme | Pitch-black AMOLED (#000000) with a white accent — no light theme, no accent picker, same as King Info |

## Honest limits

Android does not allow an app to uninstall system or pre-installed apps without root. This app never
asks for root. System apps are listed for reference with a shortcut to Android's app info screen,
where they can usually be disabled. The System apps tab says this in a dismissible banner, and the
FAQ, store listing and web pages say it too.

## Architecture

```
MainActivity (SplashScreen API, edge-to-edge)
  └── KingNavHost  (Navigation Compose)
        ├── HomeScreen      ← HomeViewModel      (manual ViewModelProvider.Factory)
        ├── SettingsScreen  ← SettingsViewModel
        ├── AboutScreen
        ├── ChangelogScreen (full in-app release history)
        └── FaqScreen       (categories + search)

data/
  AppInfo.kt            model, SortOrder, AppFilter
  AppRepository.kt      PackageManager + StorageStatsManager reads
  SettingsRepository.kt DataStore preferences + JSON backup
util/
  UsageAccess.kt        optional usage-access check + last-used map
  IconCache.kt          LruCache of app icons as ImageBitmap
  Formatters.kt         size / date / relative-time
  ShareUtil.kt          txt export, app info, Play Store, launch
ui/theme/Theme.kt       pitch-black colour scheme + typography
```

MVVM with manual factories, no DI framework — the same pattern as the other King apps.

## Permissions

| Permission | Why |
|---|---|
| `QUERY_ALL_PACKAGES` | Core purpose: listing and uninstalling installed apps. Needs the Play Console permission declaration form. |
| `REQUEST_DELETE_PACKAGES` | Hands a package to Android's uninstall dialog. |
| `PACKAGE_USAGE_STATS` | Optional, user-granted in system settings. Only for last-used dates and real app sizes. |

No `INTERNET` permission is declared — the app cannot send anything anywhere.

## Building

1. Open the folder in Android Studio (Gradle 8.11.1 wrapper, JDK 17).
2. Run the `app` configuration on a device or emulator (API 26+).
3. For a signed release, set these in `C:\Users\Yash\.gradle\gradle.properties`:

```
KSAR_STORE_FILE=<path to keystore>
KSAR_STORE_PASSWORD=...
KSAR_KEY_ALIAS=...
KSAR_KEY_PASSWORD=...
```

Then `gradlew bundleRelease` (or Build → Generate Signed Bundle in Android Studio).

> `gradle/wrapper/gradle-wrapper.jar` is not included in the source zip. Android Studio regenerates
> it on first sync, or run `gradle wrapper --gradle-version 8.11.1` once.

## Play Console

- Store assets: `docs/store-assets/icon-512.png`, `docs/store-assets/feature-graphic-1024x500.png`
- Web pages (host on the TeamExyKings GitHub Pages repo under `King-System-App-Remover/`):
  `index.html`, `privacy-policy.html`, `terms-of-service.html`
- Permission declaration required for `QUERY_ALL_PACKAGES` — use case: device search / app
  management, as the app's only purpose is listing and uninstalling installed apps.
- Trader status: "No, trader status does not apply" (free app, consistent with the other King apps).

## Project layout

```
.
├── index.html, privacy-policy.html, terms-of-service.html
├── README.md, CHANGELOG.md, RELEASE_NOTES_1.0.0.md
├── build.gradle.kts, settings.gradle.kts, gradle.properties
├── gradle/libs.versions.toml, gradle/wrapper/gradle-wrapper.properties
├── docs/store-assets/
└── app/
    ├── build.gradle.kts, proguard-rules.pro
    └── src/main/{AndroidManifest.xml, java/..., res/...}
```

---

© 2026 TeamExyKings. All rights reserved. Built by Yashwanth Ram Somireddy.
