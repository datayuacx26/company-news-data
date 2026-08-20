---
schema_version: "1.0.0"
document_id: "fb4c67aac1313e0ef03380c2f385b0b0c76eabc291ee3259b8aa011fe43e03d1"
company_key: "yc-greywing"
company: "Greywing"
source_id: "yc-greywing-news-import-fac42ec76768"
canonical_url: "https://www.grey-wing.com/blog/mastering-your-data-with-ai"
published_at: null
first_seen_at: "2026-07-21T22:08:18.820655+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:9ecc3e19f8d164d7b296ccdb636fca6dfc89a564b582609b0cd74492103e026e"
---

# What it takes to master your data with AI

# What it takes to master your data with AI


And why we have a unique proposition when it comes to Maritime.


Hrishi Olickel


Published on 15th Jan 2024


7 minutes read


The chartering teams we’ve spoken to understand how important their data is. They take great pains to collect it, and they work pretty hard to protect it.


As of 2023, the number of data-led decisions is still pretty small, save for the companies that can afford expensive Data Analytics teams. Even in those cases, the time and resource cost of using data to drive decisions is high, and it gets used sparingly.


The problem is retrieval. The best-kept information is only as good as your ability to find it and shape it to understand it. Let me put it to you this way, when trying to figure out which routes to fix your vessels on, how useful is this data?


Bit daunting, but you’re probably used to that, aren’t you?


Well, how about now?


That is what SeaGPT can do for you today. It can understand billions of rows of nested data, as well as emails, PDAs, PDFs, PDFs of PDAs, you name it. Instead of reflecting this information back at you, it can answer your questions with charts and tables that only contain relevant information aggregating the data across your organization.


It does a lot more - like fetch real-time information from the web, GDS (Global Distribution System) searches, and complex planning - but solving data retrieval alone can be a major force multiplier for your commercial team.


## How does it work?


There are four open problems in data science that Greywing needed to solve, to bring SeaGPT Commercial to market. Let's go through each one:


## Schema and Typing


This is often the biggest problem facing data teams. While most companies have their information stored somewhere, it needs a lot of restructuring and cleaning before it can be useful. Additionally, the cost of cleaning and restructuring this data can grow exponentially with the amount of data - both for human-led efforts, and traditional machine learning approaches. If you have been quoted a price for doing such work, you would know that the price increases with complexity and number of datasets.


At Greywing, we've spent the last four years building out automated dynamic typing and \[ETL\] pipelines for maritime data. We've connected to over thirty different suppliers and stakeholders in the ecosystem. This means that we have crossed the critical mass of operational expertise and data required for our system to be well-versed in common mistakes, different data schemas, and export formats. And with time, Generative AI has solved the final piece.


### Recommended Reading


[Why Maritime needs integrated systems.](https://www.grey-wing.com/blog/dollars-time-and-strategy-why-maritime-needs-integrated-systems)


This means that today, you can send documents or large databases to SeaGPT, and it pulls on the combined power of Greywing to understand that information, and neatly organize it in a manner that can be retrieved. Most organizations attempting the same solution today will find this to be the biggest hurdle to cross from a resource and feasibility perspective.


The key question you have to ask for new AI tooling is:


> “ *What is the marginal cost to my data? How much will this cost to extend?”*


## Conversational queries


Once you have well-organized data (do not worry, well-organized from an AI perspective is quite a bit different from the human one), the next task is converting user demands:


> *'How did this route perform over the last month?'*
>
>
> *'How quickly can I unload this cargo?'*


...into dataset-specific questions that databases can understand.


SQL is the common go-to in this regard, where[sqlcoder](https://defog.ai/sqlcoder-demo/) is the current state-of-the-art solution -before Greywing. However, sqlcoder - and similar solutions - can only operate on a small number of datasets. Additionally, all of them require specific models that can be quite expensive to run and maintain - not to mention prohibitively expensive to keep up-to-date as AI advancements happen.


### Recommended Reading


[LLMs and Tuning for Reliability in Maritime](https://www.grey-wing.com/blog/reliability-in-maritime-llms)


To perform this step appropriately, any system needs to maintain the context of:


1. The user's past questions - humans are likely to say ' *this* ' and ' *that* ' instead of 'dataset CARGO_FIXTURES_2023-01-02'. Carrying this context forward and knowing when to add and drop things is crucial to making your team interact naturally with data.
2. User and company preferences - each team within your organization, as well as within your organization, can have very different preferences on how they would prefer to interact. This might be keeping track of useful information, approval workflows, or even specific charting formats that are preferred across the organization.
3. Available extents of the information - it can be very easy to look for information where none exist or to forget to merge information across boundaries. If you recently migrated tools (as recent as a few years ago), some of your information might be sitting somewhere else and need to be merged with the latest format, before this data can be used.
4. Datasets across the entire organization - creating proper metadata about all the available information - something we just touched on - can itself be a massive undertaking. Without this, all other current systems will fly blind in terms of finding the specific needles in your haystack.


Fortunately, having worked across different teams and verticals in maritime, this is a problem we've had to solve early in our process. If we couldn't dynamically generate limits, content-based metadata, and transformations for data, we would need a team 10x the size of Greywing to make a dent in transforming this data.


### Recommended Reading


[How did Greywing get started on Chartering?](https://www.grey-wing.com/blog/greywing-does-commercial)


Additionally, our team-of-experts based architecture makes it easy to swap underlying models, as the key innovations - and benefits - are delivered through AI orchestration, rather than relying on a fine-tuned model or expensive-to-train models. This also allows us to use the right model for the job - in a single path, we might use as many as four different models to optimize for intelligence, context size, speed, security, or cost.


The question that is most pertinent here is: *Can my AI tools keep up with the pace of AI? What is my cost to upgrade?*


## Reflection


[Agent-based architectures](https://developer.nvidia.com/blog/introduction-to-llm-agents/) are the holy grail of modern AI. In most situations, being able to reflect on results and improve itself is what sets SeaGPT - and other such systems - apart from single-shot AI solutions. Much like humans, being able to iteratively learn and improve - often right in front of you - can make the difference between the wrong answer and the right one.


When you add AI to your organization, the key question you should be asking is: Can it reflect?


### Recommended Watch


[Can SeaGPT fix our mistake?](https://www.youtube.com/watch?v=99Ak1Ispplg)


## Tool usage and planning


Finally, retrieval can be useful, but the elements that supercharge them are **tools** . Tools are additional things your copilot can use - from charts and mathematical expressions to web searches and hotel lookups.


OpenAI and Greywing today are the only providers offering assistants that can use multiple tools in complex planning, to deliver a complete solution. Tools can be the difference between *'Here's that data in a large table'* , and *'I've made the charts you need and put them into a presentation. I've also added in missing costs from the web'* .


Being able to use large amounts of tools (Greywing today counts 18), adding new tools specific to your organizational needs, and doing so without confusing AI agents is still an open area of research.


The forward-looking question that organizations trying to orient themselves to the future should ask is: **What tools are on offer?**


## **Building a complete copilot for maritime**


Modern Generative AI will transform work - and maritime - as we know it. Similar to the invention of Excel, companies and individuals with and without it will quickly differ in their ability to scale and operate efficiently, with data-led approaches.


However, today's Generative AI models are simply the last piece of a much larger solution. They provide large amounts of intelligence, but this intelligence needs careful work and orchestration to be operated in a way that leads to trustworthy assistants that can act as a force multiplier for your team. Hopefully this has helped you understand how we at Greywing are thinking about and building these agents of the future.


Want to stay updated on all things AI we're building for Maritime? Sign up to our monthly newsletter[here](https://www.grey-wing.com/contact-us) .


## Make better decisions with Greywing


Talk to us about your fleet, or follow what we're building.


## Read more from **Proteus**


[Back to blog](https://www.grey-wing.com/blog)[10 minute read Proteus Superficially adopting AI or genuinely leveraging it? How to think about your AI Strategy in 2024. Nick Clarke @NickGreywing 12th Apr 2024](https://www.grey-wing.com/blog/superficially-adopting-ai-or-genuinely-leveraging-it)


[3 minute read Proteus Proteus Vs Ocean Oracle… which is better? Can I pit our two newest products against each other? Nick Clarke @NickGreywing 19th Feb 2024](https://www.grey-wing.com/blog/proteus-vs-ocean-oracle-which-is-better)


[5 minute read Proteus How we *had* to build OceanOracle How we were forced to become the first company to build a visual AI for documents Hrishi Olickel 1st Feb 2024](https://www.grey-wing.com/blog/how-we-had-to-build-ocean-oracle)


[5 minute read Proteus Inception: The origin story of Ocean Oracle Unlike in ancient history, we're giving you real data. Nick Clarke @NickGreywing 30th Jan 2024](https://www.grey-wing.com/blog/the-origin-story-of-ocean-oracle)
