# A working pipeline (HyperFrames + ElevenLabs)

This is one way to build it. The storytelling passes in SKILL.md work with any editor or renderer.

## Data model

Write the story as a list of beats, not slides:

```python
b(scene, look, say, text, cuts, orange=None, hold=0.0, box=None, date=None, fx=(), dur=None)
# scene  lines sharing an id are voiced as ONE take
# say    narration with audio tags; None = a silent beat (give dur)
# text   on-screen headline (can be shorter than what's said); orange = the accent phrase
# cuts   2–3 pictures split across the line: ("gen","07") ("arc","file.jpg","caption") ("panel","name") ("type","name") ("black",)
# fx     cues such as clock1/clock2/clock0, flash, fire, modem, hit, music_off/music_on
```

## Voice: one take per scene, cut at the gaps

1. Join the scene's `say` strings with spaces and send them to `POST /v1/text-to-speech/{voice}/with-timestamps` with `model_id: eleven_v3` and `stability: 0.5`. Use 0.0 for more expression, 1.0 for steadier delivery.
2. The response's `alignment.character_start_times_seconds` covers every character, tags included. For each line:
   - find the first character after any leading `[tag]`, and the last character;
   - cut the take at the midpoint of the gap between this line and the next;
   - add a 20–80 ms fade at each cut.
3. Store each line's word start times: regex `\[[^\]]*\]|[^\s\[]+`, skipping tags. Map the headline words onto them in proportion, so the captions land on the spoken words.
4. On the timeline: line start = cursor, line end = start + segment length + hold. Place each segment with `adelay` in an ffmpeg `amix`.
5. Check with speech-to-text (`/v1/speech-to-text`, `scribe_v1`): no tags read aloud, names said correctly.

## Score: one piece composed to the edit

Once the timeline is built, cut it into sections at the story's boundaries and send a composition plan:

```json
{"model_id": "music_v1", "composition_plan": {
  "positive_global_styles": ["cinematic documentary score", "instrumental", "one coherent piece in one key"],
  "negative_global_styles": ["vocals"],
  "sections": [{"section_name": "cold open", "positive_local_styles": ["hushed", "low drone", "ticking"],
                "negative_local_styles": ["drums"], "duration_ms": 23800, "lines": []}]}}
```

A 5-minute, 10-section plan worked in one request. Cache the result by a hash of the plan, so the score is only recomposed when the edit's timings change. Duck it under the voice with `sidechaincompress=threshold=0.03:ratio=5:attack=15:release=400`, then `loudnorm I=-14`.

## Pictures

- **Generated scenes:** keep one fixed character description and repeat it word for word in every prompt (hair, glasses, freckles, cardigan; age per era). Generate wide, detail and reaction shots for each beat. Download right away, because hosted URLs expire.
- **Moving pictures:** slow push in, pull out or pan. Montage cuts get a small push and a big year label.
- **Boxes and tags:** draw them inside the moving image layer, positioned as fractions of the image, so they track during the push.
- **Explainers:** start from a real photo, then blocks (downscale the crop to e.g. 20 × 11 and map each cell to its nearest palette colour), then numbers.

## Render

- **HyperFrames:** a paused GSAP timeline on `window.__timelines["main"]`. Use `immediateRender:false` on mid-timeline `fromTo`. Full-frame overlays start at opacity 0.
- **Audio:** render the picture silent, then mux in the external mix. HyperFrames can drop many short audio clips, which is why the mix is done separately.
