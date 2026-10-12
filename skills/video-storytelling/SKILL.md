---
name: video-storytelling
description: Turn a short explainer or history video from "a slideshow with narration" into a story with tension, a turn and a payoff. Use when writing or restructuring a script for a YouTube explainer, history featurette, product story or tutorial intro, when someone says a video "flows like a PowerPoint", "lacks tension", "feels like an AI video" or "the voice sounds unnatural", or when planning shots, pacing, captions, voice and music for a narrated video.
---

# Video storytelling

A narrated explainer feels like PowerPoint when **every sentence gets one picture, one caption and one cut, in date order, at one speed**. The fix is mostly in the script and the edit, not the graphics. Work through the four passes below in order: story, script, edit, sound.

## Pass 1 · Find the story (before writing a line)

Answer these in one line each. If you can't, the video isn't ready to write.

1. **The question.** What does the viewer want to know by second 10? ("Why did people try to burn a file format?")
2. **The person.** Whose life does this happen *to*? Use a real person if you have the rights and facts; otherwise an invented everyday character, said plainly to be invented ("This is Ruth. She isn't real, but millions like her were."). Never generate images of real people.
3. **The stakes.** What could this person lose?
4. **The plant.** One small detail early that matters a lot later. Name it and tell the viewer to remember it.
5. **The turn.** The moment it goes wrong. This is where the music changes.
6. **The clock.** A deadline or countdown the viewer can watch (an expiry date, a launch, a bill).
7. **The payoff.** Return to the opening image or question, now understood. Then deliver anything the title promised.

## Pass 2 · Shape the script

Use this five-part arc. Rough share of runtime in brackets.

| Part | Job | Pattern |
|---|---|---|
| **Cold open** (8%) | Start in the most dramatic moment, out of order | Concrete scene → a strange detail → "someone wanted it gone" → hard cut to black → title |
| **Setup** (30%) | Meet the person; explain only what the turn needs | Bridge line back in time ("To understand why, we go back to…"), the person, the ordinary world, the plant |
| **Rise** (20%) | Things go well; speed up | Montage, years flipping, energy, no narration for a few seconds |
| **Turn** (25%) | The plant comes back; tension | "But remember those three letters?" Music changes, the clock appears, silences get longer |
| **Payoff** (17%) | Resolution, callback, promise kept | The clock runs out, the person is OK, a callback to the cold open, then the title's promise |

Script rules:
- **Write for the ear.** Short sentences. One idea per line. Spell numbers the way they're said ("nineteen ninety-nine").
- **Explain inside the story.** Explainers sit between story beats, never before the viewer cares. Use one physical metaphor (a box of crayons) and stick with it.
- **Use bridges, not jumps.** Every change of time or place needs a line that pulls the viewer across ("But a GIF could only hold one still picture. For now."), plus a moment of picture (years flipping) before the next part.
- **Earn the silence.** After the big beats ("Gone.", the bill, the reveal), write in a hold of 1–2 seconds.
- **Plain words.** No "genuinely", no hype, no profanity unless the author uses it.

## Pass 3 · Edit (pictures, captions, pace)

- **Several pictures per line.** Wide → detail → the person's reaction. A 6-second line usually wants 2–3 cuts. Explainer graphics count as pictures.
- **Show the thing becoming the idea.** Don't open on the diagram. Start with the real photo, then pixelate it into blocks, then put numbers on the blocks. Real object → abstraction.
- **Vary the pace on purpose.** Fast cuts in the rise. Long holds at the turn. A hard cut to black before the title.
- **Name your people on screen.** Tag the protagonist every time they appear older, younger or with someone new ("SUBJECT 01 · RUTH · TODAY", "RUTH + HER MUM"). If a viewer asks "who is this?", the tag is missing.
- **Tracking boxes ride with the picture.** Attach them to the moving image layer, not the screen, and put them on the cut that actually shows the object.
- **Captions on the spoken word.** Time every caption word to the voice's word timings, not spread evenly across the line.
- **Label reconstructions.** AI-generated scenes carry a small "reconstruction" tag; real archive photos are shown as evidence.

## Pass 4 · Voice and music

- **Voice the scene, not the sentence.** Synthetic voices sound flat when every line is generated alone. Generate each scene as one take, then cut it apart at the gaps between lines using word or character timestamps. You keep the natural flow and still control holds and cuts.
- **Direct the delivery.** With an expressive model (e.g. ElevenLabs v3), use sparing audio tags: [quietly], [whispers], [serious], [pause], [laughs softly], [curious]. One or two per scene.
- **Check the take.** Transcribe it to make sure no tags were read aloud and that the names and numbers came out right.
- **Best of all: the author's real voice.** Record it, transcribe for timings, and everything else here still works.
- **The score follows the story.** Use one piece composed in sections that match the arc (cold open, title hit, setup, explainer, wonder, rise, turn, loss, release, ending), each with its own mood and exact duration. Don't stop and start separate music beds: viewers hear a dead stop as a mistake. At the turn, drop to sparse piano and ticking instead of silence.
- **Duck the music under the voice.** Use sidechain compression. Keep sound effects to ambience the story is set in (fire, a modem). Skip effects for actions (engines, whooshes).

## Story check (run before rendering)

- [ ] Within the first 15 seconds, is there a question the viewer wants answered?
- [ ] Is there a person, and are they named on screen every time they reappear?
- [ ] Is something planted early and paid off later, out loud?
- [ ] Is there a visible clock or deadline in the turn?
- [ ] Does every jump in time have a bridge line plus a beat of picture?
- [ ] Do no more than two lines in a row follow the "one line, one picture" pattern?
- [ ] Are there at least three deliberate silences?
- [ ] Does the music change at the turn without stopping dead?
- [ ] Does the ending call back to the cold open and keep the title's promise?
- [ ] Is every fact checked, with invented characters and AI reconstructions labelled?

## Reference

- [references/script-template.md](references/script-template.md): a fill-in beat sheet in the shape above.
- [references/pipeline.md](references/pipeline.md): one working pipeline (HyperFrames + ElevenLabs v3 + composed score) with the per-scene voice and word-timed captions technique.
