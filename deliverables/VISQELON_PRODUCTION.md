# VISQELON — "Why Do Humans Have Two Eyes?"

**Master:** 75.00 s · 1080×1920 (9:16) · 30 fps · H.264 High + AAC stereo · 2,250 frames
`deliverables/VISQELON_Why_Two_Eyes_1080x1920.mp4`

Vertical deliverable built from the supplied VISQELON script. The repo's
`main_script2video.py` pipeline needs LLM / image / video API keys that are not
configured in this checkout, so the film was assembled locally: generated
keyframes, synthesized narration, a synthesized ambient score, and a Python
frame renderer driving ffmpeg.

---

## Timeline

| # | Time | Section | Plates |
|---|------|---------|--------|
| 1 | 0.00–10.00 | 01 · THE QUESTION | eye macro → two-eyed face |
| 2 | 10.00–25.00 | 02 · EVOLUTION | mirrored body figure → fossil skull |
| 3 | 25.00–42.00 | 03 · BINOCULAR VISION | rain street → two-eyed face |
| 4 | 42.00–58.00 | 04 · NEUROSCIENCE | retina → brain → cortex neurons |
| 5 | 58.00–68.00 | 05 · BEYOND HUMANS | tuatara |
| 6 | 68.00–75.00 | 06 · CLOSING | two-eyed face in darkness |

## Narration placement

| Segment | Voice in | Voice out | Fit |
|---|---|---|---|
| s1 hook | 1.15 | 9.50 | 10 s section |
| s2 evolution | 11.20 | 20.73 | 15 s section |
| s3 binocular | 26.00 | 40.08 | 17 s section |
| s4 neuroscience | 41.60 | 57.66 | 16 s section |
| s5 beyond humans | 58.60 | 67.68 | 10 s section |
| s6 closing | 68.55 | 74.76 | 7 s section |

Narration is 63.3 s inside the 75 s container. The selected narrator read the
script at 72.2 s, which left no breathing room, so all six segments were tempo
lifted **1.14× with pitch preserved** before mixing. This is the one deliberate
change to the vocal performance, and it is reproducible: `atempo=1.14` is applied
in the build, not baked into the deliverable's source audio.

## Picture

**Camera.** Ken-Burns moves per shot (push / pull / lateral drift), eased so
motion settles rather than slides. Focal anchors keep faces and the optic disc
out of the crop.

**Grade.** Split-tone (cool shadows, warm highlights), filmic S-curve, 13 % mid
desaturation, animated grain, vignette, and a graduated scrim through the lower
third so captions hold against any texture.

**Type.** Space Grotesk Bold/Medium for headlines and chrome; Inter Medium for
body captions. Typography sits on a blurred per-line scrim.

**Chrome.** `VISQELON` wordmark, section index + label, running timecode, and a
hairline progress rail.

**Diagrams** (hybrid treatment):

- `symmetry` 11.0–17.2 s — warm axis through the figure; paired cyan nodes for
  eyes, ears, nostrils, lungs.
- `disparity` 34.2–41.7 s — two offset retinal views converging into one
  labelled depth field.
- `pathway` 47.7–52.9 s — retina → optic nerve → LGN → visual cortex, traced
  with a travelling pulse.
- `parietal` 58.7–67.6 s — crown marker with expanding light rings, annotated
  as light-sensing rather than image-forming.

## Audio

- **Voice:** single narrator, high-passed, statement-compressed.
- **Score:** synthesized in `src/score.py` — A-minor drone bed in detuned pairs,
  five-note bell motif, sub pulses through the neuroscience section, noise risers
  landing on section boundaries.
- **Mix:** computed numerically in `src/mix2.py`; score ducked under the voice
  with a 25 ms attack / 320 ms release envelope down to 40 %.
- **Master:** −14.7 LUFS, −3.3 dBTP, true stereo (L/R correlation ≈ 0.995).

## Rebuild

```bash
cd .working_dir/visqelon
python src/score.py audio/score.wav
python src/mix2.py
python src/render.py segs/a.mp4 0 37.5 &
python src/render.py segs/b.mp4 37.5 75 &
wait
python src/finish.py        # joins halves, muxes audio, copies to deliverables/
```

Rendering costs ~16 s of wall time per second of footage per process; running
both halves concurrently takes ~25 minutes total.

## Caveat that has now bitten once

`.working_dir/` is gitignored. The previous build of this film was lost when the
sandbox reset between turns. The MP4 is therefore **also** written to
`deliverables/` inside the repo by `src/finish.py`. Keep it there.
