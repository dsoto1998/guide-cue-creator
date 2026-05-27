# Guide Cue Creator

Reads an Ableton Live session (`.als`), matches locators to audio cue files, and renders a single stereo WAV (`guide_cues.wav`) ready to drop onto a dedicated guide cue track at beat 0.

---

## What's Included

```
cue_creator_v1.3.html    ← the app (open this)
GUIDE CUES/              ← section cue audio library (bring your own)
docs/                    ← user guides in 7 languages
```

---

## Requirements

- **Chrome or Edge** (recommended) — zero-click folder access after first run
- **Firefox** — works, re-pick folder once per browser session if needed
- Internet connection (loads pako, Fuse.js, Dexie from CDN)

---

## Quick Start

1. Double-click `cue_creator_v1.3.html` — opens in your default browser
2. Click **Select Folder** and pick the folder containing `GUIDE CUES/`
3. Select a **language** from the dropdown
4. Drag your `.als` file onto the upload area (or click to browse)
5. Review the cue table — green = good match, yellow = low confidence, red = no match
6. Click **Preview** (or press **Spacebar**) to audition
7. Click **Render WAV** — downloads `guide_cues.wav`
8. Drag `guide_cues.wav` into your Ableton session at beat 0 on a dedicated track

On Chrome/Edge, the folder choice is remembered. Step 2 is a one-time setup.

---

## What the App Does

- Places each section cue **1 bar before** its locator
- Generates a **count-off** for the first 2 bars (beats 0–7 for 4/4)
- Suppresses section cues that fall inside the count-off region
- Skips locators named `count off` or `next song`
- Supports tempo automation (variable BPM sessions)

---

## Supported Time Signatures

3/4 · 4/4 · 6/8 · 12/8

---

## Supported Languages

Audio cues: English · Spanish · French · Indonesian · Korean · Filipino · Portuguese

UI display: English · Korean · Tagalog · Indonesian · French · Spanish · Portuguese

---

## Documentation

User guides are in [`docs/`](docs/) — available in all 7 supported languages.

---

## Cue Folder Structure

```
GUIDE CUES/
  English Cues/
    Song Sections/     ← section cues + count-off numbers (1–6, Intro)
  Spanish Cues/
    Song Sections/
  ...
```

Count-off audio (numbers 1–6 and Intro) is sourced from the same `Song Sections/` folder. No separate folder required.

---

## Output Specs

| Setting | Value |
|---------|-------|
| Sample rate | 48,000 Hz |
| Channels | Stereo |
| Bit depth | 16-bit PCM |
| File | `guide_cues.wav` |
