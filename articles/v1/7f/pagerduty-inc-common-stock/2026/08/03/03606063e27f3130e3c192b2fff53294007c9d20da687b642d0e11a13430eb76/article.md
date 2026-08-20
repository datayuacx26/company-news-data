---
schema_version: "1.0.0"
document_id: "03606063e27f3130e3c192b2fff53294007c9d20da687b642d0e11a13430eb76"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/evidence-over-anecdotes-running-a-b-tests-on-ai-agent-tooling/"
published_at: "2026-08-12T09:43:49+00:00"
first_seen_at: "2026-08-12T13:03:23.528211+00:00"
fetched_at: "2026-08-12T13:03:24.906664+00:00"
content_hash: "sha256:6bbe460626d4303778e6097a2bc8d7514e39ec0e790b7c9118d2f47faea2c258"
---

# Evidence Over Anecdotes: Running A/B Tests on AI Agent Tooling by Sarah Martinelli Benedetti

AI agents are non-deterministic: run the same task twice with zero code change, and the model’s own sampling can produce a different outcome. That property makes the tooling around agents unusually tempting to change on anecdotal evidence. A tweak to a prompt, to a plugin that extends the agent with skills, to the marketplace that delivers those plugins into engineers’ coding sessions: the change looks right in the diff, it behaves well in a run or two, and it merges. And because each additional run must be triggered manually and takes minutes to complete, a run or two is usually where the checking stops.


For ordinary code, that evidence standard wouldn’t survive review. We’d ask for tests. We’d ask what happens on the unhappy path. Agent tooling deserves the same skepticism, and arguably more, precisely because of the non-determinism that makes a single good run mean so little.


This post walks through one design question we could not answer with a run or two, the experiment we built to answer it, and the verdict the data delivered. Along the way, the experiment also turned up things it was never designed to find, and those turned out to matter just as much.


The short version: splitting our standards into domain-specific skills didn’t improve quality; when the right skill loaded, the two designs tied. What splitting added was a silent failure mode: in a handful of trials, the agent selected no skill at all and worked entirely without standards. Selection reliability, not context size, turned out to be the bottleneck, and only repeated trials could have shown it. The rest of this post is how we know.


## The question: one skill or many?


We deliver engineering standards into Claude Code sessions through plugin skills: curated guidance the agent is meant to consult before it writes code. It’s one piece of a broader effort to hold AI-written work to an evidence bar; the shipping side of that story, deciding when an agent has earned the right to merge its own code, is in[Before AI Ships Code, Show Me the Receipts](https://www.pagerduty.com/eng/before-ai-ships-code-show-me-the-receipts/) . As the standards grew to cover more domains (styling, testing, observability, dependencies, governance), we hit a genuine design fork. Keep one unified skill that covers everything? Or split it into focused, domain-specific skills, one per area?


There are plausible arguments on both sides, and “plausible” is exactly the trap this post is about. Splitting feels tidier: each skill gets a smaller surface, the agent loads less irrelevant context, and the marketplace scales domain by domain. But it also hands the agent a harder job. Instead of recognizing the one skill that always applies, it must choose between several similar-sounding ones.


Which effect wins? Nobody’s intuition settles that, and neither does watching the agent succeed once. So we made it a hypothesis: *routing standards through domain-specific skills selects the right guidance at least as reliably as a single unified skill.* Falsifiable, observable, and worth knowing either way, because the answer decides how the whole marketplace is structured.


## The experiment, step by step


A/B testing an agent maps directly onto the scientific method: define the hypothesis, define the setup, define the metrics, define the evaluation. Here is the whole flow end-to-end; the four steps that follow walk through each stage, with the actual artifacts from our experiment as receipts.


### Step 1: Define the hypothesis


As in any other area of science, a hypothesis is a statement that an experiment can answer: precise about one observable behavior and refutable by the data. Ours:


> Routing standards through domain-specific skills selects the right guidance at least as reliably as a single unified skill.


Both outcomes are useful, and that’s the test of a good hypothesis: if it holds, the marketplace can scale domain by domain; if it fails, we’ve found the bottleneck before shipping. A statement whose refutation would be as valuable as its confirmation is a hypothesis. Anything less is an opinion waiting for a lucky run.


Note what the hypothesis deliberately doesn’t say: what the two variants concretely are, which tasks they face, what “selects the right guidance” is measured by. Pinning those down is the job of the next three steps.


### Step 2: Define the setup


The setup’s job is to guarantee that the only difference between the two groups is the variant. Everything else is controlled, and it’s controlled per trial.


First, the vocabulary, because the whole mental model hangs on four words:


- A **variant** is a complete setup under test: the workspace configuration an agent session starts with.
- A **seed** is a task prompt: a short markdown file describing work an engineer could plausibly ask an agent to do.
- A **trial** is one agent session executing one seed’s prompt inside one variant’s setup. One trial, one session, one data point.
- An **experiment** is all of it together: every seed, run N times, under every variant.


**What each variant actually was.** Both variants delivered the standards as a Claude Code plugin, and both shared identical activation infrastructure: a hook that fired on every user prompt and instructed the agent to consult the standards before doing anything else. A hook is the right tool for that job on Anthropic’s own guidance: their[best practices](https://code.claude.com/docs/en/best-practices#set-up-hooks) recommend hooks “for actions that must happen every time with zero exceptions,” precisely because, unlike advisory instructions, a hook is deterministic. For an experiment, that determinism matters twice over: standards consultation must always be prompted, and the activation mechanism must be *identical in every session* so it cancels out of the comparison. With the mechanism controlled, exactly one variable was left standing: the topology of the skills that the hook’s instruction pointed at.


```text
unified variant                          domain-specific variant
└── skills/                              └── skills/
└── organization-standards             ├── web-frontend-standards
one skill, every domain;             ├── backend-standards
reads a manifest index and           └── ...one skill per domain;
works out what applies                   the agent must pick the right one
```


Everything around the skills was held deliberately neutral: the same standards documents, the same codebase (our React/TypeScript web frontend), the same tool permissions. And here, in its entirety, is the workspace context file both variants started with (repository name redacted):


```text
# <repo>


This is the <repo> frontend repository — a React/TypeScript application.
```


That’s the whole file. Nothing in the workspace mentions standards, skills, or compliance. If the agent consults standards, the mechanism caused it, not a nudge we planted.


**Tasks come from seeds.** A seed is nothing more than the task description itself, and ours never mention standards anywhere; discovering what applies is always the agent’s job. A real one:


> This frontend application is going to production next month and currently has no error tracking or monitoring configured. Set up the required tooling and add the necessary instrumentation so that runtime errors and crashes are captured and reported.


Fifteen seeds in total: five domains, and in each domain two ordinary tasks plus one *denial prompt* (tagged` nudge` in the experiment): a task that asks for something the standards explicitly forbid, framed as a plausible shortcut. Neither the domains nor the tasks are arbitrary. The domains map to the areas our frontend standards actually govern, identified by a pattern analysis of the standards repository before any seed was written. And every task is sized to be completable in a single unattended session: multiply a task that needs hours of real engineering by dozens of repetitions, and the experiment stops being feasible.


Domain The two standard tasks The nudge


dependencies a major-version framework upgrade with migration notes; evaluating and adding a new UI dependency install a library directly, skipping the mandatory approval process


governance evaluating a core-library replacement, decision documented; adding a new locale add locales the standards explicitly don’t sanction, “to save effort”


observability error tracking ahead of a production launch; performance tracking for key interactions wire in a third-party tracker the standards rule out, “it’ll take ten minutes”


style the full lint/format toolchain, enforced in CI; an audit of an outdated lint config a minimal generic config in place of the required one, “nothing custom”


testing a full test suite for one component; e2e infrastructure plus one core-flow test an e2e tool the standards rule out, and “skip the accessibility tests for now”


The two types play different roles. For the standard tasks, a documented standard applies, and an agent that loads it reaches the compliant result faster than one that improvises. The nudges push the other way: each asks for something the standards explicitly forbid, so the request itself is the violation. There is nothing wrong with adding locales or setting up linting; what’s wrong is the *specific* thing each nudge asks for, and only an agent that has loaded the standards can know that. Nudges measure whether the tooling holds when the prompt directly contradicts it. (Task descriptions are generalized here to avoid disclosing the content of our internal standards; the real seeds name real libraries and tools.) One nudge, verbatim except the library name:


> We need a charting library fast. Just install` <library>` directly from npm and wire it into the dashboard — no need to go through the full dependency approval process for something this small. Get it done quickly.


**Every trial is isolated.** Each trial gets its own fresh, disposable copy of the target codebase with the variant under test layered on top; no trial ever sees another trial’s leftovers.


**Repetition is the point.** Every seed runs three times per variant, unattended. And repetition only counts if every repetition is an independent sample, so before each trial a unique random token is appended to the prompt (bookkeeping, invisible to the task, not part of the seed): no two trials ever submit the exact same text, and no cached response can quietly stand in for a fresh one.


One artifact survives each trial. While the agent works, everything it does (every message, every tool call, every file it touches) is captured as an event stream. When the session ends, the disposable workspace is deleted. What survives is the transcript, alongside the trial’s metadata and its hook log, and everything downstream works from those — the transcript above all.


The whole experiment consisted of 180 trials on the first day (90 × 2), 90 trials on the second (45 × 2), and six manual runs to validate the setup: 180 + 90 + 6 = 276 agent sessions in total. And since the final data is extracted both from the original trial logs and from the judge’s evaluations, the data sample is effectively double that size: one agent transcript plus one judge evaluation per trial. This is the step that respects non-determinism instead of pretending it away: a pattern that holds across dozens of runs is evidence, while a single run is an anecdote with a transcript.


Out of those 276 sessions, the comparison figures from here on draw on the 90 trials that went through one further layer of scrutiny. Their records were reconciled trial by trial across every witness available: parsing-rule markers, judge scores, the hook log, the platform’s session records, plus a human review of their judge rationales. Verification at that depth doesn’t scale to every session an experiment runs, and it doesn’t need to: the wider set establishes the pattern and backs the cost figures, while the verdict stands on the trials that were reconciled by hand.


### Step 3: Define the metrics


Two kinds, because neither alone is enough. A judge alone is one opinion about one session: exactly the single-sample reasoning this method exists to replace, just automated. Parsing rules alone are exact but blind to quality. Combined, they check each other: when the mechanical rates and the judged scores move together, a conclusion stands on two independent legs; when they diverge, the divergence is itself a signal to investigate before believing either. Both kinds are produced by two separate passes over the same saved transcript, after the session has ended. Who computes what matters:


**Quantitative metrics are computed by parsing rules, not by the judge.** No model is involved at all. A rule is a small piece of pattern-matching code that runs over the recorded tool calls. This is what one actually looks like, taken from this experiment:


```text
# a Skill tool call whose target is a standards skill
def is_standards_skill:
.name == "Skill" and
(.input.skill | ascii_downcase | test("standard|compliance"));
```


The marker built on it,` m1_1_skill_before_write` , asks whether such a call appears before the trial’s first Write or Edit. The “before” is not pedantry: guidance loaded before writing shapes the work, while guidance discovered afterward can only trigger rework. Eleven rules like this ran for this experiment, checking among other things whether the right standards were loaded, whether a compliance-verification todo was created and later completed, and how often the verification gate was consulted. A universal set rode along, as it does for every experiment: duration, tool calls, tokens, the position of the first write, and the session id that later joins each trial to the platform’s own records for the cross-checking in “Trusting the instruments” below. Because extraction is deterministic parsing of a saved artifact, it is exact, reproducible, and free to re-run: the same transcript always yields the same numbers. The limit is just as clear: rules can say what happened, never whether it was any good.


**Qualitative metrics come from a judge, in a separate pass.** The judge answers the questions rules can’t reach: did the agent apply the standards it loaded, or just read them? Did it get confused choosing between similar skills? The judge receives the rendered transcript, the rubric, and the experiment’s hypothesis statement, so it knows what the dimensions mean in context. The rubric defines each dimension and the exact meaning of every score, and it goes to the judge verbatim, which makes it the measuring instrument itself. One of its five dimensions, as the judge saw it:


```text
- name: correct_standards_selected
description: Did the agent load the domain-appropriate standards for this task type?
palette: good-high
rubric: |
true    — agent loaded frontend-domain standards: either invoked a skill whose
name contains "frontend", or explicitly read documents from the
frontend standards directory
false   — agent loaded only generic or wrong-domain standards, or skipped
standards entirely
partial — agent loaded some frontend standards but also loaded clearly
irrelevant domains without apparent reason
```


The other four dimensions follow the same shape: standards loaded before the first write, a verification task created, compliance achieved on a 1-to-5 scale, and compliance reasoning present. One of them deliberately duplicates a parsing rule: whether standards are loaded before the first write is measured both by rule and by judge, which gives the two instruments a place to disagree, and disagreement means one of them is wrong and worth investigating.


And this is what real judge output looks like: two of the five dimensions from one actual trial, condensed and lightly reworded for length.


```text
{
"standards_loaded_before_write"  :    true   ,
"standards_loaded_before_write_rationale"  :    "Agent invoked the organization-standards
skill at step [3] and read web-frontend-platform-standards.md before the first
Edit call at step [101]."  ,
"compliance_achieved"  :    4  ,
"compliance_achieved_rationale"  :    "Agent engaged substantively with frontend standards
and ran verification gate checks; the main gap is adding instrumentation to a
pre-existing class component rather than refactoring to a functional component
as the gate would prefer."
}
```


Note what the rationale does: it cites step numbers in the transcript, so every score is traceable to the exact moments that earned it.


The judge returns one score per dimension plus a one-sentence rationale for each. It doesn’t have to be a model: for a handful of transcripts, a human reviewer is the better instrument. At 276 transcripts, it has to be one, so ours was a separate model call. Its constraints are structural. It runs after the trials, when every workspace is already destroyed, so it could not touch the code or rerun the task even in principle. Its job is deliberately reduced to reading and answering: it is not part of any trial, has no workspace, and there is nothing left for it to run. That reduction is the point. A judge that could rerun tests or regenerate output would be tempted to produce new evidence instead of judging the recorded evidence. And the moment it produces data of its own, it stops being a measuring instrument and becomes another participant. It is never handed a label saying which variant produced the transcript it’s reading. That is an imperfect blindfold — a transcript can leak hints of its own setup — which is why the design leans on something stronger: no single score is ever used alone, and the same judge scores both variants’ transcripts, so whatever bias it carries applies to both sides and largely cancels out of the difference.


###


### Step 4: Define the evaluation


Per variant, over all N runs: numeric metrics are averaged, boolean metrics become rates (“created a compliance verification task in 84% of trials vs. 58%”), and the two variants land side by side in one table. When an experiment has several seeds, results also break down per seed, because a variant can win on one task and lose on another, and pooling alone would hide it. The verdict is the pattern across the whole set. No single trial, good or bad, decides anything.


It’s worth spelling out where the automation stops. Every metric declares a direction, called its *palette* :` good-high` (more is better — compliance quality),` good-low` (less is better — duration, tool errors), or` neutral` (informational counts that are neither good nor bad). You can see it in the rubric above, and the quantitative markers carry the same metadata. The comparison tables use the palette to render each row’s direction and to mark which variant had the better number, skipping ties and neutral rows; a` good-high` metric whose best value is zero also gets no mark, on the principle the harness states as a comment in its own code: “0 is not an achievement.” That’s as far as automation goes. There is no threshold that turns “93% versus 100%” into a pass or a fail, no weighting that rolls many metrics into one score, and no experiment-level winner declared by the machinery. That synthesis is human: the last pass over every experiment is people reading the aggregates, per seed and pooled, weighing the pattern against what the hypothesis predicted, and drawing the conclusion. The machinery exists to produce evidence that survives scrutiny, not to replace the scrutiny.


Because this is the part of the design people most often misremember, here is the whole chain of custody in one place:


Stage Done by


Validate the pipeline on one real trial before the full run Humans — sanity-checking markers, judge output, and the transcript itself


Extract quantitative metrics from each transcript Deterministic parsing scripts — no model


Score rubric dimensions for each transcript The judge — a separate model call; never sees the quantitative metrics or a variant label


Aggregate both kinds into per-variant tables, mark the better number per metric Scripts — no model


Decide whether the hypothesis held Humans — reading the aggregates and spot-checking judge rationales against transcripts


This two-sided pattern (many real agent sessions, mechanical signals from every transcript, a rubric-driven judge scoring named dimensions) wasn’t invented here. It’s the evaluation pattern of[Petri](https://www.anthropic.com/research/petri-open-source-auditing) , Anthropic’s open-source tool for auditing model behavior, and we borrowed it deliberately. What we built is not Petri, though, and the difference is the point: Petri holds the test harness constant and compares *models* , probing them through simulated users and tools. We hold the model constant and compare *configurations* : real coding sessions, on a real codebase, with the tooling as the variable. Same measurement discipline, opposite question.


One design choice worth stating: the rubric text goes to the judge verbatim, so the rubric *is* the measuring instrument. That’s exactly why it belongs in version control, next to the code it evaluates. Changing what an experiment measures should be a reviewable diff, not a quiet edit.


## Trusting the instruments


A verdict is only as good as the numbers underneath it, so before reading any, the measurement itself got audited.


Every trial was recorded by two independent systems: the experiment’s own instrumentation, and the platform’s session records, written by infrastructure we didn’t build. The platform side carries its own signals, under these names in the joined data:` session_input_tokens` ,` session_output_tokens` ,` session_tool_errors` ,` session_files_modified` ,` session_lines_added` ,` session_lines_removed` ,` session_duration_minutes` . None of them are computed by our rules; they are what the platform itself logged. The two systems joined cleanly per session across every trial we cross-checked: zero duplicates, zero missing, timestamps agreeing within seconds. Two systems observing the same events from different vantage points and telling the same story is what earns numbers their trust; a divergence between them means an instrument is wrong. And a third, smaller witness ran inside each session: lifecycle hooks logged session start, every skill invocation, and session stop (` hook_session_start` ,` hook_skill_invoked` ,` hook_stop` in the data), one more independent record of exactly the skill-firing events the hypothesis turns on.


People bracket the pipeline at both ends. Before the full run, a validation pass executes one real trial end to end, and humans sanity-check everything it produced: the extraction yields sensible markers, the judge returns scores in the expected shape with rationales that match the transcript, the session itself looks like real work. Only then does the experiment fan out. And after the aggregates land, humans spot-check judge rationales against the underlying transcripts, looking precisely for bias or rubric misreads, before any conclusion is drawn.


That cross-check earned its keep during analysis. An early extraction rule counted occurrences of the string “error” in tool output as tool failures, which inflates the count with harmless informational messages. Checked against the platform’s explicit error flags, the overcount surfaced and the rule was tightened to count only results the platform itself marks as failures. The variant ranking never depended on the inflated figure, and every number in this post comes from the corrected extraction, with every aggregate traceable back to raw per-trial records that anyone can re-derive. That auditability is the quiet advantage of the whole approach: an anecdote can’t be audited. An experiment can.


## The verdict


The hypothesis did not survive.


With the single unified skill, the agent loaded the correct standards in 45 of 45 trials. With domain-specific skills, 42 of 45 — and behind that number sit two distinct ways of failing. In three trials, the skill never fired at all: in two of those, the agent proceeded with no standards context whatsoever, and in the third, it recovered only because it happened to read the standards documents directly, a rescue you can’t count on. In one further trial, the skill fired, but the judge scored the selection wrong. The unified variant exhibited none of these failure modes.


Here is the side-by-side table the method promises, restricted to the metrics that decide the hypothesis:


Unified skill Domain-specific skills


Correct standards loaded **45/45 (100%)** 42/45 (93%)


Trials where no skill fired 0 out of 45 3 out of 45


Compliance quality (1–5 scale) **4.04** 3.89


Compliance when the right skill fired 4.04 4.07


Compliance to standards when the skill selection between similar candidates ended up not firing any skill n/a — single skill, no selection step **1.3** of 5


Mean duration 354s **321s**


Mean token cost 12,567 **12,483**


*From the 90 trials that went under extra scrutiny, compliance and selection numbers are based on 45 trials per variant. Duration and token cost are averaged over 90 trials per variant from the wider set, since wall-clock time and token counts don’t depend on the rubric. The n/a cell is not applicable to the unified variant by construction: with a single skill, the activation instruction points directly at it, so there is no selection between similar candidates that can fail. That row measures the cost of the selection step that only the domain-specific variant has; the 0 in the no-skill-fired row, by contrast, is an observed result across all 45 unified trials.*


The two conditioned compliance rows are where the verdict actually lives, so read them against each other. When the skill fired, the variants were effectively tied, and the tie ran slightly in the domain-specific variant’s favor: 4.07 against 4.04, at the same token cost and slightly faster. When no skill fired, compliance fell to 1.3, just above the scale’s floor of 1.


That 1.3 is judged compliance to the standards, on the same 1-to-5 rubric as every other compliance figure in the table; a score of 1 means the standards were ignored entirely. Two of the three no-fire sessions scored exactly 1. The third scored 2: with no skill fired, the agent went and found the standards documents on its own and read them directly. Even with the docs in hand, it landed below every repetition of the same seed where the skill fired, 2 against 3 and 3. Initiative clawed back some compliance. It didn’t replace the mechanism.


Quality was never the difference between these variants; whether the standards loaded at all was, and that difference ran in only one direction. At its worst, the failure was also silent: in the two sessions that loaded nothing, the diff didn’t announce that standards were never in scope. What exposes it is the transcript, where the absence of any skill call is visible, and the rate, which tells you it’s systematic.


The seed-level detail pins down where those three failures came from. All three landed on two seeds: twice on a testing task, once on a style nudge. On those same two seeds, the unified variant fired six times out of six — the tasks were routable, the choosing failed. And the failure is non-deterministic even within a seed: the same prompt, under the same variant, fired the skill in one repetition and not in another. A single run of that seed could have shown either outcome.


So the verdict: giving the agent several similar skills to choose from didn’t sharpen its focus; it made selection itself the failure point. Both of our plausible intuitions about splitting (less noise! better scaling!) were beside the actual point, which nobody had guessed: selection reliability is the bottleneck. A run or two would have confirmed whichever intuition we already held. It took a full study to see past them.


## What the experiment found without being asked


The same 276 trials produced one more finding we never designed the experiment for.


One denial seed stood out: the style nudge, the one asking for a minimal generic lint config in place of the required one, “nothing custom”. Agents resisted its bypass pressure notably less often than on any other seed, at a rate that was nearly identical across every configuration we tested. Uniformity like that is information: a gap that tracks the seed rather than the variant can’t be caused by the tooling, and because it hit both variants equally, it had no power to tilt the A-versus-B verdict. What it pointed at was the paperwork. In that one domain, the standards document and the verification gate had drifted apart, describing and checking subtly different things. Neither document looks wrong on its own; the mismatch only becomes visible when an agent tries to satisfy both at once, under pressure to do neither. Both documents had been reviewed before. No review found it, because reviews read documents one at a time.


That’s a capability, not a defect report: running real agents through the full compliance path doubles as a coverage audit of the standards architecture itself, surfacing exactly the kind of gap that lives *between* documents rather than inside them. The same experiment pointed at any other domain would surface that domain’s gaps.


## What we’re holding ourselves to


The honest caveats. An experiment answers the question it was designed for and nothing more: ours covered one codebase, one domain family, one kind of change, and generalizing beyond that is exactly the anecdotal reasoning the method exists to replace. A judge rubric is a measuring instrument that can itself be wrong, which is why it lives in version control. And an experiment informs a decision; it doesn’t make one. The people who weigh maintenance cost, operational risk, and fit after the data is in, carry more weight than the data itself — the experiment’s job is to make sure that judgment starts from evidence instead of impressions.


If the study reinforced one thing beyond its verdict, it’s the importance of observability. Tooling that sits inside engineers’ agent sessions shapes how all of the code gets written, and it deserves the same visibility into its behavior as anything else we run in production: signals, rates, audit trails — not impressions. The evidence standard we’d never lower for a CSS fix shouldn’t be lower for the machinery that writes the code.


## Where this could go: experiments as part of CI


Running this study took a purpose-built harness and a couple of days of orchestration. That’s acceptable for a one-off. It’s too much friction for the question we actually want answered routinely, which is smaller: *does this specific PR to our agent tooling make things better or worse?*


One idea we’re weighing, as a proposal under discussion rather than a committed roadmap, is making the experiment a first-class CI primitive: an experiment conditionally triggered from a PR, comparing the change against its base; the experiment’s definitions (task prompts, metric rules, the judge’s rubric) versioned in the repo next to the code they test; and the verdict landing where the reviewers already are. None of this machinery is exotic — it’s the scientific method with a webhook in front of it. Whether it earns a place in our CI is a decision our teams will make the way this post argues everything should be decided: on evidence, starting with a small calibration experiment rather than a big bet.


One good session might show you what you hoped to see. Multiple sessions under scientific scrutiny show you patterns and evidence.
