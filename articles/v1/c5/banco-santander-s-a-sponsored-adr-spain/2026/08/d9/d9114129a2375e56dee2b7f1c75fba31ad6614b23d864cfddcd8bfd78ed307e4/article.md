---
schema_version: "1.0.0"
document_id: "d9114129a2375e56dee2b7f1c75fba31ad6614b23d864cfddcd8bfd78ed307e4"
company_key: "banco-santander-s-a-sponsored-adr-spain"
company: "Banco Santander S.A. Sponsored ADR (Spain)"
source_id: "banco-santander-s-a-sponsored-adr-spain-rss-14b90327b079"
canonical_url: "https://www.santander.com/en/stories/optimize-your-guardrail-policy-against-attacks-the-way-youd-optimize-a-model-against-a-loss"
published_at: "2026-08-14T07:00:00+00:00"
first_seen_at: "2026-08-14T07:02:49.756346+00:00"
fetched_at: "2026-08-14T07:02:52.222137+00:00"
content_hash: "sha256:cbff3a95eaed4f79a61cb8e8c0ab3732dd4e84d65e544e533e003b7ebda91278"
---

# Optimize your guardrail policy against attacks the way you'd optimize a model against a loss

[Communications Santander](https://www.santander.com/en/press-room/topics-of-interest)


CEST


[Linkedin](https://www.linkedin.com/company/banco-santander)


[Instagram](https://www.instagram.com/santander.global/)


[X](https://x.com/bancosantander)


In many working environments, fine-tuning a model simply is not an option. The model may be hosted or provided by a third party, or the cost, complexity, and governance requirements of retraining may rule it out. When the weights cannot be changed, one of the most accessible levers is the policy or prompt around the model. The problem is that tuning that lever is usually a manual and time-consuming process. A few words are changed, the evaluations are run again, further edits are made, and the cycle repeats. Quite often, a small change that fixes one case quietly breaks behaviour that had already been secured. The result is a trial-and-error process with little experimental memory and poor scalability.


Experience across different use cases within the bank repeatedly exposed this same pattern. That accumulated experience highlighted the value of a systematic way to search and optimize prompts and policies instead of relying on manual trial and error. That is the motivation behind **[autoguardrails](https://github.com/SantanderAI/autoguardrails)** — and the perspective that makes the rest of the design easier to understand.


##


Search over policy.md, not train.py


Autoguardrails is an alignment-research scaffold in the spirit of Karpathy's autoresearch. The difference lies in what gets optimized. While autoresearch searches over training code, autoguardrails **searches over a single mutable surface: policy.md** . Everything else (the evaluator, the attack suite, the judge) remains frozen. The guardrail policy is the only element that changes between runs.


The objective is deliberately simple:


- **Minimise attack success rate (ASR)** - the fraction of adversarial prompts (jailbreaks, obfuscation, injection) that get through. Lower is better.
- **Subject to a benign-pass floor** - so the system can't cheat by refusing everything. A guardrail that blocks all traffic has an ASR of zero but is also useless.


That second constraint is the key challenge. Driving attack success to zero is relatively easy if an assistant becomes overly restrictive; the difficult part is reducing attack success while still answering legitimate requests.


To obtain that result the harness deliberately minimal: Python 3.10+, standard library only, offline by default. The loop is straightforward: establish a baseline, propose a candidate policy.md, measure its performance, and keep it only if it genuinely improved.


# 1. establish a baseline against the fixed eval suite
python -m autoguardrails baseline --reset --repeat 2 --notes "initial baseline"


# 2. edit policy.md (the ONLY file you change between runs), then score it
python -m autoguardrails candidate --repeat 2 --notes "cover jailbreak and obfuscation"


# 3. see where things stand
python -m autoguardrails status
cat results.tsv


The moving parts map cleanly onto files:


- **policy.md** — the only file you edit between runs; the guardrail policy, in prose.
- **eval_suite.jsonl** — the fixed set of attack and benign test cases.
- **judge_prompt.md** — the frozen judgment logic that decides pass/fail.
- **results.tsv** — an append-only log, so every experiment stays on the record.


And we made the acceptance rule strict on purpose:


*keep a candidate only if **ASR improves** **and benign-pass does not fall by more than 2 percentage points.***


That single line is the discipline made executable. It stops the classic failure mode where a policy tweak quietly trades away legitimate answers to look safer on paper.


##


Pointing it at a real model


By default, the system runs offline, but it can be connected to any OpenAI-compatible endpoint. Two separate sets of environment variables are used: one for the target model (the one being guarded) and one for the judge.


export AUTOGUARDRAILS_TARGET_PROVIDER=... AUTOGUARDRAILS_TARGET_MODEL=...


export AUTOGUARDRAILS_TARGET_API_BASE=... AUTOGUARDRAILS_TARGET_API_KEY=...


export AUTOGUARDRAILS_JUDGE_PROVIDER=... AUTOGUARDRAILS_JUDGE_MODEL=...


export AUTOGUARDRAILS_JUDGE_API_BASE=... AUTOGUARDRAILS_JUDGE_API_KEY=...


Separating target and judge matters: the model under evaluation should not be responsible for grading its own defences.


##


Why it is designed this way


In practice, guardrail systems often evolve into collections of *ad hoc* rules that are difficult to evaluate. The idea here is to reframe the guardrail as a **measurable artifact with an objective and a frozen benchmark** . That's the only way improvements become real rather than moving goalpost. The policy is legible (prose in one file), the evaluation is fixed, and the log is append-only, so the history can't quietly disappear. In a regulated institution, those properties are particularly important. Transparency, traceability, and auditability are not optional extras; they are core requirements.


It's also an honest scope as this idea is not claiming to have solved jailbreaks. Autoguardrails is a disciplined loop for making a policy measurably better against a fixed suite, while guarding against the “refuse everything” solution. Publishing the project openly also allows others to pressure-test the approach, identify weaknesses, and propose improvements rather than leaving the work confined to an internal drive.


##


Run it, break it


The purpose of opening the project is to make it usable, testable, and open to challenge. A good first exercise: swap *eval_suite.jsonl* for attacks that matter in a particular domain, set a benign-pass floor that reflects real requirements, and see how far a few careful edits to policy.md move the ASR. Finding where the system breaks is part of the value. Each failure can become a new evaluation case, a new hypothesis, and an opportunity to improve the policy.


The next phase of AI will not be determined only by who has access to the most advanced technology, but by how responsibly, and how collectively that technology is built and shared.


*(Commands and file names above are quoted from the repo's README at the time of writing — check the repo for the current interface before running it)*
