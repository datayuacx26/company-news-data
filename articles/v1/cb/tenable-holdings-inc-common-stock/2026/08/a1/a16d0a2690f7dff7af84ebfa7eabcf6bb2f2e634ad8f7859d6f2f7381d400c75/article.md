---
schema_version: "1.0.0"
document_id: "a16d0a2690f7dff7af84ebfa7eabcf6bb2f2e634ad8f7859d6f2f7381d400c75"
company_key: "tenable-holdings-inc-common-stock"
company: "Tenable Holdings Inc."
source_id: "tenable-holdings-inc-common-stock-news-import-a6b69c49a265"
canonical_url: "https://www.tenable.com/blog/testing-claude-mythos-preview-for-code-security-tenable"
published_at: "2026-08-03T10:00:00+00:00"
first_seen_at: "2026-08-03T10:50:54.720546+00:00"
fetched_at: "2026-08-03T11:29:23.074207+00:00"
content_hash: "sha256:794bf5e5c310f91719da95c6f57892b09bbf5701112b9a19ba313baf7d1265a8"
---

# How Claude Mythos Preview is changing code security at Tenable

Tenable spent 30 days running frontier AI models against our own code. It didn’t just find bugs — it proved they’re real, with reproducible exploits. That fundamentally changes code security from ranking potential code defects to a much higher signal focused on the findings that matter. Read on to learn how it reshaped our security team's work, what it cost, and why your program is next.


### **Key takeaways:**


1. **Now code security starts with proof, not suspicions.** Frontier AI instantly builds working exploits and proves which flaws are genuinely dangerous in your source code. Now remediations are confirmed issues, not just ranked lists of maybes.


2. **The durable asset is the harness, not the model.** Frontier AI models get the attention, but the durable asset for security teams is the harness: the orchestration and systems around the model that turn suspected flaws into proven, reproducible exploits engineers can act on.


3. **Frontier AI doesn’t replace senior researchers; it makes one as productive as five.** The scarce resource is still the expert who writes the threat model and judges what’s real. Buy the compute without funding that person, and you get a very fast way to generate findings no one can use.


We’ve been running[Claude Mythos Preview](https://www.tenable.com/cybersecurity-guide/principles/claude-mythos-preview) against our own code now for over 30 days, and one thing is crystal clear: Code security is fundamentally changing, and we believe there’s no turning back.


At Tenable, our security team already had security testing agents that drove our applications, exercised API endpoints, and ran our predefined checks. But until recently, the agents couldn’t handle the harder part of code testing: finding previously unidentified flaws and proving their exploitability. As part of our work testing Anthropic’s Claude Mythos Preview for[Project Glasswing](https://www.tenable.com/blog/anthropic-claude-mythos-tenable-joins-project-glasswing) , we built an agentic code security harness and powered it with this Anthropic frontier LLM, running it against our code and service repositories with pinned commits to ensure reproducible results.


The work of code security is changing, but not in the way hype-driven blogs suggest. And certainly not for free. We’ve found the costs are measured in two currencies: dollars and senior-engineer hours. What follows is an account from the security practitioner’s perspective of Tenable’s internal security team and how we leveraged frontier AI: where the model proved its value, where it fell short, and what you should consider before investing further.


## From ranking guesses to ranking proof with frontier AI


For well over a decade, the scarce resource for security teams was analyst attention. We built a whole discipline around it — reachability heuristics, exploitability guesswork, etc. — all to decide what a human security analyst should look at first in your own code.


Now a frontier AI model in a harness collapses that. The ranking doesn’t go away, it gets a proof instead of a guess. When a finding arrives with a working exploit, you’re no longer ranking by how likely it is to matter; you’re ranking by what you’ve already proven does.


When an LLM can surface a suspected flaw and drive a working exploit against a running build, the difficult question is no longer “which among the thousands of static findings deserves a human first?” It becomes “which of the code defects are real, and can we prove it?” A bug that shows up with a reproducible proof-of-concept sorts itself. One that can’t be reproduced goes to a validation queue.


This does not mean fewer bugs. In fact, it means many more findings, especially early on, because the model surfaces threats and exposures in your source code that traditional tooling never would. Instead, what changes with frontier AI is that practically all of the bugs that reach a human analyst arrive pre-sorted by proof instead of by score. It becomes a short list of things you can already reproduce, plus a holding pen of candidates the harness is still chewing on.


## The model gets the headlines; the harness does the work


This is not to say that the model doesn’t matter. It does, very much so. Claude Mythos Preview reads unfamiliar code and reasons about code abuse better than anything we’ve seen before. The harness acts as its orchestrator, guiding the model from finding to proving.


We designed our harness to operate in a loop. The threat model feeds the harness context. It scopes a static pass to specific abuse cases in specific code paths. It drives targeted exploitation against a running build. It feeds the result back to confirm exploitability or kill a false positive. That live exploitation step is the furthest “shift left” we’ve dragged dynamic testing to date. This means the model doesn’t guess whether an injection fires; it fires at real targets and actively reads what comes back.


Here’s the part that should decide where you spend your engineering time. The model underneath will change: a better one ships, pricing changes, a provider sunsets the endpoint you built against. **If your program is a pile of clever prompts wired to one model, then every single one will require its own migration. But if your program is a harness that treats the model as an interchangeable component, then replacing it with any new or updated version of the model amounts to a simple config change and an afternoon of re-benchmarking.**


Now the harness is the asset for security teams. The model is a consumable.


### Blindly pointing coding agents at repos doesn’t work


Every team starting out asks the same thing: “We already have a coding agent. Can’t we just tell it to scan our code and find bugs?” We tried exactly that before building anything. It’s the cheapest experiment and it would have been embarrassing to skip. It doesn’t hold up, and the reasons are structural. Prompting harder won’t help.


Standard coding agents are engineered for linear pursuit, following a single hypothesis through to the end. Effective code security requires the inverse. It’s a broad, horizontal exercise that involves managing numerous concurrent theories, most of which will inevitably collapse. Direct an agent toward a complex service and it exhausts its context window mapping the architecture, leaving minimal capacity for the actual hunt.


The harness earns its keep on three problems the model can’t solve alone:


- **State:** so a run that dies at hour six resumes instead of restarting, with the system remembering which paths it already cleared
- **Parallelism with focus:** so fifty narrow hunters each chew one attack class against one component and nobody drowns in context.
- **Cross-repo reasoning:** the vulnerable code and the entry point that reaches it each typically live in different services. The flaw sits in service A, while the reachable, attacker-controlled entry point can only be found three services upstream. No single repo session sees both — but it’s that very link that often separates a finding from a real, exploitable flaw.


These are orchestration problems, and a prompt, however good, is not an orchestrator.


### Where frontier AI excels: low-severity noise, chained into real attacks


The model takes primitives that read as low-severity noise on their own and stitches them into one exploit that matters. This is the work that used to separate a senior researcher from a code security tool, and it’s exactly the work that rots in a backlog when each piece gets triaged alone as “low.” Watching the frontier model chain those issues into a proven write primitive was the moment the frontier model stopped feeling like a better SAST tool and started feeling like a security researcher who never gets tired and never gets bored.


The frontier model also prefers to run code over theorizing about it. Hand it a suspected flaw and it writes the triggering input, compiles in a scratch environment, runs it, reads the crash, and goes again. That loop closes the gap between “this looks exploitable” and “here is the input that exploits it.”


### Where the human is non-negotiable


Everything downstream is driven by the threat model. **The threat model must come from someone who understands the system from top to bottom. If you hand the frontier model a blank threat model, it produces confident, plausible, wrong output at machine speed. Hand it a sharp threat model and it comes back with findings your team can act on the same day.** Garbage in, expensive noise out, and the noise is expensive precisely because it reads so well.


The same researcher reads what the model surfaces, deciding what’s worth driving to a working exploit, triaging what comes back, and folding the lessons into the threat model so the next pass is more effective. The threat model isn’t a document you write once. It’s the living state where the entire agentic SDLC loop turns around.


This researcher has to be the most senior person, not the most available one. The skill that used to go into manual testing now goes into threat modeling and reviewing. You’re changing what experts do all day and handing them a very fast, very literal apprentice to point at the work they’ve scoped.


### From proof to action: the gates before a human analyst


Every candidate finding must clear the same gate before it reaches a human: Is there a reproducible proof-of-concept? Without one, it goes to the validation queue. With one, we walk it down a short list of questions that map to what the business actually cares about it:


- **Reachable:** If it can’t be hit from an untrusted, exposed entry point, it’s a backlog item, not live risk.
- **Automatable:** A reliable, low-skill, repeatable exploit jumps the queue; one that needs bespoke tailoring waits for the next planned release.
- **Impact:** Partial impact or a denial-of-service (DoS) attack sits in one tier; total control, arbitrary write, or remote-code execution (RCE) sits in another.
- **Crown jewels:** If it touches a critical asset or crosses a trust boundary, a major ticket becomes an emergency out-of-band patch.


**Figure 1:** Agentic finding triage


## Budget for two things: compute and senior expertise


The work is changing, but not for free, and the expense is measured in two distinct currencies. The sticker price is the loud cost that dominates the conversation, but the one that actually binds is senior engineering expertise; don’t let the token bill distract you from the specialist hours required to make it work.


### The compute bill, measured


The compute cost is real and, for once, you can measure it exactly rather than guess. Here’s a full month of one engineer’s usage: 71 harness runs across 37 different code repositories in June 2026, priced at[Mythos pricing rates](https://platform.claude.com/docs/en/about-claude/pricing) . About 10.1 billion tokens moved, which totals to roughly $41,718.


June 2026 — 71 runs, 37 services, Claude Mythos 5 list price


5-min cache writes 1.99B tok @ $12.50/MTok = $24,839 (60%)


cache reads 7.77B tok @ $1.00/MTok = $ 7,774 (19%)


output 0.15B tok @ $50.00/MTok = $ 7,266 (17%)


base input 0.18B tok @ $10.00/MTok = $ 1,839 (04%)


**total ≈ $41,718**


Cache writes were 60% of the bill. Every turn in a long agentic session adds more to the context: file contents, tool output, whatever reasoning the model just did. And each of those additions gets written to cache at a 1.25x input price before it’s ever read back. Reads are cheap, writes aren’t, and a harness that lets its working set expand ends up paying that premium on nearly every turn. The per-run spread matters as much as the total. Median run was about $47. Mean was $588. The most expensive single run was just over $9,000. A handful of deep, wide sweeps against large services carried most of the token spend.


### Why one run isn’t enough


There’s a subtler cost to consider as well: AI models are non-deterministic. If you run the same harness against the same commit twice, you’ll get two different sets of findings. What is missed on one pass, may surface in the next. In our research, each pass surfaces half as many new issues as the previous run. If the first run finds half of all the flaws, a second run will surface three-quarters, and a third run will surface nearly seven-eighths. After that, the returns are too thin to justify the tokens, which is why we ran three passes. Budget for three runs, not just one, for any service where coverage matters.


The formula, if you want to recompute against your own telemetry:


cost = 10·(input/1e6) + 12.5·(cache_write_5m/1e6) + 20·(cache_write_1h/1e6)


+ 1·(cache_read/1e6) + 50·(output/1e6)


This also tells you when to run. For some, it’s a periodic sweep, not a per-commit gate. Run it on a cadence across the fleet, and again after a service’s attack surface changes in a meaningful way.


### The expert you can’t skip


The second cost is the one that actually binds: senior expertise. All of the compute costs outlined above rest on the availability of a researcher who can write a real threat model and triage a real finding. That person is scarce, and the harness doesn’t make them optional. It makes one of them do the work of five, which is a different and better problem to have. The uncomfortable version of the ROI story: The tool comes with its cost, and if you buy the tokens without funding the expert, you've built a very fast way to generate findings nobody can act on. **Budget the tokens. Staff the expert.**


## Only verify the facts that matter


Thirty days in: **A frontier model in a harness we control dramatically changed the shape of our work rather than its speed. It doesn’t remove the expert, it changes the work. It moves analysts off testing and onto scoping and judgment.** It moves the program off ranking suspicions and onto verifying and acting on what’s proven.


Patch speed was never the binding constraint, and chasing it in isolation makes things worse. A fix shipped in two hours that skips regression testing is exactly how one vulnerability becomes two. Before patch speed enters the equation, the harness gives you something better: certainty about what’s real while eliminating the noise. Take a look at how our findings funnel:


- Far more raw candidates than a human pass can produce
- A large fraction of them killed in validation
- The repeats removed via deduplication
- What survives lands with a proof, handed directly to an engineer


We don’t know whether these proven findings represent most of the real bugs in the code, but the efficient noise elimination with this funnel and knowing what’s left is real and verifiable are worth a lot.


With these efficiency gains, you should be able to reallocate a limited change management budget to thoroughly address and mitigate the risks of the proven problems that remain. Put controls in front of the app so the bug isn’t reachable in the first place. Isolate components so a single compromise won’t take down everything. Roll a fix across the fleet in one motion so you’re not sitting exposed on nine hosts while you fix the tenth.


**The harness makes you faster at knowing. What you do with that knowledge is still on you.**


### Where to start, if you’re starting


Take one service your best person knows cold, and build the first loop around it:


- **Write the threat model by hand.** That input governs everything downstream, so it comes from your most senior person, not your most available one.
- **Run against a pinned commit** so the results can be reproduced.
- **Route the output through a triage tree that ends in real tickets** , then watch your own funnel and tune sweep depth to your risk.


And three things not to do:


- **Don’t** start with the whole fleet.
- **Don’t** start with your crown-jewels service.
- **Don’t** start by letting the frontier model write fixes.


The guesswork you’ve been fighting mostly dissolves once you have proof, but only if you’ve built the harness that makes the proof worth trusting.


## Learn more


- [Claude Mythos Preview: Prepare for your board's cybersecurity questions](https://www.tenable.com/blog/claude-mythos-prepare-for-AI-cybersecurity-questions-from-your-board-of-directors)
- [Five steps to become Mythos-ready](https://www.tenable.com/blog/5-steps-to-become-mythos-ready-ai-cybersecurity)
