# King Info

A free, offline, ad-free device information tool for Android, by **TeamExyKings**. Built from scratch in Kotlin + Jetpack Compose.

Scoped to what's realistically achievable on modern Android without root — see the FAQ in-app (and the "Known platform limits" section below) for exactly where and why some fields are unavailable or approximate.

## What it does

A menu screen (a 2-column grid of rectangular tiles, A–Z order) lists 12 sections; tapping one drills into a detail screen. There's a single **Share** action on the menu's top bar that builds a full report covering every section and shares it as one real `.txt` file.

- **Android** — version, API level, security patch, Android ID, baseband, bootloader, build fingerprint, kernel version/arch, Google Play Services version, uptime, language, timezone, and a best-effort root check.
- **Battery** — level, status, health, technology, temperature, voltage, charge counter, current, cycle count (Android 14+ where reported).
- **Codecs** — every supported media codec, searchable.
- **CPU** — instruction set, core count, per-core live clock speed (where readable), clock range, supported ABIs, crypto extension support (AES/NEON/PMULL/SHA1/SHA2).
- **Devices** — camera specs per lens (resolution, focal length, focus modes, stabilization, flash) and fingerprint-sensor presence/enrollment status.
- **Directories** — standard storage paths plus a live `/proc/mounts` dump.
- **Display** — resolution, density, refresh rate, orientation, plus a GPU summary (vendor/renderer/version) read via a throwaway off-screen GL context.
- **Network** — telephony (operator, SIM state, network type) and Wi-Fi (SSID, BSSID, IP, signal, link speed, frequency, band support).
- **Sensors** — every sensor on the device; tap one to see its live values update in real time.
- **System** — model, manufacturer, board/hardware/product, SoC (Android 12+), RAM, storage, and the full `android.hardware.*` device feature list.
- **System Files** — friendly-name → `/proc` path reference list; tap to preview the file's content (or a clear "not accessible" message).
- **Thermal** — live SoC thermal zone temperatures (where the manufacturer allows reading them).

## Theme

One fixed theme: pure AMOLED black (`#000000`) with a white accent. No light mode, no system mode, no color picker — there's nothing to configure, so there's no Settings screen.

## Known platform limits (by Android design, not app bugs)

- **No IMEI/serial number** — blocked for all third-party apps since Android 10, no workaround exists.
- **No "Apps" section** — a full installed-app list needs `QUERY_ALL_PACKAGES`, which Play Store reviews strictly and often rejects; left out by design decision rather than risk the listing.
- **No raw Vulkan limits/extensions dump** — intentionally scoped down to a readable GPU summary (vendor/renderer/version); see FAQ.
- **Wi-Fi SSID** shows `<unknown ssid>` without the location permission granted — this is Android's own restriction since 8.1, not something any app can bypass.
- **Per-core live CPU frequency** and **thermal zone data** come from sysfs paths that are world-readable on most stock Android builds but some OEMs restrict — King Info shows "Not readable" rather than a guess when that happens.
- **Fingerprint sensor manufacturer/model** is not exposed by any public API to any app — King Info can only report hardware presence and enrollment status.
- **Root detection** is a best-effort heuristic only, never a guarantee.

## Project structure

```
KingInfo/
├── app/                        Android Studio module
│   ├── build.gradle.kts        Dependencies + release signing config
│   └── src/main/
│       ├── java/com/teamexykings/kinginfo/
│       │   ├── data/            One provider per section (System/CPU/Display/.../SystemFiles), InfoModels, SectionLoader, ReportFormatter, ReportFileSharer
│       │   └── ui/              Compose screens (menu, detail, sensors, codecs, systemfiles, about, faq), theme, components
│       ├── AndroidManifest.xml  Includes the FileProvider used for the full-report .txt share
│       └── res/
│           └── xml/file_paths.xml   FileProvider's exposed cache path
├── web/kinginfo/                index.html, privacy-policy.html, terms-of-service.html
│                                 — upload to teamexykings.in/kinginfo/
├── README.md
├── CHANGELOG.md
└── RELEASE_NOTES_v1.1.0.md
```

## Opening in Android Studio

1. Open the `KingInfo` folder as a project (Android Studio will regenerate the Gradle wrapper jar on first sync if it's missing — this zip ships the wrapper properties but not the binary jar).
2. Add your release-signing keys to your **user-level** `~/.gradle/gradle.properties` (not this project's `gradle.properties`):
   ```
   KINGINFO_UPLOAD_STORE_FILE=/absolute/path/to/kinginfo-upload-key.jks
   KINGINFO_UPLOAD_STORE_PASSWORD=********
   KINGINFO_UPLOAD_KEY_ALIAS=kinginfo
   KINGINFO_UPLOAD_KEY_PASSWORD=********
   ```
   Without these, `assembleDebug` still works for local testing; only the signed release build needs them.
3. Build & run on your device to test.

## Minimum / target SDK

`minSdk 26` (Android 8.0), `targetSdk 35`.

## Files to delete if you're overwriting an older local copy

If updating from v1.0.0 or earlier, delete these (removed in v1.1.0):
```
app/src/main/java/com/teamexykings/kinginfo/ui/screens/settings/SettingsScreen.kt
app/src/main/java/com/teamexykings/kinginfo/ui/screens/settings/SettingsViewModel.kt
app/src/main/java/com/teamexykings/kinginfo/util/PrefsManager.kt
app/src/main/java/com/teamexykings/kinginfo/ui/components/GroupedSectionHeader.kt
```
If updating from v1.1.0, also delete these (replaced by *Content.kt equivalents in v1.2.0):
```
app/src/main/java/com/teamexykings/kinginfo/ui/screens/detail/DetailScreen.kt
app/src/main/java/com/teamexykings/kinginfo/ui/screens/sensors/SensorsScreen.kt
app/src/main/java/com/teamexykings/kinginfo/ui/screens/codecs/CodecsScreen.kt
app/src/main/java/com/teamexykings/kinginfo/ui/screens/systemfiles/SystemFilesScreen.kt
```
Everything else can be overwritten in place.
