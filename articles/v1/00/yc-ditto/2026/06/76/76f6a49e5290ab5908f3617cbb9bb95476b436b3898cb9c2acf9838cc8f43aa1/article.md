---
schema_version: "1.0.0"
document_id: "76f6a49e5290ab5908f3617cbb9bb95476b436b3898cb9c2acf9838cc8f43aa1"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/introducing-ditto-specs-integrate-your-style-guide-directly-into-your-design-system-in-code"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:4f8ce483cbf7f4aa3f96c76f1cb7bd7521133f9518cb46c41e2de23b1c3270fb"
---

# Ditto Blog | Introducing Ditto Specs: Integrate your style guide directly into your design system in code

Every team using AI to write product copy has run into the same wall.


You set up your style guide. You write good prompts. You get decent output. But the copy your agent writes on Monday doesn't quite match what it writes on Thursday. A teammate runs the same workflow and gets something different again. Over time, the inconsistencies pile up — not because the rules changed, but because agents don't apply them reliably.


This isn't a prompting problem, it's a structural one.


### The root cause


When an AI agent writes copy in a component, it's working with whatever is in its immediate context. It doesn't go looking for what rules should apply. It doesn't check how similar components were written. It produces something plausible and keeps going.


You can try to work around this. Some teams dump a style guide into a system prompt, or put a` STYLEGUIDE.md` in the repo root, or wire up an MCP server and hope the agent decides to call it. These approaches share the same fundamental problem: the rules and the code live in different places, and agents have to be told to bridge that gap every single time. When they try, they tend to grab the first relevant rule they find, apply it broadly, and stop. They're not distinguishing between a rule meant for CTAs and one meant for body copy. They're not checking whether they've covered every surface.


The longer your style guide gets, the worse this gets. Rules buried in a long context get ignored. Rules scoped to one surface bleed into another. And none of it holds across teammates, or runs, or time.


What teams actually need isn't a better prompt. It's the rules *where the code is* , scoped to the surfaces that need them — automatic context that agents pick up without being told to look.


### Introducing Ditto Specs


Ditto Specs are` .ditto.md` files that live in your codebase alongside your components. Each one declares the **text surfaces** a component renders — the specific pieces of user-facing copy like a headline, a button label, helper text, a tooltip — and carries the style guide rules that apply to those surfaces, synced from the Ditto platform.


` DialogueModal/`


` index.tsx`


` index.module.css`


` index.stories.tsx`


` index.test.tsx`


` index.ditto.md ← this is new`


When an agent edits copy in` DialogueModal` , the adjacent` .ditto.md` is already in its working context — the same way the CSS module or test file would be. No tool call. No prompt engineering. No hoping the agent decides to go look something up. The rules are just there, scoped to the surfaces that need them.


Specs work at every level of your codebase. A` workspace.ditto.md` at the repo root sets the floor: universal voice, tone, and brand rules that apply everywhere. Component-level specs layer on specifics. An agent working on a deeply nested component sees the whole stack, and the right rules for every surface it's touching.


### How it works


**1. Initialize**


Run` ditto-spec init` to create a config file and a` workspace.ditto.md` with your universal rules — the guardrails that should apply to every piece of copy in the repo.


**2. Scaffold**


Run` ditto-spec scaffold ComponentName --path src/components/ComponentName` to generate a spec file next to any component, with the right structure ready to fill in.


**3. Declare surfaces**


Edit the spec to add one entry per piece of user-facing text the component renders. Tag each surface by its semantic role:` actionText` tagged` \[call-to-action\]` ,` headline` tagged` \[heading, dialog-title\]` , and so on.


**4. Sync rules**


Run` ditto-spec pull` . The CLI reads your tags, matches them against your Ditto style guide, and writes the relevant rules into the managed section of each file. When rules change on the platform, re-run pull and every spec in the repo updates automatically.


**5. Commit and ship**


Spec files version with the component, travel with it if it moves, and show up as normal diffs in PRs when rules change. Your whole team is working from the same rules, all the time.


Once Ditto Specs are set up, the humans and agents working in your codebase will be able to leverage them every time, no extra effort or configuration required. From now on, you’ll be able to trust that the right rules are being referenced for the right copy, every time.


### Batteries-included agent skills


Specs are passive context by design — but the CLI also ships three interactive agent skills for Claude Code and Cursor that handle the workflows that benefit most from a human in the loop.


**` /ditto-spec-component`** — Point it at a component and it analyzes the code, identifies every text surface, suggests tags from your Ditto workspace's tag inventory, scaffolds the spec, and syncs rules. It pauses for your review before creating anything.


**` /ditto-spec-audit`** — Audits actual copy in component instances against the rules in their specs. Resolves inline strings, i18n keys, and Ditto text items, then reports violations with suggested corrections. Read-only — it never changes your code (unless you tell it to fix the issues).


**` /ditto-spec-gaps`** — Finds patterns in your copy that should be rules but aren't: terminology inconsistencies, tone mismatches, conventions that have emerged organically but never been formalized. Proposes new rules, lets you review, then creates them on the platform and syncs them into your specs.


Run` ditto-spec init --agent` to install all three. They're committed to the repo as plain markdown, so every teammate gets them automatically.


### Why not just a[styleguide.md](http://styleguide.md/) ?


It's a fair question — teams already do this, and it's better than nothing. But it breaks down in a few predictable ways.


**A single file gives every component every rule.** The more rules you have, the more noise an agent has to work through to find what's actually relevant right now. Ditto specs bring only the rules that apply to a given surface into context at the moment the copy is being written — everything else stays out of the way.


**Static files rot.** A homegrown` STYLEGUIDE.md` gets written once and slowly drifts out of date. When rules evolve, someone has to remember to update it — and then update it everywhere. Ditto specs are synced. Run` pull` and every spec in the repo reflects whatever's current on the platform.


**Freeform markdown isn't reliably actionable.** Prose style guidelines are legible to humans, but agents interpret them inconsistently. Ditto specs have a defined structure — surfaces, tags, rules, examples — that the CLI, the platform, and agents all operate on the same way, every time.


### What you unlock in beta


Ditto Specs are in beta now. Teams joining early get:


- The full CLI:` init` ,` scaffold` ,` pull` ,` check` ,` list`
- Workspace-level and component-level specs
- Tag-based rule syncing from the Ditto platform
- All three agent skills for Claude Code and Cursor
- Direct support from our team as you set things up


If your team is building with AI-assisted workflows and copy consistency is a real problem — whether that's inside a design system, across a product surface, or just keeping a distributed team aligned — this is exactly what Specs were built for.


[Book a beta kickoff call →](https://calendly.com/d/ds55-452-gwg/ai-content-systems-beta-intro-call)


We'll walk through your setup, get your first specs running, and make sure the rules you care about are actually being followed.
