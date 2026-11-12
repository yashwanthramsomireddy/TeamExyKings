# King Audio Trimmer

Free, offline audio trimmer, cutter and merger for Android by TeamExyKings (Yashwanth Ram Somireddy).

- Package: `com.teamexykings.kingaudiotrimmer`
- Version: 1.1.0 (versionCode 3)
- Min SDK 29 · Target SDK 36 · AGP 8.9.1 · Gradle 8.11.1 · Kotlin 2.0.21

## Features
- Waveform editor: drag handles, pinch/button zoom, tap cursor with "Start here / End here", exact time entry, ±0.1 s nudges, loop, edge previews
- Keep selection **or** remove selection (joined with a click guard), quick 15/30/60 s select, trim silence
- Undo / redo for every edit
- Effects: fade in/out, volume ±24 dB, normalize, speed 0.5–2×, reverse
- Merge tracks: reorder, per-track trim, gap or crossfade, mixed formats
- Ringtone maker, notification and alarm destinations (opens Sound settings; no special permission)
- Video → audio (pick a video, edit its sound)
- Output: Original (lossless cut for MP3, M4A/AAC, Opus), M4A, AAC (.aac), Opus (OGG), FLAC, WAV; bitrate, sample rate, mono/stereo; title/artist/album tags; share after saving
- Pitch Black / White / System themes, 5 fonts, 8 accents (Black hidden on dark themes, White hidden on the light theme)

## Stack
Kotlin, Jetpack Compose + Material 3, MVVM with manual ViewModel factories (no Hilt), DataStore, Navigation Compose, SplashScreen API. Audio uses only Android's MediaExtractor / MediaCodec / MediaMuxer / MediaStore — **no third-party audio library is bundled**.

## Build
1. Open the folder in Android Studio and let Gradle sync.
2. Run the `debug` configuration on your phone (installs as `...kingaudiotrimmer.debug`).
3. For a signed release bundle, add these to your **user-level** `gradle.properties` (`%USERPROFILE%\.gradle\gradle.properties`):

```
KINGAUDIOTRIMMER_UPLOAD_STORE_FILE=C:\\path\\to\\keystore.jks
KINGAUDIOTRIMMER_UPLOAD_STORE_PASSWORD=...
KINGAUDIOTRIMMER_UPLOAD_KEY_ALIAS=...
KINGAUDIOTRIMMER_UPLOAD_KEY_PASSWORD=...
```
Then Build › Generate Signed App Bundle, or `gradlew bundleRelease`.

## Project layout
```
app/src/main/java/com/teamexykings/kingaudiotrimmer/
  MainActivity.kt            splash, theme host, "Open with"/Share intents
  navigation/                routes + NavHost
  ui/theme/                  Pitch Black / White / System, fonts, 8 accents
  ui/components/             PillButton, SectionCard, AppTopBar, Controls (chips, sliders, time dialog)
  ui/export/                 ExportRunner (progress/cancel) + shared result dialogs
  ui/screens/                home, editor (+WaveformView), merge, settings, about, faq
  data/settings|recent|backup  DataStore repos, Backup/Restore (JSON)
  audio/                     PcmDecoder, WaveformExtractor (progressive + cached),
                             AudioTrimEngine (pipeline), LosslessCutter, FormatPlanner,
                             CodecEncoderSink (AAC/ADTS/Opus/FLAC), WavWriter, PcmTools
                             (scratch file, channel mixer, resampler), MediaStoreSaver
index.html, privacy-policy.html,
  terms-of-service.html        website pages (project root) → teamexykings.in/King-Audio-Trimmer/
docs/store-assets/            icon-512.png, feature-graphic-1024x500.png (Google Play)
v1.1.0.md                    release notes for the current version (latest only)
```

## How export works
`FormatPlanner` decides, and the editor shows the same sentence above the Save button.

| Situation | Path |
|---|---|
| Original format, nothing changed, source is MP3 / AAC / Opus | Lossless copy of compressed frames |
| Anything else | Decode → scratch PCM file on disk → join / reverse → resample + speed → gain + fades → encoder |

Android has no MP3 encoder, so MP3 is only ever produced by a lossless cut of an MP3. FLAC and Opus encoders depend on the phone; unavailable formats are hidden (`EncoderSupport`).

## Waveform speed
One decoding pass, bulk PCM copy, 50 peaks per second, progressive updates, result cached in the app cache (`waveforms/`). The editor opens immediately.

## Backup JSON
Settings (theme, font, accent, default fade) and the recent-files list only. No audio.

## Rules
Free forever · no ads · no subscription · no login · no analytics · no tracking · offline · private repo.
