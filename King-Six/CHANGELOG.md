# Changelog

All notable changes to King Six. Format follows Keep a Changelog; versions follow SemVer.

## [1.8.0] (versionCode 17)
### Changed
- **Everyone can score.** Free accounts get the whole scorer: teams, players, series, matches, every graph, player profiles and leaderboards, and sharing as text, image, PDF or CSV.
- **Pro now means two things:** backups to Google Drive and phone storage (including the automatic monthly copy and reminders) and export of the whole profile to Excel (Me, My matches, All matches, Teams, Players). Restoring a backup file is free.
- **All data is stored on the phone** in the app's Room (SQLite) database. The app no longer depends on Supabase for cricket data; the server holds only sign-in and subscription records. Signing out keeps your data; Delete my data erases it.
- Cloud sync, share-by-code and the Shared screen were removed. Guest mode was removed.
- New look: gradient "Start a match" hero, richer match cards with a status stripe, new default accent.
- FAQ (?) and About (i) icons next to Settings on Home (still inside Settings too).
- About: Terms of use label; Visit website opens https://teamexykings.in/; policy links open teamexykings.in/King-Six/ pages (privacy-policy, terms-of-use, refund-policy, account-deletion).
- Privacy policy, Terms of use, Refund policy, Account deletion, FAQ, Plan screen and website pages rewritten for the new model.
### Removed
- `AllAppsMarquee.kt` leftover file; old release notes and changelog entries.
