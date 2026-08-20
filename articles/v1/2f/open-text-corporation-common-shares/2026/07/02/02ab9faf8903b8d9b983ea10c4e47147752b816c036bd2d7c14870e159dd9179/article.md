---
schema_version: "1.0.0"
document_id: "02ab9faf8903b8d9b983ea10c4e47147752b816c036bd2d7c14870e159dd9179"
company_key: "open-text-corporation-common-shares"
company: "Open Text Corporation Common Shares"
source_id: "open-text-corporation-common-shares-rss-7d5a2b95ba15"
canonical_url: "https://blogs.opentext.com/the-ai-powered-security-operations-center-soc/"
published_at: "2026-07-27T20:03:32+00:00"
first_seen_at: "2026-07-27T20:32:50.065375+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:731fa30419eb24de0d3763810ab2311ffeedc12b33f65728bdb650a2832b3d3d"
---

# The AI-powered security operations center (SOC)

There’s a familiar moment in every SOC shift. Everyday security analysts sift through alerts, jump between dashboards, and write complex SIEM queries just to figure out what’s worth investigating. An AI-powered SOC can help.


The OpenText SIEM AI Bridge tackles this head-on by connecting OpenText Enterprise Security Manager (ESM) to large language models, letting analysts investigate using plain natural language instead of manual queries.


## What AI Bridge does


The AI Bridge acts as a translator between analysts and the SIEM. Ask a question in plain English — i.e. “show me failed logins for this IP” — and it retrieves the data, correlates it across alerts, and returns a reasoned, summarized answer. OpenText ESM stays the source of truth, and role-based access is fully enforced.


## Why local LLM support matters


The Bridge isn’t locked into one AI vendor — it works with Claude, Gemini, OpenAI, or Ollama running locally or air gapped. That local option is huge for security teams: sensitive event data never has to leave the organization’s environment, making it a safe, low-risk way to start an AI adoption journey without rearchitecting infrastructure or sending data to the cloud.


## The working AI-powered SOC


## Getting started in four steps


1. Getting up and running is straightforward. Import theOpenText SIEM_AI_Bridge.arb resource bundle into your OpenText ESM environment.
2. Next, copy theOpenText SIEM_AI_Bridge file to the host machine where the MCP server will be deployed.
3. From there, set up and verify connectivity with OpenText EM.
4. Finally connect any MCP-compatible AI client to the OpenText SIEM AI Bridge server.


That’s it — no rearchitecting your stack, no ripping out existing tooling.


## Seeing it in Action: Claude & Ollama


To demonstrate the AI Bridge in a real-world context, we ran it against two different LLMs side by side — Claude (cloud-based, Sonnet 4.6) and Ollama (on-premises, qwen2.5:14b on a GPU g6.4xLarge instance).


Claude Ollama


Cloud-based On-prem


LLM-Sonnet 4.6 LLM-qwen2.5:14b


Claude Client GPU g6.4xLarge


*The investigation of the IP address x.x.x.x has revealed that there have been 10 failed login attempts from this IP, involving 2 different users and originating from 1 country.*
*Next, let’s check for any process creation activities linked to this IP using the methodOpenText SIEM_processCreationByTargetIPAgent* .


*It appears that the target IP x.x.x.x has been under attack from multiple IPs primarily within the United States, categorized as Denial of Service (DoS) attacks. If you need further investigation or detailed reports on these specific attacks or other aspects, please let me know!*


## Helping with the AI journey


By layering AI reasoning on top of existing SIEM data instead of replacing it, organizations can pilot generative AI in a way that respects governance and compliance requirements already in place — a practical first step toward broader AI adoption.


## Helping analysts, not replacing them


The real win is for the humans in the SOC. Automatic correlation, reasoned summaries, and clear prioritization mean less time hunting and more time deciding. This new capability cuts Mean Time to Investigation, reduces analyst fatigue, and even surfaces attacks standard rules miss, since the LLM reasons over context instead of matching fixed signatures **.**


**Learn more and get started:**[OpenText SIEM AI Bridge | Cybersecurity Marketplace](https://marketplace.opentext.com/cybersecurity/content/arcsight-ai-bridge)
