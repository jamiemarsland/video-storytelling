# Video storytelling: a Claude Code plugin

Turns narrated explainer, history and tutorial videos from "a slideshow with a voice-over" into a story. It was built while making *The Secret History of the GIF* for the JamieWP channel.

## What's inside

- **Skill `video-storytelling`** — four passes: find the story (question, person, stakes, plant, turn, clock, payoff), shape the script (five-part arc, bridges, silences), edit (several cuts per line, real thing → abstraction, name your people, word-timed captions), and voice and music (voice the scene not the sentence, a score that follows the story). Ends with a pre-render story check.
- **`/story-beats <topic>`** — builds a beat sheet for a new video.
- **`/story-check <script>`** — reviews an existing script against the checklist and writes specific fixes.
- **`references/pipeline.md`** — one working pipeline (HyperFrames + ElevenLabs v3 per-scene takes + composed score). This is optional; the storytelling works with any editor.

## Install

```
/plugin marketplace add jamiemarsland/video-storytelling
/plugin install video-storytelling@video-storytelling
```

Or from a local copy:

```
/plugin marketplace add /path/to/video-storytelling-plugin
/plugin install video-storytelling@video-storytelling
```
