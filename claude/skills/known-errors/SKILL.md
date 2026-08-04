---
name: known-errors
description: Animora tool error codes mapped to causes and fixes. Load whenever an Animora tool call returns an error you don't immediately understand.
---

# Known errors

Errors come back as `{ error: <code>, message }`. The code is the
contract; the fix column is what to do — not "retry".

| Code | Cause | Fix |
| --- | --- | --- |
| `project_not_found` | The projectId doesn't exist **or belongs to someone else** — the API never distinguishes. | `list_projects`, use an id you own. Don't probe. |
| `no_document` | Project exists but its editor document hasn't been created (very old prompt-flow projects). | Ask the user to open the project in the studio once, or `create_project` fresh. |
| `version_conflict` | The document changed between your read and write (the user is editing live). | `read_project` again and re-derive the edit from current state. |
| `op_failed` / `invalid_result` | The edit breaks a server rule — overlapping items, trim past the asset's end, bad property. | Read the message; fix the numbers. The message names the exact violation. |
| `no_transcript` | Captions requested before transcription finished (or the media has no speech). | Wait ~30–60 s after upload and retry once; if it persists, the media likely has no usable audio. |
| `captions_failed` | No timeline segment plays that asset at frames covered by transcript words. | Check `read_project` — is the asset actually on the timeline? |
| `insufficient_credits` | Balance below the reservation for a generation. | Tell the user the required amount; they top up at animora.so/billing. Do not queue more work. |
| `in_progress` | An audio job for this project is already running. | Poll `read_project` until it lands, then continue. |
| `rate_limited` | Per-minute or daily budget hit. | Pause and resume; batch reads instead of polling hot. |
| `plan_failed` | The planner couldn't produce a valid plan from the prompt. | Rephrase with concrete facts (product name, features, CTA). If it returned a `question` earlier, answer it. |
| `asset_not_found` | Id matches neither the editor's assets nor the project's uploads. | `read_project` for editor asset ids; check the id namespace note in `animora-basics`. |
| `not_configured` | An optional integration (e.g. stock search) is off on this deployment. | Say so; offer an alternative (generated motion instead of stock). |
| HTTP 401 | Key missing, mistyped, or revoked. | Re-run the connection check in the install doc; mint a fresh key at animora.so/developers. |

Two identical failures in a row = stop and reconsider; the third
identical call will fail the same way.
