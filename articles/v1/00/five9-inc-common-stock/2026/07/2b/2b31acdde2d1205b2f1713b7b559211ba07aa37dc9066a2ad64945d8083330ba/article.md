---
schema_version: "1.0.0"
document_id: "2b31acdde2d1205b2f1713b7b559211ba07aa37dc9066a2ad64945d8083330ba"
company_key: "five9-inc-common-stock"
company: "Five9 Inc."
source_id: "five9-inc-common-stock-news-import-8c57dc7f6f62"
canonical_url: "https://www.five9.com/blog/humantic-data-is-the-missing-data-layer-for-agentic-ai"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-21T20:22:04.699230+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:d8a2233dd75944b33c3677d0a7cd818479dc12244b60b05e30a06facb2ce0354"
---

# The Missing Data Layer for Agentic AI — and Why It Comes from Humans

Image


# The Missing Data Layer for Agentic AI — and Why It Comes from Humans


- [AI Agent](https://www.five9.com/blog/category/ai-agent)
- [AI Readiness](https://www.five9.com/blog/category/ai-readiness)
- [AI & Automation](https://www.five9.com/blog/category/ai-automation)


Image


[Jonathan Rosenberg](https://www.five9.com/blog/authors/jonathan-rosenberg-0)


Jonathan has dedicated his career to transforming the telecommunications industry, and before Five9, he was CTO for the Collaboration Technology Group (CTG) at Cisco. Jonathan is also well known for his authorship of the SIP protocol, which is the foundation for modern IP-based telecommunications. Prior to Cisco, Rosenberg was the Chief Technology Strategist at Skype, where he guided the company’s technology strategy.


July 15, 2026


Every executive on earth is asking the same question right now: how do we deploy[AI agents](https://www.five9.com/faq/what-are-ai-agents) ? Very few are asking the question that actually determines the answer: where will the training data come from?


The honest answer is sitting in plain sight inside every contact center on the planet — and it is going to surprise the companies that don’t have one.


## Every wave of AI has needed its own kind of data


To train an AI model, you need data – a LOT of data. Modern Large Language Models like GPT5.5 and Gemini require nearly the entirety of the Internet to train them. Over the years, the technology has evolved. Reinforcement learning from human feedback, or RLHF, was added to provide grounding and dramatically improve the capability of these models on tasks. Most recently,[AI agents have emerged,](https://www.five9.com/news/news-releases/five9-launches-agentic-cx-ai-agents-can-reason-decide-and-take-action) taking AI beyond just conversation – writing code, onboarding new employees, handling customer support, and more. AI Agents also need to be trained on data too.


When you look at the resulting technology stack,[modern AI Agents](https://www.five9.com/products/capabilities/ai-agents) have three distinct layers of training, producing three tiers of intelligence, each requiring three distinct types of data:


At the bottom layer,[LLMs are “pre-trained”](https://www.five9.com/blog/four-safe-applications-generative-ai-contact-center) on massive data sets (pretty much the whole of the Internet), producing a raw model that’s intelligent but says the wrong thing and makes lots of mistakes. A second layer of training fixes that: labeled examples (of what good and bad outputs look like and human oversight to fine tune the model into what I’d call a “guided intelligence”. It is now usable. It knows the world. But it is not trained to do a specific task.


This is where AI agents come in. To do a specific task well, an agent needs its own kind of data. There is growing recognition that the hard part of making AI agents work isn’t the model — it’s the system feeding it what it needs to know. The discipline is so new it doesn’t have a settled name yet; you’ll hear context engineering and harness engineering and skills bandied about. Underneath all of them is one core question: what kind of data powers this phase of engineering?


The answer is – humantic data: a term that names something the industry has been doing without realizing it, and without a name for it.


Humantic data is the annotated record of how skilled human and perform work that teaches AI what good looks like by generating and maintaining the skills, hooks and documents that form the AI Agent harness. . Across fluid handoffs between AI and human agents. It is a form of labeled "training data", but the label matters less than the source. It describes the full, structured record of how skilled humans perform work – captured, annotated, and continuously refreshed – that becomes the standard for agentic processes. What separates it from generic interaction logs is the combination of raw signals with the quality metadata needed to separate good outcomes from bad ones. Without that separation, you don’t have humantic data. You just have noise.


## What is humantic data


The word is a portmanteau of “human” and “agentic” – a reminder while everyone is racing to build agentic AI, the missing ingredient is fundamentally human. The third layer of training data isn’t a bigger model or a smarter algorithm. It is a faithful record of how skilled humans do the work today – properly and improperly


The nature of humantic data depends a lot on the specific task at hand. For a customer support agent in a healthcare company, humantic data encompasses two layers.


#### Raw humantic data:


1.


Recordings of interactions (calls, chats, and emails) of human agents servicing customers for the desired use cases – prescription refills, appointment scheduling and cancellation, physician discovery, and so on.


1.


Transcripts of those recordings


1.


Screen recordings and click-captures of the web and desktop applications human agents use, including the steps they take in those applications and the information they gather


1.


[Quality Management (QM)](https://www.five9.com/faq/what-is-quality-management-qm) scores provided by human evaluators which[judge the success or failure of each interaction](https://www.five9.com/blog/agentic-future-when-quality-management-becomes-your-strategic-growth-engine) – this is one of the data points that defines what are good examples for the AI, and which are bad


1.


Customer survey results which provide another signal on how well the interaction went – also critical to distilling good and bad


1.


Disposition codes – another form of human annotation which captures results of the interaction, good for separating good from bad, and categorizing the interactions for later learning


#### Derived humantic data:


From that raw data, a much larger set of derived signals is distilled and aggregated:


1.


Call topics and intents


1.


[Customer sentiment, satisfaction, and emotion](https://www.five9.com/blog/you-cant-script-way-people-actually-talk) — which provide further signal on good vs. bad outcomes and all of the grey in between


1.


Call success paths and failure paths


1.


Knowledge: questions customers actually ask in calls, and answers human agents actually give


1.


Processes: steps taken by the human agents, including what they spoke, what they did, and how they reacted to the data they gathered while performing the task


## Three properties that make humantic data different


### 1: Humantic data captures all the variations


Put together, the raw and derived humantic data contain the information about exactly how to serve a customer across a wide variety of use cases, conversation flows, and customer situations. Volume matters. Human agents will tell you that they’ve “heard it all” – and there is a surprising number of corner cases and complexities that come up even in the most mundane of tasks.


Refilling a prescription – sounds easy, right? An AI agent just needs a tool to authenticate the customer and then a few tools for prescription lookup and refill, right? But what if the patient is calling for their child or spouse? Or refilling at a different pharmacy? Or refilling at the same pharmacy but asking when it will be ready? Or have a question about allergies, dosage or side effects? These — and countless other variations — are what real calls are about. If your AI agent only knows how to do the straightforward refill, it’s handling the cases that customers already complete on the website. When users contact a brand – it is often because there is a variation or complexity that needs handling. The hard ones, the ones that get a human on the phone, are exactly the ones it will fail. Capturing all of these variations is hard – and that is what humantic data provides. Only with all of this data in hand can a truly complete AI Agent skill for refills be developed.


### 2: Humantic data separates good results from bad


Humantic data isn’t just call recordings. It is the combination of those calls with meta-data that distinguishes a good result from bad one. This meta-data comes from human labels —survey results, QM scores, disposition codes, case notes, automation results — and from[derived signals like sentiment](https://www.five9.com/blog/beyond-human-limitations-why-traditional-quality-management-breaking-down-modern-contact) and satisfaction inferred from transcripts.


This separation allows us to better instruct AI agents on cases to emulate and what to avoid. Without it, designers are guessing blindly — pointing the agent at a pool of recordings with no way to flag which ones to copy. Imagine two human agents handling the same prescription refill: one resolves it in three minutes with a 5-star CSAT; the other turns it into a 12-minute escalation with a furious customer and a 1-star score. To an AI agent learning from raw recordings alone, both calls look like "how the work gets done." The QM scores, survey results and disposition codes are what tell it the difference.


This quality annotation layer is the hard part – and it is where most generic AI implementations fall short. Any contact center can accumulate call recordings. Not every platform has built the infrastructure to continuously label, score, and filter those recordings into a usable signal. The records along are just the raw material. The annotation pipeline is the refinery. Both matter, but only one creates better results.


### 3: Humantic data is dynamic, not static


Humantic data has another property that is important – it is constantly changing. Any manager of a modern contact center will tell you that there is always something new. The business releases new products and services all the time, causing new conversations about those products and services. Global events impact businesses and the calls they receive. Gas prices going up for example, change the nature of conversations with car rental businesses, airlines, auto manufacturers, shipping companies and more. Acquisitions, executive changes, and stock price changes reshape customer questions. Software companies have outages, bug reports, disclosed vulnerabilities and network issues — all of which bring new conversations to the contact center. Each brings a wave of new conversation patterns that human agents adapt to in real time.


Volume matters – but only when paired with currency. A large, status archive of calls from years past trains your AI on how your best people used to work. What you need is a continuous feedback loop that keeps AI agent behavior aligned with how your best people are working today. That’s the architecture that separates a capable AI agent from one that keeps getting better.


## The “so what”: Humantic data is how AI agents grow up


Why is humantic data key for building good AI agents? Because AI learns the same way people do – by watching experts, not just reading a manual.


How do you train a new human contact center agent? You have them shadow a trained one. They listen in on calls. They hear the wins and losses. They learn the flows on how to emulate successes and avoid failures. It would stand to reason that the best way to train an AI agent is going to be the same. Humantic data is similar to what is gathered by a human trainee shadowing a trained human agent!


Expanding this analogy and the picture sharpens. Training human contact center agents has two phases. The first is basic instruction: defining terminology, products and services, use cases, and the systems they’ll use. This is often done via manuals, training materials, and classroom education. This phase produces a rookie — someone who has the raw knowledge but is likely to do a poor job at the task, because they’ve never actually taken a live call yet and had the experience required to do a good job.


Much AI agent design today focuses on this first phase — arming it with knowledge articles, tools for performing tasks, terminology, and basic steps. This result is, predictably, a rookie agent. It can handle basic cases, but it is going to be nowhere near as good as the human experts, resulting in lower call resolution rates and lower satisfaction. No contact center on earth would staff their entire human labor force with rookies who’d never taken a single call — so it shouldn’t surprise anyone that contact centers staffed with rookie AI agents see lower resolution rates and lower satisfaction.


The data needed to convert the rookie AI agent to an expert AI agent is humantic data. The same kind of data that turned reinforcement learning from an academic idea into the secret weapon behind every leading LLM — just applied at the agentic layer. Which brings us back to the question every executive is really asking: how do we deploy AI agents that actually perform? The answer starts with humantic data.


## Final thoughts on humantic data


At the start, we said most executives are asking the wrong question. They're asking how to deploy AI agents — when the question that determines the answer is: where does the training data come from? Humantic data is that answer. The foundational truth of AI hasn’t changed: it requires data. With each wave of innovation in AI, we’ve seen a corresponding requirement for data. Agentic AI is the next wave, and it is clear that the hardest part of AI agents is the data required to make them effective. We call this humantic data – the annotated record of how skilled humans perform work that teaches AI what good looks like, even across fluid handoffs between AI and human agents.


An AI is only as good as the data it is trained on. A rookie who’s never taken a call can’t replace a seasoned expert. The difference between a rookie AI agent and an expert one is not a smarter model. It is the right data, structured the right way, kept current. That’s what humantic data delivers – and it is what turns a promising AI deployment into one that actually performs where it counts: containment rates, resolution accuracy, and customer satisfaction.


Image


[Jonathan Rosenberg](https://www.five9.com/blog/authors/jonathan-rosenberg-0)


## Related Articles


[Image](https://www.five9.com/blog/forget-2025-here-are-my-top-predictions-cx-2030)


## [Forget 2025 — Here Are My Top Predictions for CX in 2030](https://www.five9.com/blog/forget-2025-here-are-my-top-predictions-cx-2030)


- [AI & Automation](https://www.five9.com/blog/category/ai-automation)
- [Customer Experience (CX)](https://www.five9.com/taxonomy/term/26)


December is a time of relaxation, holidays, and family. It is a time of snow (for some) and some good food and drink. And of course — it is time for predictions! December is when pundits, analysts, influencers, executives and marketeers all come out with their views on what the future holds in 2025.


Image


[Jonathan Rosenberg](https://www.five9.com/blog/authors/jonathan-rosenberg-0)


[Image](https://www.five9.com/blog/four-safe-applications-generative-ai-contact-center)


## [Four Safe Applications Of Generative AI In The Contact Center](https://www.five9.com/blog/four-safe-applications-generative-ai-contact-center)


- [AI & Automation](https://www.five9.com/blog/category/ai-automation)
- [Customer Experience (CX)](https://www.five9.com/taxonomy/term/26)
- [Contact Center Management](https://www.five9.com/blog/category/contact-center-management)
- [Agent](https://www.five9.com/blog/category/agent)


The following post was originally published by Forbes Technology Council, authored by Jonathan Rosenberg. Pioneer in the telecommunications industry with over 25 years of experience, Jonathan Rosenberg is currently the CTO and Head of AI at Five9.


Image


[Jonathan Rosenberg](https://www.five9.com/blog/authors/jonathan-rosenberg-0)
