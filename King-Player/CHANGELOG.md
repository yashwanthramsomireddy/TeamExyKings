# Changelog

All notable changes to King Player are recorded here.
This project follows [Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-11-20

**versionCode 1 · versionName 1.0.0 · first release**

### Music
- Library sub-tabs — **Favourites, Playlists, Tracks, Albums** — in a Samsung-Music-style scrollable row, where the selected tab grows and whitens instead of carrying an underline
- Local audio library scanned through MediaStore (MP3, FLAC, AAC/M4A, WAV, OGG/Opus and more)
- Albums grid grouped out of the scanned library, with Release / Name / Artist / Track count sorting, and an album page with its own art header
- Favourites: heart any track from the list or from Now Playing; stored on the device, independent of playlists
- Now Playing with album art, title, artist, seek bar and prev / next / play-pause / shuffle / repeat
- Three-band equalizer (bass / mid / treble) with Normal, Rock, Pop, Jazz, Classical and Bass Boost presets
- Playlists: create, rename, add and remove tracks, stored on-device
- Search and sort across the library; shuffle all

### Visualizer
- **Edge visualizer** — a comet that runs around the screen border, clockwise, anti-clockwise or both ways at once, following the corner radius rather than faking four straight bars
- Four-corner rainbow arcs as an alternative, or both styles together
- Colour presets: Spectrum, Aurora, Sunset, Ice, Ember, Neon, Mono, and From artwork
- Three custom colour slots with a hue / saturation / brightness picker and quick swatches
- Adjustable thickness, speed, corner radius, trail length and glow, with a live preview in settings
- Optional "show on every screen" mode that keeps the edge light running across the whole app while a track plays
- Optional audio-reactive mode, off by default

### Classic skin player
- **Classic player** in the spirit of the old skinnable desktop players: main transport panel, equalizer panel and playlist panel, each with a two-tone bevel and an inset LCD
- Monospaced readout with elapsed time, a marquee track line, approximate bitrate from the file itself, format and duration
- Spectrum inside the LCD, fed by the real band energies when the audio-reactive mode is on
- Equalizer faders driving the device's own bands, one per band, with the classic +12 / 0 / -12 scale and a PRESETS menu
- Four skins — King Black, Classic Blue, Green Machine, Graphite — selectable in Settings or from the player's SKIN button
- Settings → Player chooses Modern or Classic as the Now Playing layout

### Home-screen widget
- Widget with album art, title, artist, progress, elapsed/total time and prev / play-pause / next
- Follows the chosen skin; one generated layout per skin, since RemoteViews cannot restyle a progress bar at runtime
- Updates on every playback change and ticks while playing; costs nothing when no widget is placed

### Appearance
- Backgrounds tinted from the current album art — Now Playing, album pages and the mini player — at Subtle, Medium, Vivid or Off, always blended back toward black so the app stays AMOLED-dark
- Pitch-black theme throughout, matching King Calc

### Video
- Local video library scanned through MediaStore (MP4, MKV, AVI, MOV, WebM, 3GP and more)
- Full-screen player with gesture controls: brightness on the left half, volume on the right half, sideways to seek, double-tap to skip ten seconds
- Subtitle support for `.srt`, `.ass`, `.ssa` and `.vtt`, loaded automatically from a sidecar file or picked manually
- Aspect ratio toggle: Fit, Fill and Zoom
- Picture-in-Picture, entered automatically on leaving the app or from the player button
- Resume a video where you left it

### Playback
- Single shared ExoPlayer for both tabs, so video audio survives a screen lock
- Foreground service with MediaSession — background and lock-screen playback with notification controls
- Audio focus handling and pause on headphone disconnect
- Sleep timer

### App
- Navigation drawer plus FAQ, About, sleep timer and Settings in the top bar
- About, FAQ, Changelog, Credits, Settings, Visualizer and Player screens
- Changelog lists every release newest first, with the latest flagged
- Credits open as a bordered panel rather than a stock dialog
- "More from TeamExyKings" opens the Play developer page
- Exit confirmation on back, so a stray back press no longer drops out of the app mid-browse
- Update check through Google Play: an update card at the bottom of About and a one-time popup on Home per released version
- Free forever, no ads, no subscription, no login, no tracking
