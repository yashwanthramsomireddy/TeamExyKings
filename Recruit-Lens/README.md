# RecruitLens
**Scan Your Resumes** · an Android app by TeamExyKings

RecruitLens reads a resume, scores it with an AI model, and keeps every candidate
in one place for an individual recruiter or a whole hiring team. It also has a
Resume Builder that turns a person's own resume into one of 16 templates (PDF or
Word).

| | |
|---|---|
| Platform | Android only (package `com.teamexykings.recruitlens`) |
| Internal drop | **v1.89** (shown as "Test build 1.89" on development and preview builds) |
| Store version | `2.0.2` (`app.json`) is the build to upload next (versionCode 24, v1.98); `2.0.1` (versionCode 23, v1.97) and `2.0.0` (versionCode 22) were built before it; `1.5.5` (versionCode 18, v1.94) and `1.5.4` (versionCode 17, v1.93) were built before that; `1.5.3` (versionCode 16, v1.92) is in closed testing; `1.5.2` (versionCode 15, v1.91) is installed and About opens; `1.5.1` (versionCode 14) did not open About; `1.5.0` (versionCode 13) was built before it; `1.4.1` (versionCode 12, v1.89) was built before it; `1.4.0` (versionCode 11, v1.88) is on the test phone; `1.3.0` (versionCode 9, v1.87) is on testers' phones; versionCode 10 was a local build that was not uploaded; `1.2.6` (versionCode 8, v1.86), `1.2.5` and `1.2.4` were internal builds; `1.1.0` (versionCode 2) is in Play closed testing |
| Stack | Expo SDK 57 · React Native 0.86 · React 19 · Expo Router · Zustand |
| Backend | Supabase (Auth, Postgres with row-level security, Edge Functions) |
| Services | OpenRouter (AI models) · Google Play Billing (the only way to pay; Razorpay was removed in v1.87) · Brevo (email) |

---

## What it does

**Everyone**
* Scan a PDF or Word resume with the AI model of your choice: a score, a
  recommendation, matched and missing skills. The file itself is never stored;
  the text is read on the server and the file is discarded.
* Candidate tracker with an 11-step status pipeline, notes, a timeline, rescan
  with duplicate detection and version history, side-by-side compare, Excel export
  (paid plans), analytics.
* **Resume Builder**: restructure your own resume into one of **16 templates**
  (ATS-friendly, modern, two-column, bold), see a sample picture first, save as an
  A4 PDF or a Word file.
* Plans and credits, transaction history, notifications, Pitch Black / Black &
  Grey / White / System themes with 12 accent colours.
* Sign in with email and password, or with **Google**. Optional
  **two-factor authentication** (authenticator app) from Settings.
* **Delete Account** from Settings (Google Play requirement).

**Organisations (company accounts)**
* Join with a domain code; the Domain Admin approves by email link.
* Teams with tech stacks, team leads, member invites (temporary password), seat
  allocation, credit pool, one-time Domain Licence, Domain Log (who scanned what).
* Roles: `member`, `team_lead`, `spoc` (Domain Admin, with an optional Sub Admin),
  `superadmin`.

**Super Admin**
* Admin panel: models, users, skills review, domain on/off, Financial tab
  (revenue against AI cost), account-deletion processing (SQL).

---

## Project layout

```
src/
  app/            Screens (Expo Router, file-based)
    (auth)/       Login, sign-up, complete profile, waiting for approval, two-factor check
    (main)/       Tabs (Upload, Tracker, Analytics, Resume, Settings) and every detail screen
    delete-account.tsx, auth-callback.tsx
  components/     Shared UI (ResumeTemplatePicker, CodeInput, SocialSignIn, ModelDropdowns ...)
  constants/      Plans, models, themes, app info (names, links)
  hooks/          Theme, auth providers, access watcher
  services/       Supabase client and every call to the backend
  store/          Zustand stores (auth, candidates, app, theme)
  utils/          Pure logic, tested without a phone (resumeTemplates, resumeDocx,
                  accountDeletion, financeFormat, mfaQr, oauthUrl ...)
assets/
  images/         Icons and splash
  resume-previews/  The 16 sample pictures shown in the template picker
scripts/          make-resume-previews.mjs (regenerates those pictures)
tests/            node:test suites (no phone needed)
docs/             PLAY_STORE_NOTES.md, sample-resume.pdf
*.html            Pages served by GitHub Pages: index, privacy-policy,
                  terms-of-service, account-deletion
app.json          App settings (name, package, permissions, plugins)
app.config.js     Build-time tweaks driven by the EAS profile
eas.json          Build and submit profiles
```

The database and the Edge Functions live in the separate **Supabase** folder
(SQL migrations 01 to 21, 22 functions, set-up guides).

---

## Run it

You need Node 22, npm, an Android phone or emulator, and a **development build**
of the app (the sign-in return link and the camera are native, so plain Expo Go is
not enough).

```bash
npm install
npx expo start --dev-client
```

### Environment variables
The app reads two public values (the Supabase address and its public "anon" key;
Row-Level Security is what protects the data, not secrecy of this key):

```
EXPO_PUBLIC_SUPABASE_URL=https://<project-ref>.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=<anon key>
```

* **On your computer**: put them in a `.env.local` file in the project root.
* **On EAS (cloud builds)**: files that Git ignores are not uploaded, so the two
  values must exist as EAS environment variables for every environment you build
  with. Check, and set what is missing:

```bash
eas env:list --environment development
eas env:list --environment preview
eas env:list --environment production
eas env:set --name EXPO_PUBLIC_SUPABASE_URL --value "https://<project-ref>.supabase.co" --environment production --visibility plaintext
eas env:set --name EXPO_PUBLIC_SUPABASE_ANON_KEY --value "<anon key>" --environment production --visibility plaintext
```

A build without them starts and then fails at once, so check **before** you
spend a build. (Set them for all three environments and it does not matter which
one a profile uses.)

---

## Tests

```bash
npm test          # node --test tests/*.test.mjs
```

198 checks cover the template engine, the Word export, finance formatting,
account-deletion wording and rules, sign-in link parsing, two-factor helpers and
error messages. The Word-export tests need `jszip` (it is a dependency, so
`npm install` is enough; without it those 8 are skipped). The database tests
(241 checks) and the Edge Function tests (171 checks) are in the Supabase folder
(`tests/`).

Regenerate the template pictures after changing a template:

```bash
node scripts/make-resume-previews.mjs     # see the header of the script for what it needs
```

---

## Builds and releases

| Profile | Result | Use |
|---|---|---|
| `development` | Debug app that loads JavaScript from your computer | Daily testing |
| `preview` | Installable APK, JavaScript inside | Final check on a phone |
| `production` | App bundle (.aab), `versionCode` raised by EAS | Google Play |

```bash
eas build --platform android --profile development
eas build --platform android --profile preview
eas build --platform android --profile production
eas submit --platform android --profile production
```

* `app.json` is the one place for settings. `app.config.js` only adds the
  "Test build" label (development and preview) and drops the developer-only "draw
  over other apps" permission (preview and production).
* Dangerous permissions (camera, microphone, storage, advertising ID) are blocked
  in `app.json`. A Play build asks the phone for no permission at all.
* There is **no over-the-air update**. A change reaches phones only through a new
  build.
* Native changes (a new native library, `app.json`) need a new build. JavaScript
  changes do not need one while testing on the development build.
* Everything about Google Play is in [`docs/PLAY_STORE_NOTES.md`](docs/PLAY_STORE_NOTES.md).

---

## Documentation

| File | What it is |
|---|---|
| [`CHANGELOG.md`](CHANGELOG.md) | What changed in every version |
| [`docs/PLAY_STORE_NOTES.md`](docs/PLAY_STORE_NOTES.md) | Google Play checklist, listing text, Data safety answers, payments decision |
| `Supabase/README.md` | Database order, Edge Functions, secrets, schedules, runbooks |
| `Supabase/SOCIAL_SIGNIN_SETUP.md` | Google sign-in, step by step |
| `index.html`, `privacy-policy.html`, `terms-of-service.html`, `account-deletion.html` | Public pages on our own domain: `https://teamexykings.in/Recruit-Lens/` (the `Recruit-Lens` folder of the TeamExyKings website repo) |

---

## Security notes

* Never commit: the Supabase **service-role** key, the Google Play service-account file, the
  OpenRouter and Brevo keys, an Android keystore, or the Google Play service
  account file. They live in Supabase secrets and in EAS. `.gitignore` already
  excludes keystores and `*service-account*.json`.
* The app ships with the Supabase **anon** key only. It is public by design; the data rules (row-level security) are what protect the data.
* All data rules are enforced in the database (row-level security plus
  restrictive rules for blocked accounts and for two-factor), not only in the app.

---

© TeamExyKings · see [LICENSE](LICENSE)
