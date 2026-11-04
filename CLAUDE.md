# Benny Bits — YouTube Shorts Production Rules

## Channel
- Channel: Benny Bits — funny animated shorts about weird TRUE stories from history.
- Style: simple round-headed cartoon characters (like the Agloe video), 3D-rendered backgrounds, bold white captions with dark outline.
- Format: vertical 1080x1920, 30fps, 45–60 seconds, MP4 (H.264 + AAC).

## THE #1 GOAL: stop the scroll
Our first video had 50% of viewers swipe away in the first seconds because it opened on a calm, static office scene. Every video must be built to beat this. Target: 70%+ "stayed to watch".

## Hook rules (first 2 seconds) — MANDATORY
1. Frame 1 must already show action or emotion: a shocked/screaming/panicking character, a strange visual, or movement. NEVER a calm establishing shot, an empty room, or a character standing still.
2. The first spoken line must reveal the most absurd part of the story immediately (e.g. "Australia sent soldiers with machine guns... to fight birds."). No slow intros, no "In the 1930s...", no greetings.
3. A big bold on-screen title (2–4 words, e.g. "AUSTRALIA VS BIRDS") visible from frame 1 until ~2.5s, in addition to captions — many people scroll with sound off.
4. Characters must be large in frame during the hook (at least 40% of frame height).

## Story structure
- 0–3s: HOOK — the absurd claim.
- 3–15s: SETUP — the problem, fast.
- 15–40s: ESCALATION — things go wrong, at least one comedic beat (setup → instant punchline, e.g. "What could go wrong?" → "Everything.").
- 40–52s: TWIST — the surprising fact or number.
- 52–60s: ENDING — a punchline that echoes the first line, so the video loops seamlessly into its start.

## Pacing
- Change the shot, camera angle, or zoom at least every 2–3 seconds. No shot longer than 4 seconds.
- Narration ~150 words per minute, energetic delivery.
- Character expressions must react to every story beat (surprise, anger, panic, smugness).
- Keep characters consistent in design across all scenes.

## Captions
- Word-by-word or 1–3 words at a time, bold white with thick dark outline, centered in the lower-middle of the frame.
- Keep captions and the title out of the bottom 20% and right 15% of the frame (YouTube UI covers those areas).
- Highlight key words (numbers, names, the punchline) in yellow.

## Sound
- Background music under the narration, ducked so the voice is always clear.
- Comic sound effects on key beats (whoosh on shot changes, boing/bonk on punchlines).
- Use only royalty-free music and SFX.

## Before delivering ANY video — self-check
1. Extract frame 0 and frame at 1s as images and look at them: is there clear action/emotion and a big title? If it's a calm scene, fix it.
2. Confirm no shot lasts longer than 4 seconds.
3. Confirm captions are readable and not in the bottom 20% / right 15%.
4. Confirm the ending line connects back to the opening line.
5. Confirm length is 45–60s and resolution is 1080x1920.
Report the results of this check when delivering.

## Deliverables for every video
1. The final MP4.
2. A title (under 50 characters, curiosity-driven, never spoils the twist, 1 emoji).
3. A description: 2 sentences that tease the story and stop before the twist, then "Follow for more funny animated stories! 🎬", then 5–6 hashtags including #shorts.
4. A pinned comment with a fun question that invites replies.

## Lessons from a top-performing reference short ("Why the Second Draw Always Wins")

### Hook = motion from frame 1
- Frame 0 must show a character mid-action (aiming, running, screaming), not posing.
- Within the first 1.5 seconds, include a dynamic camera move: follow a flying object, whip-pan, or fast push-in. The viewer's eye must be pulled into the scene immediately.

### Camera never stands still
- Every shot has camera motion: slow push-in, pull-out, pan, or tracking. No static shots.
- Prefer smooth continuous camera moves and transitions between scenes over plain hard cuts.
- A new scene, angle, or framing every 2–3 seconds (~20 scenes per 60s video).
- Use close-ups on faces for emotional beats and wide shots to show the setting.

### Paradox titles
- The title must sound like a contradiction or an impossible fact that the brain needs resolved (e.g. "Why the Second Draw Always Wins", "Why Mapmakers Draw Fake Towns").
- The video answers the paradox only near the end.

### Every line gets a visual reaction
- Each narration sentence must have an exaggerated character reaction: panic with sweat drops, a squashed face, tongue out, a smug grin, wide eyes.
- The joke must work with sound OFF, through expressions alone.

### Connect the story to the viewer
- Around the 60–70% mark, bridge the historical story to the viewer's own life: speak to them directly with "you" / "your brain" / "your body", and show a modern object (phone, car, computer) in the scene.
- Example: from cowboys drawing guns → "your body reacts before you even decide."

### End with a real "I didn't know that" fact
- Every video must carry one surprising true fact (science, psychology, history) that the viewer will want to share.

### Shift the mood visually
- Change the lighting or time of day across the story (day → sunset → interior → night) so each act feels like a new chapter.

### Updated self-check (add to the existing one)
6. Is there camera motion in every shot? Any static shot → fix it.
7. Does the title sound like a paradox?
8. Is there a moment where the story connects directly to the viewer ("you")?
9. Does every narration line have a visible character reaction?

## Narrator voice
Default narrator for every Benny Bits video (approved; matched to the Eiffel Tower short's narrator).
- Engine: Kokoro TTS v1.0 (`kokoro-onnx`, files `kokoro-v1.0.onnx` + `voices-v1.0.bin`), lang `en-us`, 24 kHz.
- Voice: 50/50 style blend → `k.get_voice_style('am_eric') * 0.5 + k.get_voice_style('am_puck') * 0.5`.
- Post-processing (ffmpeg): `rubberband=pitch=2^(4.4/12):formant=preserved:pitchq=quality,highshelf=f=3000:g=2`, then a light compressor (`acompressor=threshold=-20dB:ratio=3:attack=5:release=80`).
- Speed: 1.21 reproduces the reference pace (~220 wpm). For a 45–60 s short use ~1.0–1.05 (≈160–170 wpm) so the length rule still holds.
- Generate per sentence/phrase and insert the pauses yourself (~0.25 s between sentences, ~0.5 s before punchlines and after questions); transcribe with faster-whisper for word timestamps.
- If the user sends their own recording (e.g. ElevenLabs), use that instead.

## Visual style
Approved look for every Benny Bits video (replaces the earlier "cute" toon style).
- **Colors: bright, saturated, high contrast.** Daytime = clear saturated blue sky with white clouds and warm ground. Sunset/dusk = vivid orange / pink / purple sky. Never a dull brown, sepia or muddy grade; keep haze light.
- **Characters: hand-drawn ink look.** Rough, slightly "boiling" ink outlines with varying thickness; small round heads with no neck; thin noodle limbs; dark gloves and boots; no blush, nothing cute. Faces have attitude: grumpy, smug, sarcastic, heavy-lidded, gritted teeth, side-eye. Characters are fully opaque and crisp (no depth blur on characters).
- **Character contrast:** clothing colors that contrast with the scene (e.g. red shirt / blue overalls against gold wheat, khaki against blue sky), plus a subtle light halo (rim light) around every character so they pop off the background. Characters cast long shadows and are lit by the scene's light.
- **Camera: low, dramatic angles.** Objects pointing at the camera (guns, fingers), characters on the left/right third, 3/4 turns, leaning into the action. Never a character standing centered and facing the camera straight on. Every shot still moves (push-in, pull-out, pan, tracking).
- **Foreground:** no huge blurred shapes. Any foreground element stays small and only slightly blurred. Background depth-of-field blur is gentle so the set stays readable.
- **Interaction:** characters sit, lean, eat, hold props and react to the set; props are recognizable and period-correct (e.g. a 1930s Lewis gun: straight cylindrical shroud, flat pan magazine on top, wooden stock, metallic shine).
- **On-screen text:** bold word-by-word captions (thick white text, strong dark outline, ~88 px, normal case; key words in yellow). At most 1–2 big text overlays per video (the title in the first 2.5 s counts as one).
- **Cuts:** hard cuts (optionally with a quick camera settle) — no semi-transparent crossfades.
- Pipeline: one still 3D plate per shot (Blender/Cycles) + per-frame anchors; 2D ink characters and all camera moves animated in compositing (Remotion).
