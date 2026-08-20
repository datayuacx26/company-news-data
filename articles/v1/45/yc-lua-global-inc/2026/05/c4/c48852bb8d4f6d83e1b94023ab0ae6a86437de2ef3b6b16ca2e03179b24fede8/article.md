---
schema_version: "1.0.0"
document_id: "c48852bb8d4f6d83e1b94023ab0ae6a86437de2ef3b6b16ca2e03179b24fede8"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/the-eu-ai-act-doesnt-have-a-category-for-agents-it-catches-them-anyway/"
published_at: "2026-05-28T15:56:11+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:083243000cb34ea118c89a25aef30fdfe9667ceb747bba19c345a16e1e0e32ac"
---

# The EU AI Act Doesn't Have a Category for Agents. It Catches Them Anyway.

If you ship agents that send emails, take payments, qualify leads, screen CVs, or have any kind of conversation with anyone in Europe, Brussels now considers you a regulated product. Fines top out at €35M, or 7% of global turnover, whichever hurts more. Higher than GDPR. The first chunk of the law has been live for 15 months. The next chunk hits in 67 days. EU enterprise buyers are already gating contracts on it. Most builders still think it's a 2027 problem.


**It isn't.**


Twenty days ago, Brussels delayed part of the Act. The heavy compliance regime for what it calls "high-risk" systems got pushed back by about eighteen months. Every legal blog called it breathing room. Every founder I know read the headline and went back to shipping. The deferral covered one specific slice. Everything else is still on the original timeline. The penalty ceiling didn't move. The transparency rules didn't move. The procurement reality didn't move. And the Commission's own new guideline, published eight days ago, just closed the most popular architectural escape route for multi-agent systems.


We know this because we just spent six months building the runtime to handle it. Lua has 5,000+ agents in production across 160 businesses in 10 countries. Sales agents that close deals on WhatsApp. Support agents that process claims. Booking agents that take money. These are not chatbots. They take real actions with real consequences. And every serious enterprise buyer in Europe now asks the same three questions before signing: what's your classification, where are your logs, who's your authorised representative.


Compliance is an engineering problem. Most builders are still treating it as a legal one. That category error is about to be expensive.


## >Three Dates


Three checkpoints sit on the calendar today:


**Live since February 2025.** Outright bans on subliminal manipulation, social scoring, emotion recognition in workplaces and schools, untargeted face scraping. Penalty ceiling: **€35M or 7% of global turnover.**


**67 days from now, 2 August 2026.** Transparency rules apply. Every conversational AI must tell people they're talking to AI at first interaction. **This date did not move.** The Omnibus left it alone.


**18 months out, 2 December 2027.** The heavy regime for "high-risk" systems: conformity assessments, technical documentation, deployer duties, fundamental-rights impact assessments. This is the bit that got deferred from August 2026.


The Act applies even if you have zero EU presence. A Lisbon employee using your hiring agent puts you in scope. A Berlin customer's data flowing through your support agent puts you in scope. The reach is modelled on GDPR and just as broad.


## >The Failure Mode


The Act was drafted before "agent" became the dominant deployment pattern. The drafters defined an "AI system" functionally: anything that operates with autonomy and infers outputs from inputs. Most builders read that and think chatbot. The same definition catches anything that plans, calls tools, and executes autonomously.


The Commission's AI Office confirmed this in writing earlier this month. From the official FAQ: *"the rules applicable to AI systems and GPAI models under the AI Act also apply to AI agents."* And the kicker for anyone shipping on top of a frontier model: *"factors like the level of autonomy or tool use of the model can be decisive in the designation of the model as a model with systemic risk."*


The category was broad before agents existed. It is now retroactively catching them.


> The Act regulates AI systems. AI agents are AI systems. The drafters didn't need a new category. They wrote a definition that was always going to catch this.


## >The Anti-Circumvention Rule


Eight days ago, on 19 May 2026, the Commission published draft guidelines on high-risk classification. The paragraph that matters: multi-agent systems cannot be split to dodge classification. The Commission's own language puts agentic AI explicitly in scope when *"linked actions or components serve in conjunction an intended high-risk purpose."*


We've been watching builders try this for months. Break a workflow into "orchestrator plus three sub-agents" because no single component looks high-risk. The Commission just closed that escape hatch. The combined configuration is one system. If you're running multi-agent supervision (Spaces, multi-step planners, anything coordinating specialists), the whole graph is the unit of analysis. Build accordingly.


## >The Chatbot Defence Is Dead


High-risk classification isn't about UI. It's about use case. The list includes:


- **Employment.** CV screening, candidate ranking, performance evaluation, task allocation.
- **Essential services.** Credit scoring, life and health insurance pricing, emergency triage, eligibility for benefits.
- **Education.** Admissions, grading, exam monitoring.
- **Biometrics, critical infrastructure, law enforcement, migration, justice.**


An agent that "just helps the team" with hiring is high-risk. An agent that "just answers questions" about loan eligibility is high-risk. A ten-person startup using an AI-powered ATS to screen EU applicants is in scope. No size threshold. No SME exemption.


The piece most builders underestimate completely: deployers carry their own obligations. If a customer uses your agent in a high-risk context, *they* must assign trained humans, retain logs for six months, report serious incidents within 15 days, and run a fundamental-rights impact assessment for credit, insurance, public services, or government work. You cannot outsource those by waving your certifications. Your enterprise customer will ask you for the technical documentation, the oversight interface, the audit logs, and the FRIA inputs. You either have them, or you don't get the deal. The contract conversation is happening now, not in 2027.


## >The Agents Are Already Misbehaving in Public


**Kiro, February.** Per the Financial Times, Amazon's Kiro coding agent decided the best course of action was to "delete and recreate the environment" of AWS Cost Explorer. Thirteen-hour outage. A senior AWS employee told the FT: *"The engineers let the AI agent resolve an issue without intervention. The outages were small but entirely foreseeable."* Amazon disputed the framing, but the autonomy question is now in the regulatory record.


**OpenClaw, also February.** A GitHub account running OpenClaw targeted Scott Shambaugh, a volunteer maintainer of matplotlib, after Shambaugh rejected its pull request. The agent published a ~2,000-word post titled *"Gatekeeping in Open Source: The Scott Shambaugh Story"* and amplified it across GitHub. Shambaugh called it *"an autonomous influence operation against a supply chain gatekeeper."* It was.


These aren't the agents you build. They're the reason the Commission's anti-circumvention guideline reads the way it does.


## >The Build Order


The good news: this is a solvable engineering problem if you start now. Here's the order we ran. Run it in this order. The dependencies matter.


**1. Inventory and classify every agent.** What tools can it call. What data it touches. Who uses it. Which high-risk category could it fall into. "Probably fine" is not a classification. Write it down. Sign it. Sharma lists missing classification rationale as one of the most common evidence gaps in deployer readiness work. Fix it before you need it.


**2. Build the audit trail before you need it.** Deployers of high-risk systems must retain auto-generated logs for at least six months. Providers must design for record-keeping. Tamper-evident logging, hash-chained and append-only, is cheap to add at design time. Retrofitting it is brutal. You will not reconstruct six months of agent behaviour from fragments while a regulator waits.


**3. Instrument oversight where it matters.** The Act requires designs that allow effective human oversight, with automation-bias mitigation and the ability to decide, in any particular situation, not to use the system. For agents this means interception before consequential tool calls: payments, contract changes, account deletions, public communications. Not a dashboard nobody watches. Reserve human approval for actions that are irreversible, regulated, or externally binding. Constrain everything else deterministically. If your agent can reason its way around a constraint, the constraint doesn't exist.


**4. Disclose AI use at first interaction.** Transparency obligations hit in 67 days. Every conversational agent needs a clear, up-front disclosure. The Commission's draft transparency guidelines explicitly name chatbots, voice assistants, *"agentic AI and coding agents,"* and *"bots on social networks."* The "obvious from context" carve-out is narrow. Don't rely on it.


**5. Pick a standard and document against it.** ISO/IEC 42001 is becoming an EU procurement requirement; coverage of the Act's process layer ranges from 40% to 80% depending on implementation. NIST AI RMF gives you a shared vocabulary with US procurement. OWASP Top 10 for Agentic AI is the most agent-specific threat model available. Pick one. Map. Document.


**6. Name the human owner.** The Act requires *"natural persons who have the necessary competence, training and authority, as well as the necessary support."* Not a committee. A person.


A practical aside on tooling. We hit this wall in production. Last month we open-sourced the runtime layer we built to handle it:` governance-sdk` , MIT licensed, zero dependencies. It sits between an agent and its tool calls, enforces policy before execution, produces a hash-chained tamper-evident audit trail, and maps controls against the AI Act, NIST AI RMF, ISO 42001, and OWASP Top 10 for Agentic AI. Framework-agnostic. Adapters for Mastra, Vercel AI SDK, OpenAI Agents, LangChain, Anthropic. Repo:[github.com/lua-ai-global/governance](https://github.com/lua-ai-global/governance?ref=blog.heylua.ai) . Docs:[heygovernance.ai](https://heygovernance.ai/?ref=blog.heylua.ai) . We open-sourced it because the enforcement layer for agents shouldn't be a closed box.


Bolted-on compliance is the expensive path. Embedded compliance from day one is mostly a matter of putting the right interception points in your runtime and writing the policy down once.


## >Compliance Compounds


The transparency rules still hit in 67 days. The prohibitions have been live for 15 months. The penalties exceed GDPR. EU procurement is already gating on AI Act readiness. The Commission's anti-circumvention guideline closed the architectural escape routes three weeks ago. The AI Office is funding adversarial agent evaluations. The agents are already misbehaving in public.


The teams that come through this well will not be the ones who hire the biggest compliance firm in Q3 2027. They'll be the ones who built classification, logging, oversight, and disclosure into their runtime this year and never had to retrofit.


Compliance compounds when you start early. It punishes you when you don't.


If you ship agents that touch Europe, the date that matters isn't December 2027. It's August. Start now.
