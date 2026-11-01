---
name: vector-animation
description: Creates resolution-independent vector animation — SVG (SMIL/CSS/JS), Lottie/dotLottie, Rive-style state machines, path morphing and line drawing — for web, apps, and video. Use for icons, loaders, illustrations, logo animations, and lightweight UI motion.
---

# Vector Animation

## When to use
- Animated icons, loaders, onboarding illustrations, logo stings, micro-interactions
- Lightweight motion for a PWA/website (small payload, crisp at any DPI)
- Vector assets that will be dropped into Remotion or After Effects

## Choosing a format
| Need | Use |
|---|---|
| Simple UI motion on the web | Inline SVG + CSS |
| Timeline control, sequencing, scroll-driven | SVG + Web Animations API or GSAP |
| Designer handoff from After Effects / complex illustration | Lottie (`.json`) / dotLottie (`.lottie`) |
| Interactive, stateful (hover/press/progress) | Rive or a JS state machine over SVG |
| In a video render | SVG components in Remotion, or `@remotion/lottie` |

## SVG authoring rules
- Clean `viewBox` (e.g. `0 0 100 100`), no fixed width/height on root; use `currentColor` or CSS variables for theming.
- Group by animatable parts with ids/classes; set `transform-box: fill-box; transform-origin: center;` for per-element rotation/scale.
- Optimize with SVGO but keep ids you animate (`--disable=cleanupIds` or a prefix plugin).
- Prefer animating `transform` and `opacity` (GPU-friendly); avoid animating filters on many nodes.

## Techniques
**Line drawing**
```css
.draw path { stroke-dasharray: var(--len); stroke-dashoffset: var(--len); animation: draw 1.2s cubic-bezier(.65,0,.35,1) forwards; }
@keyframes draw { to { stroke-dashoffset: 0; } }
```
Compute `--len` with `path.getTotalLength()`.

**Path morphing** — both paths need the same command count/structure; otherwise use a library (flubber, GSAP MorphSVG) or `@remotion/paths` `interpolatePath`.

**Stagger with WAAPI**
```js
document.querySelectorAll('.dot').forEach((el, i) =>
  el.animate([{ transform: 'scale(0)' }, { transform: 'scale(1)' }],
    { duration: 500, delay: i * 80, easing: 'cubic-bezier(.34,1.56,.64,1)', fill: 'both' }));
```

**Loop design** — first and last frames must match exactly; use `alternate` or design cyclical keyframes; idle loops 2–4s.

## Lottie
```html
<script src="https://unpkg.com/@lottiefiles/dotlottie-wc@0.3.0/dist/dotlottie-wc.js" type="module"></script>
<dotlottie-wc src="/anim/success.lottie" autoplay loop style="width:120px;height:120px"></dotlottie-wc>
```
- Keep < 100 KB where possible; avoid embedded raster images, expressions, and unsupported AE effects (most blurs/glows, 3D layers).
- Use `renderer: 'svg'` for crispness, `'canvas'` for many instances.

## Accessibility & performance
- Decorative animations: `aria-hidden="true"`; meaningful ones get `role="img"` + `<title>`.
- Respect reduced motion:
```css
@media (prefers-reduced-motion: reduce) { * { animation: none !important; transition: none !important; } }
```
- Pause offscreen animations (IntersectionObserver) to save battery on mobile.
- Target 60fps; check in DevTools Performance with 4× CPU throttle.

## Checks
- [ ] Crisp at 1× and 3× DPR
- [ ] Theming via variables works in light/dark
- [ ] Loops are seamless
- [ ] File size within budget
- [ ] Reduced-motion fallback shows a sensible static frame
