# Guide Cue Editor

## Purpose

A browser-based tool that reads an Ableton Live session (.als), identifies all locators (song section markers), places audio cue announcements exactly **1 bar before** each locator, and generates a **count-off** for the first 2 bars. Supports **multiple languages**. The final output is a single stereo WAV file (`guide_cues.wav`) that can be dragged into the session at beat 0 on a dedicated guide cue track.

---

## How It Works (High-Level Flow)

1. **Open app** — Double-click `cue_creator_v1.1.html` in Chrome or Firefox. No server needed.
2. **Select folder** — User picks the root folder containing `GUIDE CUES/`. Browser reads audio files directly via File System Access API (Chrome/Edge) or `webkitdirectory` input (Firefox).
3. **Select language** — User picks from auto-populated dropdown.
4. **Upload .als file** — Auto-triggers on file select. Decompresses gzip XML, parses locators, time signatures, BPM. Derives count-off files (1–6, Intro) from the loaded cue library.
5. **Match cues** — Each locator name is fuzzy-matched to the best audio cue file from the active language's library.
6. **Calculate placement** — Each cue is scheduled 1 bar before its locator. Count-off beats are placed at beat 0 per hardcoded schedules.
7. **Preview** — Play button (or spacebar) previews all cues + count-off in the browser with a visual timeline.
8. **Render WAV** — All cues + count-off rendered into a single stereo WAV via Web Audio API.

---

## File Structure

```
cue_creator_v1.1.html  -- App (HTML + CSS + JS). Open directly in Chrome or Firefox.
GUIDE CUES/            -- Section cue audio library (per-language). Also source of count-off files.
CLAUDE.md              -- This project reference file.
```

---

## Running the App

Double-click `cue_creator_v1.1.html` in Chrome or Firefox. No server, no Terminal.

- **Chrome/Edge**: Uses File System Access API. Folder selection persists across sessions (IndexedDB handle). Zero clicks after first run.
- **Firefox**: Uses `<input webkitdirectory>`. First run picks the folder and saves all audio to Dexie (IndexedDB). Subsequent runs auto-load from Dexie with no user gesture.

---

## Audio Cue Folder Structure

```
GUIDE CUES/
  English Cues/
    Dynamic Cues/              (ignored)
    Song Sections/             -> "English Female - Bridge 1.wav", "English Female - 1.wav", etc.
  Spanish Cues/ ...
  French Cues/ ...
  Indonesian Cues/ ...
  Korean Cues/ ...
  Philippines Cues/ ...
  Portuguese Cues/ ...
```

Section cues: `<Language> Cues/Song Sections/` — named `<Prefix> - <Section>.wav`.

Count-off files are sourced from the same `Song Sections/` folder — files whose normalized name is `"1"`–`"6"` or `"intro"` (e.g. `"English Female - 1.wav"` normalizes to `"1"`). No separate COUNT OFFS folder required.

---

## Browser Compatibility

| Browser | Folder access | Persistence |
|---------|--------------|-------------|
| Chrome / Edge | File System Access API (`showDirectoryPicker`) | ✓ IndexedDB handle — zero clicks after first run |
| Firefox | `<input webkitdirectory>` | ✓ Dexie (IndexedDB blobs) — auto-loads on return |
| Safari | `<input webkitdirectory>` | ✗ Re-pick each session |

---

## External Libraries (loaded via CDN)

| Library | Version | Purpose |
|---------|---------|---------|
| [pako](https://cdn.jsdelivr.net/npm/pako@2.1.0/dist/pako.min.js) | 2.1.0 | Decompresses .als files |
| [Fuse.js](https://cdn.jsdelivr.net/npm/fuse.js@7.0.0/dist/fuse.min.js) | 7.0.0 | Fuzzy string matching |
| [Dexie](https://cdn.jsdelivr.net/npm/dexie@3.2.4/dist/dexie.min.js) | 3.2.4 | IndexedDB wrapper for Firefox persistence |

---

## Key Globals

| Variable | Type | Description |
|----------|------|-------------|
| `data` | Object | `time_signatures[]` and `locators[]` for current session |
| `cueLibrary` | Array | Sorted list of loaded section cue file names |
| `cueFileMap` | Object | filename → `File` object (active language) |
| `fuse` | Fuse instance | Fuzzy search index |
| `sessionBPM` | Number | BPM from .als `<Manual>` value (default 120) |
| `sessionTempoEvents` | Array | `[{ beat, bpm }, ...]` piecewise tempo map from automation. Single entry = constant BPM. |
| `currentLanguage` | String | Active language (default "English") |
| `countoffFileMap` | Object | `{ "1": File, "2": File, "Intro": File, ... }` — populated from `cueFileMap` by normalized name match |
| `sessionTimeSig` | String | Time sig at beat 0 (e.g. `"4/4"`) |
| `previewSchedule` | Array | Cached `{ offset, buffer }` items — reused across seeks, cleared on new .als |
| `previewSeekOffset` | Number | Current seek position in seconds (persists through pause) |
| `isPlaying` | Boolean | Preview playback state |
| `followPlayhead` | Boolean | Whether timeline auto-scrolls during playback |
| `timelineZoom` | Number | Current zoom multiplier (default fit-to-width on load) |
| `BASE_PX_PER_BEAT` | Number | `22` — baseline pixels per quarter-note beat at zoom 1 |
| `window._manifest` | Object | `{ languages, cues }` — built from FS scan or Dexie |
| `window._cueHandles` | Object | Chrome/Edge: `lang → filename → FileSystemFileHandle` |
| `window._cueFiles` | Object | Firefox/Dexie: `lang → filename → File` |
| `window._rootHandle` | FileSystemDirectoryHandle | Chrome/Edge: current root directory handle |
| `window._savedHandle` | FileSystemDirectoryHandle | Chrome/Edge: restored handle from IndexedDB |

---

## Core Functions

### Folder / Init

| Function | Description |
|----------|-------------|
| `selectFolder()` | Chrome/Edge: calls `showDirectoryPicker`. Firefox: triggers hidden `<input webkitdirectory>`. |
| `requestFolderAccess()` | Chrome/Edge: re-requests permission for saved handle (requires user gesture). |
| `handleFolderPick(fileList)` | Firefox: parses `webkitRelativePath` from fileList, builds manifest, saves to Dexie, inits UI. |
| `initFromRoot(handle)` | Chrome/Edge: scans FS, builds manifest, calls `populateFromManifest`. |
| `populateFromManifest(manifest, folderName)` | Shared: populates language dropdown, calls `loadCueLibrary`. |
| `buildManifestFromFS(rootHandle)` | Chrome/Edge: walks GUIDE CUES directory tree, populates `_cueHandles`. |
| `buildManifestFromFileList(fileList)` | Firefox: parses `webkitRelativePath`, populates `_cueFiles`. |
| `saveFilesToDexie(fileList, manifest)` | Firefox: stores cue audio Blobs in Dexie for persistence. |
| `loadFromDexie()` | Firefox: restores `_cueFiles` from Dexie, returns manifest. |
| `refreshCache()` | Firefox: clears Dexie and re-triggers folder picker. |
| `getCueFile(lang, filename)` | Unified: returns `File` from either FS handle or Dexie map. |

### Loading & Matching

| Function | Description |
|----------|-------------|
| `loadCueLibrary()` | Calls `getCueFile` for each file in manifest. Builds Fuse.js index. Shows count only. |
| `loadCountoffFiles()` | Scans `cueFileMap` for files whose normalized name is `"1"`–`"6"` or `"intro"`. Populates `countoffFileMap`. Sync (no await). |
| `normalize(str)` | Strips language prefix, extension, punctuation. Keeps numbers. |
| `matchCue(locatorName)` | Fuzzy-matches locator name → `{ file, confidence }`. |
| `switchLanguage(lang)` | Updates language, clears preview cache, reloads cues + count-off. |

### .als Parsing

| Function | Description |
|----------|-------------|
| `processALS()` | Auto-triggered on file upload. Parses .als, extracts BPM/locators/time sigs, loads count-off files, runs matching, renders timeline, fits zoom. |
| `parseLocators(xml)` | Extracts `<Locator>` elements. Skips empty names and skip list. |
| `parseTimeSignatures(xml)` | Extracts time sig events. Falls back to global, then 4/4. |
| `getBarLengthAt(beat, timeSigs)` | Returns bar length in quarter-note beats: `numerator × (4/denominator)`. |
| `getCountoffEndBeat()` | Returns `2 × numerator × (4/denominator)` derived from `sessionTimeSig` — the beat where count-off region ends. |
| `parseTempoEvents(xml)` | Finds `<Tempo><AutomationTarget Id>`, matches to `<AutomationEnvelope>` by `<PointeeId>`, iterates children of `<Automation><Events>` (tag = `<FloatEvent>` in Live 11). Filters sentinel at `Time=-63072000`. Returns `[{beat,bpm},...]` sorted, or `[{beat:0,bpm:sessionBPM}]` fallback. |
| `beatToTime(beat)` | Converts beats → seconds via piecewise-linear integration of `sessionTempoEvents`. Step changes (duplicate beat values) produce zero-length segments that are skipped. Fast path for single-entry array. |
| `timeToBeat(seconds)` | Inverse of `beatToTime` via 60-iteration binary search. Used for playhead display and seek. |

### Preview

| Function | Description |
|----------|-------------|
| `buildPreviewSchedule()` | Decodes all section + count-off files into `previewSchedule`. Skips section cues within count-off region. Cached — only built once per .als load. |
| `startPreviewFromTime(seekSec)` | Creates new AudioContext, schedules all sources from seekSec (with buffer offset), starts animation loop. |
| `previewAudio()` | Toggle: builds schedule if needed, starts/stops playback. Also triggered by spacebar. |
| `stopPreview()` | Saves current position to `previewSeekOffset`, stops sources, cancels animation. |
| `fitToWidth()` | Sets `timelineZoom` so full session fits in the timeline wrapper. Called on load and "Reset" zoom. |

### Rendering & Export

| Function | Description |
|----------|-------------|
| `renderWAV()` | Decodes files, skips section cues within count-off region, mixes all cues + count-off into OfflineAudioContext, encodes to WAV, triggers download. |
| `encodeWAV(audioBuffer)` | Converts AudioBuffer to 16-bit PCM WAV Blob (RIFF/WAVE). No gain/normalization applied. |

### UI

| Function | Description |
|----------|-------------|
| `render()` | Calls `drawTimeline()` + `drawTable()`. |
| `drawTimeline(playheadBeat)` | Dark-themed canvas. Dynamic width based on zoom. Ruler, alternating bar shading, diamond markers for locators (blue) and cues (red), green playhead line. |
| `drawTable()` | Editable table of locator/cue rows with confidence color coding. |
| `updateLoc(i, field, value)` | Updates locator field, re-matches if name changed. |
| `zoomTimeline(factor)` | Multiplies `timelineZoom` by factor, clamps to [0.2, 6]. |
| `toggleFollow()` | Toggles `followPlayhead`. |

---

## Timeline Interactions

| Action | Behavior |
|--------|----------|
| Click timeline | Seek to beat (if playing) or set seek position (if stopped) |
| Spacebar | Toggle play/pause (ignored when focused in input/select) |
| Ctrl+scroll / pinch | Zoom toward cursor |
| Scroll (no modifier) | Horizontal scroll; auto-disables Follow Playhead during playback |
| +/− buttons | Zoom in/out by 1.4× |
| Reset button | Fit full session to width |
| Follow Playhead button | Toggle auto-scroll during playback |

---

## Skip Logic

Locators with these names (case-insensitive) are excluded from cue generation:

```javascript
const skipNames = ["count off", "next song"];
```

Additionally, **section cues whose `cue_beat` falls within the count-off region** (`0` to `getCountoffEndBeat()`) are suppressed in both preview and render to prevent overlap with count-off beats.

---

## Count-Off System

### Supported time signatures
3/4, 4/4, 6/8, 12/8

### Beat schedules (quarter-note beat positions, derived from reference .als files)

| Time Sig | Measure 1 | Measure 2 |
|----------|-----------|-----------|
| 3/4 | 1@0, 2@1, 3@2 | 1@3, 2@4, 3@5 |
| 4/4 | 1@0, 2@2 | Intro@4, 2@5, 3@6, 4@7 |
| 6/8 | 1@0, 2@1.5 | Intro@3, 2@3.5, 3@4, 4@4.5, 5@5, 6@5.5 |
| 12/8 | 1@0, 2@3 | Intro@6, 2@7.5, 3@9, 4@10.5 |

`Intro` falls back to `1` file if no `Intro`-named file exists in the cue library.

Count-off audio comes from `GUIDE CUES/<lang>/Song Sections/` — same files as section cues. Numbers `1`–`6` and `Intro` are matched by normalized filename.

Unsupported time signatures: count-off is skipped with a log warning.

### Count-off end beat formula
```
countoffEndBeat = 2 × numerator × (4 / denominator)
```
Section cues with `cue_beat < countoffEndBeat` are suppressed.

---

## Normalize Function

```javascript
function normalize(str) {
  return str
    .replace(/^.*?\s[-–]\s/, "")  // strip language prefix
    .toLowerCase()
    .replace(/[-_]/g, " ")
    .replace(/\.[^.]+$/, "")
    .replace(/[^a-z0-9\s]/g, "")
    .replace(/\s+/g, " ")
    .trim();
}
```

---

## Fuzzy Matching Details

- Fuse.js `threshold: 0.4`, `ignoreLocation: true`
- Confidence = `Math.round((1 - score) * 100)`
- Green ≥ 80%, Yellow ≥ 50%, Red < 50%, fallback = no library loaded

---

## BPM Extraction

```xml
<Tempo><Manual Value="120"/></Tempo>
```

First valid `<Manual>` value found. Default 120 if none.

---

## Cue Placement Formula

```
bar_length = getBarLengthAt(max(0, locator_beat - 0.001), timeSigs)
cue_beat   = max(0, locator_beat - bar_length)
```

The `- 0.001` ensures a time-sig change that starts exactly at the locator beat doesn't govern the bar before it (uses the preceding time sig instead).

---

## Beat-to-Time Conversion

All beat→time and time→beat conversions go through `beatToTime(beat)` and `timeToBeat(seconds)`, which integrate `sessionTempoEvents` (piecewise-linear BPM curve). For constant-BPM sessions this is identical to `beat * 60 / sessionBPM`.

### Tempo automation in .als XML

```
<Tempo>
  <Manual Value="120"/>
  <AutomationTarget Id="8"/>     ← match this Id
</Tempo>

<AutomationEnvelope>
  <EnvelopeTarget>
    <PointeeId Value="8"/>        ← to this envelope
  </EnvelopeTarget>
  <Automation>
    <Events>
      <FloatEvent Time="-63072000" Value="120" .../>   ← sentinel — filtered (beat < 0)
      <FloatEvent Time="0" Value="120" .../>
      <FloatEvent Time="288" Value="67" .../>           ← step change: two events same beat
      <FloatEvent Time="288" Value="63.4" .../>
    </Events>
  </Automation>
</AutomationEnvelope>
```

- Live 10/11: envelope is inside `<MasterTrack>`. Live 12: inside `<MainTrack>`.
- Event tag is `<FloatEvent>` (not `<AutomationEvent>`). Parser iterates all children of `<Events>` by tag-agnostic `.children` iteration.
- Step changes = two `<FloatEvent>` at the same beat. `beatToTime` handles via zero-length segment → `segLen <= 0 → continue`.

---

## WAV Rendering Specs

| Parameter | Value |
|-----------|-------|
| Sample rate | 48,000 Hz |
| Channels | 2 (stereo) |
| Bit depth | 16-bit PCM |
| Format | RIFF/WAVE |
| Output file | `guide_cues.wav` |
| Padding | 1 second after last cue |
| Gain/normalization | None — sources connect directly to destination |

---

## .als File Format

Gzip-compressed XML. Key elements:
- `<Locator>` — `<Time Value="..."/>` (beat) and `<Name Value="..."/>`
- `<TimeSignatureEvent>` — `<Numerator>` and `<Denominator>`
- `<Tempo><Manual Value="..."/>` — base BPM; `<AutomationTarget Id="..."/>` — links to tempo envelope
- `<AutomationEnvelope><EnvelopeTarget><PointeeId>` + `<Automation><Events><FloatEvent Time="..." Value="..."/>` — tempo automation (Live 10/11 inside `MasterTrack`; Live 12 inside `MainTrack`)

---

## Dexie Schema (Firefox persistence)

```javascript
db.version(1).stores({
  meta:          "key",
  cueFiles:      "id, lang",
  countoffFiles: "id, lang, timesig"   // legacy — dropped in v2
});
db.version(2).stores({
  countoffFiles: null                  // drop table; count-offs now derived from cueFiles
});
```

Files stored as Blob. On load, reconstructed as `new File([blob], filename)`.

---

## Known Constraints

- Tempo automation: linear ramps supported. Nonlinear (curved) automation may cause minor inaccuracies.
- Internet required for CDN libs (pako, Fuse.js, Dexie)
- Single .als per session
- Dynamic Cues folder ignored
- Count-off only for 3/4, 4/4, 6/8, 12/8
- Count-off requires numbered files (1–6, Intro) present in the active language's Song Sections folder
- Safari: no persistence (webkitdirectory works but no Dexie auto-load)

---

## Future Improvement Ideas

- Offline mode: bundle CDN libs locally
- Dynamic Cues support
- Count-off support for additional time signatures
- Safari persistence via OPFS
