# Guide Cue Creator

Nagbabasa ng Ableton Live session (`.als`), nagtatugma ng mga locator sa mga audio cue file, at nag-rerender ng isang stereo WAV (`guide_cues.wav`) na handa nang ilagay sa dedicated na guide cue track sa beat 0.

---

## Kasama sa Package

```
cue_creator.html    ← ang app (buksan ito)
GUIDE CUES/         ← audio cue library (kasama na)
```

---

## Mga Kinakailangan

- **Chrome o Edge** (inirerekomenda) — zero-click na access sa folder pagkatapos ng unang beses
- **Firefox** — gumagana, muling piliin ang folder nang isang beses bawat browser session kung kinakailangan
- Koneksyon sa Internet (naglo-load ng pako, Fuse.js, Dexie mula sa CDN)

---

## Mabilis na Pagsisimula

1. I-double-click ang `cue_creator.html` — magbubukas sa iyong default na browser
2. I-click ang **Piliin ang Folder** at piliin ang folder na naglalaman ng `GUIDE CUES/`
3. Pumili ng **wika** mula sa dropdown
4. I-drag ang iyong `.als` file sa upload area (o i-click para mag-browse)
5. Suriin ang cue table — berde = magandang tugma, dilaw = mababang katumpakan, pula = walang tugma
6. I-click ang **I-preview** (o pindutin ang **Spacebar**) para makinig
7. I-click ang **I-render ang WAV** — dina-download ang `guide_cues.wav`
8. I-drag ang `guide_cues.wav` sa iyong Ableton session sa beat 0 sa dedicated na track

Sa Chrome/Edge, naitatandaan ang pagpili ng folder. Ang Hakbang 2 ay isang beses lamang na setup.

---

## Ginagawa ng App

- Inilalagay ang bawat section cue **1 bar bago** ang locator nito
- Gumagawa ng **count-off** para sa unang 2 bar (beat 0–7 para sa 4/4)
- Pinipigilan ang mga section cue na nahuhulog sa loob ng count-off na rehiyon
- Nilalaktawan ang mga locator na pinangalanang `count off` o `next song`
- Sumusuporta sa tempo automation (mga session na may variable BPM)

---

## Mga Sinusuportahang Time Signature

3/4 · 4/4 · 6/8 · 12/8

---

## Mga Sinusuportahang Wika

Ingles · Espanyol · Pranses · Indonesiano · Koreano · Filipino · Portuges

---

## Mga Kontrol ng Cue Table

| Kolum | Ano ang Gagawin |
|-------|-----------------|
| Locator | Read-only — mula sa iyong .als |
| Matched Cue | I-edit para baguhin ang auto-match |
| Katumpakan | Berde ≥ 80% · Dilaw ≥ 50% · Pula < 50% |

---

## Mga Kontrol ng Timeline

| Aksyon | Resulta |
|--------|---------|
| I-click | Pumunta sa posisyon |
| Spacebar | I-play / I-pause |
| Ctrl+scroll o pinch | Mag-zoom patungo sa cursor |
| Mga button na +/− | Mag-zoom in/out |
| I-reset | Iakma ang buong session sa lapad |
| Sundan ang Playhead | I-toggle ang auto-scroll |

---

## Mga Detalye ng Output

| Setting | Halaga |
|---------|--------|
| Sample rate | 48,000 Hz |
| Mga channel | Stereo |
| Bit depth | 16-bit PCM |
| File | `guide_cues.wav` |
