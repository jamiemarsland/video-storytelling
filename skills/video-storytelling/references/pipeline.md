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
2. **Don't cut on v3's own alignment.** Its character timings run up to about 0.7 s early. Cutting on them clips the ends of words, or plays a sliver twice ("a a", "every, every").
3. Instead, run speech-to-text over the take (`/v1/speech-to-text`, `scribe_v1`, word timestamps). Match the heard words to the script words with a sequence matcher. Normalise both sides first: numbers to words ("1999" → "nineteen ninety nine"), "L-Z-W" → "lzw".
4. For each line boundary, take the end of the line's last heard word and the start of the next line's first heard word. Cut in the middle of the longest quiet run (20 ms frames under about -40 dB) between them.
5. Cut the take **end to end**: each line starts exactly where the previous one stopped. Never pull a start earlier "for safety", because that re-plays audio. Assert it in code.
6. Use the same heard word starts as caption times, so each word appears when it's said.
7. Before rendering, transcribe every line's clip and compare its first two and last two words with the script. Any extra, missing or doubled word at an edge is a cutting error.
8. Place each clip with `adelay`, then `aresample=async=1:first_pts=0`, in an ffmpeg `amix`. Without the resample, a delayed bed can silently start at 0:00.

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
