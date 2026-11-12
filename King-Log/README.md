# King Log (developer guide)

**King Log** (project and package name **KingLog**, `com.teamexykings.kinglog`) by **TeamExyKings** is a private, offline-first Android app that keeps three things in one place. It is **free, ad-free and subscription-free** (this repository is private; see [LICENSE](LICENSE)).

| Module | What it does |
|---|---|
| **Vault** | **Banking · Cards · Passwords · WiFi.** Entries are encrypted on the phone (Android Keystore AES-256-GCM). Government IDs live in Passwords under the category *Government*. Password generator, card-expiry warnings, WiFi QR code, favourites |
| **Finance** | Salary calculator (Simple / Pro), monthly records for past **and future** months (copy / clone forward, *Planned* flag), Months tab with changes and yearly summary, analytics. Encrypted on the phone |
| **EMI plan** (Finance tab) | Loans, SIPs, insurance, living costs and income by month; Plan / Grid / Pre-close views from one data model, one-off and step changes, optional per-loan payment schedule, **Excel export and import**. Encrypted on the phone and in backups |
| **Fuel Log** | Bikes and cars, fill-ups, mileage, cost per km, trends, insurance / service reminders, **Excel and JSON export**, import of KingLog JSON and the old CSV files |

Plus: one-tap **encrypted backup** of everything, a forced weekly backup on the phone, optional Google Drive and folder copies, one-click restore, an app lock (KingLog PIN / pattern + biometrics, or the phone's own screen lock), themes (pitch black by default) and a Compact / Roomy layout, reminders, app shortcuts and an in-app FAQ.

- App ID (`applicationId` and Kotlin namespace): `com.teamexykings.kinglog` · Version `2.3.0` (versionCode 13: Play needs a **higher** number than your last upload) · see [RELEASE_NOTES.md](RELEASE_NOTES.md) and [CHANGELOG.md](CHANGELOG.md)
- Stack: Kotlin 2.0.21 · Jetpack Compose (Material 3, BOM 2025.04.01) · Room 2.6.1 (KSP) · Navigation Compose · WorkManager · BiometricPrompt · AGP 8.9.1 · Gradle 8.11.1
- minSdk 26 (Android 8.0; covers the Galaxy S9 and other Android 8-10 phones) · compile / target SDK 36 · JDK 17

> **Status.** Earlier builds were compiled in Android Studio and installed on a phone by the owner. The newest screens (Home cards, the About and legal screens, the header buttons) were written without an Android SDK and checked only against stand-in Android / Compose APIs, so expect at most a few one-line compile fixes on the first Gradle sync. The pure-Kotlin logic (backup crypto and format, finance planning and maths, fuel maths, home maths, reminders, Excel writer, vault, JSON) compiles with `kotlinc` and passes **201 unit tests**.

**Before you ship a public build, read the checklist in [RELEASE_NOTES.md](RELEASE_NOTES.md):** keep `Config.ALLOW_SCREENSHOTS_FOR_TESTING = false` and check the web links in `Config.kt`.

---

## 1. Open it in Android Studio

1. Install **Android Studio** (current stable) and, in *SDK Manager*, **Android SDK Platform 36** plus the latest *Build-Tools*.
2. *File ▸ Open* the folder that contains `settings.gradle.kts`.
3. Use Studio's bundled **JDK 17** (*Settings ▸ Build ▸ Gradle ▸ Gradle JDK*).
4. Let **Gradle sync** finish (first sync needs internet), pick a device (Android 8+) and press **Run ▶**.

### Upgrading a phone that has KingLog 1.0
- KingLog 1.0 and 1.1 have the **same app ID** (`com.teamexykings.kinglog`), so 1.1 installs **over** 1.0 and keeps its data, as long as both are signed with the **same key**. On first start 1.1 migrates the data once and asks you to save the recovery key.
- **If Android says it cannot update / asks to uninstall first**, the two builds were signed with different keys (for example 1.0 was a Studio debug build and 1.1 comes from Play or is signed with your upload key). **Don't uninstall yet**: that erases the data. Back up in 1.0 first (Finance & Fuel, and the Vault with its passphrase), copy the files off the phone, then uninstall, install 1.1 and choose *Restore from a backup* on the Welcome screen (1.1 reads 1.0 backups).
- See RELEASE_NOTES.md for the full checklist.

### Build config
There is no `.env`, no API key and no backend URL. Everything you may change is in `app/src/main/java/com/teamexykings/kinglog/core/Config.kt`: web links (`PAGES` = `https://teamexykings.in/King-Log/`: privacy-policy.html, terms-of-service.html, faq.html, account-deletion.html), the Play link used by *Share App*, the support email, `ALLOW_SCREENSHOTS_FOR_TESTING`.

**The app ID lives in two places:** `applicationId` in `app/build.gradle.kts` and `android:targetPackage` in `res/xml/shortcuts.xml` (the long-press shortcuts). Change both together, and create new Google OAuth clients for the new ID.

---

## 2. Google Drive setup (only needed to test Drive backup)

The app works fully without it. For Drive backup and Google sign-in create a Google Cloud project **only for KingLog** (yours is `kinglog-512517`):

1. [Google Cloud Console](https://console.cloud.google.com) ▸ project **KingLog**.
2. *APIs & Services ▸ Library* ▸ enable **Google Drive API**.
3. *Google Auth platform ▸ Branding / Audience*: app name **King Log**, support email, developer contact; publish to *Production* before the Play release.
4. *Google Auth platform ▸ **Data access** ▸ Add or remove scopes*: add `.../auth/drive.file` (non-sensitive: King Log sees only files and folders it created itself), plus `email` and `profile`. **1.3 no longer uses `drive.appdata`**; you can remove it from the list. Because the scope changed, every user is asked to allow Drive once more after updating.
5. *Clients ▸ Create client ▸ **Android***, **one client per signing key** (package `com.teamexykings.kinglog` and that key's SHA-1). Google allows one SHA-1 per Android client, so make a separate client for each:

   | Key | Where its SHA-1 comes from | When it is the one that matters |
   |---|---|---|
   | **Debug key** | Studio ▸ Gradle ▸ `app ▸ Tasks ▸ android ▸ signingReport`, or the app: Settings ▸ Backup & restore ▸ *Show sign-in details* | Runs from Android Studio |
   | **Upload key** (`kinglog-upload.jks`, alias `key0`, SHA-1 `74:3B:02:00:AF:BF:30:D2:07:4A:91:10:06:69:33:C6:AA:14:D0:86`) | `keytool -list -v -keystore kinglog-upload.jks` (Android Studio's `jbr\bin\keytool.exe` if `keytool` is not on your PATH), or *Show sign-in details* in an app signed with it | A signed APK you install yourself |
   | **Play app signing key** (SHA-1 `7A:C0:0C:8C:C7:EA:11:B0:E2:90:AA:BA:72:C0:16:24:0D:34:A8:1C`) | Play Console ▸ *Test and release ▸ App integrity ▸ App signing key certificate*, or *Show sign-in details* on a Play install | The app installed **from Google Play** (internal, closed and production). Without this client, Drive sign-in fails with error 10 for every Play user |

   The only reliable check is the app itself: on the phone, Settings ▸ Backup & restore ▸ **Show sign-in details** prints the package and the SHA-1 of the key that signed *that* install. Put exactly that SHA-1 into a client. A custom URI scheme is **not** needed (leave *Enable custom URI scheme* off).
6. Run the app ▸ Settings ▸ Backup & restore ▸ **Connect Google Drive**. No client ID is pasted into the code. New clients can take a few minutes to start working.

**What 1.3 puts in Drive:** a normal, visible folder **`KingLog`** in My Drive with the dated folders inside (`KingLog/2026/oct-26/25-oct-2026`). *Delete Drive backups* removes only that folder. Backups that 1.2 or earlier put in the hidden app-data area are no longer reachable (the old scope is gone); restore from them with 1.2 first, or from the phone copy, then back up again. The Drive copy is an extra, not the only copy.

**Clients made for another app (for example DreamLoop's, package `com.teamexykings.dreamloop`) do not work for KingLog: Google matches the package name and the SHA-1 together.** KingLog needs its own Android clients; they can sit in the same Cloud project as your other apps, but a project of its own keeps the consent screen simple.

### If your account closes the moment you tap it
KingLog now shows Google's own error number under the *Connect* button and in the *Restore* screen:

| Error | Meaning | Fix |
|---|---|---|
| **10** (developer error) | No Android OAuth client matches this install's package + SHA-1 | Add a client with the values from *Show sign-in details*. New clients can take a few minutes (rarely hours) to start working |
| **12501** right after you pick the account | Same cause as 10 on many phones; if you simply backed out, nothing is wrong | As 10 |
| **12500** | Sign-in could not finish | Drive API enabled? Your Gmail listed as a **test user** on the consent screen? Google Play services updated? |
| **7** | No internet | Connect and retry |
| **8** | Temporary Google problem | Retry later |

The usual trap: the key you sideload (upload key or debug key) and the key Play re-signs the app with are different, so a client made for one does not work for the other.

---

## 3. Build a signed AAB / APK

**Keep the signing secrets outside the project.** `app/build.gradle.kts` reads four values and signs the `release` build only when all four are present; with none of them, `release` builds unsigned and nothing in the repository holds a password.

1. Put the keystore anywhere **outside** the project folder (for example `C:/Users/Yash/Downloads/KingLog/docs/keystroke/key-upload.jks`). `*.jks`, `*.keystore` and `/docs/` are git-ignored.
2. Open (or create) your **global** Gradle file `C:\Users\Yash\.gradle\gradle.properties` (not the one inside the project) and add, with forward slashes in the path:

   ```properties
   KINGLOG_UPLOAD_STORE_FILE=C:/Users/Yash/Downloads/KingLog/docs/keystroke/key-upload.jks
   KINGLOG_UPLOAD_STORE_PASSWORD=your-store-password
   KINGLOG_UPLOAD_KEY_ALIAS=your-key-alias
   KINGLOG_UPLOAD_KEY_PASSWORD=your-key-password
   ```
   (The keystore's alias is `key0`. `KINGLOG_UPLOAD_STORE_KEY_ALIAS` and `KINGLOG_UPLOAD_STORE_KEY_PASSWORD` are accepted as alternative names for the last two. If some of the four are missing or empty, Gradle prints a warning that the release build will be unsigned. Environment variables with the same names also work, handy for CI.) Other apps can use their own prefix in the same file, for example `KC_` for King Calc; the names do not clash.
3. *Build ▸ Generate Signed App Bundle / APK…* still works, or run `./gradlew bundleRelease` / `assembleRelease`: the build picks the values up by itself. Release has R8 minify and resource shrink on.
4. First time only: create the keystore (`keytool -genkeypair -v -keystore kinglog-upload.jks -alias kinglog -keyalg RSA -keysize 2048 -validity 10000`) and **back up the `.jks` and its passwords offline**. Lose the upload key and you must ask Google to reset it.

The current build code is **13** (`versionName 2.3.0`). Raise `versionCode` for every upload; Play rejects a number it has already seen.

---

## 4. Test checklist for 1.7

**First run / upgrade**
- [ ] Fresh install: Welcome offers *Get started* and *Restore from a backup*. *Get started* goes to the **recovery key** step, then the **app lock** step. The key step cannot finish until the key file is saved or "I wrote it down" is ticked; the Save-as screen opens in Downloads.
- [ ] Upgrade from 1.0 (same key): data appears, Finance still correct, old IDs are in Passwords ▸ *Government IDs* ▸ category Government, key step appears once.

**Lock**
- [ ] Settings ▸ App lock ▸ KingLog PIN, pattern, and *Use my phone's lock*. Background the app > 30 s ▸ lock screen. 5 wrong PINs ▸ 30 s lock-out. "Forgot PIN" uses the phone lock; data stays.
- [ ] Phone-lock mode: remove the phone's screen lock in Android settings ▸ KingLog does not lock you out.
- [ ] **Android 9 / 10 phone (for example a Galaxy S9):** installs from Play, *Use my phone's lock* and *Forgot PIN* open the phone-lock prompt without crashing.

**Backup and restore**
- [ ] Home ▸ *Back up now* ▸ `Documents/KingLog/2026/oct-26/25-oct-2026` (year, `mmm-yy`, `dd-mmm-yyyy`) has `kinglog_finance.klb`, `kinglog_emi.klb`, `kinglog_vault.klb`, `kinglog_fuel.json`, `manifest.json`. The `.klb` files are unreadable; the fuel JSON is readable.
- [ ] Connect Drive (Google asks for the new, smaller permission) ▸ back up ▸ a normal **KingLog** folder with the same year / month / day folders appears in My Drive; *Open the KingLog folder in Drive* opens it; Drive frequency (Weekly / Monthly) and Wi-Fi only work. *Delete Drive backups* removes only that folder.
- [ ] Phone, folder and Drive each get their copy even if one of them fails (the report names the one that failed). Back up the same day twice: one dated folder, the newer files. If Android reports *an older file with this name cannot be opened*, the backup lands in a `... (2)` folder instead of failing.
- [ ] Choose a folder ▸ back up ▸ `KingLog/…` appears there. *Forget* leaves the files.
- [ ] Restore ▸ *This phone* ▸ pick a date ▸ preview shows counts ▸ **Merge** keeps existing data; **Replace** first writes `_before_restore/<time>`. Wrong key ▸ "nothing was changed".
- [ ] Uninstall, reinstall, Welcome ▸ *Restore from a backup* ▸ *This phone* finds nothing and hints to use *A folder* ▸ pick **Documents** ▸ restore works with the typed recovery key.
- [ ] Restore a 1.0 backup folder (old Vault asks for the old passphrase).
- [ ] Rotate the phone during a backup or restore ▸ it still finishes.
- [ ] **Android 8 / 9:** Backup & restore shows *Allow storage access*; after allowing it, *Back up now* writes to `Documents/KingLog`; exports use the *Save as* screen and never ask for storage.
- [ ] **Clear-storage test (1.2.1):** Fuel Log ▸ Vehicles ▸ *Excel* ▸ *Save as* ▸ save in Downloads. Backup & restore ▸ *Choose a folder for a safe copy* ▸ pick Documents ▸ *Back up now*. Then Android Settings ▸ Apps ▸ KingLog ▸ Storage ▸ **Clear storage**. Check that the Excel file is still in Downloads, that the backup folder you chose still holds the dated `KingLog/...` folders, and (to learn what your phone does) whether `Documents/KingLog` is still there. Open KingLog ▸ *Restore from a backup* ▸ *A folder* ▸ pick the chosen folder ▸ restore works with the recovery key.
- [ ] **Google sign-in:** Connect Drive ▸ pick the account ▸ *Connected as …*. On failure, the message shows an error number and *Show sign-in details* lists the package + SHA-1.

**Vault**
- [ ] Four tabs; compact list, tap ▸ bottom sheet; eye reveals secrets; copy clears after 30 s; star a favourite; category chips; search never matches secrets.
- [ ] Add a password ▸ generator (length, types, skip look-alikes). Card with `exp` this month ▸ *Expires soon*; past ▸ *Expired*. WiFi ▸ QR code scans on another phone.
- [ ] Editing any entry asks to confirm first. Settings ▸ *Import old Vault JSON files*.

**Finance**
- [ ] Simple / Pro calculators match the old numbers. Pick a **future** month ▸ *Copy from last month*. *Plan ahead* ▸ clone 3 months ▸ preview lists Create / Refresh / Skip; months with real numbers are never touched.
- [ ] Change the phone date (or wait) so a planned month arrives ▸ banner asks to confirm. Months tab shows deltas and the yearly summary.

**EMI plan (new in 1.2)**
- [ ] Upgrade from 1.1 keeps everything (database moves from v2 to v3 by itself). Finance ▸ bottom bar shows *Calculator · Months · EMI plan · Analytics*; Home ▸ Analytics shortcut still opens Analytics.
- [ ] *Import from Excel* (⋮ menu) with your own workbook ▸ preview shows rows, notes and any schedule sheets with a loan to attach them to ▸ *Add these rows to my plan* / *Replace my plan with this sheet*. A file with no EMI rows is refused with a plain message.
- [ ] Plan view: pick a month ▸ Inflow, Loan EMIs, SIP, Insurance, Living, Total outflow, Savings (red when negative), *Savings without food card*. Tap a cell in Grid ▸ *This month only* or *From this month on*. A salary row linked to Finance offers *This month only* only.
- [ ] Add a loan ▸ months left or start/end date ▸ *What KingLog works out* shows end date, outstanding and next EMI split (needs interest % or loan amount + opening date + tenure). Pre-close view ▸ totals (A), (B), Grand Total match your sheet.
- [ ] Loan ▸ *Payment schedule* ▸ calculated or *I'll enter it* ▸ edit one row ▸ it is marked *edited* ▸ *Restore* brings the calculated row back ▸ check line shows schedule total vs loan amount.
- [ ] ⋮ ▸ *Export to Excel* ▸ warning ▸ phone lock / PIN prompt ▸ Android *Save as* screen (opens in Downloads) ▸ the saved file opens in Excel with *EMI Planner*, *Pre Close* and one sheet per schedule; formulas recalculate when edited.
- [ ] Back up ▸ restore **Merge** into a phone with a different plan: rows match by section + name + bank, nothing is duplicated; **Replace** makes it match the backup. Restoring a 1.1 backup (no EMI file) still works.

**Fuel Log**
- [ ] Vehicles ▸ add; fill-ups auto-fill litres / price / total; odometer warning. Tank card, Stats, Records. The tabs read **Records, Vehicles, Stats** left to right; the km/l on each tank and in Stats has two decimals.
- [ ] Records and Stats (with two or more vehicles): the **Type** drop-down (All / Bikes / Cars) and the **Vehicle** drop-down (All, or one vehicle of that type) filter both tabs; choosing a new type resets the vehicle to All.
- [ ] Stats: the chips read **All, 3M, 6M, 12M** with **All** selected on opening; the six summary tiles are the same size; MONTHLY SPEND shows each month's total, turned 90°, above its bar.
- [ ] Vehicles tab: press and hold a vehicle ▸ selection mode (ticks, "N selected", **Select all** / **Clear**, **Delete**); the back button leaves selection mode; Delete asks first and says how many fill-ups go with the vehicles.
- [ ] Vehicles tab ▸ **Excel** and **JSON** export open the Android *Save as* screen (Downloads suggested) and then offer sharing; the `.xlsx` opens in Excel / Sheets with three sheets. **Import** the JSON and `docs/sample-data/Vehicles.csv` + `Fuel_Log.csv`; a second import skips duplicates.

**Settings, About and names (1.3)**
- [ ] The launcher label and every title say **King Log**; the folder in Documents and Drive is still `KingLog`.
- [ ] Settings ▸ HELP & ABOUT has **FAQ** and **About King Log**. Settings ▸ PRIVACY & DATA has only *Delete your data* (a dialog with the three places your data can be and how to remove each, and a link to the web page; no email). Settings ▸ RESET has *Reset settings to default* (asks first, then puts theme, colours, Finance mode, screenshot blocking, reminders and the backup schedule back to first-run values and leaves data, app lock, recovery key, Drive link and backup folder alone) and *Reset everything* (you must type RESET; it deletes all data and settings, removes the app lock, backup key and Drive link, and shows the welcome screens again; backup files stay).
- [ ] **Reset everything:** add a PIN, a vehicle and a Vault entry, make a backup, then Settings ▸ RESET ▸ *Reset everything* ▸ the button stays disabled until you type RESET ▸ the app shows the welcome screens, no lock, empty Vault / Fuel, default theme ▸ "Restore from a backup" brings the data back with the recovery key you saved.
- [ ] About, top to bottom: turning logo (a coin-spin now and then, and a thin 1 dp white outline on its rounded corner that breathes and flickers briefly; nothing is drawn behind the tile), **Visit Website** (opens the site in the browser), **Privacy Policy**, **Terms of Use** and **Delete your data** (each opens its own screen inside the app, like Credits, with an *Open this page on the web* button to teamexykings.in/King-Log), **Credits** (its own screen, grouped by tech stack), **Support / Contact** (mail with subject `KingLog Support/Request - <date time>` and the greeting body), **Share App** (Android share sheet with the Play link).
- [ ] Put the app in the background on About: the logo and its outline stop (no CPU). Phone setting *Remove animations* on: the logo stays still and the outline stays on, steady.

**Home, legal screens and logo (2.2)**
- [ ] Home, top to bottom: the logo tile, a greeting with the date, and round **i** (About) and **?** (FAQ) buttons next to the Settings gear; three quick buttons (+ Fill-up, + Expense, + Password); a wide **Finance** card (this month's savings as a big number, a line of your last up to 6 recorded months that draws itself in, shortcut chips); **Vault** and **Fuel Log** side by side (Vault: entry count, a bar split by category and four tiles; Fuel: this month's spend, small bars of the last 6 months and four tiles); and a **Backup** card with a ring that empties over a week (green = backed up, amber = old or none yet, red = no key or a problem) and a button-looking tag. Compact (the default) keeps every chart but draws them smaller; Roomy gives them more height.
- [ ] About ▸ **Privacy Policy**, **Terms of Use**, **Delete your data**: each opens a screen in the app with the same text as the web page, a back arrow, tappable links and an *Open this page on the web* button. `LegalDataTest` fails if the app text differs from the root `*.html` pages; after editing those pages run `python3 tools/gen_legal.py . app/src/main/java/com/teamexykings/kinglog/ui/settings/LegalData.kt`.
- [ ] Launcher icon: pitch-black tile with a white crown over a log book; the book's three lines carry a coin (Finance), a fuel drop (Fuel Log) and a keyhole (Vault). Check it round, squircle and as the themed (monochrome) icon on Android 13+, and the small status-bar icon when a reminder shows.

**Fit to the phone (1.9)**
- [ ] On a small phone (Galaxy S9, or any phone with Display size set large): the bottom tab bar in Finance and Fuel shows the open tab's full name on one line; the recovery key on the key screen and in *Show my recovery key* is on one line (it shrinks); About fits without cutting any text.
- [ ] Settings ▸ Appearance ▸ **Layout**: *Compact* (the default for new installs) makes every screen tighter; *Roomy* gives everything more space. Anyone who already chose a layout keeps it. *Reset settings to default* returns it to Compact.
- [ ] Welcome ▸ *Lock King Log*: the button is pinned at the bottom (the step scrolls above it). Choose *Phone lock* ▸ after the fingerprint check the button says **Continue**, not *Skip for now*.

**Reminders**
- [ ] Settings ▸ Reminders ▸ turn on (asks for notification permission) ▸ set a vehicle's insurance to tomorrow ▸ notice appears once; the lock screen shows no vehicle name.

**Shortcuts and FAQ**
- [ ] Long-press the icon ▸ *Add fill-up / Add expense / Open Vault* (unlock first if a lock is set). Settings ▸ FAQ opens with a search bar and one chip per category (All, Getting started, Privacy and security, Backups, and so on, each with its count); typing narrows every category, a chip narrows to one.

---

## 5. Project layout

```
app/src/main/java/com/teamexykings/kinglog/
├─ MainActivity.kt, KingLogApp.kt   entry, manual DI container (AppContainer), shortcut hand-off
├─ core/       Config, Format (₹), Csv, MiniJson, ColorUtil, Credits (grouped by tech stack), DensityMath (Compact layout, text-size limit, shrink-to-fit)
├─ data/       Room entities / DAOs / DB (v3), repositories (incl. EmiRepository), Prefs, LegacyMigrator (1.0 → 1.1)
├─ domain/     FinanceCalc, FinancePlanner, FinanceModels, FuelCalc, ReminderPlanner, EmiModels, EmiCalc, EmiSchedules, EmiPlanMerge      (pure Kotlin)
├─ vault/      VaultSchema, VaultJson, VaultCrypto (PBKDF2 + AES-GCM), PasswordGenerator, WifiQr, IdsMigration / CardExpiry   (pure Kotlin)
├─ backup/     BackupFormat, BackupLayout (YYYY/mmm-YY/DD-mmm-YYYY folders), FinanceJson, FuelJson, LegacyFuelCsv, XlsxWriter, FuelXlsx, XlsxBook, EmiXlsx, EmiImport, BackupStoreApi, FileBackupStore, SignInHelp   (pure Kotlin)
│              BackupStore (Device / Folder / Drive), BackupService, RestoreService, BackupManager, DriveClient, Exporter, SigningInfo
├─ security/   LockManager, PatternCode, RecoveryKey, BackupKeyStore, KeystoreRowCipher
├─ work/       Workers (weekly local backup, optional Drive backup, daily reminders)
└─ ui/         theme (incl. Density: Roomy / Compact), components (incl. GlowingLogo with its blinking white edge, FitText), home (HomeScreen holds the data, HomeCards draws the cards), lock, vault, finance, emi, fuel, backup, settings (FAQ, About, Credits, Privacy / Terms / Delete-your-data screens, links), nav
app/src/test/  186 unit tests (pure Kotlin)
docs/          git-ignored: docs/play-store (Play Console artwork and listing text) and docs/sample-data (demo Vehicles.csv + Fuel_Log.csv for import testing)
tools/         faq_content.py + gen_faq.py (FAQ source), gen_legal.py (in-app Privacy / Terms / Delete-your-data text from the web pages in the project root), logo_geometry.py + make_icons.py (logo and icons), see tools/README.md
index.html, faq.html, privacy-policy.html, terms-of-service.html, account-deletion.html, assets/   the website pages for teamexykings.in/King-Log/ (upload these to the King-Log folder of the site repo; they are also the source of the in-app Privacy / Terms / Delete-your-data screens)
```

Run the unit tests: `./gradlew testDebugUnitTest`.

### Backup format v2
```
KingLog/YYYY/mmm-YY/DD-mmm-YYYY/      e.g. KingLog/2026/oct-26/25-oct-2026/   (1.0 to 1.2 used KingLog/2026/10/25: still found and restored)
  kinglog_finance.klb   "KLV"0x01 | iterations | salt16 | iv12 | AES-GCM ciphertext   (key: PBKDF2-HMAC-SHA256, 600 000 rounds, from the recovery key)
  kinglog_emi.klb       same layout and key (EMI plan; new in 1.2, older backups simply do not have it)
  kinglog_vault.klb     same layout, same salt, its own random IV
  kinglog_fuel.json     plain JSON (vehicles + fill-ups; distance and mileage are calculated, never stored)
  manifest.json         app version, counts, SHA-256 and size of each file; written last
KingLog/_before_restore/<yyyyMMdd-HHmmss>/   safety copies made before a Replace restore (3 kept)
```
Restore reads and checks everything first (manifest ▸ checksums ▸ decrypt ▸ parse), then applies it in one go. Merge de-duplicates; Replace makes the phone match the backup after taking a safety copy.

### How the data is stored
- **Vault:** every row is a JSON map encrypted with an AES-256-GCM key in the Android Keystore (`kinglog_vault_key_v1`), stored as `Base64(iv | ciphertext+tag)`.
- **Finance:** each month and the salary profile are JSON encrypted with the same key; only year, month, planned flag and save time are readable columns.
- **EMI plan:** the whole plan (rows, one-off and step changes, schedules, settings) is one JSON document encrypted with the same Keystore key and stored in a single `emi_plan` row (DB v3, migration 2→3 only adds the table).
- **Fuel:** plain Room tables (vehicles, fuel_logs); no secrets.
- **Lock:** PIN / pattern as a salted PBKDF2-HMAC-SHA256 hash (120 000 rounds) in EncryptedSharedPreferences. The backup key (recovery key or passphrase) is kept in EncryptedSharedPreferences too, and is shown only after the owner proves identity.
- If the Keystore key is ever lost, rows show as unreadable. Restore from a backup brings them back.

### How the numbers work
- **Mileage (per fill):** full-to-full: distance since the previous *full* fill ÷ litres added since then. A partial fill carries its litres forward. A fill after *Missed fill-up* or with a non-increasing odometer has no distance.
- **Average mileage / cost per km:** Σ valid distance ÷ Σ fuel of those same fills.
- **Finance:** gross, deductions incl. food card, net bank, gratuity accrual `round(basic × 0.0481)`, gratuity `basic × years × 15 / 26`, PVL per day = monthly ÷ divisor (default 26). TDS is only what you type.

---

## 6. Play Console notes

- **Data safety:** the developer collects nothing. Data stays on the device (Vault and Finance encrypted with a Keystore key). Backups go only where the user sends them: `Documents/KingLog` on the phone, a `KingLog` folder in the user's own Google Drive (per-file `drive.file` access), or a folder the user picks. Finance, EMI plan and Vault backup files are encrypted with the user's key, Fuel backups are plain JSON. The Excel export of the EMI plan (like the Fuel export) is a plain file the owner saves through Android's Save-as screen after a warning and an identity check; it is the owner's own file, not stored inside KingLog. No analytics, no ads, no data sharing. Users can erase everything in Settings and delete Drive backups from Backup & restore.
- **Permissions:** `INTERNET` (Drive only), `USE_BIOMETRIC` (unlock), `POST_NOTIFICATIONS` (only after the user turns reminders on), `READ/WRITE_EXTERNAL_STORAGE` with `maxSdkVersion=28` (Android 8-9 only, for backups in Documents; exports use the Save-as screen and need no permission; never requested on Android 10+). WorkManager adds a few standard ones (`RECEIVE_BOOT_COMPLETED`, `WAKE_LOCK`, `ACCESS_NETWORK_STATE`).
- Compliance review (children, fonts, session replay, email, subscriptions, DMCA) and the Play form answers: [LEGAL_AUDIT.md](LEGAL_AUDIT.md).
- `allowBackup=false` and data-extraction rules exclude all app data from Android auto-backup and device transfer. Backups are made by KingLog itself.
- Content rating: Everyone. Category: Finance or Tools. Pricing: free, ad-free and subscription-free, no in-app purchases and no donation links. Privacy policy: `https://teamexykings.in/King-Log/privacy-policy.html` (King Log's own text is in `privacy-policy.html`; the page at that address must be the KingLog one). Terms: `.../terms-of-service.html`. Account / data deletion URL (Data safety form): `.../account-deletion.html`. Target audience: **18 and over** (no children's programme).
- Store icon: `app/src/main/ic_launcher-playstore.png` (512×512). Listing text (name, short and full description, what's new): `docs/play-store/store-listing.md`. Feature graphic: `docs/play-store/feature-graphic-1024x500.png` (made by `tools/make_feature_graphic.py`). Screenshots still to take.

## 7. Licence

© 2026 TeamExyKings. All rights reserved. Free, ad-free and subscription-free for personal use. See [LICENSE](LICENSE).
