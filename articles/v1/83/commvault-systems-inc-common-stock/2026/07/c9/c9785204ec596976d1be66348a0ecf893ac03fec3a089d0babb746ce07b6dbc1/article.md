---
schema_version: "1.0.0"
document_id: "c9785204ec596976d1be66348a0ecf893ac03fec3a089d0babb746ce07b6dbc1"
company_key: "commvault-systems-inc-common-stock"
company: "Commvault Systems Inc."
source_id: "commvault-systems-inc-common-stock-news-import-d7ff9e033aa3"
canonical_url: "https://www.commvault.com/blogs/resilience-over-panic-four-steps-for-the-age-of-frontier-ai"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-21T14:34:38.669856+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:62440de5a0d9efafedc2f83cf9263310118d5e3c236342ac2efdf4efc0ff6a4f"
---

# Resilience Over Panic: Four Steps for the Age of Frontier AI

For years, the working assumption in enterprise security was that a mature prevention stack could hold the line long enough for defenders to respond. Frontier AI is shaking that premise, as the latest models shrink the timeline for discovering and exploiting vulnerabilities from days or weeks down to almost real time.


Launched to assess the potential security risks posed by its own[Mythos](https://www.anthropic.com/claude/mythos) model, Anthropic’s[Project Glasswing](https://www.anthropic.com/glasswing) initiative has already grown to[nearly 200 companies](https://www.anthropic.com/news/expanding-project-glasswing) and surfaced approximately 10,000 critical or high-severity vulnerabilities. Meanwhile,[OpenAI’s GPT-5.5](https://openai.com/index/introducing-gpt-5-5/) is showing comparable capabilities.


In a recent webinar, Commvault Chief Technology and AI Officer Pranay Ahlawat and Field CTO Vidya Shankaran joined me to explore the new timeline for vulnerability management, the rising importance of recovery validation, and how teams should be thinking about resilience today.


[Register for the on-demand webinar](https://discover.commvault.com/webinar-resilience-over-patching-registration.html) .


### Key takeaways


- As frontier AI capability doubles on an accelerating schedule, advanced capabilities that help compress the time between vulnerability discovery and exploitation will reach adversaries within six to nine months.
- Recovering an agentic AI system requires synchronizing data sources, agent configurations, and non-human identities simultaneously; restoring any single element in isolation can create gaps that surface only when something downstream breaks.
- Backup and recovery solve different problems: Backup confirms that data exists somewhere safe, while recovery confirms that an organization can actually return to a clean, working state.
- ResOps™ (resilience operations) frames recovery as a cross-functional discipline. It brings security, operations, and technology teams together around a shared definition of what clean actually means.
- A four-step framework – defining a minimum viable company, isolating and testing crown jewel workloads, evaluating recovery for risk, and running full recovery drills – gives organizations a practical starting point.


### Frontier AI Changes the Math on Vulnerability Management


The power of frontier AI now doubles approximately[every four months](https://www.cfr.org/articles/why-anthropic-is-sounding-the-alarm-on-the-next-generation-of-ai) , much faster than even a few years ago. Although Mythos-type models have yet to be released publicly, adversaries may soon gain open-source access to Mythos-like capabilities, including:


- An effectively unlimited context window.
- The ability to construct an attack harness by reverse-compiling code and building containers to find attack vectors.
- Vulnerability chaining, which is the linking of individually minor weaknesses into a serious exploit.


This has serious implications. Two out of three organizations currently carry more than 100,000 unpatched vulnerabilities, with an average fix life of approximately 240 days. In the past, security teams have dismissed many vulnerabilities as too difficult for an average adversary to chain together, but automation has rendered that viewpoint nearly obsolete.


At the same time, the use of AI for code creation – roughly 41% of new code is now AI-generated, and GitHub saw a 25% year-over-year increase in commits – is expanding the vulnerability surface faster than remediation can address it. The ability to uncover new zero-day vulnerabilities at scale compounds the problem.


When the time from discovery to exploitation approaches zero, the window for defensive action effectively closes.


### Sneak Peek: The Rapidly Changing Future of AI


This clip highlights a critical reality: Advanced AI capabilities rarely remain exclusive for long. As frontier AI innovations diffuse into broader ecosystems, organizations must prepare for a future where increasingly sophisticated offensive capabilities become more widely available.


### The New Metric for Resilience: Mean Time to Clean Recovery


Backup and recovery solve fundamentally different problems. Backup only confirms that data has been copied somewhere safe, but it says nothing about whether the organization can actually return to a working state. And that’s where things can get complicated.


Two challenges often come between a successful backup and a successful recovery.


1. Restoring a complex environment means recovering the application, virtual machines, network configuration, Active Directory, and transactional databases that support it, all in the correct sequence.
2. You have to make sure that the data you’re restoring is free of malware or backdoors – something seven out of 10 organizations recovering from a cyber incident are currently unable to validate.


A recovery that meets its time target while reintroducing an active threat can be worse than no recovery at all.


To get clearer visibility into their resilience, organizations have begun using the[mean time to clean recovery (MTCR)](https://www.commvault.com/shift-virtual/on-demand/introducing-mtcr-the-missing-metric-for-cyber-recovery-success) metric, which combines recovery time objective (RTO), the time required to validate that recovered data is actually clean, and a final human validation step before systems return to production. The recovery target for MTCR is the[minimum viable company](https://www.commvault.com/minimum-viability/minimum-viability-guide) : the roughly 30% of an environment, sequenced by dependency, that has to come back online for the organization to keep functioning.


### Cleanroom as a Testing Tool


Recovery testing, the final human validation step in MTCR, typically means standing up a separate environment from live production systems – a time-consuming task when every minute counts. While cleanrooms are sometimes seen as an element of backup, a cloud-based cleanroom can also play a proactive role in recovery by providing an isolated environment to orchestrate and test complex recoveries before they are restored to production.


The same isolated environment can also help serve as a forensic tool, letting teams stand up two versions of a backup side-by-side to better understand what changed during an incident. And because it’s cloud-native and consumption-based, organizations can avoid standing up dedicated infrastructure just to test recovery.


### Four Steps to Operational Resilience


Commvault’s four-step framework for building measurable operational resilience builds on these ideas.


- **Step 1:** Define the minimum viable company: A business-oriented view of what has to come back, in what sequence, and with what dependencies, for the organization to function again, rather than a flat inventory of databases and virtual machines.
- **Step 2:** Make sure the systems supporting that minimum viable company sit in an air-gapped, immutable, network-segmented environment that can be spun up and down quickly. For crown jewel workloads, this should be tested on a 45-day cadence.
- **Step 3:** Evaluate recovery for risk before declaring it complete, since reintroducing a backdoor or piece of malware during recovery defeats the purpose of the exercise and leaves little time for a second attempt.
- **Step 4:** Treat recovery as more than a tabletop exercise. Perform recoveries with the same people and processes that would be involved in a real incident, alongside the automation behind them.


### When AI Becomes the Recovery Problem


A large share of enterprises are already running AI systems in production, but only about 20% of them have actually tested their recoverability, leaving them vulnerable in the event of an incident. This is especially significant in light of the three ways AI changes resilience architecture.


First, AI expands the surface area that needs protection, from vector databases and model weights to agent configurations and the endpoints, such as[Claude Cowork](https://claude.com/product/cowork) or[Google Antigravity](https://antigravity.google/) , where employees actually interact with agents.


It also introduces a fan-out problem, where a single update from an agent can cascade through a mesh of connected systems in ways that are far less predictable than a traditional three-tier application.


Finally, AI makes recovery itself more complex, since restoring an agentic system means synchronizing memory, state, transactional data, and non-human identities (NHIs) – the credentials and permissions assigned to AI agents rather than people – all at once.


The customers furthest along on agentic deployments have already made these systems part of their minimum viable company. At every stage of maturity, the emphasis is on restoring data sources, agent configurations, and supporting elements like weights and biases together, rather than as separate efforts, since misalignment between any of those pieces can introduce risk that a single point of recovery wouldn’t catch.


### Taking Action on Post-Mythos Resilience


As a starting point to reduce risk from frontier AI, map your organization’s crown jewel systems and confirm that they sit in an air-gapped environment. With your minimum viable company defined, run cleanroom drills to establish a recoverability and MTCR baseline across tier-one workloads. This should be your anchor, board-level metric for resilience, showing clearly how quickly your business can resume essential operations following an incident.


Testing is critical for surfacing gaps in business, technology, and process understanding. Often, some of the biggest problems are organizational.[ResOps™ (resilience operations)](https://www.commvault.com/explore/resilience-operations) , can address these.


A framework rather than a product, ResOps brings security, operations, and technology teams together around a shared view of what resilient design and recovery validation should look like. ResOps formalizes the growing industry recognition that cyber recovery is a cross-functional problem that requires stakeholders from across the business who each have a stake in the outcome.


In the post-Mythos era, that coordination is critical to both support ongoing readiness and enable a fast, effective response to an incident. Frontier AI makes a tested, well-defined clean recovery process a baseline requirement.


### See the Full Webinar


Watch the full Resilience Over Panic session on demand to explore our four-step framework in more detail, including the requirements for agentic AI recovery.


Register[here](https://discover.commvault.com/webinar-resilience-over-patching-registration.html) for the webinar.


### FAQs


**Q: What is mean time to clean recovery (MTCR)?**


**A:** Mean time to clean recovery (MTCR) measures how long it takes an organization to return to a verified, clean operating state after an incident. It’s a broader measure than simply how long it takes to restore data. It combines the traditional recovery time objective (RTO) with the additional time needed to confirm recovered data is free of malware or backdoors, plus a final human validation step before systems return to production.


Organizations are increasingly treating MTCR, rather than recovery speed alone, as the board-level metric for resilience, since a fast recovery that reintroduces an active threat can cause more damage than a slower, verified one.


**Q: How is MTCR different from RTO?**


**A:** RTO measures how quickly systems and data can be restored after a disruption. MTCR includes RTO as one component, but adds the time needed to confirm that restored data is clean and the time spent on human validation before systems go back into production. In a cyber incident specifically, a system can meet its RTO and still fall short of true resilience if the restored environment is reinfected shortly afterward.


**Q: What is a minimum viable company, and how is it different from a full disaster recovery plan?**


**A:** A minimum viable company, sometimes called a minimum viable business, is the smaller, business-prioritized subset of systems, data, and dependencies an organization needs back online to keep functioning after an incident, rather than its entire IT estate.


A full disaster recovery plan typically aims to restore everything, in time; a minimum viable company definition forces an organization to decide in advance what truly has to come back first, and in what sequence, to avoid an operational shutdown.


**Q: What’s the difference between a tabletop exercise and a live recovery drill?**


**A:** A tabletop exercise is a paper-based walkthrough of an incident response plan, typically used to test decision-making and communication among stakeholders without actually executing any technical recovery steps.


A live recovery drill goes further by actually performing a recovery, using the real tools, automation, and people involved, to confirm the process works in practice, beyond what a paper exercise can show. Organizations that rely only on tabletop exercises may have a resilience plan that looks sound on review but hasn’t been tested against the operational details that tend to make real incidents take longer than expected.


**Q: What are non-human identities (NHIs), and why do they complicate AI recovery?**


**A:** NHIs are the credentials, permissions, and access rights assigned to software components, such as AI agents, rather than to individual people. As organizations deploy more agentic AI, the number of NHIs in an environment grows, and each one needs to be accounted for during a recovery alongside more familiar elements like databases and transactional systems.


Recovering an agentic AI system typically requires synchronizing NHIs with the rest of the AI stack, since restoring data or configurations without restoring the correct agent permissions can leave gaps that are difficult to detect until something breaks downstream.


**Q: How should an organization get started with ResOps™ (resilience operations)?**


**A:** ResOps is a cross-functional framework that brings security, operations, and technology teams together around a shared definition of resilient design, distinct from any single product.


Organizations can begin by identifying a small number of core applications and running an initial recovery test to establish a baseline MTCR, rather than trying to formalize the entire discipline at once. That early baseline gives security, operations, and governance teams a concrete reference point for tracking improvement. It also helps build the cross-team habits that ResOps depends on over time.


[Michael Thelander](https://www.linkedin.com/in/michaeljthelander/) *is Senior Director of Product Marketing at Commvault.*
