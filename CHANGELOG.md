# Changelog

All notable changes to Guide Cue Creator are documented here.

---

## [v1.3] — 2026-05-27

### Added
- **UI language selector** — display language independent of audio cue language. Supports English, Korean, Tagalog, Indonesian, French, Spanish, Portuguese.
- **Multi-language documentation** — user guides (README + PDF) in all 7 supported languages, organized under `docs/<Language>/`.

### Changed
- **Count-off sourced from Song Sections** — numbered cue files (1–6, Intro) in `GUIDE CUES/<lang>/Song Sections/` are now used directly as count-off audio. No separate `COUNT OFFS/` folder required.
- **Fixed 12/8 count-off schedule** — corrected beat positions to: measure 1: 1@0, 2@3; measure 2: 1@6, 2@7.5, 3@9, 4@10.5 (quarter-note beats).
- **Dexie schema v2** — dropped `countoffFiles` table (count-offs now derived from `cueFiles`).

---

## [v1.2] — 2026-05-27

### Added
- Tempo automation support — piecewise-linear BPM curve from Ableton automation envelopes.
- Variable-BPM beat-to-time conversion via `beatToTime` / `timeToBeat`.

### Fixed
- Tempo automation parsing for Live 10/11 (`<FloatEvent>` tag, `MasterTrack` envelope location).
- Sentinel event at `Time=-63072000` now filtered correctly.

---

## [v1.1] — 2026-05-27

### Added
- Firefox persistence via Dexie (IndexedDB blobs) — no re-pick needed after first run.
- Chrome/Edge handle persistence via IndexedDB — zero clicks on return.
- Timeline zoom (Ctrl+scroll, pinch, +/− buttons), Follow Playhead toggle.
- Seek by clicking timeline; spacebar play/pause.

---

## [v1.0] — 2026-05-22

### Initial release
- Reads Ableton `.als` files, matches locators to audio cue files via fuzzy matching (Fuse.js).
- Generates count-off for first 2 bars (3/4, 4/4, 6/8, 12/8).
- Renders `guide_cues.wav` (48kHz stereo 16-bit PCM).
- Chrome/Edge: File System Access API. Firefox: `webkitdirectory`.
- Multi-language audio cue support.
