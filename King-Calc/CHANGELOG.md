# Changelog

All notable changes to King Calc are documented here. Older released versions are trimmed from this file over time — only the latest released version and any work in progress are kept.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project follows [Semantic Versioning](https://semver.org/).

## [4.9.0] (versionCode 39) - next build

### Changed
- Calculator header: the four quick-access icons reordered to FAQ, About, History, Settings — Settings now sits in the top-right corner

### Added
- Export icon (PDF or image) on Loan/EMI, Chit Fund and Split bill, shown once there's data to export — shares a branded report (header, footer, generated date) through the system share sheet
- Changelog button in About, between Terms of use and Credits — shows every version ever released, newest first
- Rate us button in About, between Support/Contact and Share King Calc — opens the Play Store listing directly

### Fixed
- Keyboard opening then immediately closing when tapping into a number field (BMI and other screens) — caused by the app's own status-bar/navigation-bar theme colors fighting with Android's own edge-to-edge handling; removed the now-redundant native theme flags so Android owns that transition cleanly
