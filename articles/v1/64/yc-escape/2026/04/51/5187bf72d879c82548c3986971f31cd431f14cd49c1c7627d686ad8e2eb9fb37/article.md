---
schema_version: "1.0.0"
document_id: "5187bf72d879c82548c3986971f31cd431f14cd49c1c7627d686ad8e2eb9fb37"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/ai-pentesting-agents/"
published_at: "2026-04-30T20:22:03+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T21:14:22.706316+00:00"
content_hash: "sha256:4b3483e3eb8d3277ba7148e840f31e49cd02670c108f28772205ca0709d2b856"
---

# Escape AI Pentesting Agents 2.0 - A Deep Dive

When people ask us what agentic pentesting actually *is* , we usually start by saying what it isn't.


It isn't a chatbot that writes Burp payloads for you. It isn't a "prompt-the-LLM-and-hope" system that hallucinates vulnerabilities and then calls it a day.


When we introduced last year how[agentic pentesting](https://escape.tech/blog/agentic-pentesting/) architecture works at Escape,[we laid out the three-layer architecture we use](https://watch.getcontrast.io/register/escape-from-business-logic-vulnerabilities-to-actionable-insights-ai-powered-pentesting-asm-in-action) : a coordinator agent that orchestrates, specialized agents that do the actual offensive work, and sandboxed tools that keep the whole thing safe and deterministic. That piece is the *what* and the *why* .


This article is the *who* . Specifically: which agents live inside the platform at the moment we're writing this article, what each one is actually good at, and where we're heading as a product (because let's be honest, at the AI development speed, late May you can expect an entirely new model from us!)


We wanted to write this for the people we've seen benefit most from our AI pentesting product: offensive security team leads (case study coming soon), who can tell within about 90 seconds of a demo whether a tool is doing real work. If that's you, hopefully you'll find something useful here.


If you find something wrong, please tell us, our door is always open. We know we've got lots to improve.


## Current state: the multi-agent pentest


Before we walk through the specialized agents, we want to tell you where the product is going, because it changes how you should read the rest of this article.


Today, we ship specialized agents (one per vulnerability class) alongside something called the **Multi-Agent Pentest** . The specialized agents are excellent at what they do. But the direction of travel is consolidation. One orchestrator agent that reasons about the whole application, spawns the right sub-agents for the job, and chains attacks together the way a real pentester would.


Escape's Agentic Architecture presented previously


Here's what the Multi-Agent Pentest actually is: a coordinator agent orchestrates a team of specialized child agents inside a sandboxed environment equipped with real attacker tools — a browser, an HTTP proxy, a terminal. It runs a complete campaign from reconnaissance to exploitation, thinks out loud the entire time, and delivers findings backed by executable proof. Actual` curl` requests. Actual responses. Exploit scripts. Step-by-step reasoning you can read.


That last part matters more than it sounds. If you've ever tried to convince a skeptical engineer that the scanner finding is real, you know the drill: you end up re-running the attack yourself, turning a "finding" into a working PoC, and then forwarding *your* PoC to the dev team because the scanner's write-up wasn't enough. The Multi-Agent Pentest skips that step. The PoC is the output.


Unlike a DAST scanner that runs a single step attack scenarios, the Multi-Agent Pentest does a few things scanners structurally can't:


- **Chains multiple techniques.** XSS → token theft → privilege escalation, for example. This is the stuff that gets human pentesters excited and that no rule-based scanner has ever found.
- **Adapts its strategy in real time.** Based on server responses, error messages, state changes. A 403 doesn't end the conversation — it starts a new one.
- **Produces evidence-rich findings:** Working exploits with reasoning traces attached.


💡


Next steps are expected mid-May. More on that to come.


Escape's new AI pentesting version is built on a multi-agent architecture: an orchestrator holding the state of the ongoing pentest, relying on multiple agents that can perform assessments, handle reporting, handle the “context” of the pentest (access support documents, discovered assets through the ASM, metadata about these assets, past vulnerabilities, …), vulnerability-specific skills, along tools allowing to execute code or navigate applications with a web browser.


The reason we focus not only on the models underlying the product is that the orchestration layer can significantly impact the quality of findings.[In this benchmark, we compared scanning results for AI pentesting tools](https://escape.tech/blog/benchmarking-agentic-ai-pentesting-tools/) like Shannon, Strix, and PentAGI that were based on DeepSeek v3.2. The three DeepSeek tools diverge widely on Duck Store (6/20 vs 9/20 vs 1/20) despite sharing the same model. The key variable is how each tool structures the agent's action loop, manages tool calls, and handles authentication. It proved that when evaluating agentic pentesting tools, the model card can be a secondary concern. Evaluate the orchestration layer first.


Now, let's get into the specialists we're currently using at Escape.


## The Coordinator Agent: the brain, not the hands


A quick architectural note before the specialists. The coordinator agent doesn't perform testing itself. Its system prompt is blunt about this: *"You are a COORDINATION AGENT ONLY. You do NOT perform any security testing, vulnerability assessment, or technical work yourself."* It analyzes the target scope, decomposes the work into independent tasks, delegates to specialists, and keeps everyone inside the guardrails.


Why split the brain from the hands? Two reasons.


First: accuracy. A single general-purpose agent trying to do recon, XSS, BOLA, SQLi, and business logic at the same time is a model fighting its own context window. Specialized agents with focused prompts and narrow tool access perform materially better on their specific task — and they hallucinate less.


Second: safety. When an agent's job is tightly scoped, its permissions can be tightly scoped too. The coordinator never touches your application directly. The specialists touch it only through sandboxed tools. Every HTTP request goes through a configurable proxy. Every command runs in an isolated environment. You keep full control over what can be tested, what can't, and how.


This is also what makes the whole thing programmable. Public API, CLI, event hooks. If the platform can do something, your scripts can too. Scans run on every push. Security gates live in your CI/CD without your team having to babysit them.


## The specialized agents


These are the agents you can run today, either independently or via the Multi-Agent Pentest. We'll go through each one with the same structure: what it is, what it catches, and why I think it matters.


### Agentic Crawler


**What it does in a nutshell:** The agent that learns your application the way a real user would.


If you've run a traditional scanner against a modern SPA, you already know the punchline. The crawler follows links, hits the login page, fails to authenticate, and reports back that it found seventeen endpoints, out of the four hundred you actually have. The rest of your attack surface, the part that only exists after sign-up, project creation, document upload, role assignment, is invisible to it.


The Agentic Crawler reasons about the app the way a person would. It fills out forms. It completes multi-step flows like sign-up, checkout, and dashboard navigation. It understands that it needs to create an account, then a project, then a document, before certain pages even exist. Before any offensive testing starts, it's built a map of the real attack surface — not the surface your scanner thinks you have.


This one agent is arguably the most underrated piece of the whole system. Because without strong exploration, every other agent downstream is testing half an application.


💡


[Discover how Applied Systems transformed their scanning with Escape from complex authentication to confident coverage](https://escape.tech/blog/from-complex-authentication-to-confident-coverage-applied-systems/)


### XSS Agent


**What it does in a nutshell:** Finds the places where an attacker can inject a script that ends up in someone's browser.


What it catches:


- **Reflected XSS** : scripts that fire immediately inside the server's response.
- **Stored XSS** : scripts that get saved to the database and later execute in someone else's browser. These are the ones that turn into real account takeovers.
- **DOM-based XSS:** client-side-only, never hits the server. Traditional scanners are especially weak here because their entire model assumes request–response symmetry.
- **CSP bypasses**
- **Framework-specific attacks:** React, Vue, Angular, Svelte. The frameworks escape most things by default; the XSS Agent knows where they don't.


Business impact: session hijacking, account takeover, phishing campaigns launched from your own legitimate domain. If your app has a rich text editor, a comments system, or any user-generated content surface, this is where you want a real agent — not a pattern matcher — doing the looking.


### SQLi Agent


**What it does in a nutshell:** Finds endpoints where an attacker can manipulate your database queries.


The SQLi Agent is opinionated about where it looks. Not every endpoint is equally likely to talk to a database, so it prioritizes the ones that are: search, filters, exports, reporting, admin views, anything that smells like a` WHERE` clause. This is how a human tester approaches it, and it's meaningfully faster than the "blast every parameter" approach you get from legacy tools.


What it catches:


- **Classic injection** (union-based, error-based)
- **Blind / time-based injection:** where the application never tells you it's vulnerable, and you have to infer it from response timing or boolean conditions. This is where agentic testing genuinely shines, because the "inference" step is a reasoning step.
- **NoSQL injection:** MongoDB, and the family.


Business impact: leak of customer data, authentication bypass, full database takeover. The old classics, still very much alive in 2026, especially in the long tail of internal tools and admin panels that no one has pentested in three years.


### BOLA Agent


**What it does in a nutshell:** Logs in as multiple users and tries to break access control between them.


This is, hands down, the most impactful agent we ship for multi-tenant SaaS. We've watched BOLA findings land in customer Slack channels and seen the "oh" moment on the other end of the call.


The agent logs in as several users — across roles, across tenants — in parallel. Then it tries to read or modify another user's data, or another tenant's data, using the first user's session. It doesn't guess IDs randomly; it understands the object relationships it discovered during crawling, then targets the ones most likely to matter.


What it catches:


- **IDOR / BOLA** (accessing another user's objects by tweaking an ID)
- **Privilege escalation:** a member performing admin-only actions.
- **Tenant isolation breaks:** one customer sees another customer's data in a multi-tenant SaaS. On that note, we wrote[a guide on implementing multi-user testing](https://escape.tech/blog/multi-user-dast-testing-real-world-examples/) in another product,[DAST](https://escape.tech/product/dast) .
- **Authentication and function-level autorization bypass**


Example of BOLA + remediation tailored to React


Business impact: cross-customer data leaks (catastrophic for a SaaS), GDPR and SOC 2 non-compliance, and regulatory exposure. If you've ever lived through a BOLA finding in production, you know it's the kind of thing that ends up on a status page and a legal hold.


### Business Logic Agent


**What it does in a nutshell:** Breaks real business workflows, not just technical vulnerabilities.


Business logic is the part of security testing that scanners have historically been useless at, and for a good reason: there's no signature for "customer refunded before paying." The rules live in the application, not in a CVE database.


💡


[Our CTO has written about our proprietary business logic security testing algorithm and what makes it innovative here](https://escape.tech/blog/escape-proprietary-algorithm/)


The Business Logic Agent tests payments, refunds, coupons, subscriptions, loyalty points, and the surrounding state machines. It looks for the kinds of issues that only a human tester would normally catch, and it does so by actually executing the flows, not by pattern-matching on parameter names.


0:00


/ 0:22


What it catches:


- **Broken sequences** : refunding without paying, shipping before payment clears, activating a subscription before the transaction settles.
- **Replay and missing idempotency** : replaying a purchase request, stacking coupons, re-submitting a one-time action.
- **State manipulation:** tampering with price fields, modifying order status, forcing transitions the state machine shouldn't allow.
- **Race conditions:** concurrent requests designed to double a result. Redeeming a gift card from two tabs at once is the canonical example.
- **Ledger inconsistencies:** a balance that doesn't match the transaction history.


Business impact: direct financial loss, fraud, and accounting discrepancies. And often, the kinds of fraud that only get detected at quarter-end reconciliation, by which point it's very much not a security problem anymore, it actually becomes a finance problem.


### Regression Testing Agent (or the fix validator)


**What it does in a nutshell:** Replays previous vulnerabilities to check whether the fix actually worked.


This is the agent I get the most excited about personally, because it solves a problem that's embarrassingly common and nobody has a good answer for: *did the fix ship, and did it actually work?*


You feed the Regression Testing Agent a prior pentest report. For example, in PDF format, as a Markdown, bug bounty export, whatever you have, and it re-executes each vulnerability against the current version of your application. These automated regression tests can be run on every build.


Example of regression testing within Escape


The same vulnerability never ships twice, and your security posture compounds instead of resetting at each new engagement. Yesterday's finding is today's CI gate.


Ideal use cases:


- Validating a fix before release.
- Recurring validation campaigns after a human pentest.
- Keeping closed issues closed when the codebase churns underneath them.
- Converting a static PDF deliverable into a living, executable test suite.


If your security program has ever paid for a pentest, gotten a PDF, triaged it, marked things "fixed," and then been unable to cheaply verify six months later that they're still fixed, this agent is for you.


## How the pieces fit together


A quick note on intelligence sharing, because it's the part of the architecture that's easy to miss and, in my opinion, does most of the heavy lifting.


Agents don't run in isolation. They share findings through an intelligence layer. When the Agentic Crawler discovers an authenticated API endpoint, it signals the BOLA Agent to test authorization on it. When the XSS Agent identifies input reflection in a form, the Adversarial Vulnerability Validator is triggered to confirm the exploit works end-to-end. Hooks fire on specific events, for example, on chat completion, tool execution, vulnerability discovery, and that's what enables the chaining.


💡


The advantages of this approach is ****that it's very fast.****
We can complete a pentest on a web application in 2 hours instead of days, and it gets closer to human quality every day.


Also, it is programmable, so it means that you always have full control over what the agentic pentesting tool like Escape is doing on your application, what it can test, what it cannot test, how the test is performed.


You can also launch tests at scale through a public API or a CLI, so you can integrate it into your process from end to end, and this is a very powerful, scalable approach.


This is what we mean when we say agentic pentesting is a force multiplier for small teams. You're not replacing your senior application security engineer. You're giving them the leverage to act like ten of them.


## What this doesn't do (and where we think the field still has work to do)


We promised we wouldn't write a marketing piece that skips the limitations, so here goes.


Agentic pentesting (ours or anyone else's) is not a silver bullet. Early-generation models, and even some current ones without proper guardrails, hallucinate. They'll occasionally "prove" an exploit that doesn't exist, or miss a multi-step chain that requires the kind of lateral thinking humans are still better at. Some agents struggle with nonstandard authentication, niche protocols, or heavily custom environments where the learned priors don't apply.


The way we address this at Escape is with validators, deterministic tooling, and, honestly, a lot of engineering work on the plumbing between agents. The Adversarial Vulnerability Validator exists specifically to confirm exploits work in real-world conditions before they show up in your findings list. Every request goes through a proxy you can inspect. Every result is reproducible.


But we'd still encourage you to pair agentic testing with human expertise where it matters most, high-value targets, novel business logic, and compliance-critical flows. The framing we use internally is that agents handle the breadth, humans handle the depth that still needs humans. That ratio is shifting every quarter in favor of the agents, but it's not one yet.


If you want the full platform story, including how the specialized agents slot into the broader offensive security program, the[AI Pentesting product page](https://escape.tech/product/ai-pentesting) is the right next stop. As we mentioned, we're also bringing a lot of changes to our AI pentesting product in the upcoming weeks.


And if you want to see it run against something real,[book a demo](https://escape.tech/book-a-demo) and bring an application you actually care about. That's the only way you'll know whether this is a step forward for your program or just another tool in the stack.


Either way, we'd love to hear what you find.


---


Want to learn more about Escape features? Discover the following articles:


- [Claude x Escape integration: Register for the upcoming webinar](https://watch.getcontrast.io/register/escape-claude-x-security-engineering?utm_source=website&utm_medium=banner&__hstc=241536093.15b64536288df1760e39b35c893d0af9.1768563555634.1777568540127.1777575340462.303&__hssc=241536093.19.1777575340462&__hsfp=b967c72849be682b77bf8e523864f42f)
- [Escape x Wiz Integration](https://escape.tech/blog/doubleverify-enhanced-api-discovery-wiz-escape/)
- [How to Visualize Web & API Coverage with Screenshots and Validate Attack Paths in Escape](https://escape.tech/blog/how-to-visualize-coverage-and-validate-attack-paths/)
- [How to Implement Multi-User Testing in DAST: Real-World Examples](https://escape.tech/blog/multi-user-dast-testing-real-world-examples/)
- [Fixing Vulnerabilities Directly in your IDE with Escape MCP](https://escape.tech/blog/fixing-vulnerabilities-directly-in-your-ide/)
