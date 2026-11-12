# Changelog

All notable changes to King QR are recorded here.
This file keeps only the latest released version; the full history lives in the app's own Changelog screen (About → Changelog).

## [1.0.0] — 12 November 2026

**versionCode 1 · versionName 1.0.0**

First release.

### Scanning
- Live camera scanning with CameraX and Google ML Kit's on-device barcode model.
- Supports QR Code, Aztec, Data Matrix, PDF417, EAN-13, EAN-8, UPC-A, UPC-E, Code 39, Code 93, Code 128, ITF and Codabar.
- Scan frame overlay with corner brackets and a sweeping scan line; torch toggle and front/back camera flip.
- Scanned payloads are classified and parsed: links, Wi-Fi credentials, UPI payment requests, MECARD/vCard contacts, email, phone, SMS, geo, product codes and plain text.
- Result sheet shows the parsed fields plus the raw value, with Copy, Share and a type-aware Open action.
- Repeat detections of the same code are collapsed so the sheet does not flicker.

### Generating
- QR codes for text, links, Wi-Fi networks, UPI payment requests and contact cards.
- Codes are rendered dark-on-light with a four-module quiet zone and whole-pixel modules, so they stay crisp and scannable at any size.
- Payload length is checked against the QR capacity before encoding, with a clear message when it is too long.
- Save to `Pictures/King QR` or share the PNG straight to another app.

### History
- Every scan and generated code is stored on the device, newest first, with no cap.
- Search across values, types and formats; filter by All, Scanned, Generated or Starred.
- Star, delete individual entries, or clear everything.
- Backup and restore the full history as a JSON file from Settings.

### Appearance
- Pitch-black AMOLED theme by default, plus Dark, Light and System.
- Seven accent colours: white, blue, green, purple, amber, cyan and rose.

### Settings
- Vibrate on scan, copy automatically, open links automatically, save scans to history.

### Privacy
- No `INTERNET` permission — the app cannot send anything anywhere.
- Camera frames are analysed in memory and discarded; nothing is recorded.
- No accounts, no analytics, no advertising SDKs, no tracking.

### Other
- About screen with version, Visit website, Privacy policy, Terms of use, Changelog, Credits, Support / Contact, Rate us and Share.
- FAQ screen with categories and search.
- Splash screen via the AndroidX SplashScreen API.
