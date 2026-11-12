# Changelog

All notable changes to King App Backup are recorded here.
Only the latest released version is kept in this file; the full history lives in the app's
Changelog dialog (About → Changelog).

## [1.0.0] — 12 November 2026

**versionCode 1 · versionName 1.0.0**

### Added
- Installed tab listing every app on the device with icon, version, size and last-updated date.
- Archived tab listing everything already saved in the backup folder.
- Multi-select backup with a progress overlay and a per-run summary (saved / skipped / failed).
- Storage Access Framework folder picker — one folder, granted by you, nothing else readable.
- Split APK support: apps delivered as App Bundles are archived as a single `.apks` file.
- Restore from the Archived tab — single APKs through Android's installer, `.apks` bundles through
  a PackageInstaller session.
- Share selected packages through the system share sheet.
- Uninstall selected apps from the Installed tab, queued one at a time.
- Search and sort (name, size, date) on both tabs.
- Settings: backup path, theme, keep old version apk, show system apps, back up split APKs.
- About screen with website, privacy policy, terms of use, changelog, credits, support, rate and share.
- FAQ screen with categories and search.
- Pitch black AMOLED theme by default, plus Dark and Light.

### Notes
- No internet permission. Nothing is collected, nothing is sent.
- Without root access Android only permits the APK to be copied — app data is not part of a backup.
