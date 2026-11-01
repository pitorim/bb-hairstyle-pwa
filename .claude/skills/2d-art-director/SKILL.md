---
name: 2d-art-director
description: Defines and enforces the visual direction for 2D animation and video work — style guides, color scripts, character/prop/background consistency, and art review. Use when starting a new animated piece, establishing a look, writing a style bible, or reviewing frames for visual consistency.
---

# 2D Art Director

You own the *look* of the project. Every other animation skill consumes the decisions made here.

## When to use
- Kicking off a new animation, explainer, promo, or social video
- The user asks for "a style", "a look", "a brand feel", or visual consistency
- Reviewing generated frames/assets that feel inconsistent

## Workflow

1. **Brief intake** — Capture: audience, platform (9:16 / 1:1 / 16:9), duration, tone (playful, premium, calm…), brand assets (logo, fonts, colors), references.
2. **Style pillars** — Write 3 short pillars (e.g. "Soft shapes, no hard corners", "Warm pastel palette", "Bouncy but never chaotic"). Every later decision must trace back to a pillar.
3. **Palette** — Define tokens, not ad-hoc hex values:
   - `bg`, `surface`, `ink` (line/text), `primary`, `accent`, `shadow`, `highlight`
   - Max 5–7 core hues. Check text/background contrast ≥ 4.5:1.
   - Provide a dark-scene variant if the piece changes lighting.
4. **Shape language** — Circles = friendly, squares = stable, triangles = dynamic/danger. Pick a dominant shape per character/element.
5. **Line & rendering rules** — Line weight (or lineless), corner radius, shading style (flat / cel / soft gradient), texture overlay (grain %, paper), shadow direction (fixed light source, e.g. top-left).
6. **Typography** — Display + body font, size scale (e.g. 1.25 ratio), max 2 families, kinetic-type rules.
7. **Color script** — One thumbnail color swatch per scene/beat showing emotional progression.
8. **Model sheets** — For each character/key prop: turnaround (front, ¾, side, back), expression sheet, scale chart vs. other characters.

## Deliverable: `STYLE_GUIDE.md`
```md
# Style Guide — <Project>
## Pillars
## Palette (tokens + hex + usage)
## Shape language
## Line / shading / texture
## Typography
## Color script (scene → palette shift)
## Characters & props (links to model sheets)
## Do / Don't (with examples)
```
Also export tokens as code when the target is code-based (Remotion/CSS):
```ts
export const theme = {
  bg: '#FFF7F0', surface: '#FFFFFF', ink: '#2B2333',
  primary: '#E86A92', accent: '#7BC6CC', shadow: '#2B233322',
  font: { display: 'Fredoka', body: 'Inter' },
  radius: 24,
} as const;
```

## Review checklist
- [ ] Every color used exists in the palette tokens
- [ ] Light direction is consistent across shots
- [ ] Characters stay on-model (proportions, colors, line weight)
- [ ] Silhouettes read clearly in solid black
- [ ] Focal point is obvious in each frame (contrast, saturation, or isolation)
- [ ] Text is legible at the smallest target (mobile, muted autoplay)
- [ ] Safe areas respected (title-safe 90%, platform UI overlays for Reels/TikTok)

## Handoffs
- → `professional-storyboarding` for shot planning
- → `2d-character-animation` / `vector-animation` for asset build
- → `video-quality-control` for final visual sign-off
