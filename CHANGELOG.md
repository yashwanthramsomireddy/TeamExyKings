# Changelog

All notable changes to the TeamExyKings brand site will be documented here.

---

## [v1.0.0] — 2026-09-12

### Initial Release

First public version of the TeamExyKings brand site (`index.html`), a single-file, framework-free HTML/CSS/JS website.

### Added

**Brand & Identity**
- Custom crown logo — two-symbol SVG system: monochrome `tekCrown` for theme-adaptive watermarks, full-color `tekCrownMark` (white/black/red gem) for the nav brand mark
- Two-tone wordmark: "TeamExy" in white + "Kings" in red, both with black outline stroke, using Anton font
- Black (dark) and Blue (light) dual-theme with smooth toggle — persisted via `localStorage`

**Navigation**
- Sticky top nav with brand mark + wordmark
- Theme toggle button (🌙 / ☀️)
- Smooth-scroll anchor links to all sections

**Hero Section**
- Full-screen hero with tagline and CTA buttons
- Animated crown wheel watermark (donut/spoke pattern using `tekCrown`)

**Current Projects Section**
- Project cards: Bookmark Tab Manager (BTM), DebloatKit, PurgeKit, QuickOff, QuickOff Watch, RecruitLens, Device Care Plus
- Each card: icon, title, description, platform tags, status badge, and placeholder CTA links (to be wired in v1.1)

**Legacy Archive Section**
- Tabbed view: Current Wheel vs Legacy Wheel
- Legacy build cards across 5 device families: PC Tools, Android Apps, Samsung One UI, Windows, Web & Browser
- 39 legacy builds documented with version history

**Community Section**
- Discord, YouTube, GitHub, Reddit, and LinkedIn placeholder cards

**Team Section**
- 16 developer profiles with role, stack tags, and GitHub/LinkedIn placeholder links

**About Section**
- Brand story, mission statement, and founding year

**Footer**
- Brand mark, tagline, copyright, and social icon links

**Technical**
- Zero dependencies — no frameworks, no npm, no build step
- Google Fonts: Anton, Inter, Space Grotesk, JetBrains Mono
- CSS custom properties for full theme support (`--bg`, `--text`, `--accent`, etc.)
- `localStorage` theme persistence
- Mobile-responsive layout

---

## [v1.1.0] — Upcoming

### Planned

- Wire real URLs for all project CTA buttons (BTM, DebloatKit, PurgeKit, QuickOff, etc.)
- Add live GitHub repo links for all team members
- Add real social/community links (Discord invite, YouTube channel, Reddit)
- Connect custom domain: `teamexykings.in`
- Add `CNAME` file for GitHub Pages custom domain
- SEO meta tags and Open Graph image
