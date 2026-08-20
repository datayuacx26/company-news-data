---
schema_version: "1.0.0"
document_id: "01858f0347a525bbeb6ea8ab2b07e811c26107813ec241f063ea20ddf0e9fc85"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/how-cascade-exploited-an-ai-agent-in-production/"
published_at: "2026-08-13T22:13:09+00:00"
first_seen_at: "2026-08-13T22:41:52.669817+00:00"
fetched_at: "2026-08-13T22:41:54.029937+00:00"
content_hash: "sha256:ee213fa0dbc8260ee1fcd63e7a2ea0318feabc6ac46606561a4ac584f60ce271"
---

# AI vs AI: How Cascade exploited an AI agent in production

We pointed[Cascade](https://escape.tech/blog/introducing-cascade-the-multi-agent-penetration-testing/) , Escape's[AI pentesting engine](https://escape.tech/product/ai-pentesting) , at a production AI agent that a large organization runs internally. The agent had a guardrail built to catch[prompt injection](https://docs.escape.tech/documentation/reference/vulnerabilities/llm_prompt_injection/#description) . Cascade got past it on the second try.


The trick wasn't a smarter payload, just a different pretext, and the agent handed over its entire[system prompt](https://docs.escape.tech/documentation/reference/vulnerabilities/llm_system_prompt_leak/) : the tools it could call, the rules it followed, and the session details attached to the conversation.


That alone gives an attacker what they need to go straight at the agent's tools instead of guessing at them blind.


## The target


The target was an internal AI assistant. Authenticated employees could ask it questions about documents they'd uploaded, and it answered using a mix of retrieval and tool calls. Like most agents shipped this way, it had a guardrail sitting between the user and the model, there to catch prompt injection before it reached the model.


## The bypass


****Severity: high (full system prompt and tool metadata disclosure).**** Requires an authenticated session. The guardrail stopped Cascade exactly once.


Cascade's first move was the obvious one, the move every guardrail is built for: ask directly for the system prompt. That got blocked and logged, exactly as designed.


The second message wasn't a stronger version of the first. It was a different request. Cascade wrapped the same ask in a context the agent had no reason to distrust, something closer to a research or documentation request, phrased the way a colleague or an academic would phrase it. Nothing about it read as an attack.


The agent obliged. Several thousand characters came back: the full list of tools it could reach, the rules for when it was allowed to call each one, the exact citation format it used, and the session identifiers tied to the conversation. It handed all of that over because the request sounded reasonable, not because a technical control broke.


That last point is the one worth dwelling on. The guardrail wasn't beaten by a cleverer string, it was talked out of doing its job, the same way a good pretext gets a helpful employee to read a password over the phone.


Cascade didn't find a vulnerability in the code. It found the sentence that made the agent want to help. No human wrote that sentence. Cascade did, after reading the refusal it got the first time.


## **Reproducing the bypass**


The screenshots below are a reproduction. The engagement is under NDA, so we rebuilt the setup in a lab: same guardrail behavior as the assessment, a synthetic system prompt of comparable length and structure, and a reworded injection.


Cascade treated the refusal as information rather than a dead end.


It kept the objective and changed the register in a new chat.


Synthetic system prompt returned in full


The whole thing came back at once, around 8,200 characters in the real engagement. Knowledge cutoff, tool-calling protocol, the full tool list, the citation format, the session identifier.


## What the leak opens up


A system prompt is the rulebook an attacker would otherwise have to reconstruct one blocked request at a time. With the tool list and the constraints in hand, there's no more guessing about which tools exist or what sets them off - an attacker can go straight at the ones worth targeting. The session details that came with it push things further still, toward tracking or replaying a specific conversation.


## What this says about AI pentesting and the state of security in the next few years


The bypass itself isn't the interesting part. Getting a guardrail to reconsider a blocked request by changing the framing is a known move. **What's newer is how building that bypass looked from the inside: no wordlist, no brute force, just one AI system reading how another one reasons and finding the angle nobody had told it to watch.**


That's the shift worth naming. Defensive agents are built to reason about intent rather than match strings, so testing them credibly now takes an attacker that reasons too, one that treats a rejection as information rather than a dead end. A scanner with a fixed payload list gets a single shot, the one that gets logged and blocked. Cascade got a second shot because it understood why the first one failed.


Throwing known jailbreaks at a chatbot doesn't really describe AI security testing anymore. What's happening now looks more like two reasoning systems working against each other in real time, one holding a line and one probing for where the line actually gives. That isn't a future scenario. It's an ordinary engagement today, and it's where offensive security is headed.


## Fixing it


The fix isn't a better keyword list. A few things help more.


- Semantic input classification looks at what a request is trying to do rather than the specific words it uses.
- Output-side filtering blocks system-prompt and configuration disclosure regardless of how the request got there, so it catches the cases the input side misses.
- Least-privilege prompting keeps anything out of the system prompt that would seriously help an attacker if it leaked, on the assumption that something eventually will.
- And re-testing has to be continuous, because a guardrail that holds today against known phrasing tells you nothing about whether it holds against a reframing no one has tried yet (or a smarter AI model).


## Conclusion


Two things happened in this engagement: a production agent gave up its full configuration, and it did so to another AI that just asked better. Fixing that doesn't take new tooling on the target's side. Catching it does take testing at the same cadence, and with the same kind of reasoning, as the thing trying to get in.


If your security program tests AI agents the way it tests everything else, once a year against a snapshot, it's worth asking what has already slipped past that filter in the months since.


Want to see what Cascade finds on your own AI agents?[Book a demo](https://escape.tech/book-a-demo) and we'll run it against your attack surface.
