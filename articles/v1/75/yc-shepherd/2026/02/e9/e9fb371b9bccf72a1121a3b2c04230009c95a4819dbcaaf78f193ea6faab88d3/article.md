---
schema_version: "1.0.0"
document_id: "e9fb371b9bccf72a1121a3b2c04230009c95a4819dbcaaf78f193ea6faab88d3"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/building-the-bloomberg-terminal-for-commercial-insurance"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-24T01:19:36.558879+00:00"
fetched_at: "2026-07-28T22:19:38.682305+00:00"
content_hash: "sha256:8dc1d506deaab5fa4083fe2f3737127f88759013ceca798cd0d23fc6c6dd3474"
---

# Building the Bloomberg Terminal for Commercial Insurance

### License a PAS or Own the Core?


Early at Shepherd, we had to make a foundational call: license a third party Policy Administration System or build the system of record ourselves.


We ran full demos with Guidewire, Duck Creek, Socotra and many more. These are serious platforms built for serious carriers. But they are optimized for incumbents layering technology onto legacy operating models, not for a company trying to be AI native from day one.


Two issues became clear immediately: economics and control.


### Avoiding a Permanent Tax on Growth


Many PAS vendors price as a percentage of Gross Written Premium, often pushing toward 2 percent of the book. It is positioned as alignment. “We grow when you grow.” In practice, it becomes a permanent tax on underwriting margin.


If your differentiation is underwriting quality and segmentation, you do not want to give away basis points indefinitely to infrastructure. Software should scale with usage and compute, not clip revenue forever. We did not want a vendor whose upside depended on taxing our growth. We wanted operating leverage as we scaled.


### Controlling the System of Record


In commercial insurance, the system of record is not just back office plumbing. It is the operating system of the company.


Buying a PAS means inheriting someone else’s abstractions, schemas, APIs, release cycles, and roadmap. Want to introduce a new rating factor. Rethink endorsement handling. Change how submissions are modeled. You are working inside someone else’s constraints.


For a traditional carrier optimizing for stability, that tradeoff can make sense. For a company betting on speed, segmentation, and AI driven iteration, it does not. If you do not control the system of record, you do not control your destiny.


### The Choice: Build


So we built it.


This was not about ego. It was about ownership. When you build, there is no vendor to escalate to. Every outage, migration, and design flaw is yours. There is no throat to choke except your own. But so is every improvement and every architectural decision.


Building forced us to understand insurance at the implementation layer, not just the business layer.


We had to design how a policy is represented in the database. Is it immutable with versioned snapshots. Is it event driven. How do endorsements modify prior state without corrupting history. How do we generate legally binding documents from structured data. How do we guarantee auditability across quote, bind, cancel, reinstate, and rewrite flows.


We had to model rating logic directly. ISO loss costs, schedule credits, tech modifiers, minimum premiums, attachment points, excess layers, state specific rules. Those are not spreadsheet abstractions. They are code paths that must execute deterministically every time.


We had to build submission intake that handles messy real world inputs. PDFs, Excel schedules, incomplete broker emails, missing payroll splits. That meant creating structured schemas that could absorb ambiguity without breaking downstream workflows.


We had to own quote and policy number lifecycles. Clearance logic. Referral triggers. Authority checks. Document versioning. State filings. All of it.


There is no way to outsource that learning. When you implement the primitives yourself, your engineers understand how underwriting actually works, where it breaks, and where leverage exists.


### From PAS to Bloomberg Terminal


Over time, the system stopped feeling like a traditional PAS. It became the nerve center of the business.


Underwriting workflows, pricing execution, document generation, referral logic, risk signals, analytics, integrations. All operating inside a single coherent environment.


Functionally, it behaves more like a Bloomberg Terminal for commercial insurance than a static admin tool. Underwriters do not swivel between disconnected systems. They operate inside a unified platform where data, pricing, and workflow intelligence live together.


Owning that foundation means new data signals can be ingested quickly. Rating logic can evolve without waiting on external roadmaps. Underwriting workflows can be redesigned without negotiating API constraints. The platform compounds as the business compounds.


### Velocity, Autonomy, and the AI Shift


Owning the system changed how we build.


When an underwriter flags friction, we ship. When actuarial wants to test a new factor, we modify the model. When we rethink segmentation, we refactor architecture instead of submitting a vendor ticket.


That flexibility also underpins our ambition to increase underwriter autonomy over time, progressing beyond guardrails toward systems that assist and eventually orchestrate workflows.


The broader shift in AI makes the tradeoff even clearer. Execution is cheaper. Boilerplate is commoditized. The constraint is no longer writing CRUD applications. It is judgment, architecture, and domain depth.


If you are willing to operate as a true technology company, owning data models, infrastructure, and workflow orchestration end to end, building becomes a rational strategic choice.


But it requires leadership. It requires deciding that you are not a business that uses software. You are in the business of selecting the best risk AND owning the technology behind it.


### Owning the Core


Building the system increased responsibility and removed excuses. But it gave us margin control, architectural freedom, product velocity, and deep domain expertise.


In commercial insurance, your system of record determines how fast you can think.


We chose to own the core.


‍
