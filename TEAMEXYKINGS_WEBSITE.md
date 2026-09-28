# TeamExyKings Website — Context File
> Paste this at the start of any new chat to give Claude full context about the website.

---

## Brand & Owner
- **Brand:** TeamExyKings
- **Owner:** Yashwanth Ram Somireddy
- **Role:** BI Team Lead, Sterling Software Pvt. Ltd., Chennai
- **Domain:** teamexykings.in (hosted on GitHub Pages — free)
- **GitHub:** https://github.com/yashwanthramsomireddy
- **LinkedIn:** https://linkedin.com/in/yashwanth-ram-somireddy
- **Email:** yashwanthramsomireddy@gmail.com

---

## Website Structure
- **Single file site:** `index.html` (no framework, no build tool, plain HTML/CSS/JS)
- **Hosted on:** GitHub Pages (repo: `teamexykings.github.io`)
- **Custom domain:** teamexykings.in (Porkbun, renewed yearly ~₹600)
- **Design:** Dark theme (#0D0D0F bg, #3B82F6 accent blue), Inter + Space Grotesk fonts
- **Mobile friendly:** Yes

## Pages / Sections
1. **Nav** — Brand logo + links (Projects, Team, About, GitHub)
2. **Hero** — Brand name, tagline, CTA buttons
3. **Stats Bar** — Live Projects count, Open Source, Free, Platforms
4. **Projects** — Filter by category, auto-rendered from PROJECTS array
5. **Team** — Founder card (Yashwanth) + placeholder for ~10 members
6. **About** — Brand description + contact details
7. **Footer** — Copyright

---

## How to Add a New Project
Projects are stored as a JavaScript array inside `index.html`.
Search for `const PROJECTS = [` in the file and add a new entry:

```js
{
  id: 5,                          // next number in sequence
  name: "Your App Name",
  shortname: "ShortName",
  description: "One or two lines describing what it does.",
  category: "Windows Tool",       // options below
  icon: "🔧",                     // any emoji
  github: "https://github.com/yashwanthramsomireddy/your-repo",
  store: null,                    // or Chrome Web Store / Play Store URL
  status: "live"
}
```

**Available categories** (add new ones freely — filter bar auto-updates):
- `Browser Extension`
- `Android Tool`
- `Windows Tool`
- *(add new categories as needed)*

---

## Current Projects (as of Sep 2026)

| # | Name | Category | GitHub | Store |
|---|------|----------|--------|-------|
| 1 | Bookmark Tab Manager (BTM) | Browser Extension | ✅ | Chrome + Firefox |
| 2 | DebloatKit | Android Tool | ✅ | — |
| 3 | PurgeKit | Windows Tool | ✅ | — |
| 4 | QuickOff | Windows Tool | ✅ | — |

---

## Team Section
- **Founder:** Yashwanth Ram Somireddy (card already present)
- **Remaining members:** ~10 devs to be added later
- Team cards are hardcoded in HTML under `id="team"` section
- To add a member, copy the `.team-card` block and fill in name, role, links

### Team Card Template (copy-paste into HTML):
```html
<div class="team-card">
  <div class="team-avatar">👤</div>
  <div class="team-name">Member Name</div>
  <div class="team-role">Role Title</div>
  <div class="team-links">
    <a class="team-link" href="https://linkedin.com/in/..." target="_blank">LinkedIn</a>
    <a class="team-link" href="https://github.com/..." target="_blank">GitHub</a>
  </div>
</div>
```

---

## Design Tokens (for any UI changes)
```css
--bg: #0D0D0F;           /* main background */
--bg2: #141418;          /* card background */
--bg3: #1C1C22;          /* input/icon background */
--border: #2A2A32;       /* borders */
--accent: #3B82F6;       /* electric blue — primary accent */
--accent-dim: rgba(59,130,246,0.12);
--text: #E8E8F0;         /* main text */
--text-muted: #7A7A90;   /* secondary text */
--radius: 12px;
--radius-sm: 8px;
Font: Inter (body), Space Grotesk (headings/brand)
```

---

## Deployment Steps (for reference)
1. Edit `index.html` locally
2. Push to GitHub repo `teamexykings.github.io` (main branch)
3. GitHub Pages auto-deploys in ~1 minute
4. Live at teamexykings.in

---

## Common Tasks for New Chat
- **"Add a new project"** → update PROJECTS array in index.html
- **"Add a team member"** → copy team card HTML block, fill details
- **"Change accent color"** → update `--accent` CSS variable
- **"Update contact info"** → find About section in HTML
- **"Add a new section"** → follow existing section pattern with `<section>` tag

---

*Last updated: September 2026*
