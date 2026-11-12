# King Six

**Score it. Share it. Own it.**
A ball-by-ball cricket scorer for backyard, club and street cricket. Google sign-in, all data on your phone, no ads, no tracking.

By **TeamExyKings** · built by Yashwanth Ram Somireddy · https://teamexykings.in
Package `com.teamexykings.kingsix` · version **1.8.0 (versionCode 17)** · PRIVATE repository, no public licence file.

## How it works (1.8.0)
- **Mandatory Google sign-in** (Credential Manager) exchanged with Supabase Auth. Internet is needed to sign in, subscribe and delete the account; scoring works offline.
- **All cricket data lives on the phone** in the app's Room (SQLite) database. Supabase holds only the account and the subscription record. Cloud sync and match codes were removed from the app.
- **Free = everything for scoring:** teams, players, series, matches, all graphs and stats, share as text, image, PDF or CSV. **Pro (yearly, Rs 500) = backups to Google Drive and phone storage (incl. automatic monthly copy) and export of the whole profile to Excel.** Restoring a backup file is free.
- **Subscription verified by the server** (Edge Function `verify-subscription` checks Google Play); `play-rtdn` keeps renewals current. After the paid year: 7-day grace week, then Free.
- Backend, SQL, functions and secrets tracker live in `docs/supabase` (secrets never in zips).

## Local setup (cloud settings)
Create `local.properties` in this folder (it is git-ignored, never commit it):
```
SUPABASE_URL=https://xxxx.supabase.co
SUPABASE_ANON_KEY=<anon public key>
GOOGLE_WEB_CLIENT_ID=<web client id>.apps.googleusercontent.com
```
Without these the app builds but the sign-in screen says the build has no cloud settings.

## Earlier features (v1.1.0)
Home, wizard with super over, scorer (11 dismissals, free hits, undo, retire, penalty, partnership strip), scorecards, Man of the Match, Analysis tab (match, player, team graphs), series with points table, text/image/PDF/CSV export, file backup/restore, onboarding, themes.

## Release signing
Put these in your USER-level gradle.properties (Windows `C:\Users\<you>\.gradle\gradle.properties`), never in the project:
```
KINGSIX_UPLOAD_STORE_FILE=C:/path/outside/the/project/key-upload.jks
KINGSIX_UPLOAD_STORE_PASSWORD=...
KINGSIX_UPLOAD_KEY_ALIAS=...
KINGSIX_UPLOAD_KEY_PASSWORD=...
```
Keep the .jks outside the project folder too, and back it up.

## Build the test APK
Requirements: Android Studio Ladybug (2024.2) or newer, JDK 17 (bundled with Studio), Android SDK 35.

1. **File → Open** → choose this `king-six` folder. Let Gradle sync finish (first sync downloads dependencies).
2. Connect the Galaxy S9 (USB debugging on) or start an emulator.
3. **Run ▶** to install, **or** build an APK: **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
   Output: `app/build/outputs/apk/debug/app-debug.apk` (package `com.teamexykings.kingsix.debug`, so it installs beside any release build).
4. Command line: `./gradlew assembleDebug` · unit tests: `./gradlew test`.

Release signing is intentionally not configured. When you make a release build, create the keystore **outside** the repo, back it up, and read it from an untracked `keystore.properties` (already in `.gitignore`).

## Honest status of this source drop
- The pure Kotlin layers (`domain/engine`, `domain/stats`, `domain/plan`) were compiled with the real Kotlin compiler and its **49 unit tests pass** (engine, analytics, stats, plan rules, sync rules).
- The Android/Compose/Room/Hilt layers could **not** be compiled in the environment this was written in (no Android SDK or Maven access). They were carefully reviewed, but the first Gradle sync/build on your machine is the real compile. If it reports an error, send the message and it is usually a one-line fix.
- Typography uses Nunito from Google Play Services; on a phone without it the system font is used automatically.

## Project map
```
app/src/main/java/com/teamexykings/kingsix
  domain/engine   CricketEngine, Analytics, MatchRules, ScorecardState   (pure Kotlin, unit-tested)
  domain/stats    StatsEngine (careers, team records, series table)
  domain/plan     PlanLogic (Free / Pro / Expired rules)
  data/billing    BillingManager (Google Play Billing)
  data/backup     BackupRepository (JSON file export / import)
  data/cloud      AuthRepository, GoogleSignIn, CloudApi, SyncManager, EntitlementService, ShareRepository
  data/db         Room entities + DAOs (BallEvent is the single source of truth)
  data/repository MatchRepository, TeamRepository, PreferencesRepository, MatchSummaryLoader
  export          ScorecardExporter (text / PNG / PDF via FileProvider, no storage permission)
  ui/...          Compose screens: home, matches (+wizard, scorer, break, result), analysis, series, plan, backup, teams, settings, onboarding, theme
store-assets/     512 px Play Store icon
```
Design rule: totals are never stored as truth. Every scorecard is recalculated from the ball events, which makes undo, edits and sync safe.

## Security & policy notes
- Our manifest has INTERNET and ACCESS_NETWORK_STATE (needed for sign-in and sync). Android system backup of the database is switched off because the cloud is the backup.
- Row-level security: each account reads only its own rows; only Pro accounts can write; subscriptions are written only by Edge Functions with the service role.
- Libraries are Apache-2.0 / MIT / BSD only. No AGPL/GPL.
- No API keys in this repo. `local.properties` holds the Supabase URL, anon key and Google web client id (all public-by-design values, still git-ignored). The Google Play service-account key lives only in Supabase secrets.
- Keystore stays outside the repo; back it up.
- No ads, no tracking, no betting, no odds, no fantasy.
