---
schema_version: "1.0.0"
document_id: "c9c18b84d1b8ff229c0f452019586cf04444085dfb218494da01147b9fc0f725"
company_key: "yc-decisional-ai"
company: "Decisional AI"
source_id: "yc-decisional-ai-news-import-6844c8207cfa"
canonical_url: "https://www.decisional.com/blog/jagged-edges-llm"
published_at: "2026-01-08T00:00:00+00:00"
first_seen_at: "2026-07-24T04:01:29.236634+00:00"
fetched_at: "2026-07-28T22:23:47.437673+00:00"
content_hash: "sha256:4cb98939ca659ed44437e4a5fcdadfb3c13aa5e65b216d7730038b66bad07726"
---

# Principles of Building Along the Jagged Edges of LLM Intelligence

"Just adding AI" has burned a lot of founders, PMs, enterprises adopting AI products. I propose a set of alternative principles that define what it means to be "AI Native". This post is meant for PMs and founders building AI Products.


### Five Principles for AI Native Products


1. **Use AI along the Jagged Edges**
2. **Reduce AI in everything else around the jagged edge**
3. **Extract context progressively**
4. **Target 100% workflow completion**
5. **Take the user to a minimum viable review**


## Background


There are a lot of year-in-reviews by really smart people in the AI space.


I love reading these because it gives a sense of what people believe is consensus and a hint at what people disagree with since it is a stake in the sand. If you didn't have the time, here are some simple arguments that you should be paying attention to.


- **"AGI takeoff" scenario likely** - AI progress in 2025 was more incremental than expected
- **Technology Diffusion takes time** : It has been gradual like all technological revolutions before it.
- **"No Free Lunch for the Foundation Labs"** : The business impact is that foundation model labs now NEED TO CHOOSE the capabilities they want to boost performance on. No surprising capabilities are likely out of the box and thus a huge push towards certain economically valuable tasks like Excel Editing, Presentation Making, Science, Math & Research. Cue the companies in this supply chain doing well - Surge/Mercor and other experts on demand services.


> "The main strength of pre-training is that … you don't have to think hard about what data to put into pre-training … it's very natural data … it's like the whole world as projected by people onto text, and pre-training tries to capture that … Now, once you leave that regime, you have to think about what goal you are optimizing for, what the data is, and that becomes a design choice … You just had to train on everything for pre-training." - Ilya on Dwarkesh Podcast


What was remarkable to me was that there was almost no to very little disagreement among the smartest people in AI at a fundamental level i.e Andrej Karpathy, Ilya Sutskevar, Dan Wang, Dwarkesh, etc.


## The Consensus


We are far away from a human level general learner mainly due to a lack of the ability for AI models to be continual learners and generalise. They can't learn new skills easily.


Reinforcement Learning with Verifiable Reward is what foundation labs are investing in to push the capability curve. If you want a simple way of understanding it: getting performance out of AI models this way is much harder than just training on internet data like GPT-3/4 did. What came out of O1 and R1 (reasoning models) is using this technique—taking human-graded solutions, incorporating a reward signal for better answers, and improving the model's ability on specific tasks.


## Jagged (AI) vs Human (Round)


If you want to understand the potential trajectory of intelligence and how to build products here you need to know that human intelligence is **ROUND** whereas AI intelligence is **JAGGED** . The average human is a good all-rounder at a suite of tasks like driving, excel editing, presentation making, stacking boxes, etc. with the occasional Prodigy that spikes in one direction.


> "LLMs… display amusingly jagged performance characteristics — they are at the same time a genius polymath and a confused and cognitively challenged grade schooler…" - Karpathy in 2025 LLM Year in review


This means that Agents are going to be superhuman in some ways (Coding, Information Research, Math, etc.) and incredibly poor compared to average human intelligence in other ways. And the foundation labs are going to spend a lot of resources on using RLVR to improve progress along narrow directions but it is not going to be filling gaps along all the deficient areas (presentation formatting or Excel editing).


Figure 1.1 - Shows AI intelligence and Human Intelligence superimposed on each other


## What it Means for AI Products


So does this mean that the average knowledge worker can get back to work and not have to worry about AI? Should businesses not try to deploy AI Agents?


Not quite. The cost per unit of labor has changed for a certain set of tasks in every job description. This is along the lines of coding, web research, etc. These tasks now should be channeled through AI. The rest of the stuff is really where you need to fill in the gaps. If you believe that the cost of inference is going to reduce it will not be viable for humans to compete in these areas. This has always been the steady march of technological progress.


Reading Steel, Steam and Infinite Minds by Notion co-founder Ivan really paints a picture of what work will be like - managing infinite minds.


> "My co-founder Simon was what we call a 10× programmer, but he rarely writes code these days. Walk by his desk and you'll see him orchestrating three or four AI coding agents at once, and they don't just type faster, they think, which together makes him a 30-40× engineer. He queues tasks before lunch or bed, letting them work while he's away. He's become a manager of infinite minds."


### Managers of Jagged Intelligence


One interesting aspect of managing jagged edges is that you need to get them to perform tasks that outperform human capability and fill in the other gaps. The key numbers:


- 2 years ago: AI could do tasks taking humans ~9 minutes
- Now: AI can do tasks taking humans ~4+ hours (at 50% reliability)
- Extrapolated: Soon AI will handle tasks taking humans weeks


If you believe all this to be true you should believe in building products that live within this world of infinite jagged edges and revise your priors accordingly.


### The Practical Handbook for Building AI Agent Products


## Principle #1 - Use AI Along the Jagged Edges


Coding, Research, Web Search, Imagegen. These all work and your primary flow should incorporate these in order to bring some value to the table that exceeds human ability.


## Principle #2 - Reduce AI in Everything Else Around the Jagged Edge


To fill in for the average intelligence of these AI models, DO NOT RELY on them completing avg IQ tasks, reduce the AI in all of this. Use reliable software engineering. Set guardrails to protect AI from doing things it is not supposed to be doing. E.g asking the user before sending an email.


AI SDRs that auto-send emails without approval have burned countless companies. The ones that work (like Instantly or Smartlead) use AI for copywriting but keep sends, sequences, and timing in deterministic logic.


AI needs to draft, humans approve and software executes.


## Principle #3 - Extract Context Progressively


Since you are using AI that is superhuman in certain ways, you need to get all the context it needs to deliver the best possible results. Most of the time users are lazy so get started with a simple prompt, but use questions and clarifications to eke out relevant information that can affect the quality of the result.


**Examples:** Plan mode in Claude Code, Clarification widget in OpenAI Shopping Research


## Principle #4 - 100% Workflow Completion


> "AI doesn't do it end-to-end. It does it middle-to-middle." - Balaji


AI lands in the realm of "gets you 60% there" which is generally a failure. AI lands in this territory precisely because of its jagged nature. It excels at the meaty middle and fumbles the first mile and last mile.


This is why you cannot treat AI as a reliable employee that just gets the job done. Products need to be designed to exploit the peaks and compensate for the valleys.


This is why copying code from ChatGPT created the opportunity for AI IDEs - they closed the loop. If your product is not aiming to get 100% of a workflow done, you are not going to cut it and making the right tradeoffs to get you to 100% is what builders should focus on.


## Principle #5 - Reduce the Burden of Review


Users don't trust AI to get everything right, and they shouldn't. But forcing them to review everything forfeits all the gains in productivity. As the capability of the model gets better, the burden of review should be getting smaller, so it is up to the product to define the layers of review that users can drill into. (Cursor has moved from code editing to full agents that write, execute and test code). In the case of code it is compilation and tests. In the case of research work it may involve reading the entire body of work (more burdensome less popular use case). In the case of Image generation it might be looking at the asset (quick review means products like midjourney are doing well here).


**Example:** Vibecoding platforms (Replit, Lovable, etc) have code viewable only on clicking on a separate tab. They first allow you to review what the app looks like by rendering the code on the front end. The code and database may be hidden in the back end.


Here's the design heuristic: you could have predicted vibe coding in the GPT-3 era by asking one question: "What if users only reviewed the final website, not the frontend code, backend code, and infra underneath?"


## Conclusion


The foundation model labs will keep pushing the jagged edges outward, that's their job. Your job is to build products that ride those edges without pretending the gaps don't exist. Draft with AI, approve with humans, execute with software. Extract context progressively, close the loop on workflows, and collapse the review burden to the minimum viable artifact.


Apply that question to your domain!


Thanks to Mayank Juneja & Adit Sanghvi for their inputs!


## References


- [Dwarkesh Patel](https://www.dwarkesh.com/p/thoughts-on-ai-progress-dec-2025) : A long-form interviewer and synthesizer of frontier AI thinking who regularly extracts first-principles views from top researchers and founders.
- [Andrej Karpathy](https://karpathy.bearblog.dev/year-in-review-2025/) : A hands-on AI builder and educator (ex-OpenAI, ex-Tesla) with rare intuition for what today's models can and cannot actually do in production.
- [Dan Wang](https://danwang.co/2025-letter/) : A China and industrial-tech analyst who connects AI narratives to manufacturing, supply chains, and real economic power.
- [Foundation Capital](https://foundationcapital.com/where-ai-is-headed-in-2026/) : An early-stage venture firm offering a ground-truth view of what AI products are realistically getting adopted inside companies.
- [Noah Smith (Noahpinion)](https://www.noahpinion.blog/p/noahpinions-2025-year-in-review) : An economist-turned-writer who situates AI within macroeconomics, investment cycles, labor markets, and geopolitics.
- [The Free Press](https://www.thefp.com/p/what-we-got-wrong-this-year) : A multi-author outlet providing contrarian, institution-focused retrospectives that stress-test dominant political and cultural narratives.
- [Stratechery (Ben Thompson)](https://stratechery.com/2025/the-2025-stratechery-year-in-review) : A tech strategy analyst known for clarifying platform incentives, business models, and how AI reshapes competitive dynamics at the company and nation level.
- [Ilya Sutskever (interview)](https://www.dwarkesh.com/p/ilya-sutskever-2) : A leading AI researcher and co-founder of OpenAI whose thinking reflects the frontier research mindset shaping long-term AI trajectories.
