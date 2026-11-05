# Always Infinity UGC — v4 "effects only" (no new text)

The clip already has burned-in Hebrew captions, so v4 adds **zero text**. Everything
is visual: 11 segments (scene cuts + punch-in jump cuts), a transition on every
scene cut, zoom/rotation keyframes, and 17 scene and character effects.

All transition and effect names below were verified against VectCutAPI's CapCut
metadata (`pyJianYingDraft/metadata`).

## Mechanics this prompt relies on

- `transition` on `add_video` plays **from that segment into the next one**.
- Jump-cut punch-ins: split one continuous shot into two segments and give the
  second one `scale_x` / `scale_y` > 1 with no transition between them.
- `add_video_keyframe` times are **timeline seconds**; each keyframe is attached to
  the segment that contains it. Keyframe times are kept off exact cut points, and
  keyframes are only used on segments that have no `scale_x` / `scale_y` set.
- No speed ramps: the clip has a voice-over, and speed changes would distort it.

## Edit plan

| # | Timeline (s) | Scene | Framing | Out transition | Effects |
|---|---|---|---|---|---|
| 1 | 0.00–1.60 | Boxes on satin | zoom-out + tilt keyframes | `Twinkle_Zoom` | `Glowburst`, `Gold_Sparkles` |
| 2 | 1.60–2.87 | Wrapper close-up | — | `RGB_Glitch` | `Light_leak` |
| 3 | 2.87–4.43 | Pad close-up | slow push-in keyframes | `White_Flash` | `Camera_Focus`, `Halo` |
| 4a | 4.43–6.00 | Talking head | wide | jump cut | `Heart_Lenses` (character) |
| 4b | 6.00–7.57 | Talking head | punch-in 1.15 | `Axis_Rotation` | `Aura` (character) |
| 5 | 7.57–10.13 | Walking POV | — | `Shake_3` | `Motion_Blur`, `Leak_2` |
| 6 | 10.13–12.17 | Selfie | slow push-in keyframes | `Pull_in` | `Starlight` (character) |
| 7a | 12.17–14.20 | Box pile | wide | jump cut | — |
| 7b | 14.20–17.17 | Hero box | punch-in 1.2 | `White_Flash` | `Glowburst`, `Sparkle_Spiral` |
| 8a | 17.17–20.15 | Booth, dancing | beat-punch keyframes | jump cut | `Camera_Shake`, `Confetti_II`, `Disco_Ball_1` |
| 8b | 20.15–23.15 | Booth finale | punch-in 1.12 | — | `Fireworks_2`, `Club_Hearts` (character) |

## Prompt

Close CapCut before running. Replace the source path if the file lives elsewhere.

```
Use the capcut MCP tools to turn my clip into a high-energy, professional UGC ad using ONLY visual effects, transitions and camera moves. Do NOT add any text, titles, captions or stickers: the video already has Hebrew captions burned in. Create a NEW draft; do not modify earlier drafts.

Source: C:\Users\User\Videos\ugc\IMG_5104.MOV (23.15 s, 1080x1920, 30 fps, Hebrew voice-over). Keep the original audio at volume 1.0 on every segment and never change speed.

STEP 1 — create_draft 1080x1920.

STEP 2 — VIDEO SEGMENTS. Add the same source file 11 times, back to back, on track_name "main". For each segment: start/end = source in/out, target_start = same value as start. A transition plays from that segment into the next one. "No transition" means a hard jump cut.
   1.  start 0.00,  end 1.60,  target_start 0.00,  transition "Twinkle_Zoom", transition_duration 0.30
   2.  start 1.60,  end 2.87,  target_start 1.60,  transition "RGB_Glitch",   transition_duration 0.30
   3.  start 2.87,  end 4.43,  target_start 2.87,  transition "White_Flash",  transition_duration 0.25
   4.  start 4.43,  end 6.00,  target_start 4.43,  no transition
   5.  start 6.00,  end 7.57,  target_start 6.00,  scale_x 1.15, scale_y 1.15, transition "Axis_Rotation", transition_duration 0.40
   6.  start 7.57,  end 10.13, target_start 7.57,  transition "Shake_3",      transition_duration 0.30
   7.  start 10.13, end 12.17, target_start 10.13, transition "Pull_in",      transition_duration 0.35
   8.  start 12.17, end 14.20, target_start 12.17, no transition
   9.  start 14.20, end 17.17, target_start 14.20, scale_x 1.2, scale_y 1.2, transition "White_Flash", transition_duration 0.30
   10. start 17.17, end 20.15, target_start 17.17, no transition
   11. start 20.15, end 23.15, target_start 20.15, scale_x 1.12, scale_y 1.12, no transition

STEP 3 — CAMERA MOVES with add_video_keyframe on track "main" (batch mode; times are timeline seconds):
   a. Hook slam (segment 1): property_types ["uniform_scale","uniform_scale","rotation","rotation"], times [0.0, 0.5, 0.0, 0.5], values ["1.25","1.0","-4","0"]
   b. Pad push-in (segment 3): property_types ["uniform_scale","uniform_scale"], times [2.95, 4.35], values ["1.0","1.12"]
   c. Selfie push-in (segment 7): property_types ["uniform_scale","uniform_scale"], times [10.2, 12.1], values ["1.0","1.1"]
   d. Dance beat punches (segment 10): property_types all "uniform_scale", times [18.7, 18.85, 19.05, 19.4, 19.6], values ["1.0","1.1","1.0","1.1","1.0"]

STEP 4 — EFFECTS with add_effect. Every effect goes on its OWN track (fx_01 ... fx_17) so they can overlap.
   Scene effects (effect_category "scene"):
   1.  "Glowburst"       0.00-0.50   fx_01   hook burst on the first frame
   2.  "Gold_Sparkles"   0.00-1.60   fx_02   premium sparkle on the boxes
   3.  "Light_leak"      1.60-2.87   fx_03   warm leak over the wrapper
   4.  "Camera_Focus"    2.90-3.50   fx_04   focus pull onto the pad
   5.  "Halo"            3.50-4.43   fx_05   soft glow on the pad
   6.  "Motion_Blur"     7.60-8.00   fx_06   whip energy into the walk
   7.  "Leak_2"          8.00-10.10  fx_07   sunny light leak on the walk
   8.  "Glowburst"       14.20-14.70 fx_08   flash on the hero punch-in
   9.  "Sparkle_Spiral"  14.70-17.10 fx_09   sparkles around the hero box
   10. "Camera_Shake"    17.20-17.60 fx_10   impact on the booth reveal
   11. "Confetti_II"     17.20-19.50 fx_11   celebration at the booth
   12. "Disco_Ball_1"    18.70-20.10 fx_12   party lights on the dance
   13. "Fireworks_2"     20.15-23.15 fx_13   finale
   Character effects (effect_category "character"):
   14. "Heart_Lenses"    5.00-6.00   fx_14   heart eyes while she smiles
   15. "Aura"            6.00-7.50   fx_15   glow on the talking-head punch-in
   16. "Starlight"       10.20-12.10 fx_16   sparkle on the selfie
   17. "Club_Hearts"     20.15-23.15 fx_17   hearts in the finale

STEP 5 — save_draft with project_name "ugc-always-v4-fx".

RULES:
- If any transition, effect or keyframe is rejected, skip only that item, continue, and list it in your report. Do not invent replacement names.
- Do not add text, stickers, filters, speed changes or music unless I ask.
- Finish with a table of every call (step, item, success/failure, error message), then the draft_id and the folder the draft was saved to.
```

## Optional audio layer (only if you have the files)

Sound design is half of "pro" UGC. Append to STEP 4 before saving:

```
STEP 4b — AUDIO:
- add_audio C:\Users\User\Music\ugc\music.mp3, start 0, end 23.15, target_start 0, volume 0.10, track_name "music"
- add_audio C:\Users\User\Music\ugc\whoosh.mp3 at target_start 1.45, 2.72, 4.30, 7.40, 9.95, 12.00 and 17.02 (one call each), volume 0.5, track_name "sfx"
```

Use royalty-free music and SFX. Keep music low so the voice-over stays clear.

## 2-minute manual polish in CapCut

1. **Color pop**: select all clips → Adjust → saturation +10, contrast +8,
   sharpen +10 → apply to all.
2. **Watch at full speed** on a phone-size preview. If one effect hides the burned
   captions, shorten or delete it.
3. **Disclosure**: if this is a paid collaboration, turn on the platform's
   "Paid partnership" label when posting (Instagram/TikTok), since the video has no text.
4. **Export**: 1080p, 30 fps, MP4.

## Dial it down / up

- Too much? Delete in this order: `Club_Hearts`, `Disco_Ball_1`, `Heart_Lenses`,
  `Light_leak`, `Halo`. Keep the hook (`Glowburst` + zoom slam), the hero box
  (punch-in + `Sparkle_Spiral`) and the booth reveal (`Camera_Shake` + `Confetti_II`).
- Even crazier: swap transitions for `Rotate_CW_II` or `Squeeze`; add scene effects
  `Strobe`, `Rainbow_Sparkle`, `Neon` or `Edge_Glow`; character effects
  `Speed_Streaks` (walk) or `Light_Trails` (dance).
