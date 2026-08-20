---
schema_version: "1.0.0"
document_id: "ec0f9c6694ff7914938cc4427ba6fa261acc5899e4a2c431454e82112c7b4b5f"
company_key: "yc-cargo"
company: "Cargo"
source_id: "yc-cargo-news-import-f4a8864e899b"
canonical_url: "https://www.getcargo.ai/blog/ai-loop-engineering-for-gtm"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-17T13:47:34.559717+00:00"
fetched_at: "2026-08-17T13:47:35.308858+00:00"
content_hash: "sha256:f0fefa5877f2af5077d530badd8b835b51f32dcca936268da37f73e547f3f2b2"
---

# AI loop engineering for GTM: how you build an autonomous revenue engine

**Loop engineering is the practice of designing the system that prompts your agent, instead of prompting it yourself.** A loop has five parts: a trigger that starts it without you, a goal stated as a verifiable end state, the actions it’s allowed to take, verification that decides when it’s done, and memory that carries across runs.


The term came out of the coding-agent world in mid-2026, and the people who coined it describe the shift the same way: they don’t prompt the model anymore, they have loops running that prompt it for them.


Four of those five parts port to go-to-market easily. The fifth is the whole problem, and it’s why most GTM agent projects stall one week after an impressive demo.


## Coding agents inherited a free verifier. Revenue didn’t.#


Loop engineering worked in software first because software ships with its verifier already built. Tests pass or fail. Types check or they don’t. The build is green or red. An agent can run for two hours unattended because something other than a human can tell it in seconds whether it worked, and because being wrong costs a red pipeline you fix before anyone sees it.


Point the same loop at your pipeline. The agent picks an account, decides a persona is worth contacting, drafts a first touch. What’s the test?


There isn’t one. There are three problems sitting where the test should be.


**The signal is late.** A reply lands four days out, a meeting three weeks out, a closed-won deal six months out. By the time the loop learns anything it has run four hundred more times.


**The signal is noisy.** Reply rates move because the market moved, because a competitor announced something, because it was August. And the volumes are too thin to separate those from your change quickly: against a 3 percent baseline, telling a real lift to 4 percent apart from noise takes roughly 5,300 sends per arm at 80 percent power. Most teams have rewritten the prompt four times before they’ve sent that many. Attribution in revenue is a hard statistical problem, not a boolean.


**The action is irreversible.** A failing test costs nothing. A wrong email reaches a real person at a real account, and there’s no rollback. The blast radius of a GTM loop points outward, at accounts you spent years earning.


So teams do the obvious thing and put a human where the verifier should be. Someone reads every draft, approves every list, checks every score. That works, and it’s where nearly every AI-SDR project lives today. It also caps the system at one person’s reading speed, which means you bought a faster way to generate work for a bottleneck you already had.


**In coding, loop engineering is mostly about designing the loop. In GTM, it’s mostly about building the verifier.** Everything below is that build, in the order it pays.


## The five layers, and why the order isn’t negotiable#


This isn’t a feature list. It’s a dependency chain. Each layer is close to worthless without the previous one, and each one makes the next one cheap.


### The state contract


Every record in the motion sits in exactly one state, from one shared vocabulary: eligible, prioritized, assigned, enrolled, replied, qualified, meeting, opportunity, won, lost, nurture. Every transition carries five things: the reason, the trigger that caused it, the policy version that decided it, the owner, and a cooldown before anything touches that record again.


This sounds like project management. It’s the schema your loop runs on. Without it, “the agent decided” is a sentence with no object: nothing changed that you can name, so there’s nothing to verify, nothing to attribute, and nothing to roll back.


The cooldown is the part people skip and regret. It’s the only thing between a healthy engine and four agents touching the same account in a week because each one independently found it interesting.


### The decision ledger


Every material decision gets an ID: the inputs, the policy version that decided, the confidence it carried, the action that followed, whether a human overrode it, what it cost, and the state transitions it produced downstream.


The ledger answers the one question no GTM stack answers today: **which decision created this outcome.** Not which campaign. Which decision. When a meeting lands you can walk back from the meeting to the reply, to the message, to the score, to the signal that started it, reading the policy version live at each step.


It’s also your eval set. Six months in, the ledger is a labelled history of decisions and what happened to them, which is exactly what you need to test a change before you ship it.


### The closed reply loop


Be precise about what this layer is, because it’s easy to overclaim here. The reply is not the verifier. It’s the outcome signal, and it carries every flaw from the section above: it lands late, it’s statistically noisy, and plenty of correct decisions never produce one at all.


The verifier is what you do with that signal. It’s structured human judgment, and it’s this layer plus the next one: the reply loop routes each outcome to the right person or to no person at all, and the reason codes below capture what that person concluded in a form the engine can use. That is the test suite revenue doesn’t ship with. You build it out of people’s judgment and then spend every subsequent layer using less of it per decision.


Most stacks throw the signal away before any of that can happen. Cargo does everything up to the send and hands the message to the sequencer your reps already live in, which is a deliberate boundary. The mistake is letting the outcome stay over there: the notification that comes back carries no reply content, no owner, and no link, so nothing downstream can route on it.


Close it. Every reply gets classified and drives a deterministic next state, in three tiers:


- **Automatic** : unsubscribe, out of office, clear negative, simple referral, meeting interest, sequence stop, CRM update. Unambiguous, no judgment to add.
- **Proposed, then approved** : objections, pricing questions, competitive comparisons. The engine drafts the next move, a human presses go.
- **Human only** : legal, security, negotiation, anything that changes a contract.


The tiering isn’t a safety blanket over the whole system, it’s a scalpel. Volume sits in tier one and runs unattended. Judgment sits in tier three and always will. Tier two is the interesting one, because it shrinks as the engine earns trust, and it’s measurable: the share of tier-two proposals a human approves unedited is the cleanest autonomy metric you’ll get.


### Structured human feedback


Every human edit, rejection, and exclusion carries a reason code: wrong account, wrong persona, weak evidence, bad timing, unsupported claim, wrong channel.


Small piece of engineering, and it’s the one that changes the economics. Without it, a reviewer who fixes a bad draft has removed one bad draft. With it, the same click is a labelled example pointing at the part of the engine that’s broken. Enough “wrong account” and your scoring is wrong, not your copy. Enough “weak evidence” and your enrichment is thin. Enough “bad timing” and your trigger is firing on a stale signal.


The rule worth writing on the wall: **interventions have to improve the engine, not compensate for it.** Someone who has been fixing the same class of output for a month isn’t a reviewer, they’re a load-bearing part of your architecture, and nobody decided that on purpose.


### The policy loop


Now the engine can propose changes to itself, and this is where compounding starts.


Observe the ledger, propose a policy change, replay it against history to see what it would have decided differently, produce a diff, let a human merge it, deploy to a cohort rather than everywhere, measure against a control, then expand or roll back.


Two of those steps do different jobs and it matters which is which. **Replay tells you what the new policy would have decided. It cannot tell you what would have happened.** Re-run last quarter under new weights and you get a decision diff: these 340 accounts drop out of tier one, these 90 enter, this cohort changes owner. Whether those 90 would have replied is counterfactual, and no amount of history answers it. Coding replay predicts outcomes because the test is deterministic. GTM replay predicts actions only.


That’s still most of the value, because most bad policy changes are obviously bad as decisions. A change that quietly drops your best segment, or triples the volume going to one owner, is visible in the diff before anyone receives an email. Replay de-risks the policy. The cohort deploy against a control is the step that measures the outcome, and it’s the only one that does.


Replay needs the state contract and the ledger built properly, which is why almost nobody has it. Without the ledger you get neither half: you’re shipping a hunch to production and calling it a test.


That’s the full loop mapped onto revenue. The state contract and the ledger are memory. The reply loop and the reason codes are verification. The policy loop is the loop that improves the loop.


## Three loops, three speeds#


The other thing the coding world worked out first: an autonomous system isn’t one loop, it’s nested loops running at different speeds.


**The run loop** , seconds to minutes. One record, one decision, one action. This is the loop most GTM teams have, and having only this one is why their agents never get better. A loop with no outer loop is a scheduled job with a language model in it.


**The review loop** , hours to a day. A human sees the work and approves it or corrects it with a reason code. This is where your training signal comes from, so it’s worth optimizing for reviewer throughput rather than raw output volume.


**The learning loop** , weeks. Policy changes proposed, replayed, merged, deployed to a cohort, measured. This is the one that makes the engine an asset instead of an expense, and it’s the last one anybody builds.


Count your loops. One is automation. Two is a supervised engine. Three is an engine that improves.


## Autonomy is an output, not a setting#


Nobody should decide “the agent handles this now” in a meeting. Autonomy is a level a specific loop earns, on evidence, one motion at a time.


Keep this scale separate from the one above it. The five layers are what you build once, for the whole engine. Autonomy levels are what each individual loop earns on top of them, and they run from L1 to L5 following the convention from self-driving:


- **L1** manual, lives in someone’s head or inbox
- **L2** tooling assists, a human drives every step
- **L3** an agent produces the work, a human reviews and approves
- **L4** approval is a rubber stamp, spot checks only
- **L5** it ships on green checks, humans audit the log


Four rules keep it safe. A loop climbs a rung only after a run of clean cycles at the current one, and the evidence is a streak of zero-edit approvals, not a feeling. Demotion is instant on any reverted decision, caught hallucination, or failed eval: earned slowly, lost immediately. Every class of work has a ceiling set by blast radius rather than by track record, so anything that reaches a customer keeps a human veto permanently. And the kill switch is one commit, so any one person can stop everything in seconds.


We hold ourselves to this in public too: no autonomy claim ahead of the evidence. Cargo’s own revenue engine runs as agents that produce work as diffs, and the current distribution is the honest version of the argument. Three of its seven classes of work have climbed to L4, where a green build merges itself after a 24 hour veto window: the daily log, the Slack scan, the meeting scribe. The other four still need a human to press merge, and the three that can reach a customer or change production carry a ceiling that keeps them there permanently.


One of those four carries an extra condition worth stealing. It may not climb while the artifacts it produces reach nobody, whatever its approval streak says. The streak measures how often a human corrected the agent, and a fleet producing internal documents maxes that out trivially: when we checked, 185 merged pull requests in fourteen days had produced 105 archive entries, exactly one of which had reached a person. A promotion metric that a busy engine can satisfy without touching a customer is measuring the engine’s enthusiasm, not its autonomy. The ladder is the plan, not the press release.


## What this looks like in a repo#


A loop you can’t diff is a loop you can’t govern. Which is easier to show than to argue.


Here is the whole thing in one repository, using[Manifest](https://github.com/getcargohq/cargo-manifest) , the open-source template we extracted from Cargo’s own GTM. The prose directories are for your team and your agents to read.` infra/` is what actually runs, defined with the[Cargo CDK](https://www.npmjs.com/package/@cargo-ai/cdk) and reconciled by one command.


```text
your-gtm/
plan/          the goal, and the outcomes it breaks into
context/       what the company knows: ICP, personas, objections, positioning
initiatives/   bounded efforts with success criteria you can check
cadence/       weekly plans, daily logs, the carryover queue
infra/
models/      the tables: accounts, contacts, signals, decisions
tools/       typed units of work
agents/      workers with a prompt, a step budget, and what they may use
plays/       the loops
context.ts   pushes context/ into the workspace your agents read from
evals/         regression tests that gate a prompt change
outputs/       what happened, append-only


```


### The state contract is columns on a model


```text
// infra/models/accounts.ts
export   const   accounts   =   defineModel  (  "gtm_accounts"  , {
kind:   "native"  ,
extractSlug:   "defineAccount"  ,
additionalColumns: [
{
kind:   "custom"  ,
slug:   "motion_state"  ,
type:   "string"  ,
description:
"eligible | prioritized | enrolled | replied | qualified | meeting | won | lost | nurture"  ,
},
{
kind:   "custom"  ,
slug:   "state_reason"  ,
type:   "string"  ,
description:   "Why the last transition happened, in one sentence."  ,
},
{
kind:   "custom"  ,
slug:   "policy_version"  ,
type:   "string"  ,
description:   "The version of the policy that decided it."  ,
},
{
kind:   "custom"  ,
slug:   "cooldown_until"  ,
type:   "date"  ,
description:   "No play may touch this record before this date."  ,
},
],
});


```


The declared list is authoritative: deploy adds what’s missing, updates what changed, drops what you deleted. Your state machine has a migration path, which is not a sentence anyone gets to say about a field added by hand in a UI two quarters ago.


### The loop itself: a play, and the agent never writes the state


```text
// infra/plays/qualify.ts
const   POLICY_VERSION   =   "qualify.v3"  ;


const   qualifyFlow   =   defineWorkflow  (
"qualify-account"  ,
{
input: z.  object  ({ id: z.  string  (), name: z.  string  (), domain: z.  string  () }),
output: z.  object  ({ verdict: z.  string  (), reason: z.  string  () }),
uses: { qualifier },
},
({   input  ,   uses  ,   model   })   =>   {
// The agent judges. It gets a prompt and a schema, nothing else.
const   q   =   uses.  qualifier  ({
prompt:   `Qualify ${  input  .  name  } (${  input  .  domain  }) against the ICP.`  ,
output: { type:   "jsonSchema"  , jsonSchema: verdictSchema },
}).answer   as   Ref  <{   verdict  :   string  ;   reason  :   string   }>;


// The workflow writes the state. Deterministic, every run.
model.  customColumn  ({
modelUuid: accounts.uuid,
id: input.id,
mappings: [
{ columnSlug:   "custom__motion_state"  , value:   "prioritized"   },
{ columnSlug:   "custom__state_reason"  , value: q.reason },
{ columnSlug:   "custom__policy_version"  , value:   POLICY_VERSION   },
],
});


return   { verdict: q.verdict, reason: q.reason };
},
);


export   const   qualify   =   definePlay  (  "qualify"  , {
model: accounts,
// The trigger: new rows, not a person clicking run.
changeKinds: [  "added"  ],
workflow: qualifyFlow,
// The switch lives in git. Flipping it in a UI doesn't survive a deploy.
isEnabled:   true  ,
// Below this share of successful runs, the batch is unhealthy and says so.
healthThreshold:   95  ,
});


```


Read who does what. The agent hands back a verdict and a reason against a schema. The workflow writes the column. The agent never gets a writable model, so it cannot route a record, skip a step, or invent a state that isn’t in the contract. **Code decides, agents write** stops being a principle and becomes a property of the file. Four independent builds we’ve watched arrived at that split the hard way, three of them customer builds rather than ours.


### The decision ledger is another model, written by the same run


```text
// still inside qualifyFlow
model.  insert  ({
modelUuid: decisions.uuid,
mappings: [
{ columnSlug:   "custom__account_id"  , value: input.id },
{ columnSlug:   "custom__decision"  , value: q.verdict },
{ columnSlug:   "custom__reason"  , value: q.reason },
{ columnSlug:   "custom__policy_version"  , value:   POLICY_VERSION   },
],
});


```


One row per decision, in a table you own and can query. Bump` POLICY_VERSION` in the same pull request that changes the policy and every row afterwards is attributable to a diff you can read.


### The watching is declared too


```text
export   const   qualifyHealth   =   defineAlert  (  "qualify-error-rate"  , {
description:   "Error rate on the qualification loop, hourly."  ,
schedule: { type:   "cron"  , cron:   "0 * * * *"   },
scope: { kind:   "spans"  , workflow: qualify },
threshold: { metric:   "errorRate"  , operator:   "gte"  , value:   5   },
actions: [  alertToolAction  ({ ref: notifyOps, config: { severity:   "high"   } })],
});


```


A loop that can fail quietly eventually will, and you’ll hear about it from a customer rather than from a dashboard. Four lines, and this one can’t.


### The reply loop and the reason codes are the same two moves again


No new machinery, which is the point of the order. The reply loop is the same play with a reply row as its trigger and the three tiers as a filter. Reason codes are one more column on the state contract, written by the reviewer’s action instead of the agent’s, landing in the ledger beside the decision they judge. Everything past the first two layers is variations on a shape you’ve already built.


### The policy loop is the pull request


You do not need to build this one. It falls out of the repo.


A change to the weights, the prompt, the trigger, or the state machine is a diff. CI runs` cargo-ai cdk plan` and posts it against the live workspace as a comment, the way you’d review a terraform plan. A human merges, merging deploys, and rolling back is` git revert` . The autonomy level of each loop is a file in` .github/` , so promoting L3 to L4 is itself a reviewed change. Evals in` evals/` gate the prompt change that started it.


The governance every autonomous system eventually needs turns out to be the version control you already know how to use.


### And the context is in the repo too


```text
// infra/context.ts
export   const   context   =   defineContext  ({ dir:   "../context"   });


```


One line, and the markdown your team writes about the ICP, the personas, and the objections becomes what the agents read at runtime. When an agent gets the wrong account right, the fix is a paragraph in a file, and every loop downstream inherits it.


## What this buys you#


Throughput stops being capped by a reviewer, because tiering by reversibility pushes volume through the automatic tier and concentrates attention where judgment changes the answer. A quarter that went badly stops being an argument and becomes a query against the ledger, which your CRM has never been able to give you.


The one that matters is compounding, and it isn’t the model getting better. It’s your ledger getting longer. Every classified reply, every reason code, every replayed policy change makes the next decision better, on evidence you own, in a system you can audit. A prompt is worth one good answer. A loop with memory and a verifier is an asset that appreciates.


## Where to start on Monday#


Two things, in this order, and neither of them needs an agent.


Put every record in one motion into a single state machine with a reason on every transition. Then give every human intervention a reason code. That’s the state contract and the reason codes, and everything above them gets cheaper once they exist.


Clone[Manifest](https://github.com/getcargohq/cargo-manifest) if you want the repo above instead of a blank directory. It’s MIT, and the prose directories are useful with or without Cargo underneath them. When you get to` infra/` , the[cookbooks](https://github.com/getcargohq/cargo-cookbooks) scaffold the common loops as CDK definitions, so` cargo-ai cdk init my-tam --from getcargohq/cargo-cookbooks/tam-building` starts you on a working play rather than an empty file.


This is the second half of an argument. The first half,[GTM software was built for humans. Agents are the new users.](https://www.getcargo.ai/blog/gtm-infrastructure-for-ai-agents) , is about the infrastructure agents need underneath them. This one is about what you build on top of it.


## Key takeaways#


- **Loop engineering is designing the system that prompts the agent** : trigger, goal, actions, verification, memory. Coding agents inherited verification for free; revenue signals are late, noisy, and irreversible, so in GTM building the verifier is the whole job.
- **The reply is the outcome signal, not the verifier.** The verifier is structured human judgment applied to it: tiered routing, then a reason code on every intervention.
- **Build in dependency order** : state contract, decision ledger, closed reply loop, reason codes on every intervention, then the policy loop.
- **Replay catches bad decisions, not bad outcomes.** A decision diff against history is what de-risks a policy change; a cohort deploy against a control is the only thing that measures what it did.
- **Count your loops.** One is automation, two is a supervised engine, three is an engine that improves.
- **Autonomy is earned per motion** , promoted on a streak of zero-edit approvals, demoted instantly, with a ceiling set by blast radius and a kill switch that always works.


## Frequently asked questions#
