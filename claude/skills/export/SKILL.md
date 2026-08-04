---
name: export
description: Getting a finished MP4 out of an Animora project — what exports cost, how verification gates the charge, and what to tell the user. Use when asked to export, download, or deliver the final video.
---

# Export

Exports render the timeline into a downloadable MP4 on Animora's render
infrastructure, from an immutable snapshot — edits made after you start
an export don't change that export.

## How the user exports

Exports run from the studio: `https://animora.so/projects/<projectId>/edit`
→ **Export** → *MP4 (H.264)*. The sheet shows the exact charge before
confirming. Point the user there once the timeline is ready; recent
exports with download links appear in the same sheet.

## Pricing

3 credits per **started** 30 seconds of timeline (a 45-second video is
2 bands = 6 credits). The charge happens when the render starts and is
**fully refunded if the render fails** — the verified-before-charged
rule applies to exports exactly as it does to generation.

Projects created through the studio's screen-recording demo flow bill
differently: one flat bundle covers the whole flow including the first
verified export.

## Your role as the agent

1. Confirm the timeline is complete (`read_project`: no missing beats,
   captions and audio where expected).
2. Tell the user the expected cost using the band math above.
3. Send them to the Export button with the studio link.
4. Never claim an MP4 exists until an export shows `completed` with a
   download URL in the studio.
