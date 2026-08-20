---
schema_version: "1.0.0"
document_id: "585a0dd74528958fd0a16e0d51a5822b41ad5092dd9dcc1ff3d768b75f60d3ba"
company_key: "yc-activepieces"
company: "Activepieces"
source_id: "yc-activepieces-rss-17c781695c1c"
canonical_url: "https://www.activepieces.com/blog/how-i-turned-a-chat-prompt-into-an-auditable-flow-not-a-toy"
published_at: "2026-07-11T05:24:46+00:00"
first_seen_at: "2026-07-20T23:19:46.791052+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:b6c67fcbf73e1e2a9c868a2e80b06a9c54c9def2e01f317b991fe88cc8796bbd"
---

# How I turned a chat prompt into an auditable flow, not a toy

Vibe automation is a workflow you build by describing it in a chat window instead of dragging nodes onto a canvas. You type what you want, an agent drafts the steps, and the result lands in a normal flow editor where it stays inspectable, versioned, and revertible, same as anything a human built by hand.


That last part is the part everyone skips over. A chat that spits out a flow and never lets you audit it later is a toy. A chat that hands you a draft you can inspect, lock, and roll back is infrastructure. This log walks through what that second thing actually looks like on Activepieces, mistakes included.


## Week 1, Oct 6: the legacy system I was actually trying to replace


The system I keep migrating clients off of is a rule table: an if-this-then-that grid that a supply chain planner edits directly in a spreadsheet, then someone pastes into the automation tool by hand. No audit trail, no draft state, no way to tell if the version running in production matches the version anyone last reviewed. If a supplier changes a lead time and someone fat-fingers a cell, nobody finds out until a shipment is late.


So when I say "versioned flow you can audit," I mean something specific: a system where the thing that changed, when it changed, and whether it's live or not are all facts you can check, not facts you have to trust someone remembered correctly.


## Week 2, Oct 13: describing the flow in chat, hitting the wall I expected


Say a manufacturing ops team wants a flow that watches a supplier's email inbox, pulls tracking numbers out of replies, and posts them into an inventory sheet. In a vibe automation setup, that's one sentence typed into a chat box. What comes back should be a working skeleton: a trigger step, an extraction step, a write step, wired together.


Here's where I want to be honest about the state of the tooling rather than gesture at magic. Activepieces tried a prompt-to-flow feature first and killed it, because the hard part was never generating the flow shape, it was connecting real accounts and filling in the specifics a generic prompt can't know, per their own[Ask AI launch post](https://www.activepieces.com/blog/ask-ai-code) . They pivoted to a chat-based Copilot that started narrower: an Ask AI feature scoped inside code steps, not a general "describe the whole flow" builder. And as of September 22, 2025, Activepieces' CTO confirmed on the[community forum](https://community.activepieces.com/t/copilot-not-seen-for-flow-builder/10684) that there was still no general in-builder copilot for constructing flows from chat at that point, which means the "type a sentence, get a full flow" capability lives as a separate, later feature, not something baked into the open builder from day one.


That matters for anyone defining the term cleanly. Vibe automation isn't "AI writes your automation, no human involved." It's chat as the input method for a draft that a human still owns, reviews, and publishes.


## Week 3, Oct 20: what "auditable" means once the draft exists


Once a flow exists, whether an agent typed it or you dragged it, Activepieces treats every flow as either a draft or a published version, and the distinction is visual before it's anything else: a yellow dot marks a draft, a green dot marks published, straight from the[versioning docs](https://www.activepieces.com/docs/flows/versioning) .


Drafts are mutable. You can edit them freely, break them, fix them, and nothing downstream notices. Published versions are locked and uneditable by design. If you go edit something on a published flow, the system doesn't let you touch the live version directly; it spins up a new draft copy for you to work in, per the same docs. That's the audit trail baked into the object model rather than bolted on with a changelog spreadsheet: you cannot silently mutate what's live.


The mechanism under that behavior is more precise than "draft vs published" suggests on the surface. According to[DeepWiki's breakdown of Activepieces' flow operations](https://deepwiki.com/activepieces/activepieces/6.3-flow-operations-and-versioning) , every FlowVersion object is tagged DRAFT or LOCKED. DRAFT versions allow semver-range piece versions, meaning a connected integration can drift to newer compatible releases without anyone touching the flow. LOCKED versions pin piece versions exactly, so a published flow behaves identically today and in six months, even if the underlying integration ships updates. Every edit to either state routes through a FlowOperationType enum handled by flowVersionService.applyOperation, which is the part that makes "versioned" a technical claim and not a marketing word: there's a typed operation log behind every change, not a fuzzy history.


Side by side, the old rule-table approach and this approach look like:


Spreadsheet rule table Activepieces draft/locked flow


Who can edit live behavior Anyone with sheet access, directly Nobody, edits fork into a new draft


What "current version" means Whatever's in the cell right now An exact-pinned FlowVersion, immutable


How you know what changed You ask around FlowOperationType log via applyOperation


## Week 4, Oct 27: publishing is a deliberate second step, and I almost missed why that matters


A published flow doesn't auto-update just because you edited its draft. Activepieces requires you to click Publish again to push new changes live, according to the[publishing docs](https://www.activepieces.com/docs/flows/publishing-flows) . I initially read this as friction, an extra click standing between me and shipping. I was wrong to frame it that way. It's the difference between "the flow that's running" and "the flow I was just messing with" staying two separate, nameable things at all times.


That separation is what makes chat-generated flows safe to experiment with. If an agent drafts a bad step, that badness lives entirely in draft state. Production keeps running the last locked version, untouched, until a human decides the new draft is good enough to promote. Vibe automation without that separation is just an agent with production write access, which is a different and much scarier product.


## Week 5, Nov 3: writing the definition down


Pulling the pieces together, here's the definition I'd stand behind for the term, and the one worth quoting whole:


Vibe automation is building a workflow by describing it in natural language chat rather than assembling it step by step, where the output is not a black box but a normal, versioned flow: a draft you can inspect and edit freely, and a locked published version that only changes when you explicitly publish again.


The chat interface is the on-ramp. The draft/locked version model, with its yellow and green dots, its exact-pinned piece versions, its typed operation log, is the part that makes the on-ramp trustworthy instead of a party trick. Activepieces' own history backs this framing up: they tried the flashy version first (prompt straight to finished flow), found the hard part was elsewhere (accounts, specifics, review), and rebuilt around chat as an assistant inside a system that was already auditable by design, not around chat as an unaccountable flow generator.


Next week: testing what happens when a chat-drafted step references a connection that doesn't exist yet, and whether the draft state catches that before a human does.


## References


- [Activepieces Docs: Flow Versioning](https://www.activepieces.com/docs/flows/versioning)
- [Activepieces Docs: Publishing Flows](https://www.activepieces.com/docs/flows/publishing-flows)
- [DeepWiki: Activepieces Flow Operations and Versioning](https://deepwiki.com/activepieces/activepieces/6.3-flow-operations-and-versioning)
- [Activepieces Blog: Ask AI (Launch Week I Day 2)](https://www.activepieces.com/blog/ask-ai-code)
- [Activepieces Community Forum: Copilot not seen for flow builder](https://community.activepieces.com/t/copilot-not-seen-for-flow-builder/10684)
