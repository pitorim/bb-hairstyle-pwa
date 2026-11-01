---
name: professional-storyboarding
description: Turns a script, brief, or idea into a shot-by-shot storyboard and animatic plan — beats, shot types, camera moves, timing, transitions, VO/SFX notes. Use before animating any video so pacing and story are locked first.
---

# Professional Storyboarding

## When to use
- Any video longer than a single shot
- The user provides a script, product brief, or vague idea and wants a video
- Pacing feels off and needs restructuring

## Workflow
1. **Logline** — one sentence: who/what, change, payoff.
2. **Beat sheet** — Hook (0–3s) → Problem/Setup → Development → Climax/Reveal → CTA/Resolution. For social: hook must land in the first 1.5s, muted.
3. **Time budget** — assign seconds per beat; total must match target duration. Rule of thumb: ~2.5 words/sec of VO; on-screen text needs ≥ (words × 0.3s) + 1s.
4. **Shot list** — break beats into shots (typically 1.5–4s each for social, 3–8s for explainers).
5. **Thumbnails** — rough frames (can be described in text or simple SVG boxes) showing composition, subject placement, motion arrows.
6. **Animatic** — play boards with timing + scratch audio; fix pacing *here*, not in final animation.

## Shot vocabulary
- **Framing**: EWS, WS, MS, MCU, CU, ECU, OTS, POV, insert
- **Angle**: eye level, high (vulnerable), low (powerful), dutch (unease)
- **Camera move**: static, pan, tilt, push-in (focus/emotion), pull-out (reveal/context), truck, crane, handheld shake, rack focus
- **Transition**: cut, match cut, J/L cut (audio leads/trails), whip pan, wipe, morph, mask reveal, dip to color

## Composition rules
- Rule of thirds; eyes on the upper third line
- Leading lines toward the focal point
- 180° rule — keep screen direction consistent
- Lead room / look room in the direction of motion or gaze
- Vary shot sizes between consecutive cuts (avoid jump cuts unless intentional)

## Deliverable: `STORYBOARD.md`
```md
# Storyboard — <Project>  (16:9 · 30fps · 30s)
Logline: …

| # | Time | Dur | Shot | Camera | Action | On-screen text | VO / SFX / Music | Transition |
|---|------|-----|------|--------|--------|----------------|------------------|------------|
| 1 | 0:00 | 2.0s | CU product | push-in 5% | Bottle spins into frame, sparkle | "Frizz?" | SFX whoosh; music hit | whip → |
| 2 | 0:02 | 3.0s | MS character | static | Character sighs at messy hair | — | VO: "Bad hair day…" | cut |
```
Also output a machine-readable version when the target is code (Remotion):
```ts
export const shots = [
  { id: 'hook', from: 0, durationInFrames: 60, transition: 'whip' },
  { id: 'problem', from: 60, durationInFrames: 90, transition: 'cut' },
] as const;
```

## Checks before handoff
- [ ] Hook works muted and in the first 1.5s
- [ ] Every shot has one clear purpose
- [ ] Durations sum exactly to target
- [ ] Screen direction consistent
- [ ] CTA on screen ≥ 2s, inside safe area
- [ ] Music hits / beats marked for `sound-design-sync`
