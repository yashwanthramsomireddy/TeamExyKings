# TeamExyKings Website — Changelog

---

## v3w — 2026-09-15

### New
- **White / light theme** — re-added a full `[data-theme="white"]` CSS variable block (bg, text, borders, cards, nav, glow, particle tint) alongside the existing AMOLED black theme. All the theme-scoped `--fg02`…`--fg90` translucency tokens (used for hero badges, stat bars, summary cards, tabs, borders, etc.) are now defined per-theme — white (rgba black) and black (rgba white) — so every one of those elements re-tints correctly instead of staying locked to a dark-background assumption.
- **Round theme-toggle button** — new sun/moon icon button in the nav (desktop: next to the nav links; mobile: next to the hamburger). Click toggles theme instantly, persists the choice in `localStorage`, and updates the `theme-color` meta tag to match.
- **Readable hero text in white theme** — `.hero h1`, `.accent`, and `.dim-word` get dark-text + soft-light-shadow overrides so the hero heading stays crisp on the white background; the Three.js scene fog now switches between black and white to match, live, when the theme is toggled (no reload needed) via a new `window._tekSetHeroTheme()` hook.
- **Logo wordmark white-theme fix** — `.bw-1` ("TeamExy") flips from white-fill/black-stroke to dark-fill/white-stroke so it stays legible against the light nav background.
- **Open Graph & Twitter Card tags** — `og:title`, `og:description`, `og:image` (new 1200×630 branded social-preview image at `assets/og-image.png`), `og:url`, `twitter:card`, etc. added to `<head>`.
- **`theme-color` meta tag** — mirrors the AMOLED/white background so mobile browser chrome tints correctly; kept in sync by the theme-toggle JS.
- **PWA manifest** (`manifest.json`) — name, icons (`assets/icon-192.png`, `assets/icon-512.png`, generated from the existing crown mark), theme/background colors, standalone display — site can now be added to the home screen.
- **`sitemap.xml`** and **`robots.txt`** — added as separate root-level files (required by spec; cannot be embedded in `index.html`), with `robots.txt` pointing at the sitemap.
- **Custom `404.html`** — matches the site's AMOLED aesthetic with a link back home, replacing GitHub Pages' generic 404.
- **Privacy-friendly visit counter** — [hits.sh](https://hits.sh) SVG badge (no cookies, no signup, increments on page load) added to the footer, bottom-right corner, styled to sit quietly in both themes.

### Fixed
- **Icon-only button accessibility** — hamburger menu button now has a more descriptive `aria-label` ("Toggle navigation menu"); marquee prev/next arrows already had `aria-label`s (verified, no change needed). New theme-toggle button ships with a dynamic `aria-label` that reflects the action it will perform.
- **`.stat-num` hardcoded white text** — was `color:#fff`, invisible-ish on a white theme background; switched to `var(--text)`.
- **Footer visit badge alignment** — `.footer-visits` given `margin-left:auto` so it stays pinned to the bottom-right of the footer row even when the copyright line wraps onto its own line on narrow viewports.

---

## v3v — 2026-09-15

### Fixed
- **Yash's PayPal link resolved to a local file path** — `paypal.me/yash92duster` had no `https://` prefix, so browsers treated it as a relative link and resolved it against the current page location (showing up as `file:///C:/Users/.../paypal.me/yash92duster`). Added the missing protocol. Scanned every other URL field in the site (GitHub/LinkedIn/YouTube/XDA/PayPal/RazorPay/store links) for the same issue — nothing else was affected.

---

## v3u — 2026-09-15

### Removed
- **Team card flip** — removed entirely. Tapping a card while the marquee was mid-scroll fired the snap-to-left animation and the 3D flip at the same time, which felt chaotic and made cards hard to read/click mid-transition.

### Changed
- **Donate buttons now live on the card front** — PayPal and RazorPay buttons (for members who have them) sit in a row pinned to the bottom of the card via `margin-top:auto`, so every card stays the same height regardless of how much content it has. Members with no donate links simply don't show that row — same as before, just no flip needed to see it.
- Card height increased slightly (240px → 268px) to comfortably fit the donate row without cramping.
- Clicking a card now does exactly one thing: snaps it to the left edge of the marquee. No more competing animations.

---

## v3t — 2026-09-14

### Fixed
- **Hero stat counters still showing 16 developers** — the animated "39 Legacy Builds / 7 Active Projects / 16 Developers" counters near the top of the page were a second, separate hardcoded set (`statTargets`) that v3s's data refresh missed — only the footer ticker text got updated then. Now both read live off `PROJECTS.length`, `totalBuilds`, and `TEAM.length` at animation time, so they can never drift out of sync with the actual data again.

---

## v3s — 2026-09-14

### New
- **Favicon** — added a proper browser tab / bookmark icon: the crown mark on a black rounded square with a blue accent border, embedded as base64 PNG (32×32 tab icon, 180×180 apple-touch-icon for iOS home-screen bookmarks). Previously the site had no `<link rel="icon">` at all, so bookmarks and tabs showed a blank/generic icon.

### Fixed
- **Team card flipping on hover (desktop)** — removed the last remnant of `.team-card:hover` triggering the 3D flip. On a laptop with a mouse, simply hovering to read a link would flip the card away mid-click. Flip is now purely tap/click-driven, matching mobile.

### Changed — data refresh from updated Excel
- **Team roster rebuilt from the Team sheet** — all 17 members now match the spreadsheet exactly: names, handles, and (where marked, not `#NA`) GitHub, LinkedIn, XDA, YouTube, location, PayPal and RazorPay. Added an XDA link to the card front (previously collected but never rendered). Renamed the "UPI Donate" back-face button to "Donate via RazorPay" to match the new column. Members with no donation links get a plain, non-flippable card showing only name/handle/location — no "coming soon" filler, no flip interaction, per your instruction to not show placeholder details for `#NA` members.
- **Legacy Archive rebuilt from the Legacy Builds sheet** — all 37 builds across Note/S/A/Tab Series and Kernels now link to the real Android File Host URLs from the spreadsheet (was previously all `#` placeholders). Kernels section now reflects the sheet exactly (2 builds: Note5/Note7 UX and S7 Edge "Wanted [MM]"), replacing the old 4-build placeholder set.
- **Community links live** — WhatsApp Community and Telegram Channel now link to the real invite URLs and show a "Live" tag instead of "Soon". YouTube stays "Soon" since the sheet still has it as NA.
- **Footer stats ticker numbers corrected** — "39 Legacy Builds" → "37", "16 Developers" → "17", matching the actual rebuilt data.

---

## v3r — 2026-09-14

### Removed
- **Gyroscope parallax + tilt hint** — removed entirely. `DeviceOrientationEvent` support turned out unreliable across Android/Samsung browsers in practice, so it's gone rather than left half-working. The "↔ Tap to enable tilt" button, its CSS, and all gyro JS (`applyGyro`, `bindGyro`, calibration, double-tap recalibrate) are removed.

### Kept
- **Touch-drag parallax** — dragging a finger across the hero on mobile still moves the Three.js spheres; this is unaffected and remains the mobile interaction for the hero.

---

## v3q — 2026-09-14

### Fixed
- **Team card wouldn't flip back on tap** — `.team-card:hover` was flipping the card on touch too, because tapping a touchscreen element triggers a "sticky" `:hover` state in mobile browsers that only clears when a different element is tapped. This fought the JS `.flipped` class toggle. Gated the hover-flip rule behind `@media (hover:hover) and (pointer:fine)` so it only applies on real mouse-hover devices; touch now relies purely on the tap toggle.
- **Gyro not responding on Android (S22 Ultra)** — some Android/Samsung browsers only start firing `deviceorientation` events after an explicit user gesture (like iOS's permission gate, just undocumented). The gyro previously only auto-bound on page load, so it silently never activated on those browsers. The hero hint is now a real **"↔ Tap to enable tilt"** button — tapping it re-binds the listener and recalibrates. It still also tries auto-binding on load for browsers that don't need the gesture. Sensitivity also increased (8° tilt = full range, was 15°) and mobile camera movement doubled for a clearly visible effect. Button changes to "✓ Tilt active" once a real sensor event is confirmed, so you can tell at a glance whether it's working.

---

## v3p — 2026-09-14

### Fixed
- **Card-click-to-left snap not working outside Projects** — the card selectors passed to `setupMarqueeNav` for Legacy, Community, and Credits marquees didn't match their actual card class names (`.legacy-dev-card` → should be `.legacy-device-card`; `.comm-card` → `.community-card`; `.credit-card` → `.about-card`). Fixed all three so clicking any card in any section now snaps it to the left edge.
- **Team card flip toggling on donate-link clicks** — clicking the PayPal/UPI donate button (or any link) inside a team card no longer also toggles the flip state. Clicking anywhere else on a flipped card (not a link) now correctly flips it back to the front.

---

## v3o — 2026-09-13

### New
- **Scroll progress bar** — 2px accent gradient line fixed at the top of the viewport; fills as the user scrolls through the page.
- **Touch-drag parallax** — dragging a finger across the hero section on mobile moves the Three.js spheres (gyro supplement / fallback).
- **Tilt-to-explore hint** — "↔ Tilt to explore" label appears in the hero on mobile only; fades out after the first detected tilt movement.

### Changed
- **Welcome line** — renamed to "Welcome to TEK Family"; font size tripled to 33 px (22 px mobile).
- **Projects tabs → marquee** — all project category tabs (Browser Extension, Android, Windows, Mobile Apps) now render as horizontal marquees instead of a static grid, so the click-to-snap-left behaviour works consistently on every tab. Cards are repeated to maintain visual density when a category has few entries.
- **Gyro sensitivity** — normalisation divisor halved (30 → 15°), so a gentler tilt produces visible movement. Camera influence on mobile increased (x: 0.6 → 1.4, y: 0.3 → 0.7), lerp speed 0.04 → 0.06.

---

## v3n — 2026-09-13

### New
- **Mobile gyroscope parallax** — on mobile, tilting the phone left/right/forward/backward moves all Three.js sphere objects (same parallax effect as desktop mouse movement). Uses `DeviceOrientationEvent`; iOS 13+ requests permission on first hero tap. Double-tap the hero to re-calibrate the gyro zero point.

### Changed
- **Welcome text** — font size reduced from 18 px → 11 px (matching badge text size), uppercase + wider letter-spacing for a label feel.
- **Arrow nav snap** — `nudge()` now uses directional `ceil`/`floor` instead of `Math.round`, guaranteeing the next card always starts flush at the left visible edge regardless of current scroll position.
- **Card click → snap to left** — clicking any card in any marquee (projects, team, community, credits, legacy) snaps that card to the left visible edge before the card's own action (flip, etc.) fires.

---

## v3m — 2026-09-13

### Changed
- **Footer ticker** — replaced dev-names ticker with a stats + facts scroller: `39 Legacy Builds · 7 Active Projects · 16 Developers · Founded 2014 · Free Forever · Open Source · Android · Windows · Chrome · Samsung Galaxy ROMs 2014–2020 · Custom Kernels · TeamExyKings`. Cleaned up orphan two-ticker CSS (`.footer-tickers`, `.ticker-reverse`, `.ticker-link`, `.ticker-pipe`).
- **Team marquee order** — Yashwanth Ram Somireddy's card now appears first (moved from index 2 → 0 in `TEAM` array).
- **Projects marquee order** — Bookmark Tab Manager confirmed at index 0 (no change needed).

---

## v3l — 2026-09-13

### New
- **Welcome line** — `Welcome to the TeamExyKings Family!` added above the hero badge pill (18 px, semi-transparent; 15 px on mobile).

### Changed
- **Team section moved** — now sits below Credits in page order; nav link order updated to match. Section numbers resequenced: About 04, FAQ 05, Credits 06, Team 07.
- **Mobile hero** — reduced `.hero-content` top padding from 60 px → 24 px on mobile so the badge and headline start closer to the top. Three.js sphere objects repositioned for mobile viewport (obj1: x=1.1, obj3: x=0.7) so the wireframe geometry is actually visible in portrait.
- **Arrow nav snap** — when a hover-drift stops (mouse leaves arrow button), the marquee now snaps to the nearest clean card boundary with a 280 ms ease transition before resuming the CSS animation. Cards are never left half-cut.
- **Team card auto-reset** — cards flipped to the donate back-face (via tap/click) automatically flip back to front after 5 s. Timer resets if the card is re-tapped.

---

## v3k — 2026-09-13

### New
- **Marquee navigation arrows** — `‹` / `›` buttons added to all five scrollers: Projects (All tab), Team, Community, Credits, and each of the five Legacy series marquees. Clicking nudges the scroll in the pressed direction with a smooth 0.35 s slide, then resumes the infinite animation from the new position using a negative `animation-delay` offset. Buttons are positioned outside the `overflow:hidden` + mask-gradient container via a `.mq-outer` wrapper, so they are never clipped.

### Changed
- **Legacy tab auto-cycle** — interval increased from 3 s → 10 s, giving users more time to read each series before the tab switches.

---

## v3j — 2026-09-13

### New
- **Community cards marquee** — replaced static 3-column grid with right-to-left infinite marquee (matches Projects / Team style); pauses on hover.
- **Credits cards marquee** — replaced static grid with right-to-left infinite marquee; pauses on hover.
- **Legacy Archive smooth transitions** — tab switching now animates in with a CSS `fadeIn + translateY` keyframe instead of an instant `display:block`. Auto-cycle timer (3.5 s) removed; tabs are now user-driven only.
- **Placeholder link comments** — every `#` placeholder URL now has a `/* TODO */` comment in the JS data (`PROJECTS`, `TEAM`, `LEGACY`) and an `<!-- PLACEHOLDER -->` comment in the HTML (community cards), making future link updates easy to find in one place without hunting through the file.

### Changed
- **Footer** — removed Telegram and About quick-links from footer bottom. The footer now shows only the copyright line.
- **Disclaimer placement** — moved the Disclaimer banner from above the footer to *below* the dev-names ticker strip, so the ticker flows into the disclaimer naturally before the copyright line.

### Fixed
- **Three.js sphere reposition** — `obj1` (main Icosahedron) shifted from `x=2.2` to `x=4.0`; `obj3` (smaller accent Icosahedron) shifted from `x=0.5` to `x=2.8`. Glow halo for `obj1` updated to match. Hero text "Tools built by developers, for developers" is now fully clear of wireframe overlap. *(landed in v3j, done at end of v3i session)*

---

## v3i — 2026-09-12

### Changed
- **White theme removed entirely** — site is permanently AMOLED black. All `[data-theme="white"]` CSS blocks, the theme-toggle button, its Three.js `_tekUpdateHeroTheme` function, and the JS cycle logic were deleted.
- **Theme JS** simplified to a single `setAttribute('data-theme','black')` line.

---

## v3h — 2026-09-12

### Fixed
- **Team cards** — replaced all-cards-at-once grid with a right-to-left marquee showing 3 cards at a time (matches Projects "All" tab pattern). Cards duplicated for seamless `-50%` loop.
- **Hero text visibility** — "for developers" was grey due to `opacity:0.88` on `.dim-word`. Removed opacity; locked to `color:#ffffff !important` with strong `text-shadow`.
- **Theme dot** — was showing accent blue (`#4f8ef7`). Fixed to near-black (`#0a0a14`) with a white ring.
- **Mobile hamburger alignment** — was centered; added `flex-grow:1` to `.nav-brand` on mobile so ☰ and theme toggle sit at the right edge.

---

## v3g — 2026-09-11

### New
- **AMOLED pitch-black theme** — `--bg:#000000`, `--bg2:#08080f`, `--bg3:#0e0e18`, Three.js fog `0x000000`.
- **Hero text always visible** — locked to `color:#ffffff` with heavy `text-shadow` regardless of theme.
- **Team name marquee** — dev name scrolls left-right inside the card front.
- **Legacy Archive auto-cycle** — tabs cycle every 3.5 s when idle; pauses on hover.
- **Credits** — added Somadsul as 👑 Founder card; updated Yashwanth's credit line.

### Changed
- **Team cards** — removed ℹ️ info icon; increased card height to 240 px to prevent back-face data clipping; back face shows only PayPal/UPI donate links.

---

## v3f — 2026-09-11

### Changed
- **Team cards** — converted from info-flip JS-overlay to CSS 3D flip (perspective + `rotateY(180deg)`). Front: avatar, name, handle, links. Back: PayPal/UPI donate only.
- **Footer copyright** — removed standalone copyright banner; integrated into footer.
- **FAQ copyright notice** — updated wording.

---

## v3 — initial public version

- Three.js WebGL hero with particles and wireframe objects.
- GSAP ScrollTrigger reveal animations and stat counters.
- Projects section with tab filtering and horizontal marquee for "All" tab.
- Legacy Archive with device/build accordion, heatmap cells.
- Community, Team, About, FAQ, Credits sections.
- Single-file `index.html` — no build step, deployed via GitHub Pages to teamexykings.in.
