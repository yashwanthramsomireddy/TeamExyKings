# King Browser

A lightweight, private Android browser under the TeamExyKings brand. Free forever, no ads, no subscription, no login required, no analytics, no tracking. Google sign-in is available but entirely optional — see "Optional: Google Drive backup setup" below.

## Stack

- Kotlin, Jetpack Compose + Material3
- **GeckoView** (Mozilla) as the rendering engine — not the system WebView. This is a deliberate choice: WebView has no extension API at all, and GeckoView is the only option that gives King Browser real WebExtensions support (for BTM) without forking and maintaining a browser engine.
- MVVM with manual factories (no Hilt) — see `viewmodel/ViewModelFactory.kt`
- Room (bookmarks, history, downloads) + DataStore (settings)
- Navigation Compose, SplashScreen API
- Min SDK 29, Target SDK 36, AGP 8.9.1, Gradle 8.11.1

## Before you build

1. **BTM extension build.** `app/src/main/assets/extensions/btm/` currently only has a README placeholder. Copy BTM's packaged Firefox-build files (manifest.json + everything it references) into that folder before testing the Extensions screen — see the README there for details. Until you do, the Extensions screen will correctly show "needs manual install" rather than crashing.
2. **Signing.** Add to your **user-level** `~/.gradle/gradle.properties` (not this project's `gradle.properties`):
   ```
   KINGBROWSER_UPLOAD_STORE_FILE=/absolute/path/to/your/keystore.jks
   KINGBROWSER_UPLOAD_STORE_PASSWORD=...
   KINGBROWSER_UPLOAD_KEY_ALIAS=...
   KINGBROWSER_UPLOAD_KEY_PASSWORD=...
   ```
3. **Gradle wrapper jar.** This zip doesn't include `gradle/wrapper/gradle-wrapper.jar` (binary file). Open the project in Android Studio and let it sync — Android Studio will fetch/regenerate the wrapper jar automatically. If you need it manually: `gradle wrapper --gradle-version 8.11.1`.
4. First sync will download GeckoView (~tens of MB per ABI) from Mozilla's Maven repo — make sure that's reachable.

## Optional: Google Drive backup setup

Drive backup is opt-in — the app builds and runs fully without this. Skip this section if you just want to test the browser first; "Sign in with Google" will simply fail gracefully until you do this.

1. In [Google Cloud Console](https://console.cloud.google.com/), create a project (or reuse one of your existing TeamExyKings ones, e.g. the one you set up for BTM's OAuth consent screen).
2. Enable the **Google Drive API** for that project (APIs & Services → Library).
3. Configure the **OAuth consent screen** (same place you did this for BTM) — add the `drive.appdata` scope.
4. Under **Credentials → Create Credentials → OAuth client ID**, create an **Android** client:
   - Package name: `com.teamexykings.kingbrowser`
   - SHA-1 fingerprint: run `keytool -list -v -keystore <your-keystore>.jks -alias <your-alias>` and paste the SHA-1 shown. Do this once for your **debug** keystore (for testing) and once for your **release/upload** keystore (for the signed build) — you'll need two Android OAuth client entries, one per SHA-1.
   - No client ID needs to be pasted into the app's code — Google Play Services resolves it automatically from your device's installed app signature + package name matching what you registered.
5. That's it — no server, no Supabase. The whole Drive flow is on-device: Google Sign-In handles auth, and the Drive `appDataFolder` scope stores the backup JSON in a hidden, per-app area of the user's Drive.

## Project layout

```
app/src/main/java/com/teamexykings/kingbrowser/
  engine/       GeckoEngine (runtime), Tab, TabManager — the browser core
  extensions/   ExtensionManager — bundles/installs BTM as a built-in WebExtension
  data/
    db/         Room entities + DAOs (bookmarks, history, downloads)
    repo/       Repositories wrapping the DAOs
    settings/   DataStore-backed settings repository
    backup/     Backup/Restore JSON format + import/export logic
  drive/        Optional Google Sign-In + Drive appDataFolder backup/restore
  viewmodel/    One ViewModel per screen + the manual ViewModelFactory
  ui/
    theme/      Pitch Black / White / System theme, accent colour, 5 fonts
    components/ Shared pill buttons, rounded cards
    screens/    Browser, Tabs, Bookmarks, History, Downloads, Extensions,
                Settings, About (+ Credits popup), FAQ
index.html,        Web pages for teamexykings.in/King-Browser/
privacy-policy.html,
terms-of-Service.html
docs/store-assets/ icon-512.png, feature-graphic-1024x500.png — for the
                   Play Console store listing
```

## Known gaps in this v1.0.1 build (expected — report anything beyond these)

- Extensions beyond BTM aren't supported yet (by design, per your scoping decision — only a small bundled set, added later).
- Drive backup needs the Cloud Console setup above before "Sign in with Google" will succeed — until then it'll just fail to sign in, which is expected, not a bug.
- Drive/backup is a manual snapshot (Back up now / Restore), not continuous per-bookmark sync yet — see the feature roadmap for the planned "In your Google Account" vs "Only on this device" split.
- Desktop (Electron) companion app isn't started yet — Android ships first per your instruction.

## Testing & reporting back

Build and test on your phone as usual — paste any errors/stack traces back and they'll get fixed in the next zip, which will again include this README, an updated CHANGELOG.md, and release notes for that version.
