---
schema_version: "1.0.0"
document_id: "2ac6b36d338b49592a16f5d091ca34e325165ef48d65d279a57c5d61a363aba4"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/jfrog-and-openai-collaboration-on-zero-day-security-findings/"
published_at: "2026-07-27T21:30:01+00:00"
first_seen_at: "2026-07-28T09:29:32.217126+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:cb8e3ae5f654fc60af2838b8112f3a3dd6455714f1f032e1847a93364b891f74"
---

# Fast Remediation Is the New Trust Model: JFrog and OpenAI Collaboration on Zero-Day Security Findings

Just last week, OpenAI and Hugging Face jointly[disclosed](https://openai.com/index/hugging-face-model-evaluation-security-incident/) what may be the first incident of its kind: during an internal evaluation of frontier cyber capabilities, OpenAI’s models, running deliberately without production safeguards in an isolated research environment, autonomously discovered and employed chained vulnerabilities to escape its sandbox, reach the open internet, and extract evaluation answers from Hugging Face’s infrastructure.


The industry is right to pay attention. This is a preview of a world where software, not humans, probes, chains, and exploits vulnerabilities at machine speed. We want to share how the JFrog and OpenAI teams collaborate on security incidents to drive them to resolution, and why we believe the outcome demonstrates the trust model the industry now needs.


## JFrog’s role, and how we operate


During a security evaluation, OpenAI’s models identified previously unknown zero-day vulnerabilities in self-hosted Artifactory installations that could be exploited to gain unintended internet access.


OpenAI’s security team disclosed the vulnerabilities to us responsibly and immediately. Our security team treated the report with the urgency it deserved, as a genuine zero-day unknown to the world, and moved accordingly. We developed, validated, and released a fix for all JFrog customers, self-hosted and cloud alike. Cloud customers are already protected; self-hosted customers have been notified to upgrade to the fixed versions referenced in our security advisory. *([Artifactory 7.161](https://docs.jfrog.com/releases/docs/artifactory-self-managed-releases#artifactory-7161) )*


This is not the first time we’ve worked shoulder-to-shoulder with OpenAI’s security and red teams, and it won’t be the last. Our teams collaborate continuously to identify and patch vulnerabilities before they can be exploited in the wild, publish CVEs, and credit the researchers behind each finding. The software ecosystem is already reaping the benefits: pairing expert AppSec teams with sophisticated AI models means **vulnerabilities are discovered and remediated faster than ever.**


## The uncomfortable good news


There is an important, and frankly optimistic, lesson buried in this incident: AI models are becoming extraordinary zero-day discovery engines.


The same capability that lets a model find an exploit path no human had found is the capability that will let defenders find and eradicate those paths first. OpenAI made this exact point in their disclosure, and we agree: advanced cyber-capable models should be put to work helping security teams discover weaknesses before attackers do, understand how vulnerabilities chain together, and remediate them at machine speed.


But this only works under one condition: **responsible vendors must react immediately.** A zero-day found by a model and disclosed to a vendor who sits on it for weeks is a gift to attackers. A zero-day found, disclosed, patched, and shipped to every customer at top speed is the security flywheel the entire community benefits from, especially for the critical infrastructure that runs on this software. That is the standard we held ourselves to here, and the standard we will always hold.


## Our commitment


Software supply chain attacks are no longer exclusively human. Cyber models are the new red team, a powerful new supplier of security signals. The trust model for this new era is simple to state and hard to execute: you need to know exactly what’s running, detect fast, disclose responsibly, and remediate immediately, everywhere, for every customer. That is what JFrog as a trusted system of record is built for.


We will continue to work directly with OpenAI, with any security or red team that brings us an issue, and with the broader community, to make sure that whenever the next zero-day is found, whether by a human or by a model or agent, the fix reaches every customer, cloud and self-hosted alike, as fast as possible.
