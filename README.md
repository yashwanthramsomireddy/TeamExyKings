# TeamExyKings — Official Website

**teamexykings.in** · GitHub Pages · Single-file HTML

---

## What this is

The official website of TeamExyKings — a developer collective building free tools, browser extensions, and apps, with a legacy archive of custom Samsung ROMs & kernels from 2014–2020.

---

## Stack

| Layer | Choice |
|---|---|
| HTML / CSS / JS | Vanilla — no frameworks, no npm, no build step |
| 3D / WebGL | Three.js r128 (CDN) |
| Animations | GSAP 3.12.5 + ScrollTrigger (CDN) |
| 3D card tilt | Vanilla Tilt 1.8.1 (CDN) |
| Fonts | Google Fonts — Anton, Inter, Space Grotesk, JetBrains Mono |
| Hosting | GitHub Pages |
| Domain | GoDaddy → `teamexykings.in` |

Everything ships in a **single `index.html`** file. No bundler, no dependencies to install.

---

## Development

Open `index.html` directly in a browser — no server required for local preview.

All CDN scripts are loaded from `cdnjs.cloudflare.com` (allowlisted by GitHub Pages CSP).

---

## Adding Links (Placeholders)

Search the file for `/* TODO */` (JS data) or `<!-- PLACEHOLDER -->` (HTML) to find every spot waiting for a real URL:

- **PROJECTS** — `store`, `firefoxStore`, `github` fields for unreleased projects
- **TEAM** — `github`, `linkedin`, `paypal`, `upi` for each team member
- **LEGACY** — `link` field on every `builds` entry (ROM/kernel download URLs)
- **Community section** — WhatsApp, Telegram, YouTube links

Replace `"#"` with the real URL string; the UI updates automatically.

---

## Sections

| # | Section | Notes |
|---|---|---|
| 01 | Projects | Tab-filtered grid + "All" marquee |
| 02 | Legacy Archive | ROM/kernel accordion, tab-switched with CSS animation |
| 03 | Community | Right-to-left card marquee |
| 04 | Team | 16-member flip-card marquee (hover = flip, tap = donate) |
| 05 | About | Team history |
| 06 | FAQ | Expandable `<details>` accordion |
| 07 | Credits | Right-to-left card marquee |

---

## Theme

Site is **permanently AMOLED black** — no light/white theme. Color tokens live in `[data-theme="black"]` CSS block.

---

## Changelog

See [CHANGELOG.md](./CHANGELOG.md) for full version history.

---

## License & Copyright

© 2014–2026 TeamExyKings · All Rights Reserved · Chennai, India

Website content is the intellectual property of TeamExyKings. No content may be copied or redistributed without written permission.

Tools and source code released via GitHub are subject to their respective open-source licences.
