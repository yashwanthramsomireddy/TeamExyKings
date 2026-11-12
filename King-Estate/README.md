# King Estate

Offline property document checklist for Indian property transactions, by TeamExyKings.
Kotlin · Jetpack Compose · Room · DataStore · WorkManager. No account, no network, no analytics.
Current version: 2.1.0 (build 12). See CHANGELOG.md for release notes.

## Build
1. Open the folder in Android Studio and let Gradle sync (install "Android SDK Platform 36" in SDK Manager).
2. Debug run: select a device and press Run.
3. Play bundle: Build > Generate Signed Bundle / APK > Android App Bundle > release.

### Signing
Add these to your user-level `C:\Users\<you>\.gradle\gradle.properties` (never inside the project):

    KINGESTATE_UPLOAD_STORE_FILE=C:/path/to/key-upload.jks
    KINGESTATE_UPLOAD_STORE_PASSWORD=...
    KINGESTATE_UPLOAD_KEY_ALIAS=...
    KINGESTATE_UPLOAD_KEY_PASSWORD=...

## Web pages
`privacy-policy.html`, `terms-of-service.html` and `account-deletion.html` sit in the project root.
Upload them to the `King-Estate/` folder of the TeamExyKings website. The app links to:
- https://teamexykings.in/King-Estate/privacy-policy.html
- https://teamexykings.in/King-Estate/terms-of-service.html
- https://teamexykings.in/King-Estate/account-deletion.html
Run `python tools/verify_web.py .` from the project root before uploading.

## Store listing
`docs/store-assets/` holds icon-512.png, feature-graphic.png and store-listing-full-description.txt.
Open every government link in a browser before submitting; Play rejects listings with broken source links.

## Notes
- Database changes must ship a Room Migration (destructive fallback is intentionally off). Version 2 adds attachments.
- Attached files are saved through MediaStore to Downloads/King Estate/<transaction type>/<document>/.

---
© 2026 TeamExyKings. All rights reserved. Proprietary software; see LICENSE.
