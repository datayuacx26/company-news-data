---
schema_version: "1.0.0"
document_id: "71a1320cc5a7be395017bcd3fc317b3bfb6f681cad81046c0b7bfe201f55c3ae"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/lessons-from-the-field-cekura-fde-findings"
published_at: "2026-03-22T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:f57565d7b49ba6abafad2dbb7021efd0334eab8b965989f5281118e3b9f561a5"
---

# Lessons from the Field: What I Learned Setting Up AI Agents as Cekura's First FDE

## The View from Metric #1


When I joined Cekura as the founding Forward Development Engineer, my job was simple on paper: help customers set up metrics and evaluators for their voice AI agents. In practice, it meant staring at thousands of call transcripts, debugging why a "Booking Flow" metric was mysteriously failing calls that had nothing to do with bookings, and learning firsthand that the gap between "this metric sounds right" and "this metric works in production" is enormous.


Over months of deploying testing and observability across clients ranging from property maintenance dispatchers in Europe to nationwide workforce staffing platforms, I've catalogued a set of recurring mistakes, hard-won remedies, and techniques that most teams building on voice AI evaluation platforms aren't using yet. This post is the unfiltered version.


---


## 1. Read the Transcripts Before You Write a Single Metric


This sounds obvious. It isn't. The most common failure mode I see is teams writing evaluation prompts based on what they *think* their agent says, rather than what it actually says.


Voice AI transcripts are messy. Timestamps overlap. Tool calls fire mid-sentence. The agent says "Give me one moment" and then a chain of three background function calls runs before speech resumes. If you write a metric that assumes clean turn-taking, it will break on real data immediately.


**The fix:** Before writing any metric, pull 3-5 real conversations from your observability pipeline and study the raw` transcript_json` . Look at what roles appear (` Main Agent` ,` Testing Agent` ,` Function Call` ,` Function Call Result` ), how tool calls are structured, and what the actual conversation flow looks like. Metrics written without reading real data will miss edge cases that only surface in production.


Here's a concrete example. A client's agent description said "ask only one question at a time." Without reading transcripts first, the obvious metric is: fail any agent turn containing more than one question mark. Sounds objective. Sounds correct. It's wrong.


In real calls, the agent routinely said things like "Can I get your name and date of birth?" or "Is this a new issue, or an existing one?" The first is a related data cluster — two pieces of information the caller can answer in one breath. The second is an A/B rephrasing that's really just one question. Both are completely natural. Both would fail a literal "one question per turn" metric. The *spirit* of the instruction was "don't overwhelm the caller with unrelated questions" — something like "What's your name? Also, did you get our text message about the new policy?" would be a legitimate failure.


I only caught this because I read the transcripts before writing the prompt. What looked like a simple, objective check turned out to require understanding conversational context — which questions are related, which are rephrased alternatives, and which are genuinely unrelated topic-jumps. Plan for at least one iteration — that's not a failure of your prompt, it's how you discover what the data actually looks like versus what you assumed.


---


## 2. The Cross-Pollination Problem (And Why Your Metrics May Be Lying to You)


Here's a pattern I discovered the hard way. You have a voice AI agent with a large system prompt — say, 56,000 characters covering emergency handling, booking flows, cancellation flows, rescheduling, and callback procedures. You write a metric that evaluates whether the agent followed the correct emergency protocol. You pass the full agent description to the LLM judge via a template variable so it can derive the rules.


The metric fails a call. You read the explanation. The LLM flagged the agent for... not following the booking flow.


This is cross-pollination. When you dump an entire agent description into the evaluation context, the LLM reads everything and can penalize based on rules from completely unrelated flows. Your Emergency metric fails because the judge noticed the agent didn't collect a postal code — a requirement from the Booking section that has nothing to do with emergencies.


This was flagged by a client as a systemic issue across their entire metric suite. And they were right.


### The Three-Layer Fix


After several rounds of iteration, I landed on a scoping pattern that reliably prevents cross-pollination:


**Layer 1 — SCOPE & FOCUS.** Open every metric with an explicit declaration: "This metric evaluates X ONLY. IGNORE all non-X rules in the agent description." Then briefly name what other flows exist and explain that separate metrics cover them. This gives the LLM a conceptual map so it doesn't go hunting in the wrong sections.


**Layer 2 — DO NOT FLAG.** Enumerate common false positives specific to this metric. Name them by behavioral pattern ("Standard booking steps not followed," "Emergency triage questions not asked"), not by section name from the agent's prompt. This is the highest-impact addition — it directly prevents the LLM from penalizing adjacent-flow violations.


**Layer 3 — FAILURE CONDITIONS (Only These Count).** Replace open-ended failure criteria with a narrow, closed list. Instead of "fail if the agent didn't follow the correct procedure" (which invites the LLM to find creative reasons from other flows), say "only flag a failure if ONE of these specific patterns occurs" and list 3-5 concrete behavioral patterns.


One critical rule: all scoping must be generic and concept-based. Write "the emergency sections of the agent description," not "the Emergency Flow section." Write "standard bookings, rescheduling, cancellations" as concept examples, not "Service Booking Flow, Updating Appointment Flow." If you hardcode section names from one client's agent description, your metrics can't be reused across agents. I learned this after deploying the same metric suite across four different agents that all used different heading conventions for the same concepts.


---


## 3. Stop Hardcoding Identity Data in Your Evaluators (It Causes Hallucinations)


This one shows up on nearly every new client engagement. A test scenario for a scheduling agent has instructions like: "Say your name is John Smith, date of birth January 15, 1990, and you want to book an appointment."


For a short, single-purpose scenario, this seems harmless. It's not. The problem compounds in two ways.


First, when a voice-based testing agent reads these instructions, it internalizes them as unstructured text. In a long conversation — say, a multi-step flow where the caller authenticates, performs an action, then needs to re-authenticate for a second action — that persona information buried in the instructions gets lost. I watched this happen in real time with a client: they had a DOB verification scenario where the testing agent authenticated successfully, completed the first task, then was asked to verify identity again for a second action. The testing agent made up a completely wrong date of birth. Not a typo — garbage. The DOB was sitting in the middle of a paragraph of instructions, and by the time the conversation had gone through fifteen turns of booking flow, the testing agent had effectively forgotten it.


Second, if the mock backend expects specific names to return specific data (insurance plans, payment plan history, worker type), and the hardcoded values don't match what the backend recognizes, the test produces garbage results that look like agent failures but are actually test infrastructure failures.


### Start with One Profile Per Scenario


My advice: create a dedicated test profile for every single scenario, even if it feels like overkill. Don't share profiles between scenarios to start. One profile, one scenario, no ambiguity. This sounds excessive until you realize that the alternative — debugging hallucinated DOBs and mysterious backend mismatches across a 50-scenario test suite — is far more expensive.


Test profiles store identity data (name, DOB, address, insurance, patient status) as structured objects attached to the simulation. The testing agent reads from the profile reliably, even in long conversations. Your backend receives profile data as dynamic variables (for outbound and websocket-based testing), exactly like production callers would provide it.


Once you're comfortable with profiles, you'll start seeing their real power — and this is the part most teams underestimate.


### Test Profiles Are the Gateway to Mock Tools


Here's where it gets interesting. One of our clients had a medical scheduling agent with roughly 90 test scenarios covering scheduling, rescheduling, cancellation, verification, intake, safety triage, and error handling. Without mock tool enablement, every scenario required them to manually construct the entire backend state for each test conversation.


Think about what that means. If you want to test "patient tries to reschedule but has no existing appointments," you need to: create a patient identity in your system, make sure they have no upcoming appointments, define the exact response your lookup tool will return when queried, create a test profile on Cekura with that identity, and make sure no other scenario reuses that patient data in a way that would create conflicting state. And that's a simple case.


Now imagine "patient tries to cancel, tool fails three times, agent escalates to human." You need to set up the patient, create an existing appointment for them, configure the cancellation tool to fail on the first three attempts, define what the failure response looks like, then define what the eventual success or escalation path returns. You may not even know what tool calls the agent will make until you run the scenario once — at which point you discover you also need mock responses for the verification tool, the appointment lookup tool, and the transfer tool, none of which you anticipated.


The complexity gets uglier with every edge case. A successful payment requires different mock data than a declined payment. A rescheduling scenario for a patient with one upcoming appointment is different from one with three. A new patient who isn't in the system requires different tool responses than an established patient. Multiply that by 90 scenarios and you're managing a combinatorial explosion of mock state on your own infrastructure.


Test profiles with mock tools managed by the evaluation platform collapse all of this. The profile defines who the caller is. The mock tool configuration defines what the backend returns. You describe the scenario — "patient with one upcoming appointment tries to reschedule, no available slots with same provider" — and the platform handles the state management. No local mock databases. No conflicting patient records across scenarios. No surprise tool calls you forgot to mock.


Start with one profile per scenario. Get comfortable. Then lean into mock tools. The investment pays off exponentially as your test suite grows.


---


## 4. Your Triggers Are Doing More Work Than You Think


Most teams treat metric triggers as a "nice to have" — a simple on/off switch for whether a metric runs. They're actually your first line of defense for evaluation quality.


A good trigger pattern follows a positive-then-negative structure:


> Evaluate this metric when: the call involves a callback request or transfer to a human agent. Do NOT trigger if: the call is a voicemail with no human interaction, the caller hangs up before any substantive conversation, the call is an outbound notification with no engagement, or the transfer occurs as part of the standard emergency protocol (covered by the Emergency metric).


When a trigger doesn't fire, the metric outputs N/A — not True, not False. This means even binary pass/fail metrics actually have three outcomes: Pass, Fail, and Not Applicable. This is correct behavior, and it's the clean way to handle "this metric doesn't apply to this call" without forcing false positives or negatives.


The trigger handles the obvious exclusions. The metric description handles the nuanced edge cases that need transcript context to determine. Two layers of N/A handling, each operating at the right level of specificity.


---


## 5. The Clever Stuff Nobody's Using Yet


### Building Eval Suites with Conversational AI


Here's a workflow I stumbled into that dramatically accelerates evaluator creation. Instead of manually writing 50+ test scenarios from scratch, start by having a conversation with a general-purpose AI (ChatGPT, Claude, Gemini) about the agent you're testing.


Describe the agent's purpose, its main flows, its edge cases. Ask the AI to brainstorm caller personas and scenarios that would stress-test each flow. What you get back are rough scenario stems — not production-ready, but structurally sound starting points that capture the *shape* of each test case.


Then feed those stems into Cekura's` generate` endpoints. The platform takes your rough sketch and fleshes it out into a fully-formed evaluator with proper instruction formatting, expected outcome prompts, and appropriate trigger conditions. What would take a day of manual scenario writing becomes a two-hour pipeline: brainstorm with AI, extract stems, generate via API, review and refine.


The key insight is that evaluator design is a two-phase problem: *what to test* (creative, divergent thinking) and *how to test it* (structured, platform-specific formatting). General-purpose AI is excellent at the first part. Cekura's generate endpoints handle the second. Trying to do both manually is where teams get bogged down.


We've taken this a step further internally. At Cekura, we've built[Claude Code skills](https://github.com/cekura-ai/claude-skills) — reusable prompt-driven workflows — that encode our eval design and metric design best practices directly into the development environment. When I'm working with a new client, I don't start from a blank page. I invoke a skill that already knows the test profile patterns, the instruction formatting conventions, the anti-cross-pollination scoping layers, and the common pitfalls documented across every engagement. It's the same brainstorm-then-generate pipeline, except the brainstorming step has institutional memory baked in. The AI isn't just generating scenario ideas from scratch — it's generating them with knowledge of what's worked and what's failed across dozens of prior deployments.


### Dynamic Variable-Driven Metrics: The Generalization Pattern


This is the most powerful technique I've deployed and the one I think is most underutilized.


Some clients run multi-agent flows where different system prompts are active at different points in the call. A staffing platform I worked with had 11 different agent nodes — intro, hard requirements vetting, soft requirements vetting, work experience, onboarding, reconnection, and more — each with its own system prompt injected as a dynamic variable on every call.


The naive approach: write one metric that evaluates the agent against its full system prompt. This brings back the cross-pollination problem, except now it's 11 different prompts worth of instructions competing for the LLM judge's attention.


The better approach: create one metric per dynamic variable. Each metric's prompt references only its specific variable —` {{dynamic_variables.introAgentPrompt}}` for the intro metric,` {{dynamic_variables.onboarding1099AgentPrompt}}` for the 1099 onboarding metric, and so on. The LLM judges the agent's behavior against exactly the instructions that were active for that specific node, nothing more.


This pattern produces dramatically better results. When I validated it across 30 calls with 11 metrics each (330 total evaluations), trigger scoping worked perfectly — onboarding metrics returned N/A on interview-only calls, the legacy full-vetting metric correctly identified when modular transfers were in use. And the failures that surfaced were real: an 80% fail rate on end-protocol violations where agents were asking "do you have questions?" instead of performing a silent transfer. That's actionable signal, not noise from cross-pollinated flows.


The discovery workflow is straightforward: fetch 3-5 sample calls, inspect the` dynamic_variables` payload, and map each meaningful variable to a metric. System prompts (long instruction strings), configuration flags (booleans), and contextual data (prior call summaries, worker types) are all candidates for metric scoping.


### Beyond Prompts: Dynamic Variables for Trigger Scoping


Dynamic variables aren't just for metric prompts — they're equally powerful in triggers. If a client sends` employmentType: "W2"` or` onboardingFlagEnabled: true` on each call, you can write trigger conditions that scope metrics to specific worker types or feature states. A W2 onboarding metric that only fires when the dynamic variables indicate a W2 context eliminates an entire category of false evaluations on 1099 calls.


---


## 6. The Mistakes That Cost Real Money


A few quick hits — things I've seen waste testing credits, debugging time, or both:


**Missing end-call tools.** If you don't enable` TOOL_END_CALL` or` TOOL_END_CALL_ON_TRANSFER` on your evaluators, completed calls will run until the platform timeout. That's minutes of dead air per scenario, at scale across a test suite.


**Using instructions for voice characteristics.** Writing "speak in a mumbling voice with a thick accent" in your evaluator instructions does nothing. Instructions control what the simulated caller *says* . Personalities control how they *sound* — accent, interruption patterns, background noise. Use the right lever.


**Overly specific expected outcomes.** "The agent should book an appointment for March 15 at 2:30 PM with Dr. Rodriguez" will fail whenever the mock backend returns different availability. Write behavioral outcomes instead: "The agent should successfully complete the booking flow and confirm the appointment details with the caller."


**Not using N/A as a first-class outcome.** Every metric should define when it returns N/A. Without this, you're forcing pass/fail judgments on calls where the metric simply doesn't apply, which poisons your aggregate data and makes it impossible to trust your dashboards.


**Choosing the wrong metric type.** Most platforms offer both LLM-as-judge metrics and custom code metrics. Custom code feels more rigorous — you're writing Python, parsing timestamps, computing exact gaps. But voice AI transcripts are messy in ways that break sequential parsing. Agents transfer mid-tool-chain, background tasks complete after speech has already resumed, timestamps overlap. I've had custom code metrics work perfectly on 8 out of 10 test calls and produce impossible results on the other 2 because the transcript didn't guarantee the ordering my code assumed. LLM judge metrics handle these nuances naturally because they read context, not indices. Default to LLM judge. Reserve custom code for cases where you need to gate on another metric's output or do pure programmatic logic that genuinely doesn't require understanding conversation flow.


---


## Start with the Data, Not the Dashboard


If there's one thing I'd tell every team setting up AI agent evaluation for the first time, it's this: read the transcripts first. Not summaries. Not metadata. The actual, raw, timestamped conversation data with all its messy tool calls and overlapping timestamps and unexpected edge cases.


Every good metric I've built started with staring at real conversations until the signal became obvious. Every bad metric I've built started with an assumption about what the data would look like.


The tools and techniques in this post — anti-cross-pollination scoping, dynamic variable-driven metrics, trigger layering, conversational AI for eval brainstorming — are all refinements. They make good metrics great. But they can't fix a metric that was written without understanding the data it's measuring.


Pull the transcripts. Study the structure. Then build.
