---
name: edit-any-video
description: Edit uploaded footage like an editor — remove filler words, cut false starts, tighten pauses, and land reviewable cuts on a real timeline. Use when asked to edit, tighten, cut, or clean up an interview, talk, vlog, podcast, or any raw footage.
---

# Edit any video

Read `animora-basics` first if you haven't this session.

## What you are driving

Every upload gets a free understanding pass: transcription, silence
detection, and an editorial analysis (topic, chapters, quotable
moments, weak segments) visible as markers in the studio at
`https://animora.so`. Editing works through the Script surface — the
timeline rendered as a transcript document.

## The flow

1. **Footage must be in a project.** The user uploads through the
   studio (send them the project link if they haven't). Uploads
   transcribe and analyze automatically in the background — a
   `no_transcript` error right after upload usually means "not done
   yet"; wait ~30s and retry.
2. **Read the script:** `read_script { projectId }` → a timeline.md
   where every word renders as `[startMs..endMs]word` and silences as
   `silence [startMs..endMs] asset <id>` lines. The document also names
   each segment and its asset.
3. **Edit the document:**
   - Delete a word: wrap it in `~~strikethrough~~` —
     `[400..520]~~um~~`.
   - Tighten a pause: append a target length to a silence line —
     `silence [1200..2600] asset ed-asset-1 -> 300` keeps 300ms.
   - Never alter the `[startMs..endMs]` timestamps — they ARE the edit
     identity. Everything else you write is ignored.
4. **Preview, then apply:**
   - `apply_script { projectId, script, preview: true }` returns the
     planned cut ranges without touching the timeline.
   - `apply_script { projectId, script }` commits them as ripple cuts —
     later clips shift left automatically, everything stays undoable,
     and a stale script (the timeline changed since `read_script`) is
     refused with `stale_script` — just re-read and re-apply.
5. **Or one click:** `auto_edit { projectId, intent? }` runs the
   editorial planner (filler removal, false-start cuts, pause
   tightening, optionally steered by the intent). It COSTS CREDITS,
   charged only when the verified draft commits; any failure refunds.
   The draft lands reviewable in the studio with Keep / Undo buttons.

## Craft rules

- Cut at breath points, never mid-word or mid-phrase.
- Standard tighten: silence over 1s → 300ms.
- Filler list: um, uh, er, ah, like (as filler), you know, I mean,
  basically, sentence-starting "so", literal-when-empty "literally".
- False starts: cut the abandoned phrase, keep the rewording.
- When unsure between two cut boundaries, cut less.

## Hand off

End with the studio link, what you cut (word deletions, pause
compressions), and that every change is visible and undoable on the
real timeline.
