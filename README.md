# Sales Agents

A small collection of standalone AI agent definitions that help with Benji's sales and outreach work.

Each file under `agents/` is a self-contained agent definition (YAML frontmatter + instructions) that can be run independently — these are not part of the main engineering pipeline (`jpmoya/claude-agents`).

This is a first-pass roster, scoped for a small company (not a sprawling multi-agent org): a handful of agents, each with one clear job, covering cold outbound (email/LinkedIn/calls), prospecting/lead sourcing, and inbound lead handling. Every agent that drafts a message or proposes an action stops short of sending/booking/calling — a human reviews and executes the risky step, same gating pattern as JP's other pipelines. JP (CEO) can edit, reorder, or veto any of these.

## Shared references

- **`reference/jp-voice-profile.md`** — JP's writing voice (tone, length, greeting, sign-off, real client names), built from real sent emails. Every agent that writes as JP points here instead of duplicating it.

## Agents

- **`agents/lead-sourcing-researcher.md`** — builds and enriches target-account/contact lists from an ICP, geography, or trigger event; research only, no outreach.
- **`agents/outbound-sales-writer.md`** — checks Copper CRM (read-only) to confirm a prospect isn't already a customer or already mid-outreach, then writes on-brand cold/warm outbound sales emails for Benji's, in JP's voice.
- **`agents/outbound-sequencer.md`** — plans the multi-touch cadence (email/LinkedIn/call timing) across a prospect batch and tracks where each prospect sits in it.
- **`agents/inbound-lead-qualifier.md`** — triages a new inbound lead, scores fit (Hot/Warm/Cold), and drafts the qualifying reply and routing recommendation.
- **`agents/call-prep-assistant.md`** — preps a short brief/talk track before a human-placed sales call and logs the outcome after.
- **`agents/reply-and-meeting-scheduler.md`** — drafts replies to prospect responses and proposes meeting/site-visit time options.
