# Sales Agents

A small collection of standalone AI agent definitions that help with Benji's sales and outreach work.

Each file under `agents/` is a self-contained agent definition (YAML frontmatter + instructions) that can be run independently — these are not part of the main engineering pipeline (`jpmoya/claude-agents`).

This is a first-pass roster, scoped for a small company (not a sprawling multi-agent org): a handful of agents, each with one clear job, covering cold outbound (email/LinkedIn/calls), prospecting/lead sourcing, and inbound lead handling. Most agents draft only, and a human reviews and executes the risky step. The deliberate exception is the two writer agents (JP, 2026-10-30): `outbound-sales-writer` and `partnership-outbound-writer` each send the personalized 1:1 emails they write, directly, that's the point of them, bounded by two hard rules on both, never to a generic/role inbox, always personalized to the named individual. Every other send/connect/dial (LinkedIn, calls, anything else) still needs a human. JP (CEO) can edit, reorder, or veto any of these.

## Two audiences, two writers, kept separate on purpose

`outbound-sales-writer` (end-customer/tenant sales: competitor takeover, switch pitches, pricing/guarantee language) and `partnership-outbound-writer` (B2B property-management/landlord partnerships: relationship-building, never "preferred vendor," never a tenant-retention ask) write structurally different, sometimes contradictory emails to different audiences. They are kept as two separate agents rather than merged into one with runtime judgment, because the two framings actively conflict (e.g. "switch providers" language would be nonsensical and damaging sent to a landlord; "build a relationship across your portfolio" language would be weak and off-target sent to a tenant prospect) and the cost of a misclassification is an email that actively contradicts itself in front of a real prospect, not just a wrong tone.

**Routing is deterministic, decided upstream, never inferred by a writer at send time.** Each contact is already filed in a specific Copper pipeline before any writer runs: tenant/end-customer prospects live in a sales pipeline (e.g. `Vancouver Outbound`), partnership targets live in `Preferred Vendor Outreach` (pipeline_id `1147485`). `lead-sourcing-researcher` or whoever builds the target list makes that call once, up front, by what the contact actually is (a tenant/office-space decision-maker vs. a property/facility manager or landlord). The pipeline a contact sits in is the routing signal: `outbound-sequencer` + `outbound-sales-writer` handle anything in a sales pipeline, `partnership-outbound-dispatcher` + `partnership-outbound-writer` handle anything in `Preferred Vendor Outreach`. Neither writer should ever be asked to guess which audience a contact belongs to from the email content alone, that guess is made once, explicitly, when the contact is first filed, and both writer agents' Step 1 CRM check already confirms the pipeline/stage they're operating in before writing a word.

## Shared references

- **`reference/jp-voice-profile.md`** — JP's writing voice (tone, length, greeting, sign-off, real client names), built from real sent emails. Every agent that writes as JP points here instead of duplicating it.

## Agents

- **`agents/lead-sourcing-researcher.md`** — builds and enriches target-account/contact lists from an ICP, geography, or trigger event; research only, no outreach.
- **`agents/outbound-sales-writer.md`** — checks Copper CRM (read-only) to confirm a prospect isn't already a customer or already mid-outreach, then writes AND sends a personalized cold/warm outbound sales email in JP's voice directly to the named individual, for end-customer/tenant prospects. Has real send authority, never to a generic inbox.
- **`agents/outbound-sequencer.md`** — plans the multi-touch cadence (email/LinkedIn/call timing) across a prospect batch and tracks where each prospect sits in it; hands each due email touch to `outbound-sales-writer`.
- **`agents/inbound-lead-qualifier.md`** — triages a new inbound lead, scores fit (Hot/Warm/Cold), and drafts the qualifying reply and routing recommendation.
- **`agents/call-prep-assistant.md`** — preps a short brief/talk track before a human-placed sales call and logs the outcome after.
- **`agents/reply-and-meeting-scheduler.md`** — drafts replies to prospect responses and proposes meeting/site-visit time options.
- **`agents/partnership-outbound-writer.md`** — forked from `outbound-sales-writer` for B2B partnership outreach to property management companies/facility leadership (not tenant/end-customer sales); generic partnership framing, never "preferred vendor," never a named-tenant retention ask. Also has real send authority, never to a generic inbox.
- **`agents/partnership-outbound-dispatcher.md`** — forked from `outbound-sequencer` for the same partnership campaigns; plans the email + LinkedIn slow-connect + daily-follow-up cadence per target company and owns keeping Copper's Preferred Vendor Outreach pipeline (Benji's partnerships pipeline) current as standard practice; hands each due email touch to `partnership-outbound-writer`.
