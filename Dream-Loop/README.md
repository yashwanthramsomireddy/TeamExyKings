# DreamLoop

A private dream journal for Android. Your dreams live on your phone, not on someone else's server.

Built by [TeamExyKings](https://teamexykings.in) · `com.teamexykings.dreamloop` · v3.0.0

## What it does

- **Local-first journal** — every dream is stored in on-device SQLite (works offline, multiple entries per day, search, mood/flag/lucid/recurring filters, draft autosave).
- **Patterns on-device, free** — recurring-theme detection (TF-IDF + average-linkage clustering, runs on the phone), streaks, mood mix, 12-week heatmap, offline symbol glossary.
- **Gentle AI, only when asked** — free monthly interpretations; credits for deeper readings, nightmare rewrites and recurring-theme insight. Server refunds automatically if the AI fails.
- **Your own backup** — optional Google Drive backup (limited `drive.file` scope), restore offered on first launch, optional auto-backup.
- **Polished, themeable UI** — Pitch Black (default, white accent) · Grey · White · System · Custom background, plus any accent colour with automatic contrast protection.
- **Safe-area aware navigation** — custom tab bar that clears gesture bars and 3-button navigation on every device.
- **Privacy extras** — optional biometric app lock, local morning reminder, in-app account deletion.
- **Region-aware payments** — ₹ pricing in India, $ elsewhere. Payments use Google Play Billing.

## Architecture

| Concern | Where |
|---|---|
| Dreams, recurring groups | `expo-sqlite` on device (`lib/localDb.ts`) — never uploaded |
| Auth, profile, credits, purchases | Supabase (`supabase/`) — **no dream content** |
| AI | `analyze-dream` edge function → OpenRouter (JWT-authenticated, refunds on failure) |
| Payments | `lib/payments/` — Google Play Billing (`verify-play-purchase`) |
| Backup | `lib/gdrive.ts` — Android OAuth client + PKCE, refresh token in the Android Keystore |
| Theme | `theme/` — palettes, tokens, `useTheme()` / `useStyles()` |

Stack: Expo SDK 57 · React Native 0.86 · Expo Router · TypeScript · zustand · lucide icons (per-icon imports).

```
app/            screens (expo-router): (auth)/login, (tabs)/{index,journal,patterns,settings}, dream/*, recurring/*, appearance, credits, glossary, admin
components/     ui kit, nav/TabBar, dream/*, charts/*, art/Logo
theme/          colors, palettes, tokens, ThemeProvider
lib/            localDb, recurringEngine, gdrive, payments/, ai, notifications, applock, region, dates
supabase/       migrations (007 is new) + edge functions + deploy README
plugins/        withAndroidBuildTuning (release signing + Gradle memory, survives prebuild --clean)
scripts/        build_brand.py, build_screenshots.py, test-recurring.ts
store/          Play listing text, data-safety answers, Play graphics
assets/         app icons (generated from components/art/logoGeometry.ts)
```

## Build (Windows, local)

```powershell
npm install --legacy-peer-deps
# create .env in the project root (EXPO_PUBLIC_SUPABASE_URL, _SUPABASE_ANON_KEY, _APP_NAME, _SUPPORT_EMAIL, _APP_VERSION, _GOOGLE_ANDROID_CLIENT_ID, _PAYMENT_PROVIDER=play)
npx expo prebuild --platform android --clean
cd android
.\gradlew bundleRelease       # AAB for Play  → android\app\build\outputs\bundle\release\app-release.aab
.\gradlew assembleRelease     # APK           → android\app\build\outputs\apk\release\app-release.apk
```

**Signing.** `plugins/withAndroidBuildTuning.js` injects the release signing config on every prebuild. Put your keystore settings in your *user-level* Gradle file so they never touch the repo:

`C:\Users\<you>\.gradle\gradle.properties`
```
DREAMLOOP_UPLOAD_STORE_FILE=C:/Users/<you>/Downloads/DreamLoop/docs/keystroke/dreamloop-upload.jks
DREAMLOOP_UPLOAD_STORE_PASSWORD=...
DREAMLOOP_UPLOAD_KEY_ALIAS=dreamloop
DREAMLOOP_UPLOAD_KEY_PASSWORD=...
```
(If these are absent the release build falls back to the debug key, so dev builds still work.) The plugin also raises Gradle's heap/metaspace so `bundleRelease` stops warning.

## Google Drive setup (one time)

1. Google Cloud project **DreamLoop** → *APIs & Services* → enable **Google Drive API**.
2. *OAuth consent screen*: app name DreamLoop, support email, scope `…/auth/drive.file` (non-sensitive). **Publish to Production** — in *Testing* mode refresh tokens expire after 7 days.
3. *Credentials → Create OAuth client ID → Android*: package `com.teamexykings.dreamloop`, SHA-1 of the key that signs the build:
   `keytool -list -v -keystore docs\keystroke\dreamloop-upload.jks -alias dreamloop` (use Android Studio's `jbr\bin\keytool.exe`).
   Open the client → *Advanced settings* → turn **Enable custom URI scheme** ON → Save.
4. Put its client ID in `.env` as `EXPO_PUBLIC_GOOGLE_ANDROID_CLIENT_ID`, then re-run `npx expo prebuild --platform android --clean` (the reverse-client-ID scheme is registered at prebuild).
5. For the **Play Store build** create a second Android client with the *Play App Signing* SHA-1 (Play Console → App integrity) and build that release with its ID in `.env`.

No client secret is shipped in the app.

## Backend

See [`supabase/README.md`](./supabase/README.md): apply `migrations/007_hardening_and_play.sql`, deploy the edge functions, set the secrets.

## Payments

* Installed from Google Play → Play Billing automatically (Google requires this for credits). Create the five in-app products listed in `supabase/README.md`.
* `EXPO_PUBLIC_PAYMENT_PROVIDER` forces a rail; `EXPO_PUBLIC_SHOW_TOPUP=false` hides the packs until production.
* Pack prices live in Play Console (one-time products) and `constants/config.ts` — keep them in sync.

## Checks

```
npm run typecheck      # tsc --noEmit
npm run test:engine    # recurring-dream detector sanity tests
npx expo export --platform android   # Metro/Hermes bundle check
```

## Brand & store assets

`python3 scripts/build_brand.py` regenerates every icon, the splash, the notification icon and the Play icon/feature graphic from one vector definition. `python3 scripts/build_screenshots.py` re-renders the Play screenshots (design renders — swap in real device captures for the live listing if you prefer). Listing copy: [`store/play-listing.md`](./store/play-listing.md); Data safety answers: [`store/data-safety.md`](./store/data-safety.md).

## Legal pages

`index.html`, `privacy-policy.html`, `terms.html` are static and self-contained — host them at `https://teamexykings.in/Dream-Loop/`.

## License

Proprietary. © TeamExyKings. All rights reserved — see [LICENSE](./LICENSE).

## Contact

support@teamexykings.in · yashwanthramsomireddy@gmail.com
