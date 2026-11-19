# RecruitLens — Changelog
TeamExyKings

---

## v2.05 (update notice, Play profile button, exit confirmation; app version 3.0.6, versionCode 32)
- **Update notice:** a small Android module (`modules/play-update-check`) asks Google Play whether a newer version exists. If so, About shows an "An update is available" card with an **Update on Google Play** button, and the home screen shows a popup (Update / Later) once per version. Both buttons count as seen; a still newer version shows it once more. Only works for a copy installed from Google Play (a sideloaded build quietly shows nothing). Nothing is sent to our servers.
- **More apps button:** About, after Share, opens our Google Play developer page.
- **Exit confirmation:** back on the home screen asks "Exit App?" (No, Stay / Yes, Exit) instead of closing the app.
- **Payments:** `verify-play-purchase` v1.3 also rejects a purchase whose product id Google reports differently from the one sent. Redeploy that function.
- **Docs:** privacy policy (update check row, date), Credits, FAQ.
- **Tests:** `tests/updateNotice.test.mjs`, `tests/homeExitUpdate.test.mjs`.
- Everything in 3.0.5 (v2.04) and earlier is included.
