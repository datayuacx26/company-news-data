---
schema_version: "1.0.0"
document_id: "b3b7aacf6e300b6909f20f21d5dab1f3775bc532d4e685d2b17d0bdbb611048d"
company_key: "yc-cortex"
company: "Cortex"
source_id: "yc-cortex-news-import-d57bc7656b70"
canonical_url: "https://builders.cortex.io/blog/trustworthy-agents-for-opex-reviews/"
published_at: "2026-08-18T12:00:00+00:00"
first_seen_at: "2026-08-18T21:07:39.042672+00:00"
fetched_at: "2026-08-18T21:07:41.405524+00:00"
content_hash: "sha256:ac48368f617c5a4d989fd7acb3a2ed9d14b83673f80ae2218569d5bce98e732a"
---

# Building trustworthy agents for Operational Excellence reviews

## Overview


At Cortex, we run our own weekly Operational Excellence (OpEx) reviews to continually improve our engineering processes and empower our engineers to deliver an awesome experience for our customers. Since our main product at Cortex is a platform that helps organize engineering teams, it’s natural that the next evolution of our product would include a toolkit for other companies to build their own OpEx program.


The best way to do that in 2026 is to build an agentic capability that understands engineering, has access to metrics and data about our engineering processes, and can support regular OpEx reviews through data, interpretation, and open-ended question answering.


This post is a look at how our new[OpEx Review Agent](https://docs.cortex.io/improve/opex-review?utm_source=builders.cortex.io) works. As engineers, both the “why” and the “how” are kind of interesting so rather than just focusing on the technical bits it starts with a little background on where this product came from and how it helps serve engineering *as a process* . From there it covers why an agentic system fits well for the problem space and wraps up by addressing a key challenge, trustworthiness, that we had to overcome when building it.


## So what is Operational Excellence and why should I care?


If you’ve never heard of Operational Excellence before I guarantee that you have still felt its impact. OpEx is a business strategy that focuses on continual improvement, empowering everyone in an organization to deliver value to customers and reduce waste or bottlenecks. The most famous example of OpEx in action is the Toyota Production System (TPS) which is the system that gave us Kanban.


## OpEx at Cortex


I’ve worked at Cortex for a little over two years now and, starting on my second day of work, I’ve been participating in the weekly Operational Excellence (OpEx) process that my colleague[Shawn](https://www.linkedin.com/in/shawn-burke-pnw/) leads. It follows the same format every week. We look at our top-level reliability metrics, we follow up on initiatives, we talk about recent incidents, and we keep a pulse on what’s working and what needs more attention. That regular focus sets the tone for how our leadership (both ICs and Managers) thinks about making our product resilient, scalable, and awesome.


The success of that program has led to legitimate, material improvements in our product. Despite serving increasingly greater scale, system latencies are down, and we’ve added 9s to our availability SLOs. Our on-call rotations subjectively have become much less crazy and I no longer feel like I need to sleep with my laptop under my pillow. In an industry where it seems like AI has led to significant quality issues, we’ve improved on most of the key engineering metrics that we track.


This success of Cortex’s OpEx process is why our CTO,[Ganesh](https://www.linkedin.com/in/gsdatta/) , distilled down the principles that worked for us into the[DRIVE Framework](https://www.cortex.io/report/drive-framework?utm_source=builders.cortex.io) . That framework was built from real lessons not only at Cortex but at other major engineering teams in the industry.


API requests / month


111.9M


Sep 2025


→


313.3M


Jul 2026


Catalog API p99 latency


4.50s


Sep 2025


→


0.72s


Jul 2026


What our OpEx Agent does is allow you to capture these same gains without building this whole process from scratch. By just configuring the agent as a Cortex customer, you too can start driving towards these types of improvements. For example, many customers care a lot about Delivery (the D in DRIVE) and they will pay attention to things like deployment frequency and review cycle times.


Before you run your meeting, the agent wakes up, pulls key metrics that you’ve identified and runs an interpretation workflow that builds you a report highlighting what is improving and what needs focus. It ties back to the rest of the Cortex ecosystem so that teams can drill into specific problem areas with our Engineering Intelligence tools or take other actions like creating Initiatives (the I in DRIVE) to improve a trouble spot.


## What does Agentic Capability even mean


In 2026, agentic capabilities are all the rage for good reason. Used here, the term refers to a system that uses a Large Language Model (LLM) to reason about a domain and feeds that reasoning back into a product experience. Not just a chat bot, but something that proactively works to help you understand things and can exercise autonomy in making decisions about what to do next.


Even before we wrote a line of code, we knew there were some properties that our system required in order to effectively support OpEx reviews. It had to be 1) **trustworthy** and based on solid data, 2) **dynamic** and **customizable** to meet the needs of a highly varied customer base and 3) incorporate **temporal awareness** and **memory** so that as issues come in and out of focus they aren’t lost week to week. Each of these properties presented unique challenges and, while we haven’t solved them all, they are collectively important for building our OpEx capability.


The most important property for a novel data reporting system is by far **trustworthiness** and that is the one this post really focuses on.


## Trust


Agentic systems can be extraordinarily convincing but, like an engineer giving time estimates, they tend to overconfidently make claims that are based on hallucination. Hallucination is a well-documented problem, but for any system that wants to provide claims about something as critical as operational data those claims MUST be backed by solid data and grounded in reality. It sounds believable when an agent says “Deployments are up!” but it’s much more trustworthy when it says “Deployments are up 36% as seen by this metric that is identical to what is on your Deployment Health dashboard”.


There are two major decisions that our team used to ensure that hallucinations were minimized as much as possible and that what the agent produced was trustworthy. First, we use **battle-tested tools** as the underlying way that the agent interacted with data and second, we have redundant processes that verify each and every claim is **grounded** by real observed data from those trusted tools.


### The right tools for the job


Trust starts with where the data comes from. The Cortex platform offers a lot of capabilities that are useful for agents but two in particular are especially relevant. We have a Query Engine system that offers a semantic data layer for understanding key engineering metrics and a Software Catalog that understand the teams, systems, resources, and other entities that make up your engineering org.


We are fairly well-known for our Software Catalog but our Query Engine is a more recent addition to the platform that we built to massively extend the capabilities of our Engineering Intelligence product. Unlike some platforms that give an agent direct access to a SQL database and cross their fingers, we took the time to build a system that encodes our companies understanding of engineering systems and metrics into a self-describing Query API that uses both structured data and natural language to describe *what the data represents and what it can be used for* .


Every metric carries a plain-language description of what it measures and what it represents, not just a name and a unit.


While we originally built the system to help human users understand how to work with their data via our Data Explorer, it turns out that LLMs also greatly benefit from natural language descriptions. After all, they are Large *Language* Models.


We should probably write a post on Query Engine someday. It’s pretty rad.


### Grounding


Good tools guarantee that the data is real. They can’t guarantee that the agent won’t fabricate claims about the data or otherwise run off the rails. With a solid, foundational source of data from our tools we shifted to building programmatic controls that backstopped the LLMs reasoning to make sure that a claim was always grounded in observed data. Prompt engineering plays a key role in making sure that our agents have good instructions on what we would like them to do, but the flaw is that agents don’t always follow their prompts. Grounding ensures that they are doing what we ask.


Let’s take a quick aside on WHY we shouldn’t implicitly trust LLMs. LLMs, like their human operators, are inherently error prone and can hallucinate findings as part of their nature. Their outputs are a non-deterministic product of what they were exposed to during training, what they’ve been reinforced to attend to, and whatever previous inputs are in their context.


At the end of the day they are similar to fancy autocompletion engines whose primary goal is to just predict the next token. They will *always* try to guess what should come next even when the answer is nothing or when they have to make up some data to get to a conclusion. It doesn’t make them bad, it just makes them wrong a lot.


To understand how we ground the reasoning of an LLM, we need to understand the flow of data through our report generation system. The diagram below gives a glimpse of the process, with some of the details abstracted out a bit.


-


Deterministic code
-


LLM reasoning
-


Amendment loop


The gates at either end are code. Everything in between is an agent, and the step that critiques runs a more powerful model than the step that synthesizes findings.


#### 1) Take a data snapshot


Our report building workflow starts with fetching data from the configured metrics that the customer has selected. We execute programmatic queries that look back over a window of time and build a dataset that we can then put in front of an LLM for interpretation. This step is intentionally simple and doesn’t involve the LLM at all. It’s the input for subsequent reasoning stages.


#### 2) Triage


We take the raw data and generate a short list of areas to explore. This helps more with output quality than anything and saves tokens in later phases. The triage is a simple step to quickly figure out what the next rounds of reasoning should focus on.


#### 3) Evidence gathering


An agent looks at the snapshot and determines if we need additional data to do a good analysis. It performs tool calls to enrich the snapshot with whatever it finds.


#### 4) Synthesis


This is the heart of the reasoning layer and it generates the analysis that makes its way into the report. Using the data provided by previous steps, it forms “claims” on the data that are operationally and domain relevant. It produces a structured output with those claims (a mix of prose and data) that it then passes forward for critique.


#### 5) Grounding Critique


This is the step that keeps our reasoning agent honest by conducting a thorough and independent agentic review of the claims made by synthesis. It looks specifically for two critical problems that are flagged as explicit *errors* : citations for a totally bogus query and claims that contradict a citation. If an error occurs, the entire report is recycled through synthesis one more time including another critique pass to make sure errors were corrected.


In addition, the critique agent can flag a whole host of advisory critiques that can distort the accuracy of a report. This includes things like rounding or precision errors, imprecise language, overstating a claim, etc. These are not things we would explicitly *fail* a report on but things that we think should be corrected.


#### 6) Validation


So remember how LLMs hallucinate? Even with a second agent grounding claims we still want to apply a *programmatic* validation to ensure that claims are against real, actually citable data.


To do this, our reports emit a structured claim citation that we can validate by checking our database. Every query is recorded and so if the agent cites a query that never existed then we can fail validation and flag it. While currently this leads to a hard stop, we intend to recycle these through an amendment process soon that corrects them rather than rejects them wholesale.


Citation validation is *also* used by our AI chat functionality to ensure that when you are talking with an LLM about a more specific area at Cortex, you get the same protection from invalid claims. This means that if you are asking a question like “What are all the services that my team currently owns?” you should never get an answer that includes a non-existent service. Failing or correcting with some added latency is better than hallucination.


### Taming False Rejections


Our team had a ton of ideas on how to build more trust into the system via grounding, claim validation, and other mechanisms. What we *actually* found to be the real challenge was tuning the system so that it didn’t reject things that were *mostly correct* but failing grounding.


Delivery has to happen on time, while protecting not only from bad data but also from false rejections. Furthermore, a false rejection represents a lot of work/tokens/money that is being thrown away for a perfectly fine report.


An early version of our critique agent would emit a simple boolean` pass` or` fail` . We would sometimes see “all claims supported”… but then a` fail` for the report. The inconsistency was resolved by having the agent simply emit findings and then scoring those findings programmatically into` pass` or` fail` .


We also saw claims rejected because of much more trivial issues like rounding (e..g., 54% is not the same as 53.7%… but it pretty much is). This was solved more through routine prompt engineering and making it really clear what NOT to flag.


Interestingly, we’ve generally observed that synthesis is pretty good with mid-tier models such as Anthropic’s Sonnet models but that the critique agent REALLY benefits from higher capability models like Opus. Most agents are good at summarizing findings, but reasoning about correctness and accuracy requires more critical reasoning skills.


Lastly, agents love to talk/write/generate tokens. If you tell an agent “what’s wrong with this sentence” it will happily offer a correction and then when you say “how about now” it will happily offer yet another correction and repeat this process ad infinitum.


We found that giving agents a way to flag advisory or lower severity items was key for getting more accurate results. It offers a pressure release valve so that the agent can do its thing generating tokens and findings. You will *always* get more results but if the severity shifts down to nit picking then it’s safe to move along.


This last point is actually interesting because we actually learned about it from our agentic code review process! Our homegrown specialist review system touches every single PR across our organization and one of our earliest lessons was there will *always* be more issues to find if you let an agent review your code. That same finding holds true for generating reports on engineering operations. If you want to learn more about that review process, here’s a[video](https://www.youtube.com/watch?v=CADLQSH4NJA) by[Chelsea](https://www.linkedin.com/in/chelsea-hohmann/) , one of our fabulous Engineering Managers, that covers how it works.


## Summary


There’s a veritable laundry list of learnings we could share from building this system such as how we enable context aware chat on the reports so that they aren’t just static things but the springboard for conversation or how we baked in strong durability to our report generation so that our long-running agentic sessions can resume and tolerate failure or how we built these capabilities reusably so that they extend to other agentic “surfaces” (to steal a Claudeism).


So much to talk about, but let’s end on one final point about why trustworthy data is so important to us. We had the first steel thread version of the product building reports in a couple weeks of earnest work but we spent considerably more time than that refining the grounding process, eliminating false rejections, and connecting it back to the Cortex ecosystem so that it was refined and trustworthy.


As we invest in the product, we expect to continue finding ways to improve accuracy and tune the product so it’s not just merely useful but awesome.


Engineers need to trust that when a system tells them “there is something to improve here” that the finding is honest, factual, and actionable.
