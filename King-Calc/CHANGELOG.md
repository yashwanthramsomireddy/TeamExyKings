# Changelog

All notable changes to King Calc are documented here. Older released versions are trimmed from this file over time — only the latest released version and any work in progress are kept.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project follows [Semantic Versioning](https://semver.org/).

## [6.3.0] (versionCode 61)

### Added
- King Calc now checks Google Play for a newer version: the About screen shows a highlighted "An update is available" card with an "Update on Google Play" button whenever one's live, and the Calculator (Home) screen shows a one-time popup (Update / Later) the first time it notices — dismissing it either way means it won't show again until an even newer version appears
- "More TeamExyKings apps" button added to the About screen (right after Share King Calc), opening the TeamExyKings developer page on Google Play
- Pressing Back on the Calculator screen now asks "Exit App?" with No, Stay / Yes, Exit, instead of closing the app immediately
- Compare tab (Gold/Silver) now also shows 22K and 18K gold cards, alongside the existing 24K gold and silver cards
- Gold and Silver cost calculators now have a separate "Wastage charges %" field (default 0%, since not every shop charges it) alongside the existing Making charges and GST fields — raising it increases the estimated store price the same way Making charges does

### Changed
- Gold Shop Price tab: "Per gram (18K)" is now bold, matching the existing 22K/24K rows

### Notes
- The update check uses Google's own Play In-App Updates library, so King Calc still makes no internet request for it — it can only detect an update when installed through Google Play (including closed testing); a sideloaded build quietly shows nothing
