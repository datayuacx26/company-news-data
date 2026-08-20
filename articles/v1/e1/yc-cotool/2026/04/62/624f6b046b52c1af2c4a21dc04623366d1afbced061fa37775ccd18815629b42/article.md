---
schema_version: "1.0.0"
document_id: "624f6b046b52c1af2c4a21dc04623366d1afbced061fa37775ccd18815629b42"
company_key: "yc-cotool"
company: "Cotool"
source_id: "yc-cotool-news-import-db39e53ee409"
canonical_url: "https://www.cotool.ai/blog/beyond-ctfs-evaluating-ai-agents-on-real-intrusion-data"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-21T15:09:13.517815+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:be2f4e2bd35d9f395cedb785e3cce1a056a75ca9df77a213d4b9c90eb46f65a1"
---

# Beyond CTFs: Evaluating AI Agents on Real Intrusion Data

# Beyond CTFs: Evaluating AI Agents on Real Intrusion Data


We benchmarked frontier models on a real macOS infostealer intrusion. This is not a CTF, which tend to test narrow, artificial scenarios. Tasks spanned incident response, threat hunting, and detection engineering.


Research


April 13, 2026


Eddie Conk


Cofounder, Head of AI


TL;DR: We partnered with[Threat Hunting Labs](https://www.threathuntinglabs.com/) to build a new benchmark around a real macOS infostealer intrusion and evaluated 9 frontier models across incident response, threat hunting, and detection engineering. GPT-5.4 achieved the highest overall accuracy at 87%, while Gemini 3.1 Pro dominated threat hunting at 89%. Kimi K2.5 offered the best value at $0.36/task for 68% accuracy. Full interactive results are live[on our research site](https://www.cotool.ai/research/macos-threat-investigation) .


## The problem with CTFs


Most security benchmarks are built on Capture the Flag competitions. CTFs are great for training humans, but they have real limitations when used to evaluate AI agents for production security work.


### Offensive by design


CTFs overwhelmingly test offensive skills: exploitation, binary pwning, web app attacks. That's not the work we're evaluating. Defensive security operations like investigating alerts, hunting through telemetry, reconstructing kill chains, and writing detections require fundamentally different reasoning. A model that can pop a shell doesn't necessarily know how to triage a real incident.


### Toy environments, not production environments


CTF challenges are self-contained puzzles. They don't resemble what a SOC analyst actually encounters. Real investigations involve messy telemetry from multiple sources, noisy logs, and ambiguous signals across endpoint, network, and identity data. A model that can solve a contrived forensics challenge may still struggle to correlate events across 14 log sources during a real investigation.


### Data contamination


Public CTF datasets are actively mined by frontier labs for both pre-training and post-training. BOTSv3 answers are discussed in detail across the internet. Cybench and NYU CTF challenges are open source. When a model has likely seen the answers during training, benchmark scores tell you more about memorization than capability. We've found some evidence of this in our own evaluations, where models occasionally produce answers that look more like recall than reasoning.


We still run CTF-based benchmarks (BOTSv3, Cybench, NYU CTF) because they're useful reference points. But we wanted something that better approximates the work defensive security teams actually do.


## Real intrusion data, controlled environment


As part of our research effort, we partnered with[Threat Hunting Labs](https://www.threathuntinglabs.com/) to develop a more realistic dataset. We captured a genuine Odyssey Stealer compromise in a controlled lab environment. Odyssey is a macOS infostealer delivered via a trojanized Ledger Live application. It harvests credentials, exfiltrates keychain and browser data over HTTP, and installs LaunchDaemon persistence. This is real malware executing a real kill chain, not a simulation.


The resulting dataset contains 416K+ events across 14 log sources: EDR telemetry, macOS Unified Logs, Zeek network metadata, and security alerts. No production data is at risk. The intrusion was executed in an isolated environment purpose-built for this eval.


We structured the evaluation across three investigation tracks, each with 12 scored questions:


- Incident Response: Alert triage, kill-chain reconstruction, containment recommendations
- Threat Hunting: Proactive discovery of attacker TTPs, lateral movement indicators, credential access patterns
- Detection Engineering: Writing and validating correlation queries against the live dataset


Agents were given SQL-based query tools to investigate the data. Scoring uses an LLM-judged rubric weighted on technical accuracy (60%), completeness (25%), and specificity (15%).


## Results


Full interactive results on the Cotool Research site:[https://www.cotool.ai/research/macos-threat-investigation](https://www.cotool.ai/research/macos-threat-investigation)


GPT-5.4 led at 87% overall accuracy, followed by Claude Opus 4.6 (84%) and GPT-5.3 Codex (82%). GPT-5.4 was the most consistent across tracks, scoring between 83% and 92% with no weak spots. Notably, the top three models on incident response (the hardest track) all tied at 83%.


Each track rewarded different strengths. Gemini 3.1 Pro dominated threat hunting at 89%, the highest of any model on any track, but dropped to 57-58% on IR and detection engineering. GPT-5.4 and Opus 4.6 tied at 92% on detection engineering. No single capability drives performance here. Models need breadth across investigative reasoning, proactive hunting, and applied detection logic.


IR was the most expensive track across all models, consistent with its open-ended nature. Kimi K2.5 offered the best overall value at $0.36/task for 68% accuracy, roughly 8x cheaper than GPT-5.4. GPT-5.4 Mini hit $0.23/task on detection engineering while still scoring 71%.


GPT-5.4 Mini finished tracks in ~5 minutes. GPT-5.4 and GPT-5.3 Codex averaged ~37 minutes. Gemini 3.0 Flash was the slowest at 89 minutes per track, suggesting it may over-explore without converging.


Eight of nine models achieved 100% task completion. Gemini 3.1 Pro had 2 failures out of 40 runs (95%).


## Interpretation for security teams


- GPT-5.4 is the best pick for high-stakes investigations. Highest accuracy, no weak tracks, 100% reliability.
- Kimi K2.5 is the value play. 68% accuracy at $0.36/task with strong threat hunting performance (79%).
- GPT-5.4 Mini is the right choice for rapid triage. Completes tracks in ~5 minutes and scores 71% on detection engineering at just $0.23/task.
- Gemini 3.1 Pro is worth considering specifically for threat hunting (89%), but its inconsistency elsewhere limits general use.
- Claude Opus 4.6 is the strongest option in the Anthropic ecosystem. 84% overall, tied for best on detection engineering (92%).


## What's next


This is the first entry in what we're calling BlueBench: a series of benchmarks built on real intrusion data across different attack types, platforms, and environments. CTFs will remain part of our evaluation framework, but they're no longer the ceiling. If we want agents that can actually do the job, we need to test them on the job.


Full interactive results:[https://www.cotool.ai/research/macos-threat-investigation](https://www.cotool.ai/research/macos-threat-investigation)
