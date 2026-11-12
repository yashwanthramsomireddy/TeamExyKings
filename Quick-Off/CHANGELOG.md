# Changelog

All notable changes to QuickOff will be documented in this file.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).  
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.3] — 2026-08-27

### Added
- **Auto Desktop shortcut on first launch** — when QuickOff runs for the first time it automatically creates `QuickOff.lnk` on the Desktop (with the correct icon). A PowerShell script is written to the temp folder and executed silently — the same proven method `setup.bat` uses. A registry flag (`HKCU\Software\QuickOff\ShortcutCreated`) ensures this only happens once.

### Fixed
- **GitHub repo URL** corrected to `yashwanthramsomireddy/QuickOff` everywhere: About panel link, update badge link, and the GitHub Releases API check.
- **Desktop shortcut path resolution** — switched from `window.location.pathname` to `window.location.href` + `unescape()` so paths containing spaces are decoded correctly before the shortcut is created.

---

## [1.5.5] — 2026-08-27

### Fixed
- **Explorer file icon** — `setup.bat` now silently registers `HKCU\Software\Classes\htafile\DefaultIcon` on every run, so `QuickOff.hta` shows the amber bolt icon in File Explorer without any extra step.
- **Desktop shortcut icon** — `setup.bat → [3]` creates a `.lnk` shortcut on the Desktop with `IconLocation` set; the shortcut always shows the correct icon.
- **Icon auto-extracted** — `setup.bat` embeds `QuickOff.ico` as base64 and writes it to the folder automatically if it is missing.

### Known limitation
- **Title bar & taskbar icon while running** — Windows 11 ignores the `ICON=` attribute in `HTA:APPLICATION` for running HTA windows; `mshta.exe`'s own system icon is always shown instead. This is a Windows 11 OS-level restriction and cannot be overridden from within an HTA.

---

## [1.5.4] — 2026-08-27

### Fixed
- **Title bar & taskbar icon** — `launch.bat` now embeds `QuickOff.ico` as base64 and extracts it via PowerShell *before* launching the HTA. Because the HTA `ICON=` attribute is parsed at load time (before JS runs), the icon must exist on disk first — the bat guarantees this.
- **Explorer / folder icon** — `setup.bat` option `[3] Register icon` writes to `HKCU\Software\Classes\htafile\DefaultIcon` and restarts Explorer, making `QuickOff.hta` show the amber bolt icon in File Explorer.
- **Desktop shortcut with icon** — `setup.bat` option `[4]` creates a `.lnk` on the Desktop with `IconLocation` set, so the shortcut shows the correct icon too.

---

## [1.5.3] — 2026-08-27

### Fixed
- **Icon not showing in title bar / taskbar** — the HTA now auto-extracts `QuickOff.ico` next to itself on first launch using `ADODB.Stream` + `MSXML2`, so the `ICON=` attribute always finds it even if the user forgot to keep the `.ico` alongside the `.hta`.

### Changed
- **`setup.bat`** gains option `[3] Create desktop shortcut (with icon)` — uses PowerShell to create a proper `.lnk` on the Desktop with `IconLocation` pointing to `QuickOff.ico`, which fixes the **Explorer / Desktop icon** (Windows assigns icons to shortcuts, not raw `.hta` files).

---

## [1.5.2] — 2026-08-27

### Changed
- Window size updated to **250 × 330 px**.

---

## [1.5.1] — 2026-08-27

### Added
- **App logo rendered inside the widget** — `QuickOff.ico` is embedded as a base64 data URI directly in the HTA so it displays in two places: as a 48 × 48 px image at the top of the About overlay, and as a 14 × 14 px icon beside the "QuickOff" label in the footer. No external file reference needed at runtime — the logo is self-contained in the HTA.

---

## [1.5.0] — 2026-08-27

### Fixed
- **Dark theme text invisible** — all dark-mode text colours bumped to readable values: power button labels `#ccc`, delay label `#888`, status `#444` (idle) / `#60a5fa` (active), icon colours `#555`, about-overlay author `#777` / name `#ccc`. Every element now has full contrast against the `#111` background.

### Changed
- **GitHub link** updated to `https://github.com/yashwanthramsomireddy/PurgeKit` (both About panel and update-badge).
- **Abort button removed** — footer is now just the status line; the `× Abort` button is gone.
- **Window size** reduced to **240 × 270 px** — tighter and more compact.

---

## [1.4.1] — 2026-08-27

### Added
- **Custom app icon** (`QuickOff.ico`) — amber lightning bolt on a dark circle background, included at 16 / 32 / 48 / 64 / 256 px. Referenced via `ICON="QuickOff.ico"` in `HTA:APPLICATION`; appears in both the title bar and the Windows taskbar. The `.ico` file must stay in the same folder as `QuickOff.hta`.

### Changed
- Window size increased to **240 × 380 px** so all six power actions, the delay row, and the footer are fully visible without scrolling.

---

## [1.4.0] — 2026-08-27

### Changed
- **Light theme is now the default** — the widget opens in light mode; the theme toggle (☾ / ☀) switches to dark.
- **Startup Manager tab removed** — power menu is now a single focused panel with no tab bar overhead.
- **Window resized to 220 × 332 px** — all six actions, the delay row, and the footer are fully visible without clipping.
- `BORDER="none"` + `CAPTION="no"` set in HTA:APPLICATION — removes the OS title bar on Windows 10; Windows 11 enforces the title bar for `.hta` files as a system security policy and cannot be overridden.

---

## [1.3.1] — 2026-08-26

### Added
- **About panel** — `ℹ` button in the footer opens a small overlay showing the app name, version, MIT license note, author credit ("Built by Yashwanth Ram Somireddy / TeamExyKings"), and a **View on GitHub** button that opens the repo in the default browser. Fully themed (dark and light). Closes with a dedicated Close button.

---

## [1.3.0] — 2026-08-26

### Added
- **Auto-update check** — on launch, QuickOff silently queries the GitHub Releases API (`Msxml2.XMLHTTP.6.0`, async so the UI never blocks). If a newer version is detected, a green `↑ Update` badge appears in the footer and the status bar shows the available version. Clicking the badge opens the releases page in the default browser. Set `GITHUB_REPO` at the top of the HTA to your `username/repo` before publishing.
- **Startup Manager tab** — new "Startup" tab alongside the "Power" tab. Reads both `HKCU` and `HKLM` `\Software\Microsoft\Windows\CurrentVersion\Run` registry keys by running `reg query` to a temp file and parsing `REG_SZ` entries. Displays each item's name and hive; HKCU items have a **Remove** button; HKLM items are shown read-only (lock icon, admin required). Refresh button reloads the list. **Add QuickOff** button registers the current HTA path into HKCU for autostart (replaces `setup.bat` workflow from the UI).
- Window width increased from 230 px to 240 px for the wider tab bar.

---

## [1.2.0] — 2026-08-26

### Fixed
- Removed VBScript `RunCmd` sub entirely — the JS-to-VBScript bridge breaks on Windows 11, causing a script error on every button press. All shell commands now use `new ActiveXObject("WScript.Shell")` directly in JavaScript, no VBScript required.
- Close button was too small (16px) and low-contrast to see against the dark background; enlarged to 20px with a visible border.
- Window width was clipping button labels on the right; increased from 220px to 230px.

### Added
- Light / dark theme toggle (☀ / ☾ button in footer) — switches body class and recolours all elements: buttons, dividers, delay row, footer, overlay, and status text.

---

## [1.1.2] — 2026-08-26

### Changed
- Title bar (internal `#top-gap` strip) removed — widget is now fully chrome-free inside the content area.
- Close (×) button and app label moved to the footer row.
- App label colour made visible (`#555` vs near-invisible `#2a2a2a`).
- Window width increased from 200px to 220px — button labels were being clipped.
- Window height reduced to 274px to account for removed top gap (no height increase).

### Fixed
- Drag support restored — `#app-label` in the footer now acts as the drag handle.

---

## [1.1.1] — 2026-08-26

### Fixed
- **Critical bug**: `doAction()` called `hideOv()` before saving `pending`, causing `pending` to be cleared to `""` before the action check — meaning every button silently did nothing after confirming. Fixed by capturing `pending` into a local variable before calling `hideOv()`.
- **Lock not working**: wrapped `rundll32.exe user32.dll,LockWorkStation` in `cmd /c` for reliable execution from WScript.Shell context.
- **Sleep reliability**: same `cmd /c` wrapper applied to `powrprof.dll,SetSuspendState`.
- **Window not draggable**: drag handler re-added to `#top-gap` area (was lost when title bar was removed in v1.1.0).

---

## [1.1.0] — 2026-08-26

### Changed
- Complete UI redesign — pitch black (`#080808`) minimal theme
- Layout changed from 2-column card grid to compact single-column list (icon left, label right)
- Removed title/drag bar entirely — window chrome stripped to zero (`BORDER="none"`, no caption)
- Close button is now a small floating circle (top-right corner, always accessible)
- Version badge replaces app name label — ultra-subtle `QUICKOFF` text top-left
- Button height reduced to 36px — coloured left-border accent on hover instead of card fill
- Window height reduced from 480px to 306px — significantly more compact
- Confirm overlay simplified — shorter text, smaller footprint
- Abort row slimmed to single line with inline status
- Dividers between every action for clear visual separation

### Fixed
- Removed unused drag-bar VBScript handler

---

## [1.0.0] — 2026-08-26

### Added
- Floating dark power menu widget (`QuickOff.hta`) for Windows 10 and 11
- Six power actions: Hibernate, Sleep, Shut Down, Restart, Lock PC, Sign Out
- Delay timer — schedule any action by a custom number of seconds
- Countdown display in the status bar with live tick
- Abort button — cancels any active countdown or pending `shutdown /s /r` command
- Confirm overlay — every action prompts before executing
- Draggable window — click and drag the title bar to reposition
- Auto-position on launch — snaps to bottom-right of the screen
- CLI launcher (`launch.bat`) supporting all six actions plus delay and abort
- Setup script (`setup.bat`) for enabling and disabling autostart on Windows login via the `HKCU` Run registry key (no admin required)
- MIT License
- README with usage guide, compatibility table, and contributing instructions
- CHANGELOG

### Technical
- IE-compatible layout using HTML tables (CSS Grid is not supported in the HTA / mshta.exe engine)
- HTML entity icons instead of emoji (emoji rendering is broken in the IE rendering engine used by mshta.exe)
- VBScript `WScript.Shell` bridge for running system commands from JavaScript
- Pure Windows built-ins only: `mshta.exe`, `shutdown`, `rundll32`, `reg`

---

<!-- Add future releases above this line -->
