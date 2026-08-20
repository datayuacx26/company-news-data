---
schema_version: "1.0.0"
document_id: "d4f7363163d8b2de392bcabeb077959d027302729119f71741e2f2af8778df97"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/four-ai-escapes-just-redefined-responsible-ai/"
published_at: "2026-08-07T19:35:52+00:00"
first_seen_at: "2026-08-07T20:19:19.665752+00:00"
fetched_at: "2026-08-07T20:19:20.834439+00:00"
content_hash: "sha256:f49986121bb08949202ca4b6b2e14d66858bb49732498457ae3f01b8ee6750e4"
---

# Four AI Escapes Just Redefined “Responsible AI”

On July 21, OpenAI[disclosed](https://openai.com/index/hugging-face-model-evaluation-security-incident/) that its own models, running an authorized cyber evaluation, broke out of a sandbox and pulled benchmark answers from Hugging Face’s production database. On July 30, Anthropic[disclosed](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) three more cases where AI models hacked other companies in safety evaluations it was running with its partner Irregular. Claude models compromised three real organizations. The earliest of those happened in April and went undetected until late July, and in Anthropic’s words, “The two organizations we were able to reach had not previously detected the activity or contacted us.” This also may just be an opening of the floodgates as new reports such as[this one from AI Security Institute](https://www.aisi.gov.uk/blog/incident-report-unsanctioned-agent-behaviour-during-cyber-testing) drop.


Responsible AI has meant roughly one thing since 2020: Govern how the model decides; bias, transparency, data provenance, privacy, explainability. Every enterprise policy I read covers that ground. In nine days this month, the incident reports from OpenAI and Anthropic — the two firms with the best-funded AI safety programs on earth — just redefined the requirements for responsible AI. Enza Iannopollo wrote in March about how[agentic AI would redefine responsible AI](https://www.forrester.com/blogs/the-future-is-now-agentic-ai-redefines-responsible-ai/) . She was right and now has the proof.


## The Incidents Are Dead Canaries


We have been telling you since the report[Align By Design (Or Risk Decline)](https://www.forrester.com/report/align-by-design-or-risk-decline/RES181230) in 2024 that AI misalignment is inevitable and potentially costly. What happened here represents the canaries in the coal mine. What is useful in these cases is the mechanics of how it happened.


In all cases, the models did what they were told. **They did not “go rogue.”** OpenAI told its model to reach an answer and said nothing about the route to take. The model exploited a zero-day vulnerability and accessed the internet. Anthropic’s models were told they had no internet access, which was false. A partner integration “left the machines that Claude accessed as part of the evaluation with live internet access,” and neither company knew. Claude went looking for the information it had been sent to find across what it believed was a simulated network. The network was real; the intrusions were the result.


Neither failure was in an “unsafe” model, nor were they release decisions that a pre-release safety review would have caught. The failure was in how the model was instructed and how a vendor got wired in. Both incidents happened inside safety evaluations, in the operational gap between building a model and shipping an application of it, which is also where many of your agents will run as you look to deploy them.


## Your Responsible AI Policy Stops Today Where The Agent Starts


Every frontier lab publishes a “Frontier AI Safety Policy” that seeks to prevent incidents like these. This is[a link to most of them tracked by METR](https://metr.org/fsp) . July’s incidents taught us that these are not enough to keep your enterprise safe.


Open your responsible AI policy and read what it governs: bias; transparency; data provenance and fair use; privacy; explainability. None of that stops mattering when the model drives an agent. It gets worse. A single model making a bad decision is something someone can still catch. An agent carries the same flaw down a chain of decisions at machine speed, and the chain becomes impossible to follow. That is action risk. It lands beyond what your policy already covers. No enterprise AI policy I’ve seen governs it.


The labs’ safety policies only consider how to scale up their models safely by specifying test and release criteria based on model capability. You need a complementary *responsible deployment policy* , and it is not a document AI leaders write alone. Find out first what your AI governance team already runs and what your firm already buys. Enza’s research covers that market for AI governance, and much of the runtime observability is being sold right now.


You need to be looking for solutions that address:


1. **Who approves an agent to act.** Your security team will set least-agency limits. Policy decides who is allowed to raise them and on whose signature. Most AI leaders I talk to struggle to have an agent inventory, much less a catalog of agent instructions, guardrails, and accountability for actions taken.
2. **A named owner for the agent’s picture of its world.** Your agents believe what you tell them about infrastructure configuration. Your policy must certify that the sandbox is a sandbox and that the test system is not pointed at production. Both labs got parts of this wrong about their own environments, with the foremost experts in the world on staff.
3. **Kill authority, held by a person, available at 3 a.m.** Anthropic halted all cyber evaluations the same day it found transcripts suggesting a problem. Ask who can do that in your firm on a Saturday and whether they need anyone’s permission. As you connect agents to real processes and business outcomes, killing them will come with consequences.
4. **A retention rule that outlives your detection window.**[AEGIS](https://www.forrester.com/report/applying-forresters-aegis-framework-to-iam-and-ai-agents/RES189384) will tell your security team to capture the chain from goal to external effect. How long you keep it, and who can produce it under subpoena, is a policy call. Anthropic’s oldest incident sat undiscovered for roughly three months, which outlasts a lot of log retention.
5. **A liability position you have tested.** An agent you authorized, pursuing a goal you approved, can reach a third party that never contracted with you. Does your cybersecurity policy cover an authorized agent exceeding its scope or only an intruder? Check whether your vendor agreement allocates liability for autonomous action. “We had controls” has to stand up in a deposition.


## Build It Before You Need It


These questions, and the uncomfortable answers, are the proof for your business case. You will not get better evidence than these vendors’ own incident reports.


For two years, the loudest idea about AI governance has been that it slows you down. Re-price that against what just happened. Widen what responsible AI means inside your firm and fund the team that can enforce it.


Book a[guidance session with me or Enza](https://www.forrester.com/inquiry) , and we will pressure-test your agentic deployment governance against what just happened at OpenAI and Anthropic.


Share


-
-
