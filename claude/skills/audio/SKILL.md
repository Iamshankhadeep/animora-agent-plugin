---
name: audio
description: Voiceover and background music generation on an Animora project. Use when asked for narration, a voiceover, background music, or a soundtrack.
---

# Audio

Read `animora-basics` first. Audio generation bills credits and runs on
the server — the finished clip lands on the timeline whether or not
anyone is watching.

## Voiceover

`generate_audio { projectId, kind: "voiceover", prompt, startFrame }` —
`prompt` is the **exact spoken script**, not a description.

Write scripts like speech: contractions, short sentences, roughly 2.5
words per second of intended runtime (a 10-second beat fits ~25 words).
Read the numbers out ("twenty-five percent", "animora dot so").

## Music

`generate_audio { projectId, kind: "music", prompt, startFrame }` —
here `prompt` describes the track ("warm lo-fi with soft keys,
optimistic"). A single request caps at 22 seconds; for longer timelines
generate segments or loop in the studio.

## Mechanics

- The tool returns a `jobId` and queues the work; the item appears on
  the timeline when the job succeeds (poll `read_project`). One audio
  job per project runs at a time — `in_progress` means wait, not retry.
- Placement: `startFrame` is where the clip begins. For per-beat
  narration, generate one clip per scene at that scene's start frame
  rather than one monolithic track — the studio's caption tools key off
  those clips.
- Volume and fades are properties on the landed audio item
  (`set_item_property`: `decibelAdjustment`, fade durations). Duck music
  under narration by lowering the music item a few dB, not by muting.
- Failures refund the reservation automatically.
