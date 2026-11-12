# Changelog

All notable changes to King Compass are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.0.0] — 2026-11-12

versionCode 1 · First release.

### Added

**Compass**
- Heading 0–360° with 16-point cardinal direction and full name
- Compass dial with 5° ticks, cardinal letters and 30° bearing numbers
- Two dial modes — rotate the card, or keep north fixed and rotate the needle
- Live magnetic field strength in µT and Android sensor accuracy
- Interference warning when the field falls outside roughly 20–75 µT
- Tilt warning once the phone is more than about 25° off flat
- Short haptic tick each time the heading crosses a cardinal point
- Graceful fallback on devices with no magnetometer

**Level**
- Two-axis bubble level with pitch and roll to 0.1°
- Bubble rises to the high side, like a real spirit level
- Degree rings at 3°, 6° and 9°, with a 12° full-scale dial
- Turns green and ticks once within 0.3° of level
- "Calibrate to this surface" stores a zero offset; clearable at any time

**Incline**
- Slope angle from horizontal, 0–90°, read off the phone's long edge
- Grade percentage and roof pitch in x:12
- Angle from vertical alongside
- Protractor gauge with arc, slope arm and 15° ticks
- Hold freezes the reading so the phone can come off the surface first

**Calibration**
- Step-by-step guide with an animated figure-8 to follow
- Live accuracy and field-strength readouts while you calibrate
- Notes on why a compass drifts, and what to try when it still reads wrong

**Settings**
- Dial behaviour, smoothing (Low / Medium / High)
- Field strength and tilt warning toggles
- Haptic feedback and keep-screen-on toggles
- Clear level calibration; reset all settings

**Elsewhere**
- FAQ with search and five categories — Compass, Level, Incline, Privacy and
  permissions, General
- About screen with version, website, privacy policy, terms of use, credits,
  support and share
- Credits listing the tools the app is built with and where the readings come from
- Pitch-black AMOLED theme with a white accent throughout
- Landing page, privacy policy and terms of use
