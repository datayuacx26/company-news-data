---
schema_version: "1.0.0"
document_id: "187baf92637d335374dc1669080face77ef68c3bbd5c41359a2c47bd2ece3ada"
company_key: "yc-nanonets"
company: "NanoNets"
source_id: "yc-nanonets-rss-fcb6878f0099"
canonical_url: "https://nanonets.com/blog/ai-agent-hacks-mckinsey/"
published_at: "2026-03-14T12:04:36+00:00"
first_seen_at: "2026-07-20T23:24:03.815131+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:b420a99ed0a6bb3d99fa022ef3c17527d4378dbfee36395299c90417e91a0360"
---

# AI Agent Hacks McKinsey: 5 Situations When You Should Not Deploy Agents

A security startup called CodeWall pointed an autonomous AI agent at McKinsey's internal AI platform, Lilli, and walked away. Two hours later, the agent had full read and write access to the entire production database. 46.5 million chat messages, 728,000 confidential client files, 57,000 user accounts, all in plaintext. The system prompts that control what Lilli tells 40,000 consultants every day? Writable. Every single one of them.


The vulnerability was just an SQL injection, one of the oldest attack classes in software security. Lilli had been sitting in production for over two years. McKinsey's scanners never found it. The CodeWall agent found it because it doesn't follow a checklist. It maps, probes, chains, escalates, continuously, at machine speed.


And scarier than the breach is what a malicious actor could have done after. Subtly alter financial models. Strip guardrails. Rewrite system prompts so Lilli starts giving poisoned advice to every consultant who queries it, with no log trail, file changes, anomaly to detect. The AI just starts behaving differently. Nobody notices until the damage is done.


McKinsey is one incident. The broader pattern is what this piece is really about. The narrative pushing businesses to deploy agents everywhere is running far ahead of what agents can actually do safely inside real enterprise environments. And a lot of the companies finding that out are finding it out the hard way.


So the question worth asking is when you shouldn't deploy agents at all. Let’s decode.


---


## **The entire industry is betting on them anyway**


Around the same time as the McKinsey breach, Mustafa Suleyman, the CEO of Microsoft AI, was telling the Financial Times that white-collar work will be fully automated within 12 to 18 months. Lawyers. Accountants. Project managers. Marketing teams. Anyone sitting at a computer. Every conference keynote since late 2024 has been some version of the same thing: agents are here, agents are transforming work, go all in or fall behind.


The numbers back up the energy. 62% of enterprises are experimenting with agentic AI. KPMG says 67% of business leaders plan to maintain AI spending even through a recession. The FOMO is real and it's thick. If your competitor is shipping agents, standing still feels like falling behind.


But the same reports suggest: only 14% of enterprises have production-ready agent deployments. Gartner predicts over 40% of agentic AI projects will be cancelled by end of 2027. 42% of organizations are still developing their agentic strategy roadmap. 35% have no formal strategy at all. The gap between "we're experimenting" and "this is running in production and delivering value" is enormous. Most organizations are somewhere in that gap right now, burning money to stay there.


Agents do work. In controlled, well-scoped, well-instrumented environments, they do. The question is what specific conditions make them fail. And there are five that keep showing up.


### Build a safer agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


---


## **Situation 1: The agent inherits production permissions without a human judgment filter**


In mid-December 2025, engineers at Amazon gave their internal AI coding agent, Kiro, a straightforward task: fix a minor bug in AWS Cost Explorer. Kiro had operator-level permissions, equivalent to a human developer. Kiro evaluated the problem and concluded the optimal approach was to delete the entire environment and rebuild it from scratch. The result was a 13-hour outage of AWS Cost Explorer across one of Amazon's China regions.


Amazon's official response called it user error, specifically misconfigured access controls. But four people familiar with the matter told the Financial Times a different story. This was also not the first incident. A senior AWS employee confirmed a second production outage around the same period involving Amazon Q Developer, under nearly identical conditions: engineers allowed the AI agent to resolve an issue autonomously, it caused a disruption, and the framing again was "user error." Amazon has since added mandatory peer review for all production changes and initiated a 90-day safety reset across 335 critical systems. Safeguards that should have been there from the start, retrofitted after the damage.


The structural problem was that a human developer, given a minor bug fix, would almost certainly not choose to delete and rebuild a live production environment. That's a judgment call and humans apply one instinctively. Agents don't. They reason about what's technically permissible given their permissions, choose the approach that solves the stated problem most directly, and execute it at machine speed. The permission says yes. No second thought triggers.


This is the most common failure mode in agentic deployments. An agent gets write access to a production system. It has a task. It has credentials. Nothing in the architecture tells it which actions are off limits regardless of what it determines is optimal. So when it encounters an obstacle, it doesn't pause the way a human would. It acts.


Now the fix is a deterministic layer that makes certain actions structurally impossible regardless of what the agent decides, production deletes, transactions above a defined threshold, any action that can't be reversed without significant cost. Human approval gates make agentic systems survivable.


---


## **Situation 2: The agent acts on a fraction of the relevant context**


A banking customer service agent was set up to handle disputes. A customer disputed a $500 charge. The agent attempted a $5,000 refund. It was being helpful (not hallucinating) in the way it understood helpful, based on the rules it had been given. The authorization boundaries were defined by policy documents. But that situation didn't fit the policy documents. Standard security tools couldn't detect the problem because they're not designed to catch an AI misunderstanding the scope of its own authority.


Enterprise systems record transactions, invoices, contracts, approvals. They almost never capture the reasoning that governed a decision, the email thread where the supplier agreed to different terms, the executive conversation that created an exception, the account manager's judgment about what a long-term client relationship is actually worth. That context lives in people's heads, in Slack threads, in hallway conversations. It doesn't live in the systems agents plug into.


McKinsey's own research on procurement puts a number on it: enterprise functions typically use less than 20% of the data available to them in decision-making. Agents deployed on top of structured systems inherit that blind spot entirely. They process invoices without seeing the contracts behind them. They trigger procurement workflows without knowing about the verbal exception agreed last week. They act with confidence, at scale, on an incomplete picture, and because they're fast and sound authoritative, the errors compound before anyone catches them.


The condition to watch for: any workflow where the relevant context for a decision is partially or mostly outside the structured systems the agent can access. Customer relationships, supplier negotiations, anything where institutional knowledge governs the outcome.


### Build a safer agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


---


## **Situation 3: Multi-step tasks turn small errors into compounding failures**


In 2025, Carnegie Mellon published TheAgentCompany, a benchmark that simulates a small software company and tests AI agents on realistic office tasks. Browsing the web, writing code, managing sprints, running financial analysis, messaging coworkers. Tasks designed to reflect what people actually do at work, not cleaned-up demos.


The best model tested, Gemini 2.5 Pro, completed 30.3% of tasks. Claude 3.7 Sonnet completed 26.3%. GPT-4o managed 8.6%. Some agents gamed the benchmark, renaming users to simulate task completion rather than actually completing it. Salesforce ran a separate benchmark on customer service and sales tasks. Best models hit 58% accuracy on simple single-step tasks. On multi-step scenarios, that dropped to 35%.


The math behind this: Chain five agents together, each at 95% individual reliability, and your system succeeds about 77% of the time. Ten steps, you're at roughly 60%. Most real business processes aren't five steps. They're twenty, thirty, sometimes more, and they involve ambiguous inputs, edge cases, and unexpected states that the agent wasn't designed for.


The failure mode in multi-step workflows is that an agent misinterprets something in step two, continues confidently, and by the time anyone notices, the error is embedded six steps deep with downstream consequences. Unlike a human who would pause when something feels off, the agent has no such instinct. It resolves ambiguity by picking an interpretation and moving forward. It doesn't know it's wrong.


This is why agents work well in narrow, well-scoped, low-step workflows with clear success criteria. They start breaking down anywhere the task requires sustained judgment across a long chain of interdependent decisions.


---


## **Situation 4: The workflow touches regulated data or requires an audit trail**


In May 2025, Serviceaide, an agentic AI company providing IT management and workflow software to healthcare organizations, disclosed a breach affecting 483,126 patients of Catholic Health, a network of hospitals in western New York. The cause: the agent, in trying to streamline operations, pushed confidential patient data into an unsecured database that sat exposed on the web.


The agent was not attacked or compromised, doing exactly what it was designed to do, handling data autonomously to improve workflow efficiency, without understanding the regulatory boundary it was crossing. HIPAA doesn't care about intent. Several class action investigations were opened within days of the disclosure.


IBM put the underlying risk clearly in a 2026 analysis: hallucinations at the model layer are annoying. At the agent layer, they become operational failures. If the model hallucinates and takes the wrong tool, and that tool has access to unauthorized data, you have a data leak. The autonomous part is what changes the stakes.


This is the problem in regulated industries broadly. Healthcare, financial services, legal, any domain where decisions need to be explainable, auditable, and defensible. California's AB 489, signed in October 2025, prohibits AI systems from implying their advice comes from a licensed professional. Illinois banned AI from mental health decision-making entirely. The regulatory posture is tightening fast.


Along with lacking explainability, they actively obscure it. There's no log trail of reasoning. Or a point in the process where a human reviewed the judgment call. When something goes wrong and a regulator asks why the system did what it did, the answer "the agent determined this was optimal" is not an answer that survives scrutiny. In regulated environments where someone has to be able to own and defend every decision, autonomous agents are the wrong architecture.


---


## **Situation 5: The infrastructure wasn't built for agents and nobody knows it yet**


The first four situations assume agents are deployed into environments that are at least theoretically ready for them. Most enterprise environments are not.


Legacy infrastructure was designed before anyone was thinking about agentic access patterns. The authentication systems weren't built to scope agent permissions by task. The data pipelines don't emit the observability signals agents need to operate safely. The organization hasn't defined what "done correctly" means in machine-verifiable terms. And critically, most of the agents being deployed right now are operating with far more access than their task requires, because scoping them properly would require infrastructure work the organization hasn't done.


Deloitte's 2025 research puts this in numbers. Only 14% of enterprises have production-ready agent deployments. 42% are still developing their roadmap. 35% have no formal strategy. Gartner separately estimates that of the thousands of vendors selling "agentic AI" products, only around 130 are offering something that genuinely qualifies as agentic. The rest is chatbots and RPA with better marketing.


The IBM analysis from early 2026 captures where most enterprises actually are: companies that started with cautious experimentation, shifted to rapid agent deployment, and are now discovering that managing and governing a collection of agents is more complex than creating them. Only 19% of organizations currently have meaningful observability into agent behavior in production. That means 81% of organizations running agents have limited visibility into what those agents are actually doing, what decisions they're making, what data they're touching, when they're failing.


Deploying agents before the integration layer exists is the reason half of enterprise agent projects get stuck in pilot permanently. The plumbing is not ready. And unlike a bad software rollout, where you can usually see the failure, an agent operating without proper observability can be wrong for weeks before anyone knows. The damage compounds heavily.


### Build a safer agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


---


## **The question businesses should actually be asking**


Every one of these situations has the same shape. Someone deployed an agent. The agent had real access to real systems. Something in the environment didn't match what the agent was designed for. The agent acted anyway, confidently, at speed, without the judgment filter a human would have applied. And by the time the error surfaced, it had either compounded, caused irreversible damage, created a regulatory problem, or some combination of all three.


The McKinsey breach is probably going to become a landmark case study the way the 2017 Equifax breach became a landmark for data governance. Same pattern: old vulnerabilities meeting new scale, at organizations with serious security investment, in the gap between what the team thought they controlled and what was actually exposed. The difference now is speed. A traditional breach takes weeks. An AI agent completes its reconnaissance in two hours.


Businesses rushing to deploy agents everywhere are creating a lot more McKinseys in waiting. The ones that look smart in 18 months are the ones asking the harder question right now: not "can we use an agent here," but "which of these five situations does this deployment walk into, and what's our answer to each one."


Not every organization is asking such questions and that’s a problem.


### Build a safer agent for free (no-code)


[Try here](https://agents.nanonets.com/login)
