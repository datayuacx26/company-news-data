---
schema_version: "1.0.0"
document_id: "b82716ea76128fabd36032680aa009b73ef5d464bc9f4b661b3031ded002dda4"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/style-guides-for-humans-and-agents"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T22:17:13.778547+00:00"
content_hash: "sha256:b26d49620b9610fc881392336070777bbe07b04125dc4031a6fac675dc2adb49"
---

# Ditto Blog | How to build a style guide that works for for humans AND agents

Style guides have always been about scaling consistency and voice across people.


Now that AI agents are drafting and shipping real product text, style guides are also the primary interface for instructing those agents.


Style guides now serve two very different “readers” – one who builds their intuition, and one who follows instructions literally. So how do we create style guides that work for both humans AND agents?


## Content and information principles


Some things remain the same. Style guides must be concrete enough to act on, yet abstract enough to cover novel situations. Principles and rules work together when you can’t anticipate or codify every scenario.


### Big picture to nitty gritty details


Most style guides benefit from a high-level structure along these lines:


- **Foundations** - who you are, who you’re talking to, the “why” behind everything
- **Brand voice and tone** - personality attributes, tone shifts by scenario, emotional range
- **UI-specific patterns** - button labels, empty states, error messages, notifications, all the recurring surfaces where product text appears
- **Style & mechanics** - the closest thing to explicit rules: capitalization, punctuation, formatting conventions, dates
- **Word and terminology lists** - preferred terms, deprecated terms, product- or brand-specific vocabulary


Ditto's example style guide includes a set of sections we think generalize well.


Agents and humans navigate this structure differently. A human might internalize the foundations and then spend most of their time referencing the UI patterns and mechanical rules. An agent re-reads every layer from scratch each session, meaning gaps in your foundations will be reflected in every piece of text it generates.


## Approach style guides with a teaching mindset


Rather than thinking about style guides as a form of documentation, think about it as teaching material.


**Don’t just say what to do, say why it matters**


“Use sentence case” is a rule. “Use sentence case, because title case feels formal and creates visual noise in a product that’s trying to feel approachable” is a lesson.


Reasoning helps a reader (human or agent) make the right call in situations the rule doesn’t anticipate. Without the “why,” you get theoretically compliant copy that misses the point, creating more work for all the humans in the review loop.


**Capture examples that reflect on what works and what doesn’t**


Before and after pairs with annotations to explain the distinctions.


Annotations often matter more than the examples, because they surface contextual criteria.


Explain specifically why something that’s wrong fails the principle; explain what makes something work so that reasoning is transferable.


Example rules written with a teaching mindset, from the Ditto default style guide.


**For humans: internalization over time**


Humans read the style guide, apply it, get it wrong from time to time, and build their intuition.


A teaching mindset helps this because it’s intentional about building understanding and reasoning rather than just memorizing rules.


Eventually, humans can extrapolate what they’ve learned to new situations, which is the whole point of a guide intended to scale consistency and voice across people.


**For agents: surfacing implicit knowledge**


Agents don’t build their intuition across sessions. Each session starts fresh (even if referencing the same artifacts).


The value of a teaching approach for agents isn’t about teaching the agent, but in forcing *you* to articulate things otherwise left implicit and unsaid. Experienced content designers and writers apply a deep well of tacit knowledge: “we’d never say it this way,” or “there’s a better best practice for UI copy in this context.”


A teaching mindset helps draw out that tacit knowledge. If you can’t explain it, the agent can’t reliably infer it.


## With humans, assume judgement; with agents, assume (often misplaced) confidence


Humans reading a style guide bring their own interpretation and subjectivity to when a rule applies or doesn’t.


Agents bring certainty: they will apply guidance confidently whether or not it fits the situation.


**Contradictions and context hierarchies**


Style guides always have internal tensions: “Be concise” vs. “Be conversational.”


Humans can navigate these contradictions intuitively. Agents need an explicit context hierarchy – which principles and rules win when two conflict?


For example: “Be concise” and “Be conversational” will conflict in an empty state message. A concise version might say “No results found.” A conversational version might say “We couldn’t find any matches. Try adjusting your filters.” A human writer applies their judgement. An agent needs you to specify: in empty states, conversational wins over concise, because these are moments where the user might feel stuck and a warmer tone can reduce frustration.


Ditto’s Magic Draft builds on a layered context hierarchy that helps the agent decide which rules wins out based on specific situational context.


Without a hierarchy, an agent is likely to pick rules arbitrarily or try and apply it all at once.


**Gotchas: Types of rules that are safe for humans but dangerous for agents**


“Make the next step obvious”


- A human writer: looks at the actual flow, identifies the real next step, writes towards it.
- Agent: Will confidently fabricate a plausible next step if it doesn’t have specific flow context.
- This rule assumes knowledge of the product journey; without that, it’s an instruction to hallucinate helpfully.


“Focus on user benefits over product features”


- A human writer: Translates known features into known benefits based on accumulated user understanding.
- Agent: If it lacks specific, current product context, it will invent reasonable-sounding benefits that may not be real or not meaningful.
- This rule assumes grounding in actual product truth; without that, it’s an instruction to generate marketing fiction.


Terminology swaps that look simple but aren’t: “Use X instead of Y”


- While these feel like safe, mechanic rules, they’re almost always context-dependent outside of pure brand terminology.
- “Close” instead of “cancel” – works for dismissing a modal, wrong when the user is actually canceling an in-progress action with consequences.
- “Edit” instead of “manage” – works for direct manipulation of a single thing, but wrong for a UI surface for managing multiple things.
- “Done” instead of “back” – works when a task is complete, wrong when the user’s navigating without having finished anything.
- Humans will apply the swap when it makes sense and ignore it when it doesn’t. The more they’ve learned from the style guide, the more unconsciously they’ll do this. The agent will apply it everywhere, because the rule said to.
- A better approach is to include the semantic distinction and the contexts where each term is appropriate.


## Takeaways


Your style guide has always been infrastructure: part of the system that scales consistency and voice across people. Now it has a second audience.


The good news is that the work is the same in both directions. When you explain why a rule exists, humans internalize it faster and agents apply it more reliably. When you spell out contexts for when terms apply or don’t, it’s not dumbing it down, but rather surfacing knowledge that was always there, just not written down.


Start by auditing for assumed-judgement risks: rules that are safe for humans with their judgement, but dangerous for an agent that will apply them literally and confidently. These are the highest-leverage changes, because they’re places where implicit knowledge is doing the most work.


And like any infrastructure, integration and maintenance matters. Your product will grow and change, but your agents will keep following the old guidance and outdated[styleguides.md](http://styleguides.md/) with full confidence. Build review cycles into the system to keep the whole system trustworthy as the work (and the mistakes) grow.


We've been thinking about all of this while building Ditto's latest style guide updates — custom sections, layered context, better editorial presentation, full integration with Ditto’s MCP.
