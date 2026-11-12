# RecruitLens — Changelog
TeamExyKings

---

## v1.98 (black pop-ups, FAQ and About icons, Supabase temp folder; app version 2.0.2, versionCode 24)
- **Pop-ups:** every pop-up (confirmations, errors, Credits, colour picker, seat allocation) is now pitch black (AMOLED) with a 1 px white outline instead of the grey Android default. Buttons are outlined in white, the main action is filled white, destructive actions are red. In the Light theme the pop-ups are white with a black outline. The screens that used the system dialog now use one shared pop-up (`src/components/AppAlert.tsx`, colours in `src/constants/dialogTheme.js`).
- **Tab bar:** new **FAQ** (?) and **About** (i) icons next to Settings. They stay inside Settings as before.
- **About:** the six policy buttons and Visit Website use the `https://teamexykings.in/Recruit-Lens/` pages; Visit Website opens `https://teamexykings.in/`.
- **Supabase folder:** one-off and diagnostic SQL (demo CAMS organisation, remove-admin, Play review account, reset, diagnostics) now lives in `supabase/temp/` with a README of what each file does and when to run it. Numbered migrations and Edge Functions stay at the top.
- No database, Edge Function or SQL change.
