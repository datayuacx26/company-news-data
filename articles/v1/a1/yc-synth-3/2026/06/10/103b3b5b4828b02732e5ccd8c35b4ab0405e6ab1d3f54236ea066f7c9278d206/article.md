---
schema_version: "1.0.0"
document_id: "103b3b5b4828b02732e5ccd8c35b4ab0405e6ab1d3f54236ea066f7c9278d206"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/stack-handoffs"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-22T15:27:15.216799+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:320fc70ac383fcfa7049fef90c211d061afee0ef95c20913bfdfb87f0bd344cb"
---

# Your Coding Agent Needs a Sidekick

The tweet that kicked this off was simple:


> Why don't we give codex a change_effort tool?
>
>
> If I'm running a 12 hour /goal session, it'd be nice to get xhigh on core code, high on evals/testing, medium on running tests, and low on documentation.
>
>
> Surely it's not that hard for the model to make those judgements?
>
>
> — Josh (@JoshPurtell)[June 28, 2026](https://x.com/JoshPurtell/status/2071290668731773189)


That is a good request. It is also only the first knob. In a long coding session, the human should not have to live inside the transcript just to know whether the agent should think harder, run tests, ask for review, or stop before losing state.


The bigger primitive is a sidekick: an auxiliary agent that watches the core worker, summarizes progress for the operator, and sometimes does independent context work before the worker acts.


## The Conversation


Long coding sessions hit three walls at once:


1. **Context pressure** - the worker model runs out of room; compaction becomes a hidden product decision.
2. **Operator overload** - hundreds of tool calls make the transcript unusable as the default status surface.
3. **Steering fatigue** - the human is still acting as scheduler, approval router, and status reader.


[Devin Fusion](https://cognition.com/blog/devin-fusion) is the cleanest public version of the sidekick idea: a main frontier agent plus a cheaper sidekick model that can gather context and take delegated work.[OpenAI's monitoring writeup](https://openai.com/index/how-we-monitor-internal-coding-agents-misalignment/) shows the safety version: a monitor reviews agent behavior and escalates when actions stop matching intent or policy.[Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) shows the approval-routing version: routine actions can proceed, risky ones still wake the human.


Stack starts from the operator question:


> What should the human see while the agent is working, and what should another model be allowed to do on the worker's behalf?


## Stack's Answer


Stack treats sidekicks as first-class actors around the worker:


Role Job


**Worker** Edits code, runs tools, executes evals, and owns the task.


**Monitor** Watches` agent.*` events, writes` monitor.summary` , routes attention, and can request a boundary.


**Gardener** Handles queued operator work and maintenance routing.


**Handoff** Ends one segment on purpose and starts the next from typed artifacts.


Auxiliary agents around the core worker **Core worker**
Edits code, runs tools, executes evals, and owns the primary task loop.


**Event log**
One substrate for worker actions, monitor summaries, and handoff milestones.


watched by


**Monitor**
Summarizes progress, routes approvals, flags drift, and can request handoff.


**Gardener**
Queues operator work, handles maintenance, and can force an auditable boundary.


The default Stack view is calm: monitor updates like "working on", "stuck on", "progress", and "next". Pressing` a` opens Agent view, where the same timeline expands into the full worker and monitor tape. There is no private monitor store; the UI and programmatic consumers read the same event substrate.


That matters because a useful monitor is not a narrator inventing a second version of the run. It is an actor with a different job: keep the operator abreast of real progress while preserving auditability when the full tape matters.


Stack Aux makes that cheap enough to run continuously. Aux inference is exposed for roles like` monitor` and` gardener` , while` worker` and` primary` requests are rejected. The TUI labels those monitor passes as` aux` , and the Synth usage panel shows the promo pool and org-day remaining budget alongside normal usage.


## Sidekick Or Handoff?


These are different tools.


A **sidekick** helps without taking ownership. It can explore code, summarize state, propose a plan, or identify likely failure modes. The primary worker still decides what to implement.


A **handoff** moves ownership across a hard boundary. The current segment seals a packet, the operator or policy approves it, and the successor starts from that packet.


Seal, review, continue **Segment T1**
Worker runs until policy or operator asks for a boundary.


seal


**Handoff artifact**
Summary, goal state, changed files, checks, parent config.


approve


**Segment T2**
Successor bootstraps from artifacts only, not parent stdout replay.


The invariant is important: the successor prompt is built from artifact payloads, not a replay of parent stdout. That is the product distinction from ordinary compaction. A handoff is not a smaller transcript. It is a reviewable boundary with a parent config, child config, goal state, changed files, checks, and a durable record of what crossed.


## Four Experiments


The honest result from Harbor TicTacToe dogfood is split: handoffs lost, sidekicks helped, and subagents are the next result to publish.


Experiment Treatment Valid Reward Cost Duration Baseline Readout


**Mixed effort handoff** Monitor-routed same-model effort handoff 5/5 0.1198 $0.5472 952.5s No handoff: 0.1702 / $0.3417 / 628.7s Lost: lower reward, higher cost, slower.


**Mixed model handoff**` gpt-5.5/high -> deepseek-v4-pro/high` 3/5 0.1787 $1.2464 574.1s` gpt-5.5/low` : 0.2045 / $1.0080 / 292.6s Lost; strict all-run reward was 0.1072.


**Fusion-style sidekick**` deepseek-v4-pro/high` guidance +` gpt-5.5/low` primary 3/5 0.2500 $1.1238 452.0s` gpt-5.5/low` : 0.1868 / $1.0385 / 287.5s Promising: best score so far, but reliability and latency costs.


**Implementation subagents**` gpt-5.5/medium` delegates bounded implementation subtasks 5/5 0.1672 $3.9786 1023.3s` gpt-5.5/medium` : 0.1983 / $1.3031 / 372.8s Null result: valid runs, lower reward, about 3x cost and 2.7x wall time.


The best sidekick-guided run scored` 0.2932` , the best Harbor score in this sequence. It is not a victory lap. Two sidekick reps were invalid because the DeepSeek artifact came back malformed or truncated. The primary workers still ran with fallback guidance and the candidate verifiers succeeded, so the failure was sidekick artifact generation, not Harbor service startup. Counting invalid sidekick runs as zero, the all-run sidekick reward mean was` 0.1500` .


This is why the feature ships with an experimental label. Stack can seal artifacts, route successors, capture per-segment usage, separate invalid runs, and keep the operator informed. But handoff routing is not trivial. The right auxiliary pattern has to be learned, not guessed. The implementation-subagent arm made real ad hoc delegation choices, but the result was negative: more work crossed the boundary, and the frontier got worse.


Cloud SMR is why we still believe in artifact-backed continuity. Managed research runs already rely on explicit receipts, recovery boundaries, and continuation artifacts. Local Stack is a harder product surface because it is interactive, cheaper, and closer to the raw coding loop.


## What Comes Next


MAPO is the follow-up, not a hidden claim in this post.


The offline version should optimize auxiliary protocols on GameBench dev splits: no sidekick, sidekick, handoff, effort level, artifact format, timing, and acceptance preflight. The online version can adapt Stack policy from live receipts once a candidate is safe enough to graduate.


That is the punchline: handoffs are available but experimental; sidekicks look more promising for this benchmark; OSS MAPO is how we intend to make the choice consistent instead of hand-written.


Stack · public alpha


## Run agents with sidekicks, receipts, and visible progress.


Stack is the local-first cockpit for long agent sessions: watch progress, capture run evidence, and work with monitors, sidekicks, handoffs, and GameBench receipts. Open source, installable today.


Install (macOS)


` curl -fsSL https://stack.usesynth.ai/install.sh | sh`


[Download from GitHub](https://github.com/synth-laboratories/stack/releases)[Sign up for Synth](https://www.usesynth.ai/signup?product=stack&utm_source=blog&utm_medium=stack_cta&utm_campaign=stack-signup)


agent + sidekick


Distribution


Open source (MIT), public alpha. macOS (Apple Silicon) today; more platforms and Homebrew are on the way.


Core Stack is local-first. A Synth account is only needed for hosted features, API keys, and metered auxiliary inference.


Prefer git?` git clone https://github.com/synth-laboratories/stack.git && cd stack && make install`
