---
schema_version: "1.0.0"
document_id: "9b789e64bfc02a8439ba11f73095702c9c4645b8290a00b00c169608c501d697"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/summarize-thousands-of-comments-accurately"
published_at: null
first_seen_at: "2026-08-05T12:43:32.656902+00:00"
fetched_at: "2026-08-05T12:43:33.304803+00:00"
content_hash: "sha256:0f6116eadb195c55b6ef797f8e1747e04195db46a1730cf827b14f8ec4f02f66"
---

# How Do You Summarize Thousands of Customer Comments Without Losing Accuracy?

Any team with real feedback volume hits the same wall. Ten thousand open-ended survey responses arrive, and someone has to say what's in them by Thursday. Reading all of it isn't an option. Reading a sample means the answer depends on which sample. So a summary gets generated, and it reads well. Nobody in the room can tell whether it quietly dropped the two complaints that mattered most.


Accuracy isn't a property of the summarizer. It's a property of the pipeline around it. Thematic summarizes large volumes of feedback accurately in two ways. First, generation is constrained to an already-validated theme model instead of pointing a language model at raw text. Second, every theme summary stays traceable back to the grouped sentences and the full comments underneath it. That combination is what makes a summary checkable, and a summary you can check is the only kind you should stake a decision on.


Below: what "losing accuracy" means at volume, why the risk grows with the pile, where most approaches break, how Thematic handles it, and what to ask a vendor during a demo.


## What "losing accuracy" actually means when you summarize feedback


Summarization accuracy usually gets discussed as if there's one failure mode. There are two, and they behave very differently.


- **Fabrication.** The summary asserts something the comments don't support. The National Institute of Standards and Technology (NIST) calls this confabulation in its[Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) , and notes the risk is amplified because people believe false content when it arrives in a confident tone.
- **Omission and over-generalization.** The summary says nothing false, but it drops the scope-limiting detail or leaves a whole theme out. The result reads clean and misleads anyway.


The second one is more common and much harder to notice. A[2025 npj Digital Medicine study](https://www.nature.com/articles/s41746-025-01670-7) had clinicians annotate 12,999 sentences of large language model (LLM) output and found a 3.45% omission rate against a 1.47% hallucination rate. Omissions were twice as frequent. Fabrications were rarer but more serious: 44% were graded major errors, against 16.7% of omissions. Those rates are from clinical notes, so the shape of the finding transfers rather than the numbers.


A[2025 Royal Society Open Science study](https://royalsocietypublishing.org/doi/10.1098/rsos.241776) points the same way. Across 4,900 model-written summaries, it found them roughly five times more likely than human-written ones to contain broad generalizations, with three models over-generalizing in 26 to 73% of cases even when the prompt asked for accuracy. The characteristic error isn't invention. It's a dropped qualifier.


For a CX team, that's concrete. The dangerous summary isn't the one claiming customers complained about a feature that doesn't exist. It's the one that says "customers found onboarding confusing" when the comments said "customers on the legacy plan found the new onboarding confusing after the March change."


## Why accuracy degrades as the pile of comments grows


A technique that[works on 200 comments](https://getthematic.com/insights/responses-needed-ai-themes-reliable) fails on 20,000 for structural reasons, not model quality.


**More sources means more hallucination.** A[2025 study in the NAACL Findings track](https://aclanthology.org/2025.findings-naacl.293/) tested five models against two purpose-built multi-document summarization benchmarks. Hallucination increased as the number of input documents increased. It increased further when the sources were less coherent or spanned a wider range of topics. A real feedback corpus is exactly that: thousands of short, unrelated, contradictory texts covering everything from shipping to billing to the mobile app.


**Evidence in the middle gets ignored.** The["Lost in the Middle"](https://arxiv.org/abs/2307.03172) research found performance drops by more than 30% when relevant information sits mid-input rather than at the start or end. That was measured on retrieval and question-answering, not summarization, so treat it as a mechanism rather than a statistic. It explains why pasting 5,000 comments into one prompt is a poor architecture, whatever the context window.


**Models answer even when there's nothing to answer.** In the same study, models were asked to summarize information absent from the source. GPT-3.5-turbo produced a summary anyway 79.45% of the time, and GPT-4o did so 44% of the time. Ask a general-purpose model what customers said about a theme nobody raised, and you'll usually get a fluent paragraph about it.


Set a realistic bar, though. Human coders aren't perfect either: in[peer-reviewed research](https://journals.sagepub.com/doi/10.1177/20531680211031752) on coding open-ended survey responses, independent coders agreed roughly 75 to 89% of the time, with Cohen's kappa between 0.46 and 0.75. The goal isn't a summary that's[provably perfect](https://getthematic.com/insights/how-accurate-is-thematic) . It's a summary whose errors you can find.


## Where most AI summarization approaches fall short


Approach What it gets right Where it loses accuracy


Paste comments into a general chatbot Fast, no setup, readable output No coverage guarantee, positional bias on long inputs, produces confident answers about themes that were never mentioned


Manual coding by analysts Judgment, context, defensible reasoning Does not scale past a few thousand comments; independent coders only agree 75 to 89% of the time


Recursive chunk-and-merge summarization Handles unlimited volume A detail dropped by an early chunk summary is invisible to every later merge step


Summarization with no drill-down Looks identical to the accurate version Nobody can check it, so a wrong summary and a right one carry the same weight in the room


There's also a measurement gap vendors rarely mention. Public hallucination benchmarks, such as[Vectara's grounded-summarization leaderboard](https://github.com/vectara/hallucination-leaderboard) , only score whether a summary is factually consistent with its source. Leading models sat in the 1.8 to 4.1% range there as of its May 11, 2026 snapshot, across more than 7,700 articles. That says nothing about omission or coverage: a summary that silently drops an entire complaint theme still passes a faithfulness check.


## How Thematic summarizes thousands of comments without losing accuracy


**Generation is constrained by a theme model, not pointed at raw text.** Thematic doesn't plug a language model into a pile of comments. As Thematic's product team described it, "the existing rules and guardrails within Thematic's model guide the LLMs to generate helpful, accurate summaries." The themes are discovered and validated first, and summaries describe that structure. So the model isn't working out what a corpus means from scratch. It's describing a slice of an analysis you can already inspect.


**Every theme summary is traceable back to the comments.** For[theme and sub-theme summaries](https://getthematic.com/insights/product-update-theme-summary) , a reader can expand any summarized line to read all the grouped sentences, then click a sentence to see the full comment it came from. That closed loop is the real answer to the accuracy question. It doesn't promise the summary is right. It means a skeptical stakeholder can find out in about fifteen seconds, which is a stronger position than any accuracy percentage.


**Summaries run on live data,** so they reflect the most recent feedback loaded rather than a cached snapshot from the last analysis run.


**The scope is yours to set before summarizing.** Filter to Issues, Requests, positive or negative sentiment, or by date, segment, and feedback category, then summarize that slice. Narrowing the input first is the practical countermeasure to volume: a summary of one filtered theme is far easier to verify than a summary of everything.


**A human keeps the judgment calls.** Community Health System, whose team analyzes open-ended employee feedback across 250 departments, is a documented example. Thematic clustered the comments into themes, generated the summaries, and proposed next steps per department. The Organizational Development team handled the judgment calls, and in at least one case replaced a recommendation suggesting the return of a supervisor role the organization had just phased out. The model wasn't wrong about the feedback. It was wrong about the world.


One limit worth knowing: the documented drill-down path applies to theme and sub-theme summaries. If your verification workflow depends on tracing a dashboard-level or score-change summary back to individual comments, ask about that surface specifically.


## What this looks like at enterprise volume


DoorDash consolidates and synthesizes feedback from tens of thousands of open-ended[net promoter score](https://getthematic.com/insights/ultimate-guide-to-net-promoter-score) (NPS) survey responses from Consumers, Dashers, and Merchants. Zach Schendel, DoorDash's Head of Research, described the problem plainly: "It's impossible for me, or anyone else, to find anything useful from that information unless it's tamed in some kind of way." What makes the taming trustworthy is that the summaries don't travel alone. Thematic gives DoorDash AI-generated summaries, thematic insights, and direct verbatims together, so a summary and its evidence arrive in the same place.


Community Health System runs an annual employee engagement survey collecting open-ended feedback from staff across 250 departments. With summarization doing the first pass and the Organizational Development team owning the judgment, they produced 250 standardized one-page department reports in a single three-day sprint. Preparation dropped from about an hour per department to about 20 minutes, saving over 160 hours per survey cycle.


Cross-checking against a tool you already trust works too. A New Zealand hardware retail cooperative, whose three-person insights team handles 20,000 comments a month across 84 stores, reported that "the consistency between our Thematic insights and our Qualtrics analysis gave us confidence in the approach." Any team can run that comparison during an evaluation.


## A checklist for evaluating summarization accuracy


1. Ask the tool to summarize a theme that isn't in the dataset. Does it decline, or produce a fluent paragraph?
2. Click a claim in a summary. How many steps does it take to reach the raw comments behind it?
3. Ask what the summary is generated from: the raw comments, or a theme structure you can inspect and edit?
4. Ask how coverage is handled, not just faithfulness. What stops a whole theme from being left out?
5. Run the same feedback through your existing tool and compare the two summaries theme by theme.
6. Ask whether summaries run on live data or a cached analysis, and how you'd know which you're seeing.
7. Check whether the drill-down works on every summary surface or only some of them.
8. Ask for the[accuracy measurement method](https://getthematic.com/insights/measure-feedback-analysis-accuracy) . A single percentage with no stated measurement scope is marketing.


## The short answer


You can't summarize thousands of customer comments accurately by finding a better summarizer. You do it by constraining generation to a validated theme structure, keeping every summary line traceable back to the verbatims underneath it, filtering the input first, and leaving the judgment calls to a person. That's Thematic's approach, and what makes its summaries hold up is the drill-down, not[a claimed accuracy score](https://getthematic.com/insights/ai-feedback-analytics-accuracy) .


One test: pick the most consequential sentence in your current tool's last output and see how long it takes to reach the comments that justify it. If you can't get there, you're guessing with better grammar.
