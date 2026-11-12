# Changelog — King PDF Reader

All notable changes to this app are recorded here. Versions follow
[Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-11-12

versionCode 1 · First release

### Added

**Reader**
- Continuous vertical scrolling through the whole document, built on Android's own
  `PdfRenderer`
- Pinch zoom and double-tap zoom between 100% and 500%
- Pages re-rendered at each zoom step instead of being scaled up, so text stays sharp
- Night mode that inverts the page for dark-room reading
- Fit-width and fit-page layouts, with an optional gap between pages
- Tap the middle of the page to hide and show the toolbars
- Optional tap-the-edges scrolling: lower third forward, upper third back
- Page counter with a go-to-page dialog, plus a seek slider
- Clear messages for password-protected, missing, unreadable and permission-lapsed files

**Bookmarks and recent files**
- Bookmarks per document, with rename, jump and delete from a bottom sheet
- Recent files list with saved reading position, progress bar and last-opened time
- Pin a file to keep it at the top and out of "Clear recent files"
- Search recent files by name
- Opens PDFs handed over from file managers, mail clients and other apps

**Settings**
- Night mode, resume at last page, keep screen on, tap-to-scroll
- Page layout (fit width / fit page) and page gaps
- Render quality: Balanced, Sharp, Sharpest
- Clear recent files, clear all bookmarks
- Back up and restore settings, recent files and bookmarks as a JSON file

**App**
- Pitch-black AMOLED theme (`#000000`) with a white accent, locked by design
- About screen with website, privacy policy, terms of use, changelog, credits, FAQ,
  support, rate and share
- FAQ with six categories and full-text search
- About (i), FAQ (?) and Settings icons in the home screen's top-right corner
- Splash screen via the AndroidX SplashScreen API

### Notes
- No `INTERNET` permission is declared, so the app cannot make network requests
- No ads, analytics, crash reporting or accounts
- Encrypted, password-protected PDFs cannot be opened; Android's PDF engine does not
  support them
