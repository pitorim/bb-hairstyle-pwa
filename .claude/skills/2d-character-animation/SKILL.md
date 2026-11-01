---
name: 2d-character-animation
description: Animates 2D characters using the 12 principles of animation — rigging, walk/run cycles, acting, lip sync, and secondary motion — in code (Remotion/CSS/SVG) or rig-based tools. Use when a character needs to move, emote, talk, or perform.
---

# 2D Character Animation

## When to use
- A character must walk, gesture, react, talk, or idle believably
- Motion looks stiff, floaty, or robotic and needs fixing

## The 12 principles (apply every time)
1. **Squash & stretch** — preserve volume (scaleX × scaleY ≈ 1).
2. **Anticipation** — small opposite move before the main action.
3. **Staging** — one clear idea per pose; readable silhouette.
4. **Straight-ahead vs pose-to-pose** — default to pose-to-pose (keys → breakdowns → in-betweens).
5. **Follow-through & overlapping action** — hair, ears, clothing settle *after* the body stops (offset 2–6 frames).
6. **Slow in / slow out** — ease; never linear for organic motion.
7. **Arcs** — limbs and heads travel on arcs, not straight lines.
8. **Secondary action** — supports, never competes with, the primary action.
9. **Timing** — frames per action define weight and mood.
10. **Exaggeration** — push poses 10–30% beyond realistic.
11. **Solid drawing** — consistent volumes and perspective.
12. **Appeal** — clear design, asymmetry, readable expressions.

## Rig structure (cut-out / code)
Hierarchy with pivots at joints:
```
root (hips)
├─ torso → neck → head → (eyes, brows, mouth, hair[])
├─ upperArm.L → foreArm.L → hand.L
├─ upperArm.R → foreArm.R → hand.R
├─ thigh.L → shin.L → foot.L
└─ thigh.R → shin.R → foot.R
```
- Set `transform-origin` at the joint, not the part's center.
- Separate SVG groups per part; swap mouth/eye/hand shapes via state, not redraws.

## Timing cheat sheet (@ 30fps)
| Action | Frames |
|---|---|
| Blink | 4–6 (close 2, hold 0–1, open 2–3) |
| Head turn | 6–10 |
| Walk cycle (normal) | 24–32 per full cycle (2 steps) |
| Run cycle | 14–18 |
| Idle breathe | 60–90 loop |
| Take / reaction | anticipation 4–6 → hit 2 → settle 8–12 |

## Walk cycle keys
Contact → Down (lowest) → Passing → Up (highest) → Contact (other foot). Body bobs twice per cycle; arms counter-swing opposite legs; head lags hips by 1–2 frames.

## Lip sync
Use a reduced mouth set (Preston Blair / Rhubarb): `A` (MBP closed), `B` (EE/K/S), `C` (EH), `D` (AA), `E` (O), `F` (OO/W), `G` (F/V), `H` (L), `X` (rest).
- Drive from a phoneme timeline (e.g. Rhubarb Lip Sync JSON).
- Shift mouth shapes 1–2 frames **earlier** than audio — reads as in-sync.
- Hold shapes ≥ 2 frames; skip shapes that would flash for 1 frame.

## Code patterns (Remotion)
```tsx
const frame = useCurrentFrame();
const { fps } = useVideoConfig();
// Bouncy hop with squash on landing
const hop = spring({ frame, fps, config: { damping: 9, stiffness: 120 } });
const y = interpolate(hop, [0, 1], [0, -80]);
const squash = interpolate(frame % 30, [0, 3, 8], [1, 0.8, 1], { extrapolateRight: 'clamp' });
// Overlap: hair follows head with delay
const hairRot = Math.sin((frame - 4) / 6) * 6;
```

## Quality checks
- [ ] No part moves linearly unless mechanical
- [ ] Silhouette readable on every key pose
- [ ] Overlap on all soft appendages
- [ ] Feet don't slide during contact frames
- [ ] Eyes lead head turns; head leads body
- [ ] Holds are "moving holds" (subtle breathing), never dead freezes > 1s
