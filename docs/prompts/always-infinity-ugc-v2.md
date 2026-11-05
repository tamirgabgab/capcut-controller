# Always Infinity UGC — v2 enhancement prompt

Source clip: `IMG_5104.MOV` (1080x1920, 30 fps, 23.15 s, Hebrew voice-over,
word-by-word Hebrew captions already burned in at the lower third).

## Scene map (from scene detection + frame review)

| Time (s) | Scene |
|---|---|
| 0.0–1.6 | Three Always boxes on a white satin sheet |
| 1.6–2.9 | Close-up of the pad wrapper |
| 2.9–4.4 | Close-up of the pad |
| 4.4–7.6 | Talking head on the sofa, smiling, holding a box |
| 7.6–10.1 | Walking POV, pink skirt, sunny pavement |
| 10.1–12.2 | Outdoor selfie |
| 12.2–17.2 | Pile of boxes, hand picks "ZERO FEEL ZERO LEAKS" box |
| 17.2–23.15 | Always Infinity booth, dancing with boxes |

## Design rules

- Burned-in captions sit at the lower third (`transform_y` ≈ -0.55). All new text
  goes in the **top area** (`transform_y` 0.65–0.9) so nothing overlaps.
- Overlay copy is **Hebrew**, matching the voice-over.
- Claims stay on what the packaging says (Zero Feel / Zero Leaks, Infinity
  FlexProtect). Check every claim against the brand brief before publishing.
- Keep effects light: UGC that looks over-produced performs worse.
- Only animation/effect names that exist in VectCutAPI's CapCut metadata are used.

## Prompt

Replace the video path if the file lives elsewhere. Close CapCut before running.

```
Use the capcut MCP tools to build an improved UGC ad from my clip. Create a NEW draft; do not modify ugc-test-01.

Source: C:\Users\User\Videos\ugc\IMG_5104.MOV (23.15 s, 1080x1920). It already has Hebrew word-by-word captions burned in at the lower third, so put ALL new text in the top area (transform_y between 0.65 and 0.9) and never below transform_y 0.

1. create_draft 1080x1920.
2. add_video with the full clip, start 0, track_name "main", keep the original audio at full volume.
3. Hook text, 0.0-2.9 s: "מצאתי תחבושת שבאמת לא מרגישים"
   font_size 12, bold true, font_color "#FFFFFF", background_color "#1A4FBF", background_alpha 1.0, background_round_radius 0.3, transform_y 0.72, intro_animation "Pop_Up", track_name "text_hook".
4. Disclosure text, 0.0-4.4 s: "שיתוף פעולה מסחרי"
   font_size 6, font_color "#FFFFFF", shadow_enabled true, transform_y 0.9, track_name "text_disclosure".
5. Product text, 2.9-4.4 s: "גמישה וזזה איתך"
   font_size 11, bold true, font_color "#1A4FBF", background_color "#FFFFFF", background_alpha 0.95, background_round_radius 0.3, transform_y 0.72, intro_animation "Bounce_In", track_name "text_hook".
6. Lifestyle text, 7.6-10.1 s: "יום שלם בתנועה, בלי לחשוב על זה"
   font_size 10, bold true, font_color "#FFFFFF", shadow_enabled true, shadow_alpha 0.8, transform_y 0.72, intro_animation "Slide_Up", outro_animation "Fade_Out", track_name "text_body".
7. Benefit text, 13.0-17.0 s: "לא מרגישים. לא דולף."
   font_size 12, bold true, font_color "#1A4FBF", background_color "#FFFFFF", background_alpha 0.95, background_round_radius 0.3, transform_y 0.72, intro_animation "Pop_Up", track_name "text_body".
8. CTA text, 19.5-23.15 s: "תייגי חברה שחייבת לנסות"
   font_size 12, bold true, font_color "#FFFFFF", background_color "#1A4FBF", background_alpha 1.0, background_round_radius 0.3, transform_y 0.72, intro_animation "Bounce_In", track_name "text_cta".
9. add_video_keyframe on track "main", property uniform_scale, batch mode:
   times [0, 0.6, 13.2, 14.2, 17.0, 17.2], values ["1.15", "1.0", "1.0", "1.15", "1.15", "1.0"]
   (zoom-out reveal on the hook, slow push-in on the hero box, reset at the booth cut).
10. add_effect "White_Flash", effect_category "scene", 17.1-17.4 s, track_name "fx_01" (flash into the booth reveal).
11. save_draft with project_name "ugc-always-v2".

Rules: if any animation or effect name is rejected, skip it, keep going, and list it in your report. Do not invent replacement names. At the end, report every call with success or failure, the draft_id, and where the draft was saved.
```

## Optional additions

- Background music: add a line before `save_draft`:
  `add_audio from C:\Users\User\Music\track.mp3, start 0, end 23.15, volume 0.12`.
  Use a royalty-free track; keep the volume low so the voice-over stays clear.
- Variants for A/B testing: rerun with a different hook in step 3 and a new
  `project_name` (for example `"ugc-always-v2b"`). Hook ideas:
  - "POV: היום הראשון של המחזור ואת בכלל לא זוכרת"
  - "התחבושת שכולן שואלות אותי עליה"
  - "בדקתי אם באמת לא מרגישים אותה"
