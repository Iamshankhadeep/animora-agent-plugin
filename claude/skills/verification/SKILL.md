---
name: verification
description: How Animora's render verification protects credits, and how to reason about generation outcomes. Load when a generation fails, when explaining costs, or when the user worries about being charged for bad output.
---

# Verification: why failures are free

Every AI-generated component in Animora is compiled, statically checked,
and **rendered in a real headless browser** before it is allowed onto a
timeline. Frames are sampled and inspected; a component that renders
blank, crashes, or never composes is rejected.

The billing consequence is the point:

- Credits are **reserved** when generation starts.
- They **settle to actual usage** only when the verified result lands.
- A failed verification (or any terminal failure) **refunds the
  reservation automatically** — no support ticket, no manual step.

The same rule gates MP4 exports: charged at render start, auto-refunded
if the render fails. Demo-flow projects go further — the whole flow's
bundle refunds if the flow fails.

## What this means for you

- Report failures plainly and note that nothing was ultimately charged.
  Never apologize for a cost that didn't happen.
- Don't hammer a failing generation to "get your money's worth" — the
  money isn't gone, and repeated identical calls just fail identically.
  Change the prompt or consult `known-errors`.
- When the user asks "what did that cost?": `get_project` shows what
  landed; each landed generation settles to its real usage (typically
  7–20 credits for a motion overlay) plus a small flat infra fee.
- A verified item on the timeline is trustworthy — you do not need to
  re-check that it renders.
