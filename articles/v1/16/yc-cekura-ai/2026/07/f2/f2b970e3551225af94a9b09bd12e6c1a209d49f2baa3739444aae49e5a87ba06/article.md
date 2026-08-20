---
schema_version: "1.0.0"
document_id: "f2b970e3551225af94a9b09bd12e6c1a209d49f2baa3739444aae49e5a87ba06"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/shipping-self-improving-voice-agent-to-customers"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T01:18:45.997960+00:00"
fetched_at: "2026-07-30T01:18:48.157086+00:00"
content_hash: "sha256:557173db8a1a1e138c7b35fbbe0974b10a1d8db3c69ccee4f5a3ad317853884b"
---

# Shipping a Self-Improving Voice Agent to Customers: The Product and the Playbook

**In short:** Cekura's improvement engine takes a voice agent and a failing test set, diagnoses why the agent fails, proposes prompt and config edits, redeploys, and re-runs the tests until they pass.[How that loop works](https://cekura.ai/blogs/self-improving-voice-agents-closing-eval-loop) is a separate post. This one is about everything that had to be true *around* the loop before a customer would point it at their own live agent: what it looks like inside the product, and what two POCs taught us about earning that permission.


---


## A great eval loop is worthless if no one will run it on production


The engine was impressive in a demo. Then we tried to put it in front of customers, and every hard problem was a *different* problem:


- A prompt-optimization run takes five to ten minutes and moves through several phases. How do you show that without a user staring at a spinner, afraid to close the tab?
- The agent is about to rewrite a customer's production prompt. Would *you* click that button on your own live agent? What makes it safe enough to trust?
- The fix has to *start* somewhere. "Here's a text box, describe what's wrong" is a non-starter. If the customer already knew exactly what was wrong, they wouldn't need us.


None of these are eval-loop problems. They're product problems. This is how we solved them, and what two POCs (one a healthcare booking agent, the other a multi-node ElevenLabs vetting workflow) taught us along the way.


---


## Part 1: The product: from insight to fix without leaving the dashboard


### Fixes start from a diagnosis, not a blank page


The entry point isn't the optimizer. It's the **Insights page** .


Cekura continuously[distills a project's production calls into metric failure-mode insights](https://cekura.ai/blogs/call-analytics-voice-agents) : for[each metric](https://cekura.ai/blogs/voice-ai-evaluation-metrics) , the recurring ways the agent fails, grouped into categories and ranked by how often they occur. So the customer doesn't open a blank prompt editor. They open a page that already says *"your agent fails to confirm the appointment date in 18% of booking calls, here are the transcripts."*


That reframing matters. Every improvement run is anchored to a **specific, observed, quantified failure mode** , not a vague hunch. And it collapses the distance between "I see a problem" and "I'm fixing it" to a single click: right on the failure-mode row, you can[generate a test scenario](https://cekura.ai/blogs/how-to-generate-voice-agent-test-cases) that reproduces it, or hit **Improve Agent** to kick off a full optimization run against it.


### Showing a ten-minute autonomous run without a babysitter


The improvement session runs in a backend sandbox: the heavy, multi-phase agentic work happens server-side. The product's job is to make a long-running background process *feel* trustworthy and unobtrusive.


We did that with a floating **progress panel** that lives in the app's global progress stack (bottom-right, alongside other background operations). Two design decisions did most of the work:


- **It persists across navigation and refresh.** The session lives on the backend and is identified by a session ID; the panel just polls status every few seconds until the run reaches a terminal state. So you can start a run, navigate away, reload the page, come back an hour later, and the panel is still there, still accurate. Closing the tab doesn't kill the run and doesn't lose your place.
- **It surfaces phase-level progress, not a fake percentage.** You see the run move through its stages, so a five-minute wait reads as *"it's working through the diagnosis"* rather than *"is this frozen?"*


The lesson that generalized: **a long-running agent is a background job, and it should behave like every other background job in your product.** Resumable, observable, and impossible to lose by accident. Don't invent a one-off "AI is thinking" modal that traps the user on one screen.


### The diff is the trust surface


The single most important UI in the whole feature is the **diff view** .


When the agent proposes a change to a prompt or config, we never silently push it. We render it as a **before/after diff** , in a real code editor view, that a human reviews. Additions and removals are marked the way any code review marks them, and you approve the change before anything touches the agent.


The underlying capability is identical whether or not that review step exists; only the trust posture changes. "AI rewrote your production prompt" is terrifying. "AI proposed this diff, here's exactly what changes, click to apply" is a tool. If you build autonomous agents that modify things people care about, **make the change reviewable before it lands** , and make the review honest and legible.


---


## Part 2: The POC playbook: earning the right to touch a live agent


Product polish gets a customer to *look* at the button. What gets them to *press* it is knowing the blast radius is zero. Three practices did that.


### Clone before you optimize, so production is never touched


For managed providers like VAPI and ElevenLabs, the loop never edits the customer's real agent. Before optimizing, it stands up a **disposable clone** and iterates entirely on the copy.


"Clone the agent" sounds trivial and absolutely is not, because a real agent is a *graph* , not a single object:


- **Tools are shared, ID-referenced resources.** If the clone kept the original tool IDs, the optimizer would end up PATCHing *production* tools. So we clone every referenced tool first and repoint the clone at the copies, building an old-ID → new-ID map as we go.
- **Handoffs and transfers form a graph.** A VAPI squad or an ElevenLabs transfer chain points at other agents. We follow those links transitively, guarding against cycles, clone every reachable agent, and repoint every intra-graph handoff at the *cloned* members, so transfers stay inside the clone and never leak back into the live system.


If any step of the clone fails, the run **halts** . It never falls through to editing the original. The rule we enforce: *a failed clone is a stopped run, not a risky one.* That single guarantee, "we are working on a throwaway copy, your production agent is untouched until you approve a diff," did more to move POC conversations forward than any demo did.


### Redact before you reproduce


To reproduce a failure we generate a test scenario from real production transcripts, which, for a healthcare or hiring agent, contain personal data. So generation runs **through a redaction step** : the failure *mode* and the conversational *structure* survive into the test scenario; the personal data does not. You get a faithful reproduction of the bug without copying sensitive content into a test fixture. For regulated POCs this isn't a nice-to-have, it's the precondition for being allowed to run at all, and it sits on top of the same[data-handling posture](https://cekura.ai/blogs/securing-conversational-ai-observability) the rest of the platform runs on.


### Feed the learnings back into the system


Every POC changed the optimizer itself. The healthcare booking POC, the same one whose 75% → 100% run is walked through in[the eval-loop post](https://cekura.ai/blogs/self-improving-voice-agents-closing-eval-loop) , is where the value of the[full-set regression sweep](https://cekura.ai/blogs/automated-recurring-voice-agent-tests) became non-negotiable: an intermediate "100% on the subset" hid two silent regressions that only a sweep across the entire test set caught before anything shipped. The point here is the *mechanism* . POC learnings don't live in someone's head; they get written back into the skill, so the next customer inherits them automatically.


Which brings us to the POC that taught us the most.


---


## Part 3: Single-prompt is a myth at the top end


Most voice agents are a single prompt. One POC's ElevenLabs vetting agent was a **multi-node workflow** : a graph of nodes, each with its own prompt, wired together by edges. The single-prompt mental model is actively wrong for these. An optimizer that assumes one prompt will "fix" the agent by editing a prompt that never even runs on the failing turn.


Making self-improvement work here meant the loop had to understand the workflow engine's actual execution model, including how routing, transitions, and per-node variables behave, rather than just the text of a prompt. That's a real body of knowledge, and the important part isn't the specifics. **It's where that knowledge ends up.** Every hard-won lesson from a POC gets written directly back into the skill the agent runs, so the next customer on a multi-node workflow doesn't rediscover any of it. The system already knows, because a previous POC taught it. The POCs don't just improve one customer's agent; they improve the optimizer itself, in the same way[human feedback on evals compounds into better evals](https://cekura.ai/blogs/voice-evals-auto-improve-human-feedback) .


The meta-lesson for anyone building on top of voice platforms: **the top end of your customer base doesn't look like the median.** The single-prompt happy path covers most agents and none of the hardest, most valuable ones, and every one of those hard cases should make your system permanently smarter rather than getting patched and forgotten.


---


## The takeaway


Closing the eval loop was the algorithm. Making it something a customer will run on their own production agent was a different discipline entirely:


- **Start from a diagnosis, not a blank page.** Anchor every fix to an observed, quantified failure mode.
- **Treat the agent run as a first-class background job.** Resumable, observable, impossible to lose.
- **Make every change a reviewable diff.** Trust comes from legibility, not magic.
- **Guarantee zero blast radius.** Clone the whole agent graph, redact sensitive data, halt on any doubt.
- **Respect the execution model.** A multi-node workflow is not a big prompt, and pretending otherwise fails silently.


The engine is what makes an agent better. The product and the playbook are what make a customer willing to let it.
