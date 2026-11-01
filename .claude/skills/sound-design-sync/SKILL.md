---
name: sound-design-sync
description: Plans and syncs audio to animation — music selection and beat mapping, SFX spotting, voice-over timing, mixing levels, and frame-accurate audio/visual sync (including in Remotion). Use whenever a video has music, sound effects, or narration.
---

# Sound Design & Sync

## When to use
- Adding music, SFX, or VO to an animation
- Cutting visuals to the beat
- Audio feels disconnected, too loud, or out of sync

## Workflow
1. **Audio intent** — mood, energy curve (matches the color script/beat sheet), whether the video must work muted (social: yes — captions mandatory).
2. **Music** — choose BPM to fit pacing (calm 70–90, upbeat 100–128, hype 128–150). Use only licensed / royalty-free tracks; record the license in `AUDIO.md`.
3. **Beat map** — convert beats to frames:
   `framesPerBeat = fps * 60 / BPM` (e.g. 30fps @ 120BPM = 15 frames/beat).
   Mark downbeats and drops; align cuts and key motion hits to them. Detect beats with `aubio`/`librosa` or tap tempo, then store as a frame list.
4. **SFX spotting** — list every on-screen event that needs sound: whooshes (transitions), pops/clicks (UI, appear), impacts (landings), sparkles (reveals), foley (footsteps, hair swish), ambience bed.
5. **VO** — write to time: ~150 words/min (2.5 words/s). Record/generate, then cut visuals to VO, not the reverse. Produce word timestamps for captions and lip sync.
6. **Mix** — then master.

## Sync rules
- Visual impact lands **on** the sound frame; whooshes **start** 3–6 frames before the transition and peak at the cut.
- Lip sync: mouth shapes 1–2 frames before the phoneme.
- J-cut: next scene's audio starts 6–15 frames before its picture; L-cut: audio trails.
- Don't place an SFX on every event — prioritize the 3–5 that matter per shot.

## Mix targets
| Element | Level (approx.) |
|---|---|
| VO / dialogue | −12 to −6 dBFS peaks, the clearest element |
| Music under VO | duck 8–12 dB below VO (sidechain or volume curve) |
| Music alone | −14 to −10 dBFS |
| SFX | sit just under VO; transient peaks ≤ −3 dBFS |
| Master | **−14 LUFS integrated** (YouTube/IG/TikTok), true peak ≤ −1 dBTP |

Fade music in/out (≥ 0.5s) — never hard-cut a track mid-phrase; end on a musical phrase boundary.

## Remotion implementation
```tsx
import { Audio, Sequence, interpolate, staticFile } from 'remotion';

const BPM = 120, FPS = 30, fpb = (FPS * 60) / BPM; // 15
const beatFrame = (n: number) => Math.round(n * fpb);

<Audio src={staticFile('music.mp3')}
  volume={(f) => interpolate(f, [0, 15, vo.start, vo.start + 10, vo.end, vo.end + 10, total - 30, total],
    [0, 0.8, 0.8, 0.25, 0.25, 0.8, 0.8, 0], { extrapolateLeft: 'clamp', extrapolateRight: 'clamp' })} />
<Sequence from={beatFrame(8) - 4}><Audio src={staticFile('sfx/whoosh.wav')} volume={0.6} /></Sequence>
<Sequence from={vo.start}><Audio src={staticFile('vo.wav')} /></Sequence>
```
Audio-reactive visuals: `useAudioData` + `visualizeAudio` (`@remotion/media-utils`).

## Measuring loudness
```bash
ffmpeg -i out.mp4 -af loudnorm=I=-14:TP=-1:LRA=11:print_format=summary -f null -
# Normalize (two-pass recommended):
ffmpeg -i in.mp4 -af loudnorm=I=-14:TP=-1:LRA=11 -c:v copy out.mp4
```

## Deliverable: `AUDIO.md`
Track list with source + license, BPM, beat map (frames), SFX cue sheet (frame → sound → volume), VO script with timestamps, mix notes.

## Checks
- [ ] All audio licensed and credited
- [ ] Key visual hits land on beats/SFX within ±1 frame
- [ ] VO always intelligible over music
- [ ] Integrated loudness −14 LUFS ±1, true peak ≤ −1 dBTP
- [ ] Works muted (captions/on-screen text carry the message)
