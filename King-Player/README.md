# King Player

A free, offline music and video player for Android, built by **Yashwanth Ram Somireddy** under the **TeamExyKings** brand.

Free forever · No ads · No subscription · No login · No tracking

- **Package:** `com.teamexykings.kingplayer`
- **Repo:** `king-player` (private)
- **Current version:** 1.0.0 (versionCode 1)

---

## What it does

### Music tab
- Scans every local audio file (MP3, FLAC, AAC/M4A, WAV, OGG/Opus …) through Android's MediaStore
- Now Playing with album art, title, artist, seek bar and prev / next / play-pause / shuffle / repeat
- **Four-corner animated visualizer** — rainbow lightning arcs at each corner that sweep clockwise and pulse while a track plays, drawn on a Compose `Canvas`
- Three-band equalizer (bass / mid / treble) with Normal, Rock, Pop, Jazz, Classical and Bass Boost presets
- Playlists: create, rename, add and remove tracks, stored on-device
- Keeps playing with the screen locked — foreground service + `MediaSession`, with lock-screen play/pause/next/prev
- Keeps playing in the background, with a notification that persists until playback is stopped

### Video tab
- Scans every local video file (MP4, MKV, AVI, MOV, WebM, 3GP …)
- Full-screen player with gesture controls:
  - left half, swipe up/down → **brightness**
  - right half, swipe up/down → **volume**
  - swipe sideways → **seek**, double-tap left/right → ±10 s
- Subtitles: `.srt`, `.ass`, `.ssa`, `.vtt` — sidecar files next to the video load automatically, or pick one manually
- Aspect ratio toggle: Fit / Fill / Zoom
- Picture-in-Picture, entered automatically when you leave the app (Android 12+) or from the player button
- Audio keeps running when the screen locks, same foreground service as music

### Classic skin player and widget
- **Classic player**: main transport, equalizer and playlist panels with bevelled chrome and an inset LCD, in the spirit of the old skinnable desktop players
- Four skins — King Black, Classic Blue, Green Machine, Graphite — covering both the classic player and the home-screen widget
- Equalizer faders drive the device's own bands, one fader per band, rather than faking a fixed ten
- **Home-screen widget** with album art, progress and transport, following the same skin

### Everywhere
- Pitch-black AMOLED theme, same design language as King Calc
- Exit confirmation on back, and an update check through Google Play
- Sleep timer
- About / FAQ / Changelog / Settings in the standard King pill-button layout

---

## Tech

| Area | Choice |
| --- | --- |
| Language | Kotlin 2.0.21 |
| UI | Jetpack Compose + Material 3 |
| Playback | AndroidX Media3 1.5.1 (ExoPlayer), `MediaSessionService` |
| Library scan | Android MediaStore |
| Audio effects | `android.media.audiofx.Equalizer`, `BassBoost`, `Visualizer` |
| Storage | DataStore Preferences + kotlinx.serialization |
| Navigation | Navigation Compose |
| Images | Coil |
| Architecture | MVVM, manual `ViewModelProvider.Factory` (no DI framework) |
| Updates | Google Play In-App Updates (detection only; no INTERNET permission) |
| Widget | AppWidgetProvider + RemoteViews, one generated layout per skin |
| Min / Target SDK | 29 / 36 |
| AGP / Gradle | 8.9.1 / 8.11.1 |

Music and video deliberately share **one** `ExoPlayer` instance inside `PlaybackService`. That is what lets a video's audio survive a screen lock, lets PiP hand the same player a new surface without re-buffering, and keeps one consistent notification for both tabs.

---

## Building

1. Open the project root in Android Studio (Ladybug or newer) and let it sync.
2. `./gradlew assembleDebug` or Run ▶.

### Gradle wrapper jar

This zip ships `gradle/wrapper/gradle-wrapper.properties` but **not** the binary `gradle-wrapper.jar`. Either:

- copy `gradle/wrapper/gradle-wrapper.jar` from any of your other King projects, or
- let Android Studio regenerate it on first sync, or
- run `gradle wrapper --gradle-version 8.11.1` once with a local Gradle install.

### Release signing

Signing values are read from your **user-level** `~/.gradle/gradle.properties`, never from this repo:

```properties
KINGPLAYER_UPLOAD_STORE_FILE=/path/to/kingplayer-upload.jks
KINGPLAYER_UPLOAD_STORE_PASSWORD=...
KINGPLAYER_UPLOAD_KEY_ALIAS=kingplayer
KINGPLAYER_UPLOAD_KEY_PASSWORD=...
```

If those properties are absent the release build still compiles — it just comes out unsigned. Build the Play bundle with:

```
./gradlew bundleRelease
```

---

## Optional: FFmpeg decoder extension

**Read this before expecting AC3 / DTS / E-AC3 to play.**

`androidx.media3:media3-decoder-ffmpeg` is **not published to Maven Central**. Google ships the FFmpeg extension as source inside the Media3 repository, and it has to be compiled locally with the NDK to produce an `.aar`. There is no one-line dependency that bundles it.

King Player is wired for this either way:

- `PlaybackService` uses `DefaultRenderersFactory` with `EXTENSION_RENDERER_MODE_PREFER`, which looks the FFmpeg renderer up reflectively and **silently falls back** to the platform + ExoPlayer decoders when it is absent.
- `app/build.gradle.kts` scans `<project root>/libs/` for an `.aar` whose name contains `ffmpeg`. If it finds one it is added to the build and `BuildConfig.HAS_FFMPEG_EXTENSION` becomes `true`, which also flips the Credits, FAQ and Settings text.

To build the AAR:

```bash
# 1. Tools
#    - Android NDK r27 or similar, plus the usual cmake/make toolchain
git clone https://github.com/androidx/media.git
cd media/libraries/decoder_ffmpeg/src/main/jni

# 2. FFmpeg source (6.0 is the version Media3 currently targets)
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg -b release/6.0

# 3. Build the native libs for the codecs you want
FFMPEG_MODULE_PATH="$(pwd)/.." \
NDK_PATH="$ANDROID_NDK_HOME" \
HOST_PLATFORM="linux-x86_64" \
ANDROID_ABI=21 \
ENABLED_DECODERS="vorbis opus flac alac pcm_mulaw pcm_alaw mp3 amrnb amrwb aac ac3 eac3 dca mlp truehd" \
./build_ffmpeg.sh

# 4. Assemble the AAR from the media repo root
cd ../../../../..
./gradlew :lib-decoder-ffmpeg:assembleRelease
```

Then drop the resulting `lib-decoder-ffmpeg-release.aar` into `king-player/libs/` and rebuild. Nothing else needs changing.

**In practice you may not need it.** MP4/MKV/AVI/MOV containers and H.264, H.265/HEVC, VP9 and AV1 video already play on stock ExoPlayer using the device's hardware decoders. The FFmpeg extension mainly adds *audio* codecs — AC3, E-AC3, DTS, TrueHD, ALAC — that some phones do not decode natively. `libs/` is gitignored, so the AAR stays out of the repo.

---

## The microphone permission

`android.media.audiofx.Visualizer` requires `RECORD_AUDIO` even when it is attached to the app's own audio session. That is an Android platform rule, not a choice.

So King Player:

- declares `RECORD_AUDIO` in the manifest with a comment explaining why,
- keeps the audio-reactive visualizer **off by default**,
- runs a time-based animation for the corner arcs unless the user explicitly turns the reactive mode on in Settings,
- requests the permission only at that moment,
- never records, buffers or transmits anything — the capture callback produces four floats per frame and nothing else.

If you would rather not ship a microphone permission at all for the Play listing, delete the `RECORD_AUDIO` line from `AndroidManifest.xml` and the "Audio-reactive visualizer" toggle in `SettingsScreen.kt`. The visualizer keeps working on its time-based curve and nothing else breaks.

---

## The update check

About ends with an "An update is available" card, and Home shows a one-time popup, when
Google Play has a newer build. Both buttons — Update and Later — record that version code
as seen, so the prompt appears **once per released version**; a later release brings it
back once more.

It uses Google's Play In-App Updates library (`com.google.android.play:app-update-ktx`)
purely for *detection*. The query goes over on-device IPC to the Play Store app, so
King Player still declares no `INTERNET` permission and still makes no network calls of
its own. Downloading is deliberately left to the Play Store — the button just deep-links
to the listing.

Two things worth knowing before you test it:

- **It only works when the app was installed from Google Play**, closed testing included.
  A debug or sideloaded build gets nothing back, `UpdateState` stays `Unknown`, and
  neither the card nor the popup appears. That is by design, not a failure.
- Adding the library means a **Gradle sync** on your side. Worth confirming in the merged
  manifest (Build → Analyze APK, or the Merged Manifest tab) that no `INTERNET` permission
  arrived with it; the artifact does not declare one, but it is a one-minute check before
  a Play upload.

To drop the feature entirely: remove the dependency from `app/build.gradle.kts`, delete
`data/UpdateRepository.kt`, and remove the update card in `AboutScreen.kt` and the popup
in `HomeScreen.kt`. Nothing else depends on it.

---

## Skins and the widget

`PlayerSkin` in `data/model/SkinModels.kt` is the single source of truth — a flat palette
per skin (chrome, bevel highlight and shadow, LCD fill, readout colours, accent, button
colours). The classic player draws its panels from those values, so skins need no bitmap
assets and stay resolution-independent.

The home-screen widget cannot share that code: RemoteViews can host neither Compose nor a
runtime-swapped ProgressBar drawable. Each skin therefore gets its own generated layout
and drawable set:

```bash
python docs/widget-skins/generate.py    # run from the project root
```

That writes `widget_<skin>.xml` plus `widget_panel_/art_/progress_<skin>.xml` into
`app/src/main/res/`. **If you change a skin's colours in `SkinModels.kt`, mirror them in
the `SKINS` dict at the top of that script and re-run it**, or the widget and the player
will drift apart.

Widget transport buttons talk to the running service through `PlayerBus.transport`. When
the process has been fully stopped there is nothing to drive, so a button press opens the
app instead of silently doing nothing.

---

## Project layout

```
king-player/
├── app/
│   ├── build.gradle.kts
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── res/
│       └── java/com/teamexykings/kingplayer/
│           ├── KingPlayerApp.kt          manual DI container
│           ├── MainActivity.kt           splash, PiP, keep-screen-on
│           ├── core/Format.kt
│           ├── data/
│           │   ├── MediaStoreRepository.kt
│           │   ├── PlaylistRepository.kt
│           │   ├── SettingsRepository.kt
│           │   └── model/MediaModels.kt
│           ├── playback/
│           │   ├── PlaybackService.kt    MediaSessionService + ExoPlayer
│           │   ├── PlayerConnection.kt   MediaController -> Compose state
│           │   ├── PlayerBus.kt          audio session id bridge
│           │   ├── MediaItemFactory.kt
│           │   ├── EqualizerController.kt
│           │   └── VisualizerController.kt
│           └── ui/
│               ├── KingPlayerRoot.kt     nav graph + permission gate
│               ├── KingPlayerViewModel.kt
│               ├── theme/Theme.kt
│               ├── components/           CornerVisualizer, shared widgets
│               ├── home/HomeScreen.kt    drawer, tabs, top-bar icons
│               ├── music/                library, Now Playing, EQ, playlists
│               ├── video/                library, full-screen player
│               ├── skin/                 classic player + skin widgets
│               └── about/                About, FAQ, Changelog, Settings, Visualizer
├── gradle/libs.versions.toml
├── docs/play-assets/                    Play icon + feature graphic, and generate.py
├── docs/widget-skins/generate.py        regenerates the per-skin widget layouts
├── index.html · privacy-policy.html · terms-of-service.html
├── CHANGELOG.md · RELEASE_NOTES_1.0.0.md · STORE_LISTING.md · LICENSE
```

The app mark lives in `app/src/main/res/drawable/ic_launcher_foreground.xml`. It is laid
out in the 108×108 adaptive-icon viewport and every point stays inside the 33-unit safe
circle around (54, 54) so it survives round and squircle launcher masks — keep new
elements inside that circle if you edit it, and mirror any change into
`docs/play-assets/generate.py`.

Web pages are served from `teamexykings.in/King-Player/`.

---

## Privacy

King Player has no accounts, no analytics, no crash reporting and no advertising SDKs. Everything it reads — your audio and video files, playlists and settings — stays on the device.

It makes no network calls of its own and declares no `INTERNET` permission. The single outward-facing question is the update check, which asks the Play Store app already on the device whether a newer version exists; the Play Store makes any connection, nothing about the user is sent, and only the offered version code is stored so the same update is not announced twice. `privacy-policy.html` says the same thing in the words users will read.
