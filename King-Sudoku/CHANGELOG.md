# Changelog

All notable changes to King Sudoku are documented here.

## [1.0.0] - 2026-11-11 (unreleased / not yet build-tested)

### Added
- Full Sudoku engine with unique-solution generator and four technique-graded
  difficulty levels (Easy, Medium, Hard, Expert)
- Game board with notes/pencil-mark mode, erase, mistake highlighting,
  pausable timer, and auto-save
- Hint system that explains the technique used (Naked Single, Hidden Single,
  or a plain fill when a deeper guess-and-check is genuinely required)
- Daily Puzzle screen, generated on-device from the calendar date, with its
  own completion badge and streak
- Stats & Streaks screen: played/won per difficulty, best times, overall
  streak, daily-puzzle streak
- Settings: Pitch Black (default) / White / System theme, 5 font choices,
  8-swatch accent colour picker, board option toggles (highlight same
  number, auto-check mistakes, optional mistake limit)
- Local Backup (export) and Restore (import) to a single JSON file via the
  system file picker &mdash; covers settings, stats, streaks, and the
  current/daily in-progress game
- Home screen top bar: quick-access icons for Stats, About, FAQ and Settings
  (About/FAQ are also still reachable from inside the About screen as before)
- About screen: Visit Website, Privacy Policy, Terms of Use, Credits (by
  tech stack), Support/Contact (timestamped email), Share App, FAQ &mdash;
  with the app icon shown at the top, its corner brackets blinking (not a
  glow halo, matching the RecruitLens treatment)
- FAQ screen with category sections and a search bar
- Unique launcher icon: rounded gold frame, minimal 3x3 grid, solid gold
  "solved" cell with a highlight, single corner-bracket accent
- Web pages (`index.html`, `privacy-policy.html`, `terms-of-service.html`)
  for `teamexykings.in/King-Sudoku/`; Visit Website links to the main
  `teamexykings.in/` site
- Store assets: `docs/store-assets/icon-512.png`,
  `docs/store-assets/feature-graphic-1024x500.png`

### Fixed
- Build failure on `:app:assembleRelease` &mdash; Kotlin 2.0+ requires the
  Compose Compiler Gradle plugin as its own plugin; added
  `org.jetbrains.kotlin.plugin.compose` to the root and app `build.gradle.kts`
  and removed the now-obsolete `composeOptions { kotlinCompilerExtensionVersion }`
  block

### Known Issues
- Not yet build-tested on a physical device
- Font choices beyond System Default / Monospace currently fall back to the
  platform's default sans-serif until actual Inter/Nunito/Poppins `.ttf`
  files are added under `res/font/`
