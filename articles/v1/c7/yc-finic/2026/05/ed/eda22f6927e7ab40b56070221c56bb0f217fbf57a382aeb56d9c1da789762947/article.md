---
schema_version: "1.0.0"
document_id: "eda22f6927e7ab40b56070221c56bb0f217fbf57a382aeb56d9c1da789762947"
company_key: "yc-finic"
company: "Finic"
source_id: "yc-finic-news-import-190a3b31c7b4"
canonical_url: "https://finic.ai/blog/fraud-investigation-skills-for-ai-agents"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-25T04:53:11.299519+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:4051f12f465fd2d39725186e6a1d2bfa289d35f996d1db511fbb1598a16f091f"
---

# Open-source fraud investigation skills for AI agents

## TL;DR: get the skill


If you are just here for the skill, the repo is open source:[github.com/finic-ai/fraud-investigation-skills](https://github.com/finic-ai/fraud-investigation-skills) .1


Install through the Claude plugin marketplace from inside a Claude Code session:


```text
/plugin marketplace add finic-ai/fraud-investigation-skills
/plugin install fraud-investigation-skills
```


Or install manually: clone the repo, copy any skill folder into your project's` .claude/skills/` directory for Claude Code and Claude Cowork, or into` ~/.codex/skills/` for OpenAI Codex CLI. Start a session and ask your agent to investigate a case. It will load the right skill on its own.2


## A small number of organizations now produce most of the fraud you see


Fraud is no longer a long tail. Most of the loss on your books today is the work of a few well-funded rings that operate like product companies. They run multi-month campaigns, hold reserve inventory of synthetic identities, and rotate tactics on a clock measured in hours. They are not slowed down by procurement, model governance, or regulatory review, and they have the same access to the latest AI tools that your engineering team has.


Experts in the field agree that the majority of fraud events at financial institutions trace back to organized rings, even cases that initially present as customer-coerced or first-party fraud. U.S. losses enabled by generative AI are projected to grow from $12.3B in 2023 to $40B by 2027, a 32% CAGR. FinCEN issued its first formal deepfake alert in late 2024 after suspicious-activity reporting began describing AI-generated identity attacks at scale.34


Despite all of this, most fraud programs still treat each event as one alert at a time, which is roughly the equivalent of fighting a coordinated adversary with a help-desk queue. Rings can stage attacks, exfiltrate funds, and pivot tactics inside a single shift while a risk team is still scoping a model update.


## Why AI agents are the right tool for fraud investigations


Fraud investigation is one of the few workflows where the structural fit for an AI agent is almost too clean. Three reasons.


They write code and SQL fluently. Most investigation work is structured analysis: pulling events for an entity, joining across device, IP, payment, and behavior tables, building feature windows, comparing against known patterns. An agent that can read your schema and write a query has a far shorter path to evidence than an analyst stitching together BI dashboards and ad-hoc exports.


They have general knowledge of fraud trends. A foundation model has already read the public corpus on synthetic identity, money mules, account takeover, dispute rings, business email compromise, deepfake KYC bypass, and the rest of the catalog. It knows the shape of these attacks before you brief it, which means a half-decent first pass costs you nothing.


They run 24/7 and scale to whatever the queue looks like. Fraud queues do not respect business hours, and the most damaging rings deliberately stage activity overnight or across weekends. An agent that can pick up a new alert at 3 a.m. and have evidence ready by the time the shift starts changes what is even possible inside a fraud program.


## What AI agents are still missing


The gap between an agent that can investigate and an agent that investigates well is specific knowledge.


A foundation model knows what fraud looks like in general. It does not know what fraud looks like at your institution this week. The patterns a fraud director cares about today (a specific device fingerprint cluster, a new mule corridor, a synthetic-identity template that just started clearing onboarding at three peer banks) are exactly the patterns that did not exist in the pretraining set. They may not exist in the same form next week either.


In fraud, the devil is in the details. Generic knowledge tells you that synthetic identities exist. Specific knowledge tells you which combination of EIN-only credit history, phone carrier, and postal route is being used to clear onboarding right now. The difference between those two is the difference between catching a ring when they apply for an account and catching them after they have moved millions through your platform.


## Finic's Fraud Investigation Skills


Finic's Fraud Investigation Skills are the first Agent Skills purpose-built for fraud analysts and investigators. Each skill encodes a specific investigation move (entity expansion, device-cluster discovery, behavior-window comparison, dispute-ring triage, mule-network tracing) with the prompts, file structure, and reasoning patterns that consistently produce useful results inside a real case.12


They close the specific-knowledge gap by acting as a portable instruction set you drop next to your agent: written by people who see fraud across institutions, sourced from patterns visible across the 15M accounts and $50B in deposits on the Finic network, and updated regularly. The playbook your agent loads this Monday reflects what is actually happening in the wild this Monday, not what was true at training time.5


The skills get much sharper when paired with the Finic MCP server, coming soon. The MCP server provides live fraud fingerprints from across the Finic network so your investigation agent can proactively search for rings active on your platform, using the same skills but pointed at signal that is current as of this morning. The skills teach your agent how to investigate. The MCP server tells your agent what to investigate today.


If you want early access to the MCP server, write tosales@finic.ai with "early access" in the subject.


## Get the skills today


The repo is open source:[github.com/finic-ai/fraud-investigation-skills](https://github.com/finic-ai/fraud-investigation-skills) . Install through the Claude plugin marketplace or drop a skill folder into your agent manually. Then run it against a case you already know the answer to. That is the fastest way to know whether the skills earn their seat in your investigation stack.1
