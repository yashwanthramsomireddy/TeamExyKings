# TeamExyKings Website — Changelog

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
