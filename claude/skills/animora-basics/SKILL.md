---
name: animora-basics
description: MANDATORY first read before using any Animora tool — the data model, id namespaces, credit rules, and how the tools fit together. Load this whenever a task involves Animora video projects, timelines, or generation.
---

# Animora basics (read this first)

You are driving a real video editor over MCP. Every mutation you make
lands on a persistent timeline the user can open at
`https://animora.so/projects/<projectId>/edit` — treat their project the
way you would treat their codebase.

## The data model

- A **project** owns one **editor document**: tracks → items → assets.
  `read_project` returns the whole thing plus `documentVersion`.
- **Items** sit on tracks with `from` (frame) and `durationInFrames`.
  Frames run at the document's `fps` (usually 30). Types you will see:
  `video`, `audio`, `text`, `captions`, `solid`, and `generated-motion`
  (an AI-generated Remotion component).
- **Assets** are the media items point at. Mind the two id namespaces:
  timeline tools take the **editor asset id** from `read_project`;
  `add_captions` accepts either that id or the uploaded asset's uuid and
  bridges them itself.
- **Scenes** are labeled frame ranges over the timeline — metadata for
  navigation and planning, never containers that clip media.

## Non-negotiable rules

1. **Always call `read_project` before editing.** You need current item
   ids, frame positions, and the document version. Editing blind against
   a stale picture produces overlaps the server will reject.
2. **Every tool takes `projectId`.** Get it from `list_projects` or
   `create_project`. You can only touch projects the key's owner owns —
   a foreign id returns `project_not_found`, no exceptions.
3. **Generation costs credits; edits are free.** `generate_motion`,
   `edit_motion`, `generate_audio`, and `plan_scenes` bill the user's
   credit balance. Timeline edits (`move_item`, `trim_item`,
   `set_item_property`, `delete_item`, `split_at`, `add_captions`,
   `restyle_captions`) do not.
4. **Failed generations refund themselves.** Every generated component
   is render-verified before it lands; if verification fails, the
   reservation returns to the balance. Never advise the user that a
   failure cost them money.
5. **Don't loop on a failing call.** Two identical failures means stop
   and read the error — the `known-errors` skill maps each code to the
   fix.

## Typical session shape

```
list_projects                      → pick or confirm the target
create_project { title }           → when starting fresh
read_project { projectId }         → always, before edits
…generation / edit tools…
read_project { projectId }         → confirm what landed
```

Finish by giving the user their studio link:
`https://animora.so/projects/<projectId>/edit`.
