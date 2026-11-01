---
name: cinematic-motion-design
description: Designs premium, cinematic motion for 2D/2.5D video — easing curves, choreography, camera language, depth/parallax, kinetic typography, transitions, and lighting effects. Use when motion needs to feel polished, high-end, or "Apple-level" rather than template-like.
---

# Cinematic Motion Design

## When to use
- Titles, product reveals, logo stings, transitions, UI/app promo videos
- Motion feels cheap, linear, or busy

## Core principles
1. **Hierarchy in time** — the most important element moves first or last, never "at the same time as everything".
2. **Choreography / stagger** — offset related elements 2–5 frames (30fps). Stagger direction follows reading order or motion vector.
3. **Easing is personality**
   - UI / premium: `cubic-bezier(0.22, 1, 0.36, 1)` (expo-out-ish), entries 400–700ms
   - Playful: spring `damping 10–14, stiffness 100–180`
   - Heavy/luxury: `cubic-bezier(0.65, 0, 0.35, 1)`, slower 800–1200ms
   - Exits ~70% the duration of entries, ease-in
   - Never linear except for constant drift, rotation loops, or tickers
4. **Continuity** — an element that leaves should hand off motion energy to the next (match direction, match shape, morph).
5. **Restraint** — max 2–3 motion ideas per shot. Stillness makes motion meaningful.

## Camera language (2.5D)
- Separate layers into depth planes: FG / MG / BG. Parallax ratio e.g. FG 1.4×, MG 1×, BG 0.4×.
- Slow push-in (scale 1.00 → 1.05 over shot) adds tension for free.
- Add subtle camera drift (±4px noise) to static shots so they feel "shot", not "rendered".
- Motion blur: approximate with directional blur proportional to velocity, or 180° shutter-equivalent frame blending.
- Depth of field: blur BG 4–12px when focus is on FG subject.

## Kinetic typography
- Animate by word or line, not by letter, unless it's a hero title.
- Techniques: mask reveal (text slides up from behind a clip), tracking expand (letter-spacing −0.05em → 0), weight/variable-font morph, highlight sweep.
- Hold readable text ≥ (words × 0.3s) + 1s.

## Transition toolkit
| Transition | When |
|---|---|
| Match cut / shape match | Linking two ideas visually |
| Whip pan + motion blur | Energy, social pacing |
| Mask/wipe following an object | Product reveals |
| Zoom-through | Going "into" a detail |
| Morph | Logo stings, icon-to-icon |
| Light leak / flash frame | Beat drops, emphasis (≤ 3 frames, mind photosensitivity) |

## Finishing / "look"
- Subtle film grain (2–4% opacity), vignette (10–20%), chromatic aberration only on fast motion
- Soft glow/bloom on highlights for premium feel
- Gradient lighting sweep across product/text to imply a light source
- Respect `prefers-reduced-motion` when output is a web page

## Code snippets
```tsx
// Staggered mask reveal (Remotion)
const words = title.split(' ');
words.map((w, i) => {
  const p = spring({ frame: frame - i * 3, fps, config: { damping: 200 } });
  return <span style={{ display: 'inline-block', overflow: 'hidden' }}>
    <span style={{ display: 'inline-block', transform: `translateY(${(1 - p) * 110}%)` }}>{w}&nbsp;</span>
  </span>;
});
```
```css
/* Web: premium entry */
.enter { animation: rise .6s cubic-bezier(.22,1,.36,1) both; }
@keyframes rise { from { opacity:0; transform: translateY(24px) scale(.98); filter: blur(6px); } }
```

## Review checklist
- [ ] Nothing moves linearly by accident
- [ ] Each shot has a clear first-mover and focal point
- [ ] Staggers are consistent (same offset system throughout)
- [ ] Motion directions are continuous across cuts
- [ ] No flashes > 3 per second (photosensitivity, WCAG 2.3.1)
