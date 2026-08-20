---
schema_version: "1.0.0"
document_id: "1236c3ae2a49c7f602e0323de4fed21887a4c5aa461ca8082c561e8889956620"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/self-improving-voice-agents-closing-eval-loop"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:e7647310e811a5e901a8589664858d353f29f0b2813bb868da8033515b4d006b"
---

# Self-Improving Voice Agents: Closing the Eval Loop Automatically

Agent quality is bottlenecked by the manual loop: run evals, read transcripts, guess at the fix, edit the prompt or tool config, re-run, repeat. It is slow, lossy, and very easy to do badly. A misplaced sentence in your system prompt can flip the agent from "confirms the booking" to "ends the call before confirming," and you only find out three iterations later.


We built **cekura-self-improving-agent** to close that loop. You point it at a set of failing scenarios; it diagnoses the failures, proposes targeted prompt or config edits, applies them to your live agent, and re-validates, iterating until the agent reaches 100% pass rate on the full validation set or hits the iteration cap.


This post walks through what we learned building it, the architecture we landed on, and the anti-patterns we hard-coded into the skill itself.


**TL;DR:** cekura-self-improving-agent is an automated eval loop for voice agents. It diagnoses failing scenarios by class (Gap, Conflict, Ambiguity, CodeBug, Upstream), applies targeted prompt or config edits, redeploys, scans for prompt overfitting, and iterates until the full validation set hits 100% pass rate. Works with VAPI, Retell, and self-hosted agents.


## The naive loop is a trap


The temptation, when you have an LLM in the loop, is to write a single prompt:


> "Here's the failing transcript. Here's the system prompt. Rewrite the prompt to fix the failure."


This produces edits that look reasonable and fail in interesting ways. A few of the failure modes we hit repeatedly:


- **Transcript-overfit edits.** The model reads the failing transcript verbatim and writes a hyper-narrow case clause: "If the user mentions a Tuesday reschedule for their dental cleaning, confirm the time before checking insurance." The next run, the user mentions a Wednesday root canal, and the edit is dead weight.
- **Plumbing failures attributed to wording.** The agent forgets earlier turns. The LLM diagnoses "weak instruction-following," strengthens the prompt, and the failure persists, because the actual bug is the orchestration code is truncating conversation history before it reaches the model.
- **Edits land but don't deploy.** The edit lands in the user's` prompts.py` file. The live server is still running with the old code in memory. Validation comes back unchanged. You blame the prompt and edit harder.
- **Quitting too early on regressions.** Failure set hits 100% on iteration 2. You declare victory. But the edits that fixed scenarios A and B broke scenario C, and you never ran the full set to find out.


Every failure mode above is a class of mistake the skill now refuses to make. The architecture exists because the failure modes exist.


## Fix failing voice agent evals: an automated eval loop for voice agents


The skill is a thin orchestrator that walks a strict sequence of phases, each with one job:


```text
Setup → Collect → Diagnose → Apply → Sync → Overfitting Gate → Eval
│
└─ loop back to Collect


```


Each phase has hard pre-conditions (the previous phase's outputs). The orchestrator never parallelizes across phase boundaries. This is more verbose than a single monolithic prompt, and that verbosity is the point: when a failure happens, you can localize it to a phase.


### Setup: resolve the deploy path before doing anything else


Setup is a hard gate. For self-hosted agents we collect a` redeploy_command` up front — the shell command that restarts the user's server after each edit. Without it, edits land in source files the live process doesn't re-read, and the loop spends real iterations attributing the persistent failures to "weak instruction-following."


### Collect: get the right set of failures, with full context


Collect pulls in the runs, call logs, or pasted failures you pointed the skill at, and decides which ones the loop should try to fix.


Two things matter here:


- **Use the post-review verdict, not the raw machine score.** Each run has two layers of judgment — an automated verdict, and an optional human review on top. The human review is the source of truth: if a reviewer looked at a "failed" run and said "actually this is fine, the metric was noisy," the loop should respect that. Read the verdict per-run; don't read pre-review aggregate counts, which include runs the reviewer already cleared.
- **Pull enough context to diagnose, not just flag.** A failed run isn't just a verdict — it has a transcript, which steps of the scenario completed, where the agent went off-track, and whether the conversation ended cleanly or got cut short. Collect captures all of this up front so the next phase doesn't have to guess.


### Diagnose: voice agent failure diagnosis with The Five Failure Classes


Every failure gets classified into one of:


**The Five Failure Classes**


- **Gap** — the prompt does not address this case at all.
- **Conflict** — two sections of the prompt contradict each other.
- **Ambiguity** — the prompt is unclear about what to do in this situation.
- **CodeBug** — the orchestration layer prevents the agent from following the prompt, regardless of wording.
- **Upstream** — the failure originates in a tool, knowledge base, or infrastructure dependency, not the prompt.


The classification is the diagnosis. The edits flow from it. If the real problem is a broken tool or a stale knowledge base, no amount of prompt rewording fixes it, so the loop points you at the actual cause instead of rewriting around it.


The proposal is then presented to the user. In` auto_mode: false` , this is the diff-approval gate.


### Apply and Sync: write, redeploy, verify


Apply lands the edits via the right machinery — VAPI PATCH for VAPI assistants,` Edit` on source files for self-hosted agents — then runs the` redeploy_command` . Sync re-fetches the edited artifacts and verifies each changed field landed correctly. VAPI's PATCH semantics replace nested objects wholesale; an Edit call with an ambiguous anchor can land in the wrong spot. The Sync step catches both.


### Overfitting Gate: scrub transcript leakage


The Overfitting Gate is a dedicated phase that scans every applied edit for transcript leakage before validation runs. The LLM that diagnoses failures has read the failing transcripts in detail. Those transcripts leak: verbatim quotes show up in proposed edits, scenario IDs slip into prompt clauses, hardcoded test data ("the user's account number is 12345") appears in instructions.


The Overfitting Gate re-reads what was just applied, scores each edit against five overfitting signatures, and emits cleanup edits — REVISE to a generalized form, or STRIP entirely — before Eval validates. On clean iterations the Gate is a one-line pass-through. On the iterations where it matters, it's the only thing standing between a memorized fix and a passing-but-non-generalizing agent.


This gate is most likely to be skipped on iteration 2+, when edits feel incremental and the orchestrator is tempted to apply-and-validate without re-walking the pipeline. That is exactly when transcript-leak risk compounds. So we made phase-entry announcement a hard rule: if the user-facing trace doesn't say "Iteration 3 - Overfitting Gate", the phase was skipped.


### Eval: verify on the full set, then decide


Eval runs validation, re-collects failures using the same logic as Collect, and emits one of: hand back to Collect, regression sweep on the full set, declare success, or stop on the iteration cap.


The exit gate is **100% on the full set, not 100% on the failing subset.** Skipping the regression sweep masks regressions where an edit fixed scenarios A and B but broke C. The decision tree enforces this.


## When the loop should suggest a bigger change


If the same scenario fails the same way across three iterations of prompt edits, the prompt layer is demonstrably not the fix. Keep going and you're paying compute to confirm a known result. A no-change signature occurs when the same scenario fails the same way across three consecutive prompt-edit iterations, indicating the prompt layer cannot fix the underlying issue. The skill stops and surfaces a set of larger structural options instead:


- **Restructure the agent flow into explicit named states.** Replace "natural conversation guided by a long prompt" with a graph of well-defined states gated on collected fields.
- **Add a deterministic guard in code.** Enforce the missed behavior in the orchestration layer instead of asking the prompt to do it.
- **Switch to a stronger model.** Sometimes the smaller model just can't follow the instructions reliably no matter how the prompt is worded.


Each option carries real cost — a model swap can multiply per-token spend, a code-level guard is invasive, a flow restructure is a larger refactor — so the skill surfaces them and lets the user pick instead of autonomously committing.


## Voice agent pass rate: from 75% to 100% in one session


We ran the skill on a self-hosted intake agent (gpt-4o, conversation-flow logic in the user's own server) handling appointment booking for a healthcare clinic. The validation set was 20 hand-curated edge-case scenarios — mid-conversation corrections, off-topic redirects, demographic edge cases, users asking to skip qualification, users asking for alternate consultation formats, and so on.


In a single session on a healthcare appointment-booking agent, the loop moved a 20-scenario validation set from 75% to 100% pass rate across 5 iterations, catching two regressions in the final sweep that would have shipped undetected.


Here's the actual trajectory from a single end-to-end session:


Step Set size Pass rate What happened


Baseline 20 (full) 75% 5 scenarios failing


Iter 1 — failure-subset re-run 5 (failing only) 60% 3 fixed, 2 still failing


Iter 2 — failure-subset re-run 2 50% 1 fixed, 1 still failing


Iter 3 — failure-subset re-run 1 0% No-change signature; loop escalates to structural fix (flow restructure)


Iter 4 — failure-subset re-run 1 100% Restructure lands; last failing scenario passes


Regression sweep 20 (full) 90% Earlier edits regressed 2 previously-passing scenarios


Iter 5 — targeted fix 20 (full) **100%** Exit gate hit


A few things worth pointing out about this trajectory:


- **The narrowing.** The loop didn't waste compute re-running already-passing scenarios on every iteration. It tightened to the failing subset (5 then 2 then 1), iterated until that subset hit 100%, and only then ran the full set.
- **Iteration 3 is where the prompt layer ran out of room.** The same scenario failed the same way after a prompt edit. The skill recognized the no-change signature, surfaced the structural options from the section above, and escalated to a flow restructure, which landed the fix on iteration 4. A pure prompt loop would have spent more iterations producing slight rewordings of the same failed edit.
- **The regression sweep earned its keep.** After the failure subset hit 100%, the full-set sweep caught two scenarios that had silently regressed — exactly the kind of failure mode the post warns about. Without the sweep, the session would have declared victory at "100% on failing subset" and shipped a worse agent than it started with.


## Try it


To run your failing scenario set programmatically:


```text
import   os
import   requests


response = requests.post(
"https://api.cekura.ai/test_framework/v1/scenarios/run"  ,
headers={ "X-CEKURA-API-KEY"  : os.getenv( "CEKURA_API_KEY"  )},
json={
"agent_id"  :  int  (os.getenv( "AGENT_ID"  )),
"scenario_ids"  : [ int  (s)  for   s  in   os.getenv( "SCENARIO_IDS"  ,  ""  ).split( ","  )  if   s]
}
)
result = response.json()
print  ( f"Pass rate:  {result.get( 'pass_rate'  )}  % | Failing:  {result.get( 'failing_count'  ,  0  )}   scenarios"  )


```


Or invoke the self-improving loop from any MCP-enabled editor:


```text
# Install the plugin
/plugin marketplace add cekura-ai/cekura-skills


# Then invoke
"improve my VAPI agent <agent_id> using result <result_id>"


```


Or against a self-hosted agent:


```text
"improve my agent <agent_id> using scenarios <scenario_ids>, redeploy command: pm2 restart voice-agent"


```


The full skill lives at[cekura-self-improving-agent](https://github.com/cekura-ai/cekura-skills/tree/main/cekura/skills/cekura-self-improving-agent) on GitHub.


---


**Cekura works with VAPI, Retell, and self-hosted voice agents. Start closing your eval loop:[docs.cekura.ai](https://docs.cekura.ai/) .**
