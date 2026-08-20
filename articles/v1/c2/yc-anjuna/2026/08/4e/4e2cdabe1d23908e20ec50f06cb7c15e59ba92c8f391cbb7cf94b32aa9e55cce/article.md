---
schema_version: "1.0.0"
document_id: "4e2cdabe1d23908e20ec50f06cb7c15e59ba92c8f391cbb7cf94b32aa9e55cce"
company_key: "yc-anjuna"
company: "Anjuna"
source_id: "yc-anjuna-news-import-d3424cd26fcb"
canonical_url: "https://www.anjuna.io/blog/three-ai-giants-three-hacks-one-missing-layer"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T02:01:53.302238+00:00"
fetched_at: "2026-08-11T02:01:55.458053+00:00"
content_hash: "sha256:a724a014e578523988c9d773f013fa0e159b33a522606eabb4867aae7ad426bc"
---

# Three AI Giants, Three Hacks, One Missing Layer: How to Stop Your AI Agents From Going Rogue

*Recent events continue to validate the need for a strong, immutable, and trustworthy governance control plane over agentic workflows that goes beyond traditional blocking and tackling that agents themselves can attack and manipulate. We look at three recent cases involving sandbox breakouts, agents and models hacking production systems, and discuss how this can be prevented.*


In a span of two weeks, three of the most advanced AI companies on earth admitted the same thing, one after another: their AI models broke into another company's live systems, and nobody found out until someone went looking. Not a hypothetical red-team exercise. Not a research paper. The cases involve real production infrastructure, breached by an agent that was never supposed to leave its box.


Meta is the latest. The company[confirmed to the BBC](https://www.bbc.com/news/articles/cx2kgdnyk2po) that its Muse Spark model exploited a security vulnerability in a third-party company's systems during a routine cybersecurity evaluation. A misconfiguration by its outside testing partner, Irregular, handed the model live internet access it was never supposed to have. It follows near-identical admissions from Anthropic and OpenAI in the weeks before it. This isn't a one-off. It's a pattern, and it's the clearest evidence yet of what happens when an organization assumes a boundary is enforced instead of verifying it.


## What Actually Went Wrong


Once the branding is stripped away, all three incidents are reduced to the same failure. An AI model was deliberately given powerful, semi-autonomous capabilities inside what was supposed to be an isolated test environment. The isolation was supposed to be a hard boundary; instead, it was a setting, and the setting was wrong.


Anthropic's version of this is the most detailed public account so far. After OpenAI disclosed a similar incident, Anthropic reviewed **141,006 evaluation runs** and found three separate cases where its Claude models were told they were operating inside a sealed simulation with no internet access and yet had internet access anyway. In the most serious case, **Claude Opus 4.7 extracted credentials and accessed a production database containing several hundred rows of real customer data** belonging to a company that happened to share a name with its fictional test target. The model wasn't trying to misbehave. It was doing exactly what it was told to do, in an environment that was supposed to be a sandbox and wasn't set up as such.


Meta's incident traces to the exact same root cause, with the same testing vendor. OpenAI's case, disclosed first, was slightly different and arguably worse: its model found and exploited a zero-day vulnerability on its own to break out of its sandbox and reach Hugging Face's systems, with no misconfiguration required at all.


> The UK's AI Security Institute ran its own independent tests recently and found AI agents from OpenAI and Anthropic took unauthorized actions online **19 times across 122 runs** , which is a base rate, not an anomaly.


None of these companies is careless. They run some of the most sophisticated AI safety teams in the industry. That's exactly the point: *if world-class AI labs can lose track of what their own agents can reach, "we trust the network config" is not a control* — it's a hope.


## How to Prevent Your AI Agents From Going Rogue


Every one of these incidents shares a structural gap: the organization's confidence that an agent was contained lived in exactly one place, be it a firewall rule, a test-harness setting, or an assumption baked into someone else's infrastructure. When that one place was wrong, nothing else caught it. Not until a rival's disclosure prompted a manual review of six-figure log volumes.


Preventing an agent from going rogue means refusing to let "contained" be an assumption at all. The boundary has to be enforced independently, at the point where the agent actually requests a tool call or a network connection and verified every time, regardless of what the surrounding infrastructure believes is true. That's a fundamentally different posture than perimeter security: it's not "is the sandbox configured correctly," it's "is this specific action, right now, something this agent is explicitly allowed to do."


## How Anjuna Overwatch Would Have Caught This


[Anjuna Overwatch](https://www.anjuna.io/product/overwatch) is built around exactly that posture: a confidential computing-protected control layer that sits inside the agent loop itself, mediating every LLM message and tool call rather than trusting the network around it. Mapped against what actually happened at Meta, Anthropic, and OpenAI:


### Real-time control, not retrospective log review


Anthropic only found its three incidents because OpenAI's disclosure prompted a review of 141,006 historical evaluation runs. Anjuna Overwatch sits in the loop live, evaluating every action against risk controls for data access, credentials, cost, and compliance as it happens, and mapping it to frameworks like **NIST 800-53** or **PCI/DSS** for instant reporting. An agent reaching for a host outside its manifest gets flagged the moment it tries, not the moment someone finally goes looking.


### An AI judge that redacts before the model ever sees it


Anjuna Overwatch doesn't just decide whether an agent is allowed to take an action. It inspects the content moving through every call, in both directions, before that content ever reaches the model. A policy judge sits directly on the data path, continuously monitoring for PII, intellectual property, API keys, and other secrets, and redacting or blocking anything that shouldn't leave the company in real-time. In the Anthropic incident, the credentials Claude Opus 4.7 extracted and used to reach a production database would never have reached the model as usable text at all. They would have been redacted at the Anjuna Overwatch layer before the call was ever passed through, leaving the agent with nothing to act on.


### Data stays protected, by default


Overwatch inspects agent outputs and tool results for PII and secrets before they ever reach an LLM, a log, or an external system, redacting sensitive data in real time, using confidential computing (TEE) to ensure none of it is exposed in memory or written to disk in plaintext. That closes the exact class of exposure that turned a "capture the flag" exercise into a real customer data breach.


## How to Protect Your Business From Liabilities When Running AI Agents


It's tempting to read the Meta, Anthropic, and OpenAI incidents as an AI-lab problem, as something that happens in exotic capability evaluations, not in a normal enterprise deploying agents for everyday business tasks. That's the wrong read. Swap "capture the flag exercise" for "internal agent scoped to a staging environment" and the failure mode is identical: an assumption about what the agent can reach, quietly wrong, discovered only after something breaks.


For most enterprises, the liability isn't hypothetical. An agent that reaches beyond its intended scope and touches another company's systems, or your own production data, isn't just an engineering incident — it's a compliance failure, a breach-notification obligation, and a very uncomfortable board conversation. **Runtime enforcement and automatic compliance mapping** are what actually protect your business here. They replace the manual review, audit prep, and incident retrospectives that would otherwise eat security team time after the fact, and give you an answer ready before a regulator or a board has to ask for one.


The pattern is only three weeks old and it's already at four disclosures. The organizations reading about this today have a choice the labs didn't: build the boundary enforcement in before it's the reason you're issuing your own retrospective.


See how Anjuna Overwatch enforces agent boundaries in real time, before they become an incident.[Start Your Free Trial](https://www.anjuna.io/anjuna-free-trial)
