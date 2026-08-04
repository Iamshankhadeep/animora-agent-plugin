---
name: edit-timeline
description: Precise timeline surgery on an Animora project — move, trim, split, retime, restyle, or delete items, and place motion assets. Use for any "change the video" request that names timing, position, text, or layout.
---

# Edit the timeline

Read `animora-basics` first. Always `read_project` immediately before
editing — item ids and frame math must come from the current document.

## The edit tools

| Tool | What it changes |
| --- | --- |
| `move_item` | An item's start frame |
| `trim_item` | On-timeline duration and/or source in-point |
| `split_at` | One item into two at an absolute frame |
| `set_item_property` | Whitelisted per-type properties (position, size, opacity, rotation, text, volume, caption styling, video `cameraKeyframes` and `backdrop`) |
| `delete_item` | Removes the item (assets stay in the library) |
| `insert_asset` | Places a library motion asset's head revision on the timeline |

## Frame math

Everything is frames at the document's `fps`. To put an item at 4.5
seconds in a 30 fps document: `from: 135`. When the user speaks in
seconds, convert and say so ("that's frame 135 at 30 fps").

The server verifies every write: items must not overlap on a track,
trims must stay inside the asset's duration, and the composition size
is fixed. A rejected edit returns the reason — fix the numbers, don't
retry blindly.

## Camera moves on video items

Video items may carry `cameraKeyframes` — an array of
`{ timeMs, scale, focalX, focalY, easing }` (times relative to the
item's start, focal as fractions of the frame, scale 1–8, `"spring"` or
`"linear"`). Patch them via `set_item_property`. Keep times strictly
ascending, start and end moves at scale 1 unless chaining, and prefer
scales between 1.5 and 2.6 — deeper reads as a mistake. A `backdrop`
(`{ wallpaper, paddingPct, cornerRadius, shadow }`, wallpapers:
`graphite`, `aurora`, `sunset`, `ocean`) frames the footage.

## Be conservative

Make the smallest edit that satisfies the request, then `read_project`
to confirm it landed the way you described. Every change is undoable in
the studio, but the user shouldn't need to.
