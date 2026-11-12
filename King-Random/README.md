# King Random

Random numbers, dice, coin flips, a decision spin wheel and a list shuffler — in one small,
offline Android app.

A **TeamExyKings** product, built by Yashwanth Ram Somireddy.

- **Package:** `com.teamexykings.kingrandom`
- **Version:** 1.0.0 (versionCode 1)
- **Free forever · No ads · No subscription · No sign-in · No tracking**

---

## What it does

| Tool | What you get |
|---|---|
| **Random number** | Any whole-number range (negatives included), up to 1000 numbers at a time, optional "no repeats", optional sorting, quick range presets |
| **Dice roller** | D4 / D6 / D8 / D10 / D12 / D20 / D100 plus any custom 2–1000 sides, up to 10 dice, real pip faces for D6, tumbling roll animation, running total |
| **Coin flip** | 3D flip animation, single or multi-flip (up to 100), lifetime heads/tails counts, current streak and best streak |
| **Spin wheel** | Multiple named wheels, up to 30 options each, animated canvas wheel with labels, optional "remove winner after each spin" for raffle draws |
| **List shuffler** | Paste one item per line (commas work too) — shuffle everything, draw N at random, or split into 2–10 balanced teams; save and reload named lists |

Plus a searchable **FAQ**, a result **History** with per-tool filters, and **Settings** with theme,
haptics, animations, keep-screen-on and JSON backup/restore.

---

## Tech stack

- Kotlin + Jetpack Compose + Material 3
- MVVM with manual ViewModel factories (no DI framework)
- DataStore (Preferences) for all persistence
- Navigation Compose
- SplashScreen API
- kotlinx.serialization for stored collections and the backup file
- AGP 8.9.1 · Gradle 8.11.1 · JDK 17 · compileSdk 35 · minSdk 24 · targetSdk 35

No internet permission. No runtime permissions at all.

---

## Project layout

```
app/src/main/java/com/teamexykings/kingrandom/
├── KingRandomApp.kt            Application + app-wide constants (URLs, support email)
├── MainActivity.kt             Splash, theme wiring, keep-screen-on
├── data/
│   ├── Models.kt               AppSettings, HistoryEntry, Wheel, ShuffleList, CoinStats, Tool
│   └── KingRandomRepository.kt  All DataStore reads/writes + backup/restore
├── ui/theme/                   Color.kt, Theme.kt (4 schemes), Type.kt
├── ui/components/              CommonUi.kt, Fields.kt, Utils.kt (share/copy/haptics)
├── ui/navigation/              KingRandomNavHost.kt + Routes
├── ui/viewmodel/               One ViewModel per tool + SettingsViewModel + factory
└── ui/screens/                 Home, Number, Dice, Coin, Wheel, Shuffle,
                                About, Credits, FAQ, Settings, History
```

Web pages (`index.html`, `privacy-policy.html`, `terms-of-service.html`) sit at the project root
and are deployed to `teamexykings.in/King-Random/`.
Play Console assets are in `docs/store-assets/`.

---

## Before the first build

This zip does not include the Gradle wrapper binary. Copy these three items into the project root
from any of your existing King app project folders:

```
gradlew
gradlew.bat
gradle/wrapper/gradle-wrapper.jar
```

(`gradle/wrapper/gradle-wrapper.properties` is already included and pinned to Gradle 8.11.1.)

Then open the folder in Android Studio and let it sync.

## Release signing

Signing properties are read from `C:\Users\Yash\.gradle\gradle.properties` and are never committed:

```
KR_STORE_FILE=C:/path/to/king-random.jks
KR_STORE_PASSWORD=...
KR_KEY_ALIAS=...
KR_KEY_PASSWORD=...
```

If those properties are absent the release build simply stays unsigned, so a debug build works
without any setup.

Build the release bundle with:

```
gradlew bundleRelease
```

---

## Play Console checklist

- Store listing icon: `docs/store-assets/icon-512.png`
- Feature graphic: `docs/store-assets/feature-graphic-1024x500.png`
- Data safety: **no data collected, no data shared** — the app has no internet permission
- Ads: **no ads**
- Content rating: suitable for everyone
- Privacy policy URL: `https://teamexykings.in/King-Random/privacy-policy.html`
- Trader status: does not apply (free app, no purchases)

---

## Licence

Copyright © 2026 TeamExyKings (Yashwanth Ram Somireddy). All rights reserved.
This software is proprietary. See `LICENSE`.
