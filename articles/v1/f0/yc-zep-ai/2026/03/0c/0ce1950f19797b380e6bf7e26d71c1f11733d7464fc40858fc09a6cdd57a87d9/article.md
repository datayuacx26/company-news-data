---
schema_version: "1.0.0"
document_id: "0ce1950f19797b380e6bf7e26d71c1f11733d7464fc40858fc09a6cdd57a87d9"
company_key: "yc-zep-ai"
company: "Zep AI"
source_id: "yc-zep-ai-rss-0db2565bfdf1"
canonical_url: "https://blog.getzep.com/stop-letting-your-agent-decide-what-it-needs-to-know/"
published_at: "2026-03-25T15:25:16+00:00"
first_seen_at: "2026-07-24T08:02:00.467869+00:00"
fetched_at: "2026-07-28T21:56:52.525405+00:00"
content_hash: "sha256:b1d201694a17988aac9c608e0cbb7d8f617ff68f3b669f975c79551bc416a439"
---

# Stop Letting Your Agent Decide What It Needs to Know

## Key Takeaways


- An agent only calls a tool when something signals the context is there, but the most valuable context (a preference from two months ago, a threshold quietly crossed) leaves no signal in the current message.
- Every tool call is either signal-driven or speculative. A smarter model improves signal-driven retrieval but does nothing for true unknown unknowns, which by definition have no cue to follow.
- Pushing the agent to search broadly just trades the gap for cost: speculative calls add latency and tokens on every turn, and most don't pay off.
- The fix is to stop making retrieval the agent's job. Assemble the right context deterministically every turn, from user identity, domain priorities, and relational graph connections, before the agent generates a token.


Your agent has five tools. One of them —` get_interaction_history` — contains the single most important piece of context for the current request. The agent doesn't call it.


Not because it's broken. Not because the agent is bad at tool selection. Because nothing in the current message gives it any reason to.


Here's the scenario. A customer writes in to your e-commerce support agent:


> "I want to return the shoes I bought."


The agent has access to` lookup_orders` ,` check_return_eligibility` ,` get_loyalty_status` ,` get_interaction_history` , and` get_return_history` . Every fact it needs is one tool call away.


It calls` lookup_orders` . Finds the running shoes, Order #4521, purchased 45 days ago. It calls` check_return_eligibility` . The user is within their return window. It processes the return correctly, sends a shipping label, and closes the ticket.


What it missed: two months ago, in a completely separate conversation, this customer mentioned that this brand's sizing runs narrow for their wide feet. That fact — sitting in` get_interaction_history` — would have transformed the interaction. Instead of a generic return, the agent could have said: *"Last time we chatted you mentioned the narrow sizing — would you like me to suggest a wide-fit option so we get it right this time?"*


But why didn't the agent call` get_interaction_history` ? Put yourself in the agent's position. Look at what it has at this point: the user said "I want to return the shoes I bought." It called` lookup_orders` and got back Order #4521 — running shoes, $129, 45 days ago. It called` check_return_eligibility` and got back "eligible." That's everything in its context. Is there anything here that hints at a conversation about foot width from two months ago? The order details don't mention it. The eligibility check doesn't mention it. The user's message doesn't mention it. There is simply no thread to pull on.


More generally, there are exactly two reasons an agent would ever call a tool like` get_interaction_history` .


The first: the agent has some reason to believe that relevant context is there. Something in its context — the conversation, prior tool results, the user's history — gives it a signal worth pursuing. But as we just saw, nothing in this scenario signals a two-month-old conversation about foot width. There's no breadcrumb. The agent would have to already know the fact exists in order to suspect it's worth looking for — and if it already knew, it wouldn't need to look.


The second: the agent has no particular reason to believe anything is there, but searches anyway — out of an abundance of caution, a blanket instruction, or a general heuristic to check broadly. This does surface the fact. But notice what happened: the tool call was completely disconnected from the user's query. It wasn't a targeted retrieval. It was a speculative sweep, hoping to find something that justifies its cost.


These are the only two possibilities. And unknown unknowns, by definition, can only be reached by the second — because the first requires a signal that doesn't exist.


## Smarter Models Don't Fix This


The most common response to "my agent misses context" is to wait for the next model. GPT-8 will be smarter. Claude 7 will reason better. The problem will go away.


It won't. A smarter model might be better at signal-driven retrieval — better at picking up on subtle cues that relevant context exists, better at connecting dots across prior tool results. That's real progress. But no matter how smart the model gets, the binary still holds. Every tool call is either signal-driven or speculative. And for true unknown unknowns — context where there is genuinely no signal — a smarter model has exactly the same two options as a dumb one: find it by speculative search, or miss it entirely.


Model intelligence helps with Reason 1. It does nothing for Reason 2. And the hardest, most valuable context — the stuff that transforms a generic response into a great one — lives in Reason 2 territory.


## The Tool Call Tradeoff


If the agent can't reliably decide which tools to call on its own, the natural instinct is to nudge it toward calling more. There's a spectrum of strategies here, and teams typically land somewhere along it:


**Strategy 1: Let the agent call only what it thinks it needs.** This is the default. The agent reads the message, picks the tools that seem relevant, and calls them. It's fast and cheap. But as we've seen, it misses the context that isn't signaled by the current message — the wide-feet fact, the loyalty tier, the return history approaching a threshold.


**Strategy 2: Prompt the agent to be thorough.** You add a general instruction: "Before responding, always make sure you have the relevant context by searching broadly with your tools." This pushes the agent toward more speculative searches — calling tools not because something signals they're needed, but just to be safe. It catches some unknown unknowns that Strategy 1 would miss. But it adds latency, token cost, and cognitive load on the agent — and most of these speculative searches won't pay off on any given turn.


**Strategy 3: Have the agent call everything every time.** The extreme version: "Call every available tool before responding, on every turn." No team actually does this — but it illustrates where the spectrum leads. Maximum speculative search does surface the unknown unknowns. But you're paying for all of it on every turn: the latency, the tokens for usually-irrelevant results, and the dilution of the agent's attention across a bloated context window.


The spectrum is really a tradeoff between cost and coverage. On the left, you're fast and cheap but blind to unknown unknowns. On the right, you surface them — but at unsustainable cost on every turn, for context that's usually irrelevant. And even at maximum coverage, you're dependent on the LLM parsing a massive context window without getting confused or losing focus.


## What If the Agent Didn't Have to Decide?


The entire problem comes down to signals. The agent can only retrieve context it has a signal for — and the most valuable context often has no signal in the current message. Speculative search can fill the gap, but at unsustainable cost.


So what would the ideal system look like? It would utilize *every available signal* to retrieve relevant context, and deliver that context automatically on every agent turn — without the agent making a single tool call. This also eliminates the latency that tool calls bring, and removes the dependency on the LLM to correctly select tools.


Think about what signals are actually available beyond the current message:


- **The user's identity is a signal.** You're not talking to a random person — you're talking to *this* specific user, with a history, preferences, and patterns. A persistent user summary — their loyalty tier, communication style, account history — should travel with every interaction, regardless of what the current message is about. This is context that's almost always relevant and costs nothing to include.
- **The domain the agent operates in is a signal.** In e-commerce, return frequency matters. In healthcare, medication allergies matter. In financial services, risk tolerance matters. The system should be biased toward surfacing what's important in *your* specific business, even when the current message doesn't ask about it. Domain knowledge lets you prioritize what context is retrieved and how it's weighted.
- **Relational connections between facts are signals.** In a previous conversation about workout gear, this user mentioned they're training for a half marathon. That fact has virtually no semantic overlap with "I want to return the shoes I bought" — you wouldn't find it via embedding similarity search. But the relational path is clear: marathon training → running → running shoes → this order. It tells the agent this is an active runner who will need replacement shoes, turning a return into a retention opportunity. Following relational connections between facts surfaces context that text similarity alone would never find.


And when the system does return a fact, it should return **the current truth, with the history behind it.** If a user said they love Brand X six months ago and said they're done with Brand X last week, the agent needs the resolved, current fact — with temporal context showing how it changed — not just whichever version happened to rank higher in a vector search.


💡


But don't tool calls give you iterative discovery?


With tool calls, the agent can chain retrievals: call one tool, learn something, use that to inform the next call. Doesn't removing tool calls from the equation lose that iterative discovery? Not if the system preconnects related context before retrieval time. Information that would be spread across multiple rounds of tool calls — order details linked to sizing complaints linked to brand preferences linked to return patterns — can be pre-identified and preconnected, so it arrives as one coherent block rather than requiring the agent to piece it together across several round trips.


The result: in our shoe return scenario, the agent receives the order details, the loyalty status, the wide-feet conversation from two months ago, the half-marathon training context, the return count approaching a review threshold, and the recent frustration from a shipping delay — all before it generates a single token. It doesn't need to decide to look for any of this. The system already did.


*This is exactly what*[Zep](https://www.getzep.com/?ref=blog.getzep.com) *does. Zep stores context in a*[context graph](https://help.getzep.com/graph-overview?ref=blog.getzep.com) *that preconnects disparate sources — conversations, business data, user behavior — into a unified structure. On every agent turn, it automatically assembles the most relevant context from that graph, combining*[always-on user summaries](https://help.getzep.com/user-summary-instructions?ref=blog.getzep.com#utilizing-user-summary) *,*[domain-customized retrieval](https://help.getzep.com/customizing-context?ref=blog.getzep.com) *,*[graph-based relational search](https://help.getzep.com/searching-the-graph?ref=blog.getzep.com#breadth-first-search-bfs) *, and*[temporally resolved facts](https://help.getzep.com/facts?ref=blog.getzep.com#how-zep-updates-facts) *— so the right context is always there, even the context nobody thought to ask for.*
