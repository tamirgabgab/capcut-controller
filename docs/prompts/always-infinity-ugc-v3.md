# Always Infinity UGC — v3 (more effects and transitions)

Builds on [v2](always-infinity-ugc-v2.md): same clip, same Hebrew overlays, but the
clip is **split into 8 segments at the real scene cuts** so every cut gets a
transition, plus scene effects and extra zoom keyframes.

## How transitions work in VectCutAPI

- A transition is set on `add_video` via `transition` + `transition_duration`, and
  it plays **from that segment into the next one**. The last segment gets none.
- So one source file is added 8 times with `start` / `end` (source in/out) and
  `target_start` (timeline position), all on track `"main"`.

## Segment plan (cuts from ffmpeg scene detection)

| # | Source / timeline (s) | Scene | Transition out |
|---|---|---|---|
| 1 | 0.00–1.60 | Boxes on satin sheet | `Pull_in` |
| 2 | 1.60–2.87 | Wrapper close-up | `White_Flash` |
| 3 | 2.87–4.43 | Pad close-up | `Squeeze` |
| 4 | 4.43–7.57 | Talking head on sofa | `Wipe_Left` |
| 5 | 7.57–10.13 | Walking POV | `Shake_3` |
| 6 | 10.13–12.17 | Outdoor selfie | `Pull_in` |
| 7 | 12.17–17.17 | Pile of boxes, hero box | `White_Flash` |
| 8 | 17.17–23.15 | Booth, dancing | — |

## Prompt

Replace the video path if needed. Close CapCut before running.

```
Use the capcut MCP tools to build an energetic UGC ad with MANY transitions and effects. Create a NEW draft; do not modify earlier drafts.

Source: C:\Users\User\Videos\ugc\IMG_5104.MOV (23.15 s, 1080x1920). It already has Hebrew word-by-word captions burned in at the lower third, so put ALL new text in the top area (transform_y between 0.65 and 0.9).

A. create_draft 1080x1920.

B. VIDEO: add the same source file as 8 back-to-back segments on track_name "main", volume 1.0. For each: start/end = source in/out, target_start = same as start. The transition plays from that segment into the next one.
   1. start 0.00, end 1.60, target_start 0.00, transition "Pull_in", transition_duration 0.35
   2. start 1.60, end 2.87, target_start 1.60, transition "White_Flash", transition_duration 0.3
   3. start 2.87, end 4.43, target_start 2.87, transition "Squeeze", transition_duration 0.4
   4. start 4.43, end 7.57, target_start 4.43, transition "Wipe_Left", transition_duration 0.4
   5. start 7.57, end 10.13, target_start 7.57, transition "Shake_3", transition_duration 0.35
   6. start 10.13, end 12.17, target_start 10.13, transition "Pull_in", transition_duration 0.35
   7. start 12.17, end 17.17, target_start 12.17, transition "White_Flash", transition_duration 0.3
   8. start 17.17, end 23.15, target_start 17.17, no transition

C. ZOOM KEYFRAMES: add_video_keyframe on track "main", batch mode, property_types all "uniform_scale":
   times  [0,      0.6,   4.5,   7.4,    13.2,  14.2,   18.7,  19.0,   19.3]
   values ["1.15", "1.0", "1.0", "1.08", "1.0", "1.15", "1.0", "1.1", "1.0"]
   (zoom-out reveal on the hook, slow push-in on the talking head, push-in on the hero box, beat punch on the dance).

D. SCENE EFFECTS with add_effect, effect_category "scene", each on its own track:
   1. "Mini_Zoom"     0.00-0.60   track "fx_01"  (hook impact)
   2. "Camera_Focus"  2.90-3.60   track "fx_02"  (focus pull on the pad)
   3. "Motion_Blur"   7.60-8.10   track "fx_03"  (energy into the walk)
   4. "Star"          13.20-15.50 track "fx_04"  (sparkle on the hero box)
   5. "Camera_Shake"  17.20-17.70 track "fx_05"  (impact on the booth reveal)
   6. "Pulse"         18.70-20.10 track "fx_06"  (pulse on the dance)
   7. "Heart_Kisses"  20.15-23.15 track "fx_07"  (hearts under the CTA)

E. TEXT OVERLAYS (all bold true, transform_y 0.72 unless stated):
   1. 0.0-2.9 "מצאתי תחבושת שבאמת לא מרגישים": font_size 12, font_color "#FFFFFF", background_color "#1A4FBF", background_alpha 1.0, background_round_radius 0.3, intro_animation "Pop_Up", track "text_hook".
   2. 0.0-4.4 "שיתוף פעולה מסחרי": font_size 6, bold false, font_color "#FFFFFF", shadow_enabled true, transform_y 0.9, track "text_disclosure".
   3. 2.9-4.4 "גמישה וזזה איתך": font_size 11, font_color "#1A4FBF", background_color "#FFFFFF", background_alpha 0.95, background_round_radius 0.3, intro_animation "Bounce_In", outro_animation "Zoom_Out", track "text_hook".
   4. 7.6-10.1 "יום שלם בתנועה, בלי לחשוב על זה": font_size 10, font_color "#FFFFFF", shadow_enabled true, shadow_alpha 0.8, intro_animation "Slide_Up", outro_animation "Fade_Out", track "text_body".
   5. 13.0-17.0 "לא מרגישים. לא דולף.": font_size 12, font_color "#1A4FBF", background_color "#FFFFFF", background_alpha 0.95, background_round_radius 0.3, intro_animation "Zoom_In", outro_animation "Fade_Out", track "text_body".
   6. 19.5-23.15 "תייגי חברה שחייבת לנסות": font_size 12, font_color "#FFFFFF", background_color "#1A4FBF", background_alpha 1.0, background_round_radius 0.3, intro_animation "Bounce_In", track "text_cta".

F. save_draft with project_name "ugc-always-v3".

Rules: if any transition, effect or animation name is rejected, skip only that option, keep going, and list it in your report. Do not invent replacement names. At the end, give a table of every call with success or failure, then the draft_id and where the draft was saved.
```

## Tuning after the first render

- Too busy? Remove effects in this order: `Heart_Kisses`, `Pulse`, `Motion_Blur`.
  Keep `Mini_Zoom`, `Star` and `Camera_Shake`; they carry the hook, product and reveal.
- Want more? Swap transitions for `RGB_Glitch`, `Twinkle_Zoom`, `Rotate_CW_II` or
  `Axis_Rotation`; add `Lightning` or `Edge_Glow` as scene effects.
- Speech overlap: each transition blends ~0.3–0.4 s of audio from both sides. If a
  word sounds clipped, shorten that transition to 0.2 s.
