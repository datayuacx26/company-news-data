---
schema_version: "1.0.0"
document_id: "326b8490d6d6202be4c4a73f233f233f158274ccb8713d598f6ddf0e4440e951"
company_key: "yc-asteroid"
company: "Asteroid"
source_id: "yc-asteroid-news-import-1b74f0bf6416"
canonical_url: "https://asteroid.ai/blog/computer-use-in-healthcare/"
published_at: "2026-08-20T00:00:00+00:00"
first_seen_at: "2026-08-20T02:20:59.043392+00:00"
fetched_at: "2026-08-20T02:21:00.414349+00:00"
content_hash: "sha256:22d7fea03c1624ba90f644e58eeb9585e3cf2610e9ea9634b0451c3d99a620ed"
---

# Claude computer use in healthcare

As an early-access participant in Anthropic’s new computer use tool, we’ve spent the past few months hardening it against the demands of production healthcare systems. Today, that tool becomes[generally available on the Claude Platform](https://www.claude.com/blog/computer-use-skills-api-files-api) .


We’ve been deploying computer use agents for our customers since February, into EHRs and other healthcare software that doesn’t offer an API or run in a browser. Over the following four months, execution volume grew steadily, 15–20% month over month. Then, in July, usage more than doubled in a single month. Today, we run thousands of agents every week.


This growth reflects two things happening at once. We’ve seen the latest Claude models getting better at understanding what they see, deciding what to do, and planning sequences of actions. At the same time, we’ve learned how to engineer the workflows and environments around them: reducing unnecessary model calls, working around the limitations of individual applications, and putting deterministic systems in the loop where a model mistake would be unacceptable.


Below, we walk through both: what improved in the models, the engineering we’ve built around them, and what the combination now makes possible.


## Where the models got better


Across our eval and production workflows, we’re seeing the latest Claude models improve along three dimensions: action planning, visual understanding, and judgment.


### Better action planning via batching


Models are getting better at deciding how much of the computer they need to see. Computer use is expensive when every action requires a round trip: click, screenshot, think, click, screenshot, think. As models become capable of planning several actions together, that loop gets shorter.


The single-action loop quickly became a practical bottleneck for our customers. Some of them use voice AI agents that are on the phone with a patient, waiting for an Asteroid agent to navigate the EHR and book an appointment. Making the patient wait several minutes for an agent to click, screenshot, and reason after every action simply wasn’t viable.


In an effort to overcome this, we introduced batching when we moved to the Claude 4.6 models in April: the agent executes short sequences of actions before returning control to the model. What emerged was surprisingly disciplined: the median run takes just two actions between screenshots, and across more than 1,100 evaluations it never took more than ten actions without looking.


The same four actions, before and after batching


Before


Clinical Desktop 2.4 - Patient Billing


_


□


✕


Patient ID


A-1042


Amount


120.00


01


screenshot


02


click · Patient ID


03


screenshot


04


type · A-1042


05


screenshot


06


click · Amount


07


screenshot


08


type · 120.00


✓ done in 9 seconds · 8 tool calls · 4 screenshots


After


Clinical Desktop 2.4 - Patient Billing


_


□


✕


Patient ID


A-1042


Amount


120.00


01


screenshot


1 tool call · 4 batched actions


02


click · Patient ID


03


type · A-1042


04


click · Amount


05


type · 120.00


✓ done in 4 seconds · 2 tool calls · 1 screenshot


This is the direction the model providers are now moving toward as well. Anthropic’s new 2026 computer use tool introduced multi-action turns, allowing Claude to return several computer actions in a single turn. That makes batching a first-class capability of the computer use interface. When we tested the newer interaction loop on four production-style workflows, it reduced model calls by 32–52%, cut cost per task by 25–32%, and lifted completion to 100% on every workflow, up from as low as 77%:


Workflow Model calls Wall clock Completion Cost per task


EHR report extraction −32% −20% 90→100% −31%


Claim intake −48% −59% 77→100% −32%


EHR task creation −46% −7% 100% −29%


Patient payment collection −52% −31% 100% −25%


This reduction in cost turns a production workflow from marginal to viable, particularly for per-patient and per-claim workflows, where there is little opportunity to amortize the cost of an execution across many records.


### Better visual understanding


Underlying these improvements in planning is better visual understanding. After all, the model can only act confidently for several steps at a time if it reliably understands the state of the interface in front of it. One of the most noticeable improvements is the models’ ability to distinguish between interface elements that look nearly identical but require different actions.


An example we’ve observed in production is the distinction between a text field with a default value and one with only a placeholder. In a screenshot, the two can look almost indistinguishable. But they require different behavior: a placeholder can simply be clicked and overwritten, while a default value must first be cleared before entering the new value.


It may seem like a small distinction, but these are exactly the kinds of ambiguities that used to cause computer use agents to get confused. To monitor and report this failure, we built a simple evaluation around this case and found that on Claude Sonnet 4.6 and Opus 4.6, the agents mangled the existing value in over 80% of runs, and adding explicit instructions about the distinction didn’t meaningfully improve the result. When Opus 4.8 was released we reran the same experiment and found that this failure mode virtually disappeared: the model mangled the field in just 0.7% of runs, without any change to the prompts.


Placeholder vs default value


Sonnet & Opus 4.6


Clinical Desktop 2.4 - Patient Chart


_


□


✕


Notes · placeholder


Add a note


Care plan sent


Follow-up date · default value


01/01/2026


03/14/2026


03/14/2026


01


screenshot


1 tool call · 4 batched actions


02


click · Notes


03


type · Care plan sent


04


click · Follow-up date


05


type · 03/14/2026


06


screenshot


1 tool call · 2 batched actions


07


clear · Follow-up date


08


type · 03/14/2026


✗ needs correction 80% of the time · done in 7 seconds


Opus 4.8 & 5


Clinical Desktop 2.4 - Patient Chart


_


□


✕


Notes · placeholder


Add a note


Care plan sent


Follow-up date · default value


01/01/2026


03/14/2026


01


screenshot


1 tool call · 5 batched actions


02


click · Notes


03


type · Care plan sent


04


click · Follow-up date


05


clear · Follow-up date


06


type · 03/14/2026


✓ right the first time · done in 5 seconds


This matters even more in production. Before these newer models, our customers had to monitor executions closely and occasionally steer the agent when it got stuck on an input. In April, we measured that 8.9% of one customer’s executions required a human nudge. After moving to the newer Claude models, that number fell to roughly 1% in July. In August, we’ve had zero executions requiring steering.


### Better judgment and resilience


The same progression shows up in how models decide what to do when the interface doesn’t behave as expected. In production, that often means distinguishing between a problem worth recovering from, one worth waiting out, and one where the right decision is to stop.


One of our customer workflows makes these decisions particularly visible. It operates an EHR that fails to launch roughly one time in five on our Windows sandbox. A failed launch can usually be recovered by clearing the cache and trying again; a blank EHR window needs a restart; a report that takes several minutes to export usually needs patience rather than intervention; and an IP allowlist or account-policy error cannot be fixed by retrying.


We used to encode these cases explicitly in the agent’s instructions. But every recovery procedure we spell out is another piece of workflow logic we have to test and maintain. As the models improve, more of that logic is moving into the model itself. In this workflow, instructions went from roughly 80 lines to 32, while retaining the same 94% success rate. That’s an important shift for production computer use, as it reduces the amount of bespoke logic required to make an agent reliable: every failure mode that a model can recognize and recover from on its own is one less condition we need to encode, test, maintain, and monitor ourselves.


However, this increased autonomy also raises a question we take seriously: when is the model safe to act without explicit instructions? Recovering from software failures is the easy case. It happens inside a disposable sandbox, so a wrong move costs us a Windows machine and nothing else. Judgment inside the EHR is different: actions there have consequences, and we need to carefully evaluate autonomy rather than assume it. The direction we’re exploring is to treat unusual behavior as a signal: when an agent’s actions fall outside the typical distribution of previous executions, we warn the user and pause the execution for review.


## Engineering the system around the model


While newer models keep removing failure modes, they don’t remove the need to engineer around the fundamental limitations of computer use in healthcare environments. Where those limits show up, we shape the system so the model doesn’t have to solve unnecessary problems.


### Deterministic extraction instead of OCR


In production, we’ve found that the most reliable workflows are often the ones that ask the model to do less. For instance, we don’t rely on the model’s visual understanding for information where an OCR error could have clinical consequences. Extracting an ICD-10 code and diagnosis from a patient’s chart by asking the model to read the pixels on screen is risky. A model can be extremely good at navigating an EHR and still misread a character in a clinical code. The cost of that mistake is much higher than the cost of a failed navigation click.


Where possible, we therefore design the workflow so the model doesn’t need to read the information from the screen at all: if an EHR can generate a patient report, we have the agent export that data as a file. A Python process can then extract the text deterministically and return structured data to the customer. When the application doesn’t provide a useful export, we look for deterministic mechanisms within the interface itself. For example, rather than asking the model to visually verify that an MRN on the screen matches the MRN in its task, we can have it use the application’s` Ctrl+F` search to locate the exact value.


Extracting a clinical code


Clinical Desktop 2.4 - Patient Chart


_


□


✕


File


Edit


View


Reports


Help


Open


Print


Export


MRN


00429183


Patient


WILLIAMS, DANA


DOB


07/22/1958 · F


Payer


MEDICARE PART B


Provider


OKAFOR, S · PULMONOLOGY


DOS


03/14/2026


Diagnosis


R06.02 - SHORTNESS OF BREATH


Allergies


PENICILLIN


Method A · OCR


read from pixels


→


R06. O


2


✗ one misread character · a silent clinical error


Method B · File export


export file


→


parse


→


R06.02


✓ byte for byte, every run


### Discovering interface shortcuts


Careful workflow design matters beyond data extraction. We also look for ways to avoid interactions that models can perform reliably, but only slowly and expensively. Many of the EHRs we operate have useful behaviors that aren’t obvious from the UI, but once discovered can eliminate long sequences of actions.


Take a large dropdown of options. An agent navigating it the obvious way has to alternate between scrolling and taking screenshots dozens of times, paying for every interaction in latency and inference cost. Each scroll turn costs 6 to 8 seconds of model round trip, and in one recorded run the agent scrolled twelve times in a row at the same coordinate hunting for a single username: over half of that step was spent scroll-hunting.


This is where application-specific workflow design becomes important. In one specialty EHR, we found that pressing Backspace with a dropdown open immediately returned the list to the top, and although the dropdown had no visible search box, typing the clinician’s name narrowed the options to the matching entry. The fix is two lines of workflow instructions, leading to significant cost and time savings:


Metric Scrolling Backspace and type Saved


Step duration 5.6 min 2.4 min −3.2 min


Scroll and drag turns ~26 1 ~25 turns


Model turns ~46 ~23 −50%


Model cost for the step $0.83 $0.34 −59%


End-to-end execution time 7.7 min 4.2 min −3.5 min


The improvement was even larger on the worst cases. Scrolling cost scales with the length of the list while typing does not: our worst pre-change run spent twelve minutes and 51 scroll turns inside this single step. Fewer turns also shrink the context, since every turn re-reads the accumulated screenshot history; cache-read tokens per step dropped from roughly 1.7M to 0.5M, and total model turns for the whole execution roughly halved on the same input. Before the change, agents performed 257 scroll actions across twenty-one executions of this step. Since the change, they have reliably performed two.


### Optimizing the environment


Making the workflow easier for the model is only part of the job. We also engineer the environment it operates in to remove as much setup and friction as possible. Every minute an agent spends establishing a VPN connection, launching an application, navigating to the right screen, or authenticating is a minute it isn’t doing useful healthcare admin work for our customers. We therefore snapshot environments as close as possible to the first meaningful action: applications are already open, VPN connections are configured, and the machine boots into a known state.


At production volume, another constraint appears: concurrency. Our customers may need to process thousands of records a day, but many EHRs don’t allow the same account to hold multiple active sessions. Two agents sharing a login can effectively kick each other out of the application, including midway through submitting a form. Where customers have access to multiple credentials we maintain pools of authenticated profiles that agents can draw from. Where they don’t, we queue executions behind the available session. Combined with bulk operations, this lets us increase throughput within the constraints of the underlying EHR.


The environment also typically spans more than one computer. We keep agents on Linux sandboxes because they’re faster and cheaper to run at fleet scale, while much of the healthcare software they operate only runs on Windows. In those cases, the agent works from Linux through a remote desktop into a Windows machine. A file exported from the EHR therefore has to cross three environments: from the remote Windows machine, back to the agent’s sandbox, and finally to the customer through our API. We configure the bridges between them, route traffic through approved IPs, and make sure data can cross those boundaries securely.


One export, three environments


Windows machine


Clinical Desktop - Reports


Export complete


report.csv


⊞ Start


14:32


↓


→


file bridge


Linux sandbox


agent@sandbox: ~


$


ls /bridge


report.csv


$


parse report.csv


ok · 1 record


↓


→


API


Customer


Structured output


Name


WILLIAMS, DANA


Address


412 CEDAR AVE


Phone


555-0142


ICD-10


R06.02


Much of the work of production computer use happens before the agent takes its first action. The model may operate the interface, but the environment determines what it has access to and how reliably it can operate it.


### Synthetic portals for safe testing


A final constraint is that there is often no safe place to practice. Our customers usually have no access to staging versions of the portals they need integrations with, so every execution touches a production system that holds real patient data. We, on the other hand, constantly need to test: new models as they are released, prompt changes, harness improvements, cost optimizations. Running those experiments on real patient data is too risky.


So we built synthetic portals: test environments that combine the real challenges and failure modes we’ve observed in production and require roughly the same trajectories from the model.


To keep these environments representative of production, we run our own reflection loop, inspired by[Claude dreaming](https://platform.claude.com/docs/en/managed-agents/dreams) , that continuously learns from past executions. Claude reflects on execution traces to surface recurring patterns, challenges, and places where agents struggled. Those findings drive the construction of new sandbox environments, where we can run replicas of our production agents. When a new model ships, we can evaluate it within hours, without any risk of polluting a clinical portal.


## The healthcare workflows now being unlocked


The result of all this is visible in what our customers are now running in production. As models become more capable and the systems around them faster and more reliable, workflows that didn’t clear the bar for computer use six months ago increasingly do. Our customers use computer use agents to update patient rosters, identify blocked claims waiting on signatures, monitor facility bed-days, track admissions and discharges, reconcile delivered versus billed therapy, create and triage administrative notes, collect patient payments, and process claims, all inside EHRs and other systems that previously required a person at the keyboard.


We’re seeing this happen across two broad kinds of work. The first is bulk workflows, where an agent enters an EHR once and processes large amounts of data. These workflows can have striking economics because the cost of navigating into the EHR is amortized across thousands of records. In February, one of our ten-minute executions extracted data for a single patient. Today, another workflow spends roughly eight minutes in the EHR and extracts clinical and demographic data for more than 85,000 patients.


The second is unit work, where each execution corresponds to a single patient, claim, or administrative task. Here, there is much less to amortize, so every improvement in model speed and inference cost matters. Smaller models are now taking on computer use workflows that previously required the most capable ones, while the interaction-loop improvements above have cut task costs by 25–32%.


This is one reason we’re excited about Sonnet taking over work that previously required Opus. That shift has already happened on our platform, and it plays out per workflow. Every new workflow is onboarded on Opus while its instructions are still taking shape. As the workflow matures, traffic progressively migrates to Sonnet, and Opus keeps one standing job: dreaming, reflecting on past executions and powering the evals.


A workflow's lifecycle


Phase 1 · Onboarding


Phase 2 · Maturing


Phase 3 · Production


Instructions


v1 · 80 lines · draft


v2 · 54 lines


v3 · 32 lines · stable


guide ↓


→


Executions


01


✓ exploration


opus


02


✓ exploration


opus


03


✓ exploration


opus


04


✓ completed


sonnet


05


✓ completed


opus


06


✗ failed


sonnet


07


✓ completed


sonnet


08


✓ completed


sonnet


09


✓ completed


sonnet


↓ run transcripts


Dreaming:


opus


monitors and maintains the workflow


↑ refined instructions


## Where we’re going next


In six months, computer use at Asteroid has gone from a production bet to thousands of agents running every week across healthcare software. In the next six, we expect both the number and the complexity of those workflows to keep growing.


We don’t think the GUI is the final interface for agents. Where software offers an API or a browser surface, we use it: it’s faster, cheaper, and more deterministic. Over time, more software will expose interfaces designed for agents directly. But healthcare won’t make that transition overnight. Its EHRs carry decades of accumulated forms, dialogs, and domain-specific conventions, many barely intuitive to the humans they were designed for. Until healthcare software becomes agent-native, computer use agents fill the gap, increasingly capable of navigating these systems as they are.


If your team spends its days inside an EHR with no API, there’s a good chance that work is already automatable.[Book a demo](https://asteroid.ai/demo) and see what computer use can do for your workflow.
