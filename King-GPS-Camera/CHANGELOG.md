# Changelog

All notable changes to King GPS Camera.
This file carries the latest released version only; the full history lives in
the app's own Changelog screen under About.

## [1.0.0] — 2026-11-12

First release. Version code 1.

### Added
- Location stamp burned into every photo at the moment of capture: place
  headline, country flag, full street address, coordinates, altitude, accuracy,
  bearing, date, time and time zone.
- Four stamp templates — Classic, Detailed, Compact and Minimal — each
  positionable in any of the four corners.
- Independent on/off switch for every stamp field, plus a custom note line for
  a site name, job number or inspector's initials.
- Coordinate formats: decimal degrees and degrees/minutes/seconds.
- Units: metric and imperial. Five date layouts and a 12/24-hour clock.
- Adjustable stamp text size and backdrop opacity.
- Decorative location badge tile beside the text on the wide templates.
- Live viewfinder overlay drawn by the same renderer that writes the saved
  file, so the preview is the result rather than an approximation.
- GPS coordinates, altitude, bearing, speed and timestamps written into the
  photo's EXIF metadata.
- Photos saved straight into the device gallery under
  `Pictures/King GPS Camera`, with an in-app grid to browse, open, share and
  delete them.
- Camera controls: flash off/auto/on, front and rear lens, pinch-free zoom
  stops, tap-to-focus and rule-of-thirds grid lines.
- Optional mirroring for front-camera shots and an adjustable JPEG quality.
- Settings export and restore as a local JSON file.
- First-run permission screen that explains what each permission is for, and a
  working camera-only mode when location is declined.
- FAQ with search, and an About screen with changelog, credits and contact.
- Pitch-black AMOLED theme with a white accent throughout.

### Notes
- Google Play services location is used where available; the app falls back to
  Android's own `LocationManager` on devices without it.
- Reverse geocoding is the only network call the app makes. Turning off both
  the place headline and the full address stops it entirely.
