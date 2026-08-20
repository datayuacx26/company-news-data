---
schema_version: "1.0.0"
document_id: "a0ababf7fea62bdd390db9070f16d8eb212cafcdc6c78a30c33b61d996dd9d65"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/how-we-deploy-ai-and-why-we-do-it-carefully"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:61651e16c683f7126500336d51d099474de44805f26898645153863c2edcf07c"
---

# How We Deploy AI, and Why We Do It Carefully

The case for AI in correctional settings is easy to make. Probation and parole officers and case managers in facilities carry caseloads of 80 to 100 people or more. Just meeting the minimum requirements of their role takes so much time in meetings and paperwork that there’s little room for individualized attention. AI can be a huge help by transcribing interactions, turning freeform input into organized notes, and drafting detailed plans.


Many officers already use general-purpose AI tools this way. That’s validating, but also worrisome. Because AI can get things wrong, and those errors often go undetected.


A case note that mischaracterizes something someone said, or invents or ignores crucial details, doesn't just produce an inaccurate document. It can influence whether someone qualifies for programming, complicate housing, or — most crucially — impact a person’s liberty.


So when we build with AI, we don’t ride the wave of hype and scale quickly. We choose to move carefully to get it right. Even when partners are eager for more. Even when we are too.


Here's what "getting it right" actually looks like as we develop AI for modules like[Case Planning](https://www.recidiviz.org/case-planning-assistant) , Meetings, and Case Notes Insights.


## **First, Do No Harm**


Recidiviz is a nonprofit. We're not here to grow for growth’s sake, or launch new features because they’re trendy in the private sector. We build software because we believe technology, applied thoughtfully, can lead to better outcomes for people in the corrections system and the communities they're part of — and because we know that technology, deployed carelessly, risks making things worse.


We take a Hippocratic approach to our work, including AI: **first, do no harm.** Overall improvement matters, but it doesn't justify worse outcomes for any individual. If a person is kept in or moved to the wrong supervision or custody level because our system misrepresents something they said, that is a failure, regardless of how well the tool is working for everyone else. We build with that standard in mind, because we know how high the stakes are.


This article, the first in a series on our approach to AI safety, focuses on how and why we take a methodical approach to testing. Later posts will cover the technical guardrails we build into our AI systems, and why reviewing AI output should be treated as a learned management skill.


## **A Structured Rollout — and Why It Takes the Time It Takes**


We’re deliberate about following the same phased pilot process for every deployment. We start with intensive internal evaluation of every single output from AI. Then, at every stage, we keep humans closely involved as we train automated systems to take on more — but never all — of the review. We call this approach **AI safety gating** .


Two kinds of people are central to this process.


- **Trusted testers** are the officers and case managers who use the tool in the field during early phases. Their structured feedback on what's working and what isn't is most intensive in the initial phases.
- **Human reviewers** are trained evaluators from Recidiviz who read outputs and assess how they compare to defined criteria. They catch subtle distortions, missing context, and outputs that are technically accurate but misleading in practice.


The limitation to human review is that it doesn't scale; we can't have someone on our team read every case note forever. That's where **automated evaluation** comes in. A separate AI system, known as LLM-as-a-Judge (LLMAJ), does the same sort of review as humans, but faster and at higher volume. LLMAJ only knows what good looks like because human reviewers taught it: the intensive human review generates the evidence base that trains and validates our automated systems.


*LLMAJ evaluating the impact of a change that impacts AI-generated output by running the same test on the old and new versions*


Here’s how the review phases are structured for our Meetings module, which transcribes and structures important information from face-to-face meetings:


Phase Trusted Testers Human Review Coverage Volume Cap


1 — Supervised 1–2, with Recidiviz present; structured feedback 100% within 2 business days 20 meetings/week


2 — Limited Unsupervised Up to 5, independent use; structured feedback continues 100% + automated eval running in parallel 50 meetings/week


3 — Capped Unsupervised Up to 10, full caseload; feedback becomes more ad hoc 10% human review, including all flagged outputs 200 meetings/week


4 — General Availability Up to 20; feedback ad hoc Statistically meaningful human sample + 100% automated eval None


Moving between phases requires hitting defined quality gates. Advancing isn't a judgment call; it's a standard we either meet or we don't. The gates also work in reverse: a sustained spike in errors triggers a return to the previous phase.


## **Not All Errors Are Equal**


Central to how we evaluate outputs is classifying errors by their impact. Many AI systems make impressive-sounding numerical claims like “98% error free,” but those sorts of stats can only come from test data, and don’t give any useful information about real-world impact.


So we measure — and minimize — the errors that matter in a corrections context: the specific facts that shape real decisions in people's lives. We carefully define *what* counts as an error, *how severe* each kind of error is, and *how low* a rate of these errors any AI-generated output must achieve and maintain.


An output flagged **BAD** is one that could lead to the wrong administrative response — a case note asserting a client uses drugs when they said the opposite, or misattributing a statement in a way that could trigger a violation. These are rare, but when they occur, we dig in to understand the issue. We investigate the root cause and test our fixes thoroughly.


An output flagged **PARTIAL** has one or more significant inaccuracies that cause hassles or reduce trust in the system, but don’t risk adverse outcomes. Examples include action items assigned to the wrong person, errant timestamps on a transcript, or irrelevant chatter crowding out important facts in summaries. We continually improve to minimize these issues and hold them to a low percentage.


An output flagged **GOOD** is broadly accurate, with only the minor distortions you'd expect from even a human notetaker.


Before we move to the next stage, BAD and PARTIAL rates must both meet a threshold that gets stricter as we go. We also have clear criteria for rolling back at any stage — even after full release.


## **Evaluation Matters, Even After Launch**


Meeting the quality bar at launch is necessary, but not enough. Because the underlying AI models can change unpredictably, we cannot assume that systems built on top of them will have the same very low error rates next month as they did last month. That’s why responsible AI also requires continuous evaluation *after* launch, both of the core technology and of the automated evaluations.


That’s why human evaluation never stops. When the system is fully released and used across several states, human reviewers at Recidiviz still assess every single BAD and PARTIAL flag, but also a sampling of GOOD outputs. Even when automated systems are catching what they're trained to look for, humans are watching for what they aren't, and then training them to catch that too.


The Romans had a phrase that fits our approach: *festina lente* , “make haste slowly.” It’s precisely because we believe in AI’s power to help corrections officials and the people they work with that we’re taking great care to build a strong foundation. That way, we and our partner states can move faster in the months and years to come, and get it right.
