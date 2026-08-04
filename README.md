# Animora for Claude Code

Make videos from your coding agent. This plugin connects Claude Code to
[Animora](https://animora.so) — an AI video editor with a real timeline —
so your agent can create projects, plan full videos, generate motion
graphics, add voiceover and word-synced captions, and hand you a link to
review everything in the studio.

The billing difference that matters for agents: **every render is
verified before it charges, and failures auto-refund.** Agents burn
credits on failures no one watches — ours refund themselves.

## Install (two commands + a key)

```bash
claude plugin marketplace add Iamshankhadeep/animora-agent-plugin
claude plugin install animora@animora
```

When prompted, paste your Animora API key — mint one at
[animora.so/developers](https://animora.so/developers) (it starts with
`ak_`; free accounts include 300 credits, no card).

Open a **new** Claude Code session and try:

> Create an Animora project called "Acme CLI launch" and read it back.

If `create_project` and `get_project` both succeed, you're connected.

## Example prompts

- "Plan a 30-second launch video for the product in this repo — hook,
  two feature beats, and a call to action — then add a voiceover."
- "Read my latest Animora project and tighten the timeline: trim the
  second item and add word-synced captions."
- "Generate a title sting that says 'Launch Week' and place it at the
  start of my video."

## What's inside

Eight skills covering the whole workflow: the data model and credit
rules (`animora-basics`), building a launch video end to end
(`create-launch-video`), timeline editing, captions, audio, exporting,
how render verification protects your credits, and the errors you might
see with their fixes.

Agent-readable install instructions live at
[animora.so/claude](https://animora.so/claude).

## License

MIT
