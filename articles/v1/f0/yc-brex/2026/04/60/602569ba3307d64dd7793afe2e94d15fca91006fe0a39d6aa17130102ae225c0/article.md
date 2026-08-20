---
schema_version: "1.0.0"
document_id: "602569ba3307d64dd7793afe2e94d15fca91006fe0a39d6aa17130102ae225c0"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/articles/what-if-you-cant-control-your-prompt"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:3e805337804d5d0ec48984c802fe8172b547393e88280d1208b440dd377aadd3"
---

# What if 90% of your prompt is content you can't control?

## Not all context is equal


During testing, one of our security engineers put a magic model refusal string into an expense memo and the agent refused to audit the expense at all.


That failure was useful because it showed us that the problem was not just the agent's ability to read documents. It was deciding how different kinds of context should be allowed to influence the decision.


At Brex, we have been building agents that review employee expenses, gather the surrounding evidence, compare it against policy and audit standard operating procedures (SOPs), and decide what should happen next.


Policy tells the system what is allowed. The SOP tells it how to handle a case when something looks wrong.


To reach a decision, the agent has to reason over receipts, memos, calendar events, web searches, policy documents, SOPs, prior cases, merchants, and user profiles. The difficult part is deciding how each of those sources should influence the outcome.


Building audit agents taught us that the hard part is in how the decision is structured. The system needs to know what counts as evidence, what should guide the outcome, what needs extra handling, and when a judgment is ready to become a real outcome.


**Inputs for the audit agent**


The model does not receive one neat instruction. It receives a pile of inputs with very different levels of trust and authority.


## Different inputs need different handling


The challenge was making those differences real inside the system, so the model would actually treat each source with the right weight instead of seeing everything as just more context. We stopped dropping everything raw into the same prompt. Policy and SOPs stayed as reference material. Expense and case records became the factual state of the case. Memos, receipts, and web search each had to be processed before they were allowed to influence the main reasoning loop.


A memo might explain an expense, but it is not proof. A note saying "the CEO approved it" is not very helpful on its own if the system cannot really validate that claim. This forced us to treat memos as both business context and an attack surface. Receipts needed different handling too. They are high-value evidence, but they are also noisy and easy to manipulate, so what mattered was extracting the facts needed for the decision rather than handing the whole receipt interpretation to the rest of the system.


Web search also needed strong controls. It is useful for validating outside facts, but it also creates a path for private information to leak out through url parameters. We added defenses around what could be sent to search: filtering suspicious long URLs, stripping risky query parameters, and keeping sensitive case details out of requests. Those inputs were all valuable, but they could not just be dropped raw into the main reasoning loop.


## Production agents need clear system boundaries


Once we knew those inputs could not all enter the system in the same way, the next question was scope. How much context and how many tools should any one agent get at once?


Our earlier audit-agent versions had broader scope: more tools and more context handed to the agent at once. On the surface, that sounded better. But when we looked at the results, the extra scope was not improving performance. It mostly made the agent harder to control and less predictable.


We got better results by keeping the scope closer to the task. A useful exercise was to put ourselves in the agent's shoes and ask what the ideal set of inputs and tools would be for that job, and nothing more. From there, we watched executions and kept refining the inputs, outputs, and tool descriptions until the behavior became easier to steer.


**Scoping each step from input to outcome**


Better performance came from the right scope for each step, not the biggest possible context window or tool surface.


## Reviewable outputs make the system safer


Once the agent became easier to steer, the next question was how to evaluate its answers. Our audit agents can take actions with financial consequences, like locking cards, rejecting reimbursements, or changing spend limits. That means trusting the LLM is not enough.


We treated model output as a candidate for review, not a final outcome. In practice, that meant saving a draft decision that went through deterministic checks and a reviewer agent before it could become a real case outcome.


**Model output review**


Model output becomes reviewable work product instead of an immediate side effect.


That separation gave us much better control over the system. We had a place to run deterministic checks, record disagreement, retry safely, inspect citations, and understand why a decision was made before it turned into durable state.


It also made the system much easier to inspect and measure. We could see the draft decision, the evidence behind it, and the review path it went through. Because we kept snapshots of each iteration as the output was refined, we could also see how answers changed over time and use that history for continuous improvement. That made it possible to track common failure patterns, see where decisions were getting sent back, and measure how often the main agent and the reviewer agreed over time.


## The workflow makes sure each run finishes cleanly


The agent has a fair amount of freedom during the first pass. It can investigate the expense, save a draft recommendation, revise that draft after feedback, turn an approved recommendation into a real audit case, or conclude that no case is needed.


The workflow then looks at how that run ended and decides whether the job is actually done. If the agent created a real case, the workflow can move on to the next steps. If the agent reached an approved no-case conclusion, the workflow can end cleanly. If the agent stopped with a draft that still needed changes, forgot to turn an approved recommendation into a real case, or finished without recording any final outcome at all, the run is still incomplete.


**Checking for a complete outcome**


The workflow is checking for a complete outcome, not just whether the agent produced a draft.


That ended up being one of the most useful parts of the system. It meant the workflow could catch incomplete runs, retry when something went wrong, and keep the rest of the process from moving forward on a half-finished decision. It also gave us a much clearer picture of what happened in each execution: whether the agent found no issue, created a real case, or simply did not finish the job.


## Simulation shows whether the system actually holds up


Once those pieces were in place, we could start testing the system the way people would actually use it and the way they might try to break it. We could simulate manipulative memos, risky web-search scenarios, noisy receipts, and other edge cases, then see whether the agent still followed the hierarchy and safeguards we intended.


That matters because these failures rarely show up in a single clean example. Simulation lets us see whether the system keeps making the same kinds of decisions across many runs, where it starts to drift, and which protections are actually doing work.


It also connects naturally to our simulation article,[How we test our agents: by committing fraud](https://www.brex.com/journal/articles/simulation-testing-ai-audit-agent) .


## Be opinionated on the right things


Building a system like this requires being opinionated on the right things. When most of the context comes from outside the system, you need clear authority boundaries, reviewable drafts, tool access that matches the job, a workflow that can tell whether a run actually finished cleanly, and simulation that shows where the structure starts to break down under pressure.


Good AI products create value by handling messy real-world context, not by forcing people into a tiny set of allowed paths. The goal is not to strip judgment out of the product. Audit work is valuable because the system can reason across messy context that would otherwise require a human to read everything by hand. The goal is to give that judgment a structure it can operate inside.


That is the real answer to the title question. When most of your prompt is content you cannot control, the work shifts to deciding how that content enters the decision, how the decision is reviewed, and how the system recovers when a run does not end cleanly.
