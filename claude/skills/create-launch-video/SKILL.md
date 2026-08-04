---
name: create-launch-video
description: Build a launch video for a product or repo end to end — plan the beats, generate the visuals, add voiceover and captions, and hand the user a reviewable draft. Use when asked for a launch video, demo video, or promo for a codebase or product.
---

# Create a launch video

Read `animora-basics` first if you haven't this session.

## Gather the three facts

From the repo (README, package metadata) or the user, establish:
1. **Product name**
2. **One-liner** — what it does, in one sentence
3. **Call to action** — where viewers should go

Ask only for what you cannot infer; confirm what you inferred.

## Build it

1. `create_project { title: "<product> launch video" }` → keep the
   `projectId`.
2. `plan_scenes { projectId, prompt }` with a launch-structured prompt.
   A shape that works:

   > A 30-second launch video for <product> — <one-liner>. Structure:
   > a bold hook stating the problem, two or three beats showing what it
   > does (name real features), a closing beat with the call to action
   > "<CTA>". Include a voiceover.

   `plan_scenes` may return a `question` instead of planning — answer it
   (ask the user if you can't) and call again with the answer folded
   into the prompt.
3. The plan fans out generation jobs server-side. Poll
   `read_project { projectId }` every 20–30 seconds; scenes and items
   appear as jobs finish. Stop polling when item counts stabilize across
   two polls (typically a few minutes).
4. If any scene's visuals are missing after settling, the studio's plan
   checklist has per-step retry — tell the user rather than re-planning
   from scratch (a second `plan_scenes` rebuilds the timeline).

## If the user has a screen recording

The recording-first flow (upload → auto-zoom → beat plan on real
footage) currently lives in the studio: send them to
`https://animora.so` → "Turn a screen recording into a demo". It bills
as one flat bundle, settled only after a verified export. Your MCP
tools can then edit the resulting draft like any other project.

## Hand off

End with: the studio link, what you built (beats, voiceover, captions),
and that exporting happens from the Export button — verified before it
charges. Do not claim an MP4 exists until an export has completed.
