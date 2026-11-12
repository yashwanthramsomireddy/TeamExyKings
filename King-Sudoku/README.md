# King Sudoku

A free, offline, ad-free Sudoku app for Android, built under the TeamExyKings brand.

## Features

- Four difficulty levels (Easy, Medium, Hard, Expert), graded by the actual solving
  techniques a puzzle requires, not just its clue count
- Hand-written generator that guarantees every puzzle has a **unique solution**
- Notes / pencil-mark mode, undo (via Erase), mistake highlighting, pausable timer
- Daily Puzzle: generated on-device from the date, fully offline, with its own streak
- Stats & Streaks: played/won per difficulty, best times, daily streaks
- Auto-save: the current game (board, notes, timer, mistakes) survives an app kill
- Pitch Black (default), White, and System theme, 5 font choices, accent colour picker
- Local Backup / Restore to a single JSON file via the system file picker &mdash; no cloud
- No ads, no subscription, no login, no analytics, no tracking

## Tech Stack

- Kotlin, Jetpack Compose + Material3
- MVVM with manual ViewModel factories (no Hilt / DI framework)
- Jetpack DataStore (Preferences) for settings, stats, and auto-saved game state
- Navigation Compose
- AndroidX SplashScreen API
- Min SDK 29, Target SDK 36, AGP 8.9.1, Gradle 8.11.1

## Project Structure

```
app/src/main/java/com/teamexykings/kingsudoku/
├── engine/        # Pure Sudoku logic: solver, generator, technique grading/hints
├── data/
│   ├── model/     # AppSettings, GameState, Stats, BackupPayload
│   ├── datastore/ # DataStore-backed persistence (SudokuDataStore)
│   ├── repository/# SudokuRepository (engine + persistence + stats), StatsUpdater
│   └── backup/    # JsonCodec + BackupManager (SAF export/import)
├── ui/
│   ├── theme/     # Pitch Black / White / System theme, fonts, accent colour
│   ├── components/# Shared PillButton / RoundedCard
│   └── screens/   # home, game, stats, settings, about, faq
├── navigation/    # NavGraph (Routes + KingSudokuNavHost)
├── MainActivity.kt
└── KingSudokuApp.kt
```

## Building

1. Open the project root in Android Studio and let it sync (it will fetch the
   Gradle 8.11.1 wrapper automatically if `gradle/wrapper/gradle-wrapper.jar` is
   missing &mdash; if Studio complains, run **File > Sync Project with Gradle Files**,
   or copy the wrapper jar over from another King-series project).
2. For a signed release build, add to your **user-level**
   `~/.gradle/gradle.properties` (not committed to this repo):
   ```
   KINGSUDOKU_UPLOAD_STORE_FILE=/absolute/path/to/king-sudoku-upload.jks
   KINGSUDOKU_UPLOAD_STORE_PASSWORD=...
   KINGSUDOKU_UPLOAD_KEY_ALIAS=...
   KINGSUDOKU_UPLOAD_KEY_PASSWORD=...
   ```
3. Build > Generate Signed Bundle / APK, or `./gradlew assembleRelease` /
   `./gradlew bundleRelease`.

## Web Pages

`index.html`, `privacy-policy.html`, and `terms-of-service.html` at the project
root are intended to be hosted at `teamexykings.in/King-Sudoku/` (the
`King-Sudoku` folder in the `TeamExyKings` GitHub Pages repo).

## Store Assets

`docs/store-assets/` has `icon-512.png` and `feature-graphic-1024x500.png`
for the Play Store listing.

## Status

v1.0.0 &mdash; first build, not yet tested on-device.
