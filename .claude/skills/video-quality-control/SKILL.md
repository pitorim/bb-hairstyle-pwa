---
name: video-quality-control
description: Final QC for rendered videos and animations — technical specs (codec, resolution, fps, bitrate, loudness), visual/motion review via extracted frames, sync checks, accessibility, and platform compliance. Use before delivering or publishing any video.
---

# Video Quality Control

## When to use
- After any render, before handing the video to the user or publishing
- Diagnosing glitches: flicker, black frames, stutter, desync, banding, wrong aspect ratio

## 1. Technical inspection
```bash
ffprobe -v error -show_entries stream=codec_name,width,height,r_frame_rate,pix_fmt,bit_rate,channels,sample_rate:format=duration,size -of json out.mp4
```
Expected (social / web default):
| Property | Target |
|---|---|
| Container / codec | MP4, H.264 (High) + AAC 48kHz stereo |
| Pixel format | `yuv420p` |
| Frame rate | constant, matches composition (24/25/30/60) |
| Resolution | exact target (1080×1920, 1080×1080, 1920×1080) |
| Duration | matches storyboard ± 1 frame |
| Bitrate | 8–16 Mbps for 1080p (YouTube recommends ~8–12 @30fps) |
| `moov` atom | at start (`-movflags +faststart`) for web playback |

## 2. Automated defect scans
```bash
# Black frames
ffmpeg -i out.mp4 -vf blackdetect=d=0.05:pix_th=0.10 -an -f null - 2>&1 | grep black_start
# Frozen/duplicate frames (unintended holds)
ffmpeg -i out.mp4 -vf freezedetect=n=0.003:d=0.5 -an -f null - 2>&1 | grep freeze
# Silence gaps
ffmpeg -i out.mp4 -af silencedetect=n=-50dB:d=1 -vn -f null - 2>&1 | grep silence_
# Loudness
ffmpeg -i out.mp4 -af loudnorm=I=-14:TP=-1:print_format=summary -f null - 2>&1 | tail -12
```

## 3. Visual review
Extract a contact sheet and key frames, then actually look at them:
```bash
ffmpeg -i out.mp4 -vf "fps=1,scale=360:-1,tile=6x5" -frames:v 1 contact.png
ffmpeg -i out.mp4 -vf "select='eq(n\,0)+eq(n\,45)+gt(scene\,0.3)'" -vsync vfr frames/f_%03d.png
```
Check every scene boundary (first/last frame of each sequence) and every transition midpoint.

Visual checklist:
- [ ] No placeholder text, missing fonts (fallback serif), broken images, or debug outlines
- [ ] Palette matches `STYLE_GUIDE.md`; characters on-model
- [ ] Text inside title-safe area and clear of platform UI (bottom ~20% and right ~15% on Reels/TikTok)
- [ ] Text legible on a phone at arm's length; contrast ≥ 4.5:1
- [ ] No gradient banding (add 1–2% grain/dither if present)
- [ ] No pops: elements don't appear/disappear for a single frame unintentionally
- [ ] No aliasing/jaggies on thin lines; no clipped edges at frame borders

## 4. Motion review
- [ ] Watch at 100% and at 0.25× speed
- [ ] Easing consistent; no accidental linear moves
- [ ] No stutter (check constant fps; heavy frames may have rendered wrong)
- [ ] Transitions continuous in direction
- [ ] Flashing ≤ 3 per second (WCAG 2.3.1 / photosensitive epilepsy)

## 5. Audio & sync
- [ ] Integrated loudness −14 LUFS ±1, true peak ≤ −1 dBTP
- [ ] VO intelligible; music ducked under VO
- [ ] Hits and lip sync within ±1 frame
- [ ] No clicks at cut points; fades at start/end

## 6. Accessibility & delivery
- [ ] Burned-in or sidecar captions (`.srt`/`.vtt`) accurate and timed
- [ ] Works muted
- [ ] Thumbnail/cover frame exported
- [ ] Filename: `<project>_<aspect>_<duration>s_v<NN>.mp4`

## Common fixes
| Symptom | Likely cause → fix |
|---|---|
| Video won't play on iOS/Safari | `yuv444p` or High 4:4:4 → re-encode `-pix_fmt yuv420p` |
| Audio drift over time | VFR source → render/convert with constant fps (`-r 30`) |
| Flicker in Remotion | Non-deterministic code or un-awaited assets → use `random(seed)`, `<Img>`, `delayRender` |
| Banding in gradients | Low bitrate / 8-bit → raise CRF quality (lower number), add grain |
| Fonts wrong in render | Font not loaded before frame → `@remotion/google-fonts` `loadFont()` |

Re-encode to spec:
```bash
ffmpeg -i in.mp4 -c:v libx264 -profile:v high -pix_fmt yuv420p -crf 18 -preset slow -r 30 \
  -c:a aac -b:a 192k -ar 48000 -movflags +faststart out_final.mp4
```

## Report format
Summarize as: **PASS / PASS WITH NOTES / FAIL**, then a table of issues with timestamp (mm:ss:ff), severity (blocker/major/minor), description, and fix owner skill (`2d-art-director`, `cinematic-motion-design`, `sound-design-sync`, `advanced-remotion`, …).
