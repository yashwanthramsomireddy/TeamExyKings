# King Calc - Calculator

A clean, fast, free calculator for Android. Calculator, currency converter, unit converter, percentage tools and a loan/EMI calculator, in one small app.

**Free for everyone. No ads. No tracking. No account.**

A [TeamExyKings](https://teamexykings.in) product, built by Yashwanth Ram Somireddy.

<!-- Add screenshots here once available:
<p>
  <img src="docs/screenshots/calculator-black.png" width="220" />
  <img src="docs/screenshots/currency-white.png" width="220" />
</p>
-->

## Features

- **Calculator:** live result preview, tap-to-place blinking cursor to edit anywhere, copy/paste/share formulas, calculation history, `%` and `()` handling, `00` key, 15-digit-per-number limit, haptic feedback
- **Currency:** defaults to your own currency based on your phone's region, converts to every one of about 160 currencies at once in a scrollable list with a flag next to each one, search, favourites, tap a row to copy its value, works offline using the last saved rates
- **Currency rate alerts:** set a target rate for any currency pair and get a notification when it's crossed, with a per-alert on/off switch (bell icon on the Currency tab); checked whenever the tab refreshes rates
- **Gold / Silver:** today's and yesterday's spot price for gold (24K/22K, per gram and per 10g) and silver (per gram and per kg), in your own currency, with the change since yesterday
- **Units:** length, weight, temperature, area (including cent, guntha and ground), volume, speed, time and data
- **Percent:** X% of Y, and percentage change
- **BMI:** height and weight in cm/kg or ft+in/lb, your BMI, its category (Underweight/Normal/Overweight/Obese, color-coded), and the healthy weight range for your height
- **Inflation:** what an amount from one year is worth in another, for India and 14 other countries, using the World Bank's free consumer price index data (published once a year, roughly a year behind)
- **Chit Fund:** track multiple chit groups — chit value, members, commission — record each month's winning bid and whether you won it, and see what you've paid in, received and your running net position
- **Discount:** discount, then GST on the discounted price split into CGST/SGST, with the amount saved and the final price
- **VAT:** original price, VAT rate (with common-rate chips), VAT charged and total price
- **SIP / FD:** SIP maturity and returns, and fixed-deposit / lump-sum compound interest with monthly, quarterly, half-yearly or yearly compounding
- **Split bill:** equal split (bill, tip % and number of people), or **by item** — add people and items, tap who shared each item, and the tip is added proportionally to what each person ordered
- **Date difference:** the gap between two dates, or a date plus or minus N days
- **Scientific mode (optional):** sin/cos/tan, log/ln, √/x², !/% and a DEG/RAD switch, plus a "2nd" key for their inverse and alternate forms (sin⁻¹/cos⁻¹/tan⁻¹, 10ˣ/eˣ, ∛/x³, π/ℯ, 1/x), a hyperbolic row (sinh/cosh/tanh and their inverses), Rand, and a one-slot memory (mc/m+/m-/mr) (Settings > Scientific functions). Turning it on also switches on Compact keys, since the extra keys need the room
- **History pins (optional):** pin entries so Clear keeps them (Settings > Pin history entries)
- **Remembers your work:** Loan / EMI, SIP / FD, Units, Split bill's by-item entries, Inflation and Chit Fund's groups, and the Calculator's unfinished formula are kept between tab switches and app restarts (GST, Discount and VAT keep their last rate), each with a reset icon
- **Search the FAQ** (now a menu item, right below Donate), swipe sideways from anywhere to open the menu, and **Reset all tabs** or **Full reset** — back to a fresh install — (Settings)
- **Quick access:** FAQ, About, History and Settings icons sit right in the Calculator's header, in addition to the side menu
- **Export:** share a branded PDF or image of your Loan/EMI, Chit Fund or Split-bill result straight from that tab's header, once there's something to export
- **About:** a Changelog of every version ever released, and a Rate us shortcut straight to the Play Store listing
- **Backup and restore:** settings, history and saved dates to a JSON file at KingCalc/backup/ddmmyyyy/ (Settings)
- **Age (DOB):** up to 15 named people (Add Member, delete), each with remembered dates; sort A to Z, Z to A or your own drag-to-arrange order; optional birthday reminders 1 week and 1 day before, plus a confetti celebration on the day itself over whichever tab is open; a confirmed reset; age in years/months/days plus total months, weeks, days, hours, minutes and live-ticking seconds, and the next birthday
- **GST:** add or remove GST at 5/12/18/28%, with the CGST/SGST split shown
- **Loan / EMI:** monthly EMI, total interest, total payable, an amortization table, and an always-side-by-side comparison of two loans with its own per-loan schedules
- **Landscape:** side-by-side display and keypad with the extra keys always visible
- **Themes:** Black (every surface pure black — the default), Grey, White, follow the system setting, or Custom — pick any color with hue/saturation/lightness sliders (or type a hex code), and the whole app is tinted around it, with a reset button. The 4 built-in themes get 12 accent colors (including Black and White, each hidden when it would be invisible on the current theme)
- **Default tab:** choose which tab opens first, from only the tabs you've kept visible (Settings > Default tab)
- **Visible tabs:** hide any tab except Calculator and put the rest in your own order (Settings > Visible tabs > Arrange)
- **Calculator key shape:** 30 shapes with previews, plus a compact-keys option (Settings)
- **Custom background:** your own image behind the Calculator tab — full screen, upper half or lower half, with a 0–100% opacity slider and optional see-through keys and a live preview (Settings > Custom background); stored on-device only
- **Settings & side panel backgrounds:** two further, independent wallpaper options — one behind the Settings screen itself, one behind the slide-out side panel — each with 6 built-in presets (Space, Floral, Gaming, Tech, Abstract, Ocean) or your own photo, opacity, and a see-through toggle for the cards/rows in front of it (Settings > Settings background / Side panel background)
- **Side panel:** choose left or right (Settings), swipe from that edge to open it, swipe it back to close
- **Number to words:** optional, shows the result spelled out in lakh/crore (Settings)
- **Selective history clear:** check which entries to keep before clearing the rest
- **Fonts:** Inter, Poppins, JetBrains Mono, Space Grotesk, Nunito
- **Number format:** Indian (1,00,000) or international (100,000) grouping
- **Donate:** choose Razorpay or PayPal first; Razorpay just opens its page for you to enter an amount, PayPal lets you pick one (₹50/100/200/500/1000/2000 or your own) in the app first — no features are gated behind it
- **FAQ:** built-in how-to for every feature (Settings > FAQ)
- **Private by design:** everything stays on your device

## Install

- Google Play Store: https://play.google.com/store/apps/details?id=in.teamexykings.kingcalc
- APK builds: see [Releases](../../releases)

## Tech stack

- React Native + Expo (SDK 57) with Expo Router
- TypeScript
- A small built-in expression parser for safe evaluation (no `eval`, no heavy math library)
- AsyncStorage for local history, settings and cached rates
- expo-clipboard for copy and paste
- expo-image-manipulator to resize custom background photos before storing them
- Local Android Studio / Gradle build (via `expo prebuild`'s generated `android/` project) for release builds

There is no backend, database, login, payment system or analytics.

## Getting started

Requirements: Node.js 20+, npm, and the Expo Go app on your phone.

This is a private repository — clone it from your own GitHub account (no public URL to share), then:

```bash
cd KingCalc
npm install
npx expo start
```

Scan the QR code with Expo Go (Android) or the Camera app (iOS). The app runs fully in Expo Go, so no custom dev client is needed. If your phone cannot reach your computer on the same Wi-Fi, run `npx expo start --tunnel` instead.

Useful scripts: `npm run typecheck` checks the TypeScript.

The repo includes an `.npmrc` with `legacy-peer-deps=true`, so a plain `npm install` works. Some Expo SDK 57 packages declare optional peer dependencies (such as `react-dom` and `react-native-worklets`) that npm would otherwise resolve to versions that do not match the SDK.

### Release build

Builds are produced locally with Android Studio / Gradle, from the native `android/` project (generated once with `expo prebuild` and maintained directly since):

1. Bump `android.versionCode` and `expo.version` in `app.json` (and `version` in `package.json` to match)
2. Open the `android/` folder in Android Studio
3. Build → Generate Signed Bundle / APK, using the upload keystore and the `KC_UPLOAD_STORE_FILE` / `KC_UPLOAD_STORE_PASSWORD` / `KC_UPLOAD_KEY_ALIAS` / `KC_UPLOAD_KEY_PASSWORD` properties in `android/gradle.properties` (not committed — keep this file private)
4. Build an installable **.apk** first and test on a device; once confirmed, build the **.aab** for the Play Store

The Android package name is `in.teamexykings.kingcalc` (change it in `app.json` if you fork).

## Project structure

```
app/                 Expo Router screens: calculator, currency, units, percent, loan, settings, faq, about
src/calc/            Expression parser and keypad input rules
src/components/      Drawer, screen wrapper, picker, shared UI
src/unitsData.ts     Unit tables and conversion
src/currency.ts      Rates fetching, caching, currency names, locale-based default currency
src/numberToWords.ts Indian number-to-words (lakh/crore)
src/tabs.ts          Shared tab list for the drawer and Default tab setting
src/age.ts           Age/DOB maths (years, months, days, totals, next birthday)
src/reminders.ts     Plans the two birthday reminders per person, and today's matches
src/remindersIO.ts   Schedules them as local notifications
src/swipe.ts         The sideways-swipe rules for opening the menu
src/people.ts        The saved people on the Age tab (up to 15, sorting, reordering)
src/useSavedScreen.ts Keeps a screen's inputs between visits
src/invest.ts        SIP and compound-interest maths
src/split.ts         Bill split and tip maths (equal and by-item)
src/metals.ts        Gold/silver rate fetching, caching, gram conversion
src/inflation.ts     World Bank CPI fetching, caching, inflation-adjusted amounts
src/rateAlerts.ts    Currency rate alert storage and crossing checks
src/rateAlertsIO.ts  Local notifications for triggered rate alerts
src/chit.ts          Chit group storage and monthly dividend/payout maths
src/dateDiff.ts      Date difference and add/subtract days
src/bmi.ts           BMI maths (metric/imperial conversion, category, healthy weight range)
src/historyPins.ts   Pin ordering and Clear rules for History
src/backup.ts        Backup file format, naming and validation
src/backupIO.ts      Reading and writing the backup file
src/discount.ts      Discount + GST maths
src/gst.ts           GST add/remove maths (base, GST, CGST/SGST, total)
src/donate.ts        Razorpay and PayPal donation links, preset amounts
src/customBg.ts      Picks, resizes and stores a custom background image
src/locale.ts        Shared device-region detection
src/faqData.ts       Questions and answers for the in-app FAQ
src/theme.ts         Black and White theme tokens
src/fonts.ts         The five bundled fonts
src/settings.tsx     Saved settings (theme, font, number format, haptics)
assets/              Icon, adaptive icon, splash
```

## Currency rates

Exchange rates come from the free open-access endpoint of [ExchangeRate-API](https://www.exchangerate-api.com) (`open.er-api.com`, no API key). It updates once a day and requires attribution, which the app shows on the Currency and About screens. Rates are cached on the device, refreshed at most once an hour, and work offline with a "last updated" time. Rates are for information only and are not suitable for financial transactions.

## Contributing

This is a private repository; it is not open for outside issues or pull requests.

Please do not add ads, analytics, trackers or paid features. They go against the goals of this project.

## Privacy

King Calc collects no personal data. See [PRIVACY.md](PRIVACY.md) (or the hosted version, [privacy-policy.html](https://yashwanthramsomireddy.github.io/TeamExyKings/King-Calc/privacy-policy.html)) and the [Terms of Use](https://yashwanthramsomireddy.github.io/TeamExyKings/King-Calc/terms-of-service.html).

## License

All rights reserved. King Calc is not open source — the app, its name, icon, source code and TeamExyKings branding are the property of Yashwanth Ram Somireddy / TeamExyKings. See [LICENSE](LICENSE).

Third-party libraries and fonts keep their own licenses (Inter, Poppins, JetBrains Mono, Space Grotesk and Nunito are under the SIL Open Font License).

## Credits

Built by **Yashwanth Ram Somireddy** at **TeamExyKings**. Website: https://teamexykings.in
