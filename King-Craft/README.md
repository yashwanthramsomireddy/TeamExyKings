# King Craft

Free, offline PDF and image tools for Android, by **TeamExyKings**.

- **PDF Tools**: merge, split / extract pages, reorder / rotate / delete pages, compress, watermark, page numbers, lock / unlock with password, extract text, place a drawn signature image or text.
- **Image Resizer**: exact width x height (px / cm / mm / in), max file size in KB, fit / fill & crop / pad / stretch, editable presets, JPEG / PNG / WebP, optional metadata keep, before and after preview.
- **OCR**: image to text, on device (English).
- **Image to PDF**: gallery photos or camera scan, reorder and rotate, A4 / Letter / fit-to-image.
- Themes: Pitch Black (default), Dark, White, System, Custom, accent picker. About, FAQ and donate (Razorpay / PayPal, opens in the browser).

No ads. No tracking. No accounts. **No INTERNET permission**: files never leave the phone.

## Stack

Kotlin 2.0.21, Jetpack Compose (Material 3), Navigation Compose, DataStore, AGP 8.7.3, Gradle 8.9.
minSdk 26, targetSdk 35, package `com.teamexykings.kingcraft`.

Libraries (all permissive):
- `com.tom-roush:pdfbox-android` 2.0.27.0 (Apache 2.0) for PDF editing
- Android `PdfRenderer` for page previews
- ML Kit Text Recognition (bundled, offline) and ML Kit Document Scanner (via Google Play services)
- AndroidX, Compose, ExifInterface, DataStore

## Build

1. Open the `KingCraft` folder in Android Studio (Ladybug or newer). Let Gradle sync.
2. Test APK: **Build > Build Bundle(s) / APK(s) > Build APK(s)** (debug), or run on your Galaxy S9 (Android 10).
3. Release AAB: add these to `C:\Users\<you>\.gradle\gradle.properties` (keystore stays OUTSIDE the repo):

```
KRAFT_STORE_FILE=C:/keys/kingcraft-upload.jks
KRAFT_STORE_PASSWORD=...
KRAFT_KEY_ALIAS=...
KRAFT_KEY_PASSWORD=...
```

Then **Build > Generate Signed Bundle** or run `gradlew bundleRelease`. Back up the keystore.

## Notes

- Saved files go to `Downloads/KingCraft` (PDF, txt) and `Pictures/KingCraft` (images) via MediaStore.
- On first launch the app asks (optionally) for camera access; photos and files use the system pickers. On Android 8/9 it asks for storage permission only when you tap Save.
- The camera scanner is Google's on-device scanner (Play services). Without Play services, use gallery images.
- After a release build, check the merged manifest has no INTERNET permission
  (`app/build/intermediates/merged_manifests/release/.../AndroidManifest.xml`).

## Licence

King Craft is proprietary: all rights reserved (see `LICENSE`). The repository is private. Third-party libraries keep their own licences and are listed in the app under About > Credits.

## Website files

`index.html`, `privacy-policy.html` and `terms-of-service.html` sit in the project root. Copy them to the `King-Craft/` folder of the `teamexykings.github.io` repo (they are served at `https://teamexykings.in/King-Craft/`).

## Release signing

Put these in your user-level `~/.gradle/gradle.properties` (on Windows `C:\Users\<you>\.gradle\gradle.properties`), never in the project:

```properties
KINGCRAFT_UPLOAD_STORE_FILE=C:/Users/Yash/keystore/kingcraft-upload.jks
KINGCRAFT_UPLOAD_STORE_PASSWORD=...
KINGCRAFT_UPLOAD_KEY_ALIAS=...
KINGCRAFT_UPLOAD_KEY_PASSWORD=...
```
