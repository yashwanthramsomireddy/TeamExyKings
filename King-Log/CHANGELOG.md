# Changelog

All notable changes to KingLog. Versions follow `MAJOR.MINOR.PATCH`; the Android `versionCode` goes up by one for every build you ship.
Only the latest version is kept here. Earlier versions are in git history. Install notes and pre-release reminders for this build are in [RELEASE_NOTES.md](RELEASE_NOTES.md).

## [2.3.0] (versionCode 13): 2026-11-11

### Changed
- **Compact is now the default layout** (`Prefs.Defaults.DENSITY = UiDensity.COMPACT`). The two choices in Settings ▸ Appearance ▸ Layout are now called **Roomy** and **Compact** (`SettingsParts.DENSITY_LABELS`); the stored names (`DEFAULT`, `COMPACT`) are unchanged, so nothing is migrated. A person who never touched the setting sees Compact after updating; anyone who picked a layout keeps it. *Reset settings to default* returns it to Compact.
- **Compact Home keeps its charts.** The Finance line, the Fuel bars and the Vault bar are no longer hidden in Compact; they are drawn smaller (`HomeCards.kt`: line 46 dp instead of 64 dp, bars 20 dp instead of 26 dp, vault bar 24 dp instead of 32 dp). Only the spacing and the logo tile shrink.
- **About ▸ Visit Website** opens `https://teamexykings.in/` (`Config.SITE_URL`, shown as `teamexykings.in`). Privacy Policy, Terms of Use, FAQ and Delete your data keep opening `https://teamexykings.in/King-Log/...` (`Config.PAGES`).
- **Website:** `icon-192.png` is redrawn as a rounded dark tile with a larger mark (`tools/logo_geometry.py` `tile_svg`, `tools/make_icons.py`; `apple-touch-icon.png` and `icon-512.png` are regenerated too). The icon image is also removed from the top of `faq.html` and `account-deletion.html`, so all five pages now match (the browser-tab icon and link-preview image stay). `index.html` says 2.3.0 and names the layouts Compact (default) and Roomy.
- FAQ (89 answers): *What is on the Home screen?* mentions the new buttons; the Compact-layout, text-size and "Where are the privacy policy and the terms?" answers are updated. The privacy policy names the layouts "Compact or Roomy" (`LegalData.kt` regenerated).
- **Project folder layout** now matches the owner's own folder: the web pages and `assets/` sit in the project root, store artwork and listing text in `docs/play-store/`, demo import files in `docs/sample-data/`. Paths in `tools/*.py`, `LegalDataTest`, `LegalModel`, the READMEs and `LEGAL_AUDIT.md` follow. `README.md` and `CHANGELOG.md` are no longer shipped as separate copies for the website.

### Added
- **Home header: round `i` (About) and `?` (FAQ) buttons next to the Settings gear** (`HomeHeader` in `ui/home/HomeCards.kt`, `onAbout` / `onFaq` in `HomeScreen`, wired in `AppNav`). Settings ▸ About and FAQ are still there as before. The buttons have spoken labels for TalkBack.

### Removed (files to delete)
- `ui/settings/AppsMarquee.kt`, `ui/settings/CreditsDialog.kt`, `ui/settings/TeamApps.kt`: old, unused files that nothing references any more.

### Tests
- 201 unit tests pass (no new ones; `HomeMathTest` and `LegalDataTest` still cover the Home maths and the in-app legal text). The Home header and cards were compiled against stand-in Android / Compose APIs only; `AppNav`, Settings, Welcome and the backup screens were reviewed by hand.

## Roadmap (not built; decide when you want them)
- [ ] Vault ▸ Notes tab (a Notes tab only; or with import from .txt / .sdocx files and a review screen; or either plus a per-vehicle service checklist).
- [ ] Live fuel price in the Fuel Log (each person's own API key, or a daily file from your site).
- [ ] Optional password-protected Excel export for the EMI plan.
- [ ] Optional auto-wipe after many failed unlocks.
- [ ] Optional 2-step sign-in add-on.
- [ ] Encrypt the Fuel database too (SQLCipher) if it ever holds more than vehicle data.
- [ ] Excel / CSV export of Finance months (the Vault stays encrypted-backup only, by design).
