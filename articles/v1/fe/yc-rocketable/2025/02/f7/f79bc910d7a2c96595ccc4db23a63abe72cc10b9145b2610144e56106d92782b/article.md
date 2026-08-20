---
schema_version: "1.0.0"
document_id: "f79bc910d7a2c96595ccc4db23a63abe72cc10b9145b2610144e56106d92782b"
company_key: "yc-rocketable"
company: "Rocketable"
source_id: "yc-rocketable-rss-cd050a2db77a"
canonical_url: "https://rocketable.substack.com/p/ideas-every-llm-product-builder-should"
published_at: "2025-02-11T15:12:13+00:00"
first_seen_at: "2026-07-25T21:41:11.940424+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:fbf748516dd549c191885355ee088c63f3340f6e68b9a4ab0e03d57efe8bbcdf"
---

# Ideas Every LLM Product Builder Should Steal from Self-Driving Cars [Part 1]

# Ideas Every LLM Product Builder Should Steal from Self-Driving Cars \[Part 1\]


### Build systems that learn from ground truth data instead of human-crafted heuristics


[Alan Wells](https://substack.com/@rocketable)


Feb 11, 2025


Prior to starting


[Rocketable](https://open.substack.com/pub/rocketable)


, I spent a decade working at the cutting edge of sensors, hardware, and AI/ML algorithms - building products that measure, interpret, and predict the physical world (self-driving cars & trucks, ADAS systems for 2-wheeled vehicles, and an irrigation co-pilot for farmers).


This experience left me with many hard-earned lessons on how to build products that depend on probabilistic systems (where the behavior of the product is determined by the output of a model rather than deterministic code).


This post is the first in a series about how those lessons apply to building the next generation of LLM-enabled AI products (


[subscribe](https://rocketable.substack.com/subscribe) to receive future posts in the series).


#### **Lesson #1:** High performance automated systems are built by learning from raw, ground truth data rather than human-crafted heuristics.


The evolution of the two self-driving systems that have achieved fully driverless, commercial operations (Cruise at a small scale, Waymo now at a much larger scale) share a common historical thread: very early on, they started out with heuristics and rules-based systems that were crafted by the humans building the system. It turns out that these heuristics and rules led to impressive early demos, but they weren’t sufficient to deliver the performance needed for true driverless operations (without a human in the driver’s seat).


Over time, these hand-crafted heuristics were


[increasingly](https://www.getcruise.com/news/blog/2023/av-compute-deploying-to-an-edge-supercomputer/)


[replaced](https://waymo.com/blog/2018/05/google-io-recap-turning-self-driving-cars-from-scifi-to-reality-with-ai/) by learned approaches (AI models) that depend on massive amounts of training data to develop systems that can respond appropriately to the long tail of strange situations encountered in the real world.


I’m seeing a similar process repeating in LLM products, but in a different way: LLM-based products are being deployed on top of content that has been distilled & summarized by humans rather than giving LLMs access to the original source data.


Customer support chatbots are one example of this problem: all of the LLM-based customer support bots that I evaluated for Rocketable wanted me to provide a knowledge base, FAQ, or other product documentation as context for the LLM to operate against.


These hand-crafted artifacts are effectively just heuristics and abstractions that attempt to answer common questions or issues about how the product works. They are very much necessary for human customer support agents who can’t be expected to read the application’s source code to find the answer to a customer’s question.


But LLMs, like self-driving cars, have superhuman abilities in their ability to ingest and make sense of massive amounts of data. They don’t need a human to summarize that data for them - in fact, using those the hand-crafted docs instead of raw data actually removes important signal from the model and hinders (rather than helps) performance.


For most software products, the raw data that determines the “ground truth” of how the application behaves for any given user is a combination of two things: the source code of the application (the same for all users) and the data for a particular user (which drives the behavior of the source code for that user).


Not a single one of the 6 different LLM support tools I evaluated supported native integration with my source code repository and user database, so I built my own workflow directly on top of the LLM foundation model.


This custom workflow does 3 simple things:


1.


It provides the entire source code of my application to the LLM as context.


2.


When a support message is received from a user who has an account, the workflow extracts a snapshot of the user’s data from the application’s database.


3.


The LLM is asked to generate an answer that is appropriate for end-user consumption, using the source code, user-specific data, a custom system prompt, and the user’s original question as context.


Google’s Gemini models have been a game-changer for this workflow. With 2M tokens of context available on Gemini’s flagship models, there is plenty of room for my ~1.3M tokens of source code context, a snapshot of user data, a custom system prompt, and the user’s questions. I don’t need a complicated RAG system (I use


[repomix](https://github.com/yamadashy/repomix) to generate an LLM-optimized version of my entire repo).


The quality of the answers produced by this custom workflow astounded me when I first started using it with Gemini 1.5, and reasoning models have improved the quality & reliability of this approach substantially since then (when the relevant source code fits within the smaller token limits of the reasoning model


1


).


Despite the obvious opportunity to build a product that does this out of the box, I haven’t found a CS tool that replicates this workflow (if you’re building something that does this, I’d love to try it - please


[contact me](https://x.com/alanwells) ).


There are reasons that most CS tools don’t do this, but they are mostly about downside protection (you need to be confident in the LLM’s ability to not leak either private user data or your application source code when you have these data sources connected).


That brings me to the second gap I find in today’s LLM products, and the topic for the next post:


**supervised mode** .


Subscribe here to receive the next post in this series.


####


1


My current best version of this workflow uses Gemini 2.0 Flash Thinking along with an intelligently truncated version of source code context (needed to stay under the 1M token limit). Can’t wait for a reasoning model with 2M token context to be available!
