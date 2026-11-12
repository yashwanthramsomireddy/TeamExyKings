# Changelog

All notable changes to King Grid are recorded here.
This project uses [Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-11-12

versionCode 1 · versionName 1.0.0 · first release

### Added
- Photo splitting into ten grid layouts: 1×2, 2×1, 1×3, 3×1, 2×2, 2×3, 3×2, 3×3, 4×3 and 4×4.
- Live preview with the grid drawn over the cropped photo and every tile numbered.
- Carousel numbering (reading order) and feed-grid numbering (reversed, so posting 1 to N rebuilds
  the picture on a profile grid).
- Square tiles option: centre-crop for perfect squares, or off to keep the photo's proportions.
- Full-resolution tile extraction with `BitmapRegionDecoder`, with a whole-bitmap fallback for
  formats that cannot be region-decoded.
- Per-tile EXIF rotation handling, so portrait photos come out upright.
- Saving to Pictures/KingGrid in a dated folder per split, with numbered filenames and staggered
  timestamps so the gallery sorts tiles in posting order.
- Share all tiles, and open the set in the gallery.
- Settings: output format (JPEG/PNG), JPEG quality, default layout, default numbering, default
  square-tiles, tile numbers in preview, haptic feedback, reset.
- About screen with website, privacy policy, terms of use, FAQ, credits, support mail and share.
- FAQ screen with search and categories.
- Pitch-black AMOLED theme with a white accent, and the SplashScreen API on launch.
- Home screen with About, FAQ and Settings icons, quick layouts and a how-it-works card.

### Notes
- No permissions are requested on Android 10 and later; Android 9 and older declare only the legacy
  storage write permission needed to save into the gallery.
- No network code, no analytics, no account.
