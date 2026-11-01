---
name: advanced-remotion
description: Builds programmatic videos with Remotion (React) — project setup, compositions, sequences/series, transitions, springs, audio, data-driven/parametrized videos, performance, and rendering via CLI or Lambda. Use whenever a video is to be produced in code.
---

# Advanced Remotion

## When to use
- Producing MP4/WebM/GIF from code, templated/data-driven videos, captions, social clips
- Converting a storyboard (`professional-storyboarding`) and style guide (`2d-art-director`) into a render

## Setup
```bash
npx create-video@latest my-video   # choose blank/TS template
cd my-video && npm i
npx remotion studio                # preview
npx remotion render <CompId> out/video.mp4
```
Pin all `remotion` / `@remotion/*` packages to the **same exact version**.
Useful packages: `@remotion/transitions`, `@remotion/media-utils`, `@remotion/google-fonts`, `@remotion/lottie`, `@remotion/paths`, `@remotion/shapes`, `@remotion/noise`, `@remotion/captions`, `@remotion/zod-types`.

## Project structure
```
src/
  Root.tsx              # registers <Composition>s
  theme.ts              # tokens from STYLE_GUIDE
  shots.ts              # timing from STORYBOARD
  compositions/Main.tsx
  scenes/Hook.tsx …
  components/ (Character.tsx, Title.tsx, Logo.tsx)
public/ (audio, images, fonts, lottie)
```

## Core rules
- **Everything is a pure function of `useCurrentFrame()`.** No `Date.now()`, `Math.random()` (use `random(seed)`), CSS transitions/animations, or `setTimeout`-driven state.
- Use `<Img>`, `<Video>`/`<OffthreadVideo>`, `<Audio>`, `staticFile()` — they block rendering until loaded.
- Async data: `delayRender()` / `continueRender()`, or better, `calculateMetadata` on the Composition.
- Always `extrapolateLeft/Right: 'clamp'` on `interpolate` unless you want overshoot.
- Fonts: load with `@remotion/google-fonts/<Font>` `loadFont()` so renders don't fall back.

## Composition & timing
```tsx
<Composition
  id="Promo" component={Promo}
  durationInFrames={900} fps={30} width={1080} height={1920}
  schema={promoSchema} defaultProps={{ title: 'Hello' }}
  calculateMetadata={async ({ props }) => ({ durationInFrames: props.shots.reduce((a, s) => a + s.dur, 0) })}
/>
```
```tsx
import { TransitionSeries, linearTiming, springTiming } from '@remotion/transitions';
import { fade } from '@remotion/transitions/fade';
import { slide } from '@remotion/transitions/slide';

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={60}><Hook /></TransitionSeries.Sequence>
  <TransitionSeries.Transition presentation={slide({ direction: 'from-right' })} timing={springTiming({ config: { damping: 200 } })} />
  <TransitionSeries.Sequence durationInFrames={90}><Problem /></TransitionSeries.Sequence>
</TransitionSeries>
```
Note: transitions overlap scenes — total duration = sum(sequences) − sum(transitions).

## Animation helpers
```tsx
const frame = useCurrentFrame(); const { fps } = useVideoConfig();
const p = spring({ frame, fps, delay: 10, config: { damping: 12 }, durationInFrames: 30 });
const x = interpolate(p, [0, 1], [-200, 0]);
const opacity = interpolate(frame, [0, 15], [0, 1], { extrapolateRight: 'clamp', easing: Easing.bezier(0.22, 1, 0.36, 1) });
```
- SVG path draw: `@remotion/paths` `evolvePath(progress, d)`.
- Organic wobble: `@remotion/noise` `noise2D('seed', frame / 60, 0)`.
- Audio-reactive: `useAudioData()` + `visualizeAudio()` from `@remotion/media-utils`.

## Captions
Generate word-level timestamps (e.g. Whisper via `@remotion/install-whisper-cpp`), then `createTikTokStyleCaptions()` from `@remotion/captions`; highlight the active word.

## Performance
- Prefer `<OffthreadVideo>` for embedded videos.
- Memoize heavy computations with `useMemo` keyed on inputs, not frame.
- Avoid huge blur/backdrop-filter stacks; pre-render complex backgrounds to PNG.
- `--concurrency` to tune CPU usage; `--gl=angle` / `swangle` for WebGL content.

## Rendering
```bash
npx remotion render Promo out/promo.mp4 --codec=h264 --crf=18 --props='{"title":"Hi"}'
npx remotion still Promo out/thumb.png --frame=45
npx remotion render Promo out/promo.webm --codec=vp9
# Lambda: npx remotion lambda render <serve-url> Promo
```
Social presets: 1080×1920 (Reels/TikTok/Shorts), 1080×1080, 1920×1080; 30fps; h264 + AAC; `yuv420p`.

## Pre-render checklist
- [ ] `npx tsc --noEmit` passes
- [ ] Scrub every scene in Studio; check first and last frame of each Sequence
- [ ] No non-deterministic code
- [ ] Fonts and assets load (no fallback flash in stills)
- [ ] Hand off to `video-quality-control`
