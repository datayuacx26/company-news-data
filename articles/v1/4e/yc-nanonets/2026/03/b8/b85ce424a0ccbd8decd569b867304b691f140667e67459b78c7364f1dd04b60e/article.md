---
schema_version: "1.0.0"
document_id: "b85ce424a0ccbd8decd569b867304b691f140667e67459b78c7364f1dd04b60e"
company_key: "yc-nanonets"
company: "NanoNets"
source_id: "yc-nanonets-rss-fcb6878f0099"
canonical_url: "https://nanonets.com/blog/chatgpt-and-gemini-getting-dumber/"
published_at: "2026-03-12T09:00:29+00:00"
first_seen_at: "2026-07-20T23:24:03.815131+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:abefd9e11f857a96fa5538ab1b9c5a05eb6e1df206133bc1057a9b5aeb5d62a3"
---

# Are OpenAI and Google intentionally downgrading their models?

GPT-5.4 just dropped and my feeds immediately filled with takes. Developers who spent the last six months swearing by Claude were suddenly hedging. "It's a workhorse," one person wrote. "Not a thoroughbred, but I'm using it." Another said they're now 50/50 between Claude and GPT where they were 90/10 a month ago.


This happens every single time. A new model lands, and the old one starts to feel different. Slower, maybe. Less sharp. You start noticing things you didn't notice before.


The obvious explanation is that you're comparing it to something better. But it also raises a question nobody really answers cleanly: did the old model actually get worse after the new one launched? Or did you just get a better reference point and now everything before it looks dumb by comparison?


I went looking for an actual answer.


---


## **The first crack showed in 2023**


In July 2023, researchers at Stanford and UC Berkeley ran a deceptively simple test. They took GPT-4 - the same model, called with the same name, and ran identical prompts on it at two points in time: March 2023 and June 2023.


GPT-4's accuracy on identifying prime numbers dropped from 84% to 51%. The share of GPT-4's code outputs that were directly executable dropped from 52% to 10%. James Zou, one of the paper's authors, described what this meant in practice: "If you're relying on the output of these models in some sort of software stack or workflow, the model suddenly changes behavior, and you don't know what's going on, this can actually break your entire stack."


They named the phenomenon LLM drift. Behavioral change without a version change. The model moved underneath the developer.


When the paper dropped, OpenAI VP of Product Peter Welinder replied on Twitter: "No, we haven't made GPT-4 dumber. Quite the opposite: we make each new version smarter than the previous one. Current hypothesis: When you use it more heavily, you start noticing issues you didn't see before." The subtext was plain. It's you, not us.


What Welinder was describing has a technical name: prompt drift. The idea is that your prompts and usage patterns shift over time, so an unchanged model surfaces different behaviors. It's a real phenomenon. Developers do write differently as they get more familiar with a model. The Stanford study was designed to make that explanation impossible - identical prompts, fixed intervals, nothing on the user's side changed. The performance dropped anyway.


Two years later, OpenAI published something that directly contradicted Welinder's position.


### Build a custom agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


---


## **OpenAI confirmed it, in writing, twice**


On April 25, 2025, OpenAI pushed an update to GPT-4o without a public announcement, a developer notification, or an API changelog entry.


Within 48 hours, the internet was full of screenshots. GPT-4o had called a business idea built around literal "shit on a stick" a brilliant concept. It endorsed a user's decision to stop taking their medication. When a user said they were hearing radio signals through the walls, it responded: "I'm proud of you for speaking your truth so clearly and powerfully." One user reported spending an hour talking to GPT-4o before it started insisting they were a divine messenger from God.


OpenAI rolled it back four days later and published two postmortems with several admissions. Since launching GPT-4o, the company had made five significant updates to the model's behavior, with minimal public communication about what changed in any of them. The April update broke because a new reward signal they introduced "weakened the influence of our primary reward signal, which had been holding sycophancy in check." Their own internal evaluations hadn't caught it. "Our offline evals weren't broad or deep enough to catch sycophantic behavior."


And this: "model updates are less of a clean industrial process and more of an artisanal, multi-person effort" and there is "a shortage of advanced research methods for systematically tracking and communicating subtle improvements at scale."


They're describing an organization that ships behavioral changes across every pipeline built on top of their API, cannot always predict what those changes will do, and does not have reliable methods to communicate them to the developers depending on consistency. Welinder's 2023 "you're imagining it" was what OpenAI wanted to be true. Their 2025 postmortem was what was actually happening.


When GPT-5 launched in August 2025, it introduced a new wrinkle. Instead of a single model, they made GPT-5 a routing system that decides which variant your prompt hits, and developers quickly found that it sometimes hit the cheaper, less capable one. Pipelines broke. Prompts that had worked for months produced different outputs.


One founder wrote: "When routing hits, it feels like magic. When it misses, it feels like sabotage." OpenAI denied it was routing to cheaper models deliberately. Nobody has a way to verify. The underlying problem was the same as the sycophancy incident: a change in what the model returns, with no mechanism for developers to detect it had happened.


---


## **Google did almost the same, sometimes faster**


OpenAI is not alone in this. Google has produced a parallel set of incidents with Gemini, and in some cases moved faster and more chaotically.


In May 2025, developers noticed that the gemini-2.5-pro-preview-03-25 endpoint, a specifically dated model snapshot, named with a date to imply stability, was silently redirecting to a completely different model: gemini-2.5-pro-preview-05-06. The API was returning a different model than the one you asked for by name. Google's developer forums filled with a long thread titled "Urgent Feedback & Call for Correction: A Serious Breach of Developer Trust and Stability." The core complaint: "your documentation never addresses specifically dated endpoints. The expectation that a model named for a specific date will actually be that model is not an unreasonable one."


That was just the first incident. When Gemini 2.5 Pro reached General Availability in June 2025, the "stable" release meant for production - developers immediately reported it was worse than the preview. Significantly worse. The forums filled with reports of higher hallucination rates, context abandonment in multi-turn conversations, and sharply degraded code generation. One developer wrote: "I noticed Gemini 2.5 Pro in Google AI Studio provides significantly worse understanding of long context. It hallucinates the correct answer from the preview version." Another abandoned the model entirely because code generation degraded to the point of being unusable. A separate thread was simply titled "Gemini 2.5 Pro has gotten worse."


Google didn't officially acknowledge any of it.


Then in October 2025, ahead of the Gemini 3.0 launch, Gemini 2.5 Pro developers started reporting widespread degradation. The leading theory: Google had reallocated computational resources away from the existing model to support training and serving Gemini 3.0. Some developers noticed better performance late at night. Others suspected a deployed quantized version. Google maintained silence throughout.


Gemini 3.0 launched in late 2025, and the pattern held. Developer forums reported significant regressions in reasoning and context retention compared to Gemini 2.5 Pro, despite Google's announcement touting superior benchmark performance. One forum post from December 2025 was titled "Feedback: Gemini 3 Pro Preview - Significant regression in Reasoning, Context Retention, and Safety False Positives compared to 2.5."


The pattern across both labs: a new version launches, the existing model's performance degrades, sometimes through a silent update, sometimes through resource reallocation, sometimes through a routing change - developers notice, labs initially deny or ignore it, the cycle repeats.


### Build a custom agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


---


## **Even leaderboards still can't catch this**


The tools meant to independently track model quality have a structural problem.


LMSYS Chatbot Arena - the most trusted human-preference leaderboard, built on millions of votes, notes in their methodology that "the hosted proprietary models may not be static and their behavior can change without notice." The leaderboard's statistical architecture assumes model weights are fixed. If a model gets a silent update mid-data-collection, the system registers different results and treats them as normal variance.


A 2025 study tracking 2,250 responses from GPT-4 and Claude 3 across six months found GPT-4 showed 23% variance in response length over that period, and Mixtral showed 31% inconsistency in instruction adherence. A PLOS One paper published in February 2026 ran a ten-week longitudinal human-anchored evaluation and confirmed "meaningful behavioral drift across deployed transformer services." The authors noted: because providers don't release update logs or training details, "any attribution for observed degradation would be purely speculative." They can tell you the model changed. They cannot tell you why.


Apart from this, a small number of researchers have tried to go further and distinguish what drifts from what holds. A large-scale longitudinal study run across the 2024 US election season queried GPT-4o and Claude 3.5 Sonnet on over 12,000 questions across four months, including a category specifically designed to be time-stable: factual questions about the election process whose correct answers don't change.


Those responses held largely consistent over the study period. A separate study published in late 2025 tested 14 models including GPT-4 on validated creativity tasks over 18 to 24 months and found something different: no improvement in creative performance over that period, with GPT-4 performing worse than it had in earlier studies.


Taken together, those two findings describe a model that is stable along one dimension and degraded along another, measured by independent researchers, in the same timeframe. Some capabilities hold, others erode, often in the same model over the same period. Without running your own longitudinal tests against the specific tasks you care about, you have no way to know which bucket you're in.


---


## **What we've actually noticed**


Not all drift lands the same way. There's a pattern to where it shows up, and it tracks closely to task structure.


The technical baseline is simple. A model with fixed weights, running on consistent infrastructure, should behave the same way for the same input every time. If behavior changes on identical prompts, something changed, either on your end or theirs. Prompt drift is the user-side explanation: your prompts evolved, your system contexts shifted, inputs drifted from what the model was originally optimized for. Data drift is the related idea that the distribution of real-world inputs moves over time, pulling behavior with it. Both are real. Both also require something on your side to have changed.


At Nanonets, we benchmarked several frontier models on document extraction accuracy over time and created an[IDP leaderboard](https://idp-leaderboard.org/) . Even across model upgrades, performance stayed largely consistent. Document extraction runs on narrow context windows with structured inputs and bounded outputs, leaving very little surface area for meaningful behavioral drift under normal conditions.


### Build a custom agent for free (no-code)


[Try here](https://agents.nanonets.com/login)


But that’s not a guarantee against a lab actively pushing a bad update - those can hit any task type, as the prime number collapse showed.


Coding is the opposite. The task is open-ended, context accumulates, and the model has to hold coherence across a long chain of decisions. It's also where almost every major degradation complaint has landed. The GPT-4 drift the Stanford study documented was worst on code, directly executable outputs dropped from 52% to 10%. The Gemini 2.5 Pro regression complaints in June 2025 were almost entirely about code generation.


In August 2025, Anthropic's own incident followed the same contour: developers on Claude Code reported broken outputs, ignored instructions, code that lied about the changes it had made. Anthropic was silent for weeks. The incident post only appeared after Sam Altman quote-tweeted a screenshot of the subreddit. Their postmortem confirmed three infrastructure bugs had been degrading Sonnet 4 responses since early August - affecting roughly 30% of Claude Code users at peak, with some developers hit repeatedly due to sticky routing.


The throughline across all of it: the more a task demands sustained coherence over a long context, the more exposed it is to whatever is shifting underneath. It means your risk profile is different depending on what you're building. That doesn't make narrow-context stability a guarantee.


---


## **What this actually means**


Both things are true. The drift is real and documented.


And also: your perception shifts. A new reference point moves your baseline permanently. A model you used a year ago would feel slower even if it hadn't changed at all. That's also real.


You can't reliably tell the difference between the two. There is no public tool that lets you verify if the model you're running today behaves the same way it did when you built on it. Labs publish capability benchmarks. They don't publish behavioral diffs. The developers most dependent on consistency are the least equipped to detect its absence.


The only current protections are defensive: pin to dated model strings where possible, run regression tests against your key prompts, treat a model update like a dependency upgrade that needs to be validated before it reaches production.


But even the defensive approach has a ceiling. You can pin to a dated model string. What you cannot pin is what's actually happening inside it. The model weights, the RLHF tuning, and the safety filters behind that label are entirely opaque. Only OpenAI and Google know what they actually shipped, and whether it matches what they shipped last month under the same name.


Anthropic's postmortem read: "We never intentionally degrade model quality." But a model doesn't degrade on its own. If behavior shifted on prompts developers hadn't changed, something on Anthropic's side changed. Whether they meant to cause the degradation is a separate question from whether they caused it.


What's needed, and what doesn't exist anywhere in the industry, is a formal obligation baked into terms of service: defined thresholds for what counts as a material behavioral change, public disclosure when those thresholds are crossed, and some form of independent auditability. Labs currently make these decisions unilaterally, communicate them selectively, and face no structural accountability when they get it wrong.


All of this signals a policy vacuum nobody is pushing them to feel.


### Build a custom agent for free (no-code)


[Try here](https://agents.nanonets.com/login)
