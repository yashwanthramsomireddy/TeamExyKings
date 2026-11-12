# Changelog — King System App Remover

All notable changes to this project are documented here.
The full history is also available in the app under About → Changelog.

## [1.0.0] — 12 November 2026

**versionCode 1 · versionName 1.0.0**

### Added
- User apps and System apps in separate tabs, each with a live count.
- Batch uninstall: select any number of user-installed apps and work through Android's confirm
  dialogs in one queued pass, with a Stop control while the queue runs.
- Single-app uninstall from the details sheet.
- Sorting by name, size, install date, last updated, last used and package name, ascending or
  descending.
- Filter chips: All, Unused 30d+, Over 100 MB, Recently installed, Disabled.
- Search across app names and package names.
- Storage freed counter — running total of reclaimed bytes and apps removed, with a reset in
  Settings.
- Optional usage access for last-used dates and real app sizes (app + data + cache) via
  StorageStatsManager; the app falls back to APK size when it isn't granted.
- Details sheet on long-press: version, size, install and update dates, last used, installer,
  APK path, enabled state, plus Open app, App info, Play Store and Share actions.
- System apps tab is read-only with a clear explanation that Android does not allow removing
  pre-installed apps without root, and a shortcut to each app's info screen for disabling.
- Share the current list as a real `.txt` file through the system share sheet.
- Settings backup and restore as JSON.
- Settings screen: list display toggles (size, install date, last used, package name, APK path),
  hide disabled apps, confirm before uninstalling, open on System apps tab, usage access shortcut,
  storage counter reset, export/import, privacy policy, terms of use and support links.
- About screen with a breathing glow app mark, version, FAQ, privacy policy, terms of use,
  changelog, support, rate us, share and credits.
- FAQ screen with categories and search.
- Pitch-black AMOLED theme (#000000) with a white accent throughout, SplashScreen API, edge-to-edge.
