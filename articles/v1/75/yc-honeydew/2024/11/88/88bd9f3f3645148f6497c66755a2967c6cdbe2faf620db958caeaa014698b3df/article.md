---
schema_version: "1.0.0"
document_id: "88bd9f3f3645148f6497c66755a2967c6cdbe2faf620db958caeaa014698b3df"
company_key: "yc-honeydew"
company: "Honeydew"
source_id: "yc-honeydew-rss-6ec5327dfb7a"
canonical_url: "https://honeydew.ai/blog/how-we-teach-data-to-an-llm/"
published_at: "2024-11-27T14:18:09+00:00"
first_seen_at: "2026-07-20T23:20:19.939873+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:ee4f6978d130d6794df4c2af74692a652169a928f9fc61e7ee1143acb51f5118"
---

# How we teach data to an LLM

**Introduction**


How many times did you hear thoughts like “You just connect an LLM to the data and you can ask it whatever you want!”, “AI is so smart that you just connect it to your data and it would understand and generate the queries for you!” or my favorite “Its magic, you plug the AI to the data and it does everything for you!”.


Sounds fantastic, right? But if you’re reading this, you likely know by now that the[reality isn’t so simple](https://honeydew.ai/blog/talk-to-your-data-loudly/) . Connecting an LLM to your data and expecting instant results isn’t a “plug-and-play” scenario.


As much as I’d love to live in a utopian world, I’m a pragmatist (aren’t we all?). The truth is that AI, no matter how sophisticated, doesn’t automatically understand your database schema, relationships, or nuances. Why? Because it lacks **context.**


“So, let’s give it context!” you might think. But here’s the catch: this often leads to constant prompt adjustments, an unstable system, and a lack of trust—both from you and your stakeholders. Sound familiar? You’ve probably poured months or even years into this dream, only to feel like it’s slipping away.


Let’s shift gears and focus on what **does** work.


How do you teach your data to your AI? Or in other words, how do you ground AI in the context of your business and your data?


The technical part of the solution is a semantic layer as the place to keep the context. However, before technicalities what important to understand is the process.


There is one major concept underpinning teach AI with a semantic layer: Treat it like an **inexperienced analyst** you’ve just hired.


As a manager you have practical toolbox to be used (or give the hire) to onboard the new hire, and an agenda that leads your approch. The same teaching tools apply to when thinking of a semantic layer such as Honeydew as a framework for context.


### **The Approach: Start Simple, Then Build**


When onboarding a new analyst, you wouldn’t hand over the entire database schema on day one, would you? Of course not. You’d start small, gradually exposing them to more information as they gain confidence and expertise. The same methodology applies to training your LLM.


### **1. Simplify the Schema**


Think of the data as one single table - at first you do not wish the analyst to understand the entire schema and relations between tables, for simplicity you can tell him “think of it as on single table with attributes and metrics”. Furthermore, avoid exposing foreign keys, complex joins, or internal attributes.


Focus only on **metrics** and **attributes** that a business user would directly reference.


- Tools like Honeydew can handle the lineage and dependencies behind the scenes, so the AI doesn’t need to worry about those complexities (or god forbid write complex SQL queries !)


### **2. Avoid Ambiguity**


Just as you wouldn’t confuse an analyst with unclear terminology, don’t confuse your LLM. If the analyst sees metrics or attributes with similar names he would necessary know which one to use and come ask you, right? Steer clear of similarly named attributes or metrics unless you clearly define their meanings.


### **3. Incremental Exposure**


Start small: Prepare 15–20 business questions, beginning with simple ones and gradually increasing complexity. Expose the AI to new metrics and attributes step by step, expanding its “domain” over time.


### **4. Handle Uncertainty**


Such as you would instruct the analyst to ask question and not jump to conclusion the same with the AI: Include an explicit instruction in the prompt for the AI to declare when it doesn’t know the answer. (No hallucinations allowed!)


### **Your Semantic Layer Toolbox for LLM to data Training**


In Honeydew, these are the tools and techniques that will help you “train” your AI:


1. **Domain Creation**


- Use[Domains](https://honeydew.ai/docs/domains) to define a focused subset of metrics and attributes.
- Start with a small domain and gradually add attributes and metrics as you gain confidence the AI answers all the business questions.


1. **Metadata Descriptions**


- Leverage[metadata](https://honeydew.ai/docs/governance/metadata) to provide clear descriptions of metrics and attributes.
- Include synonyms or create derived metrics with multiple names to handle varied phrasing.


1. **Time Management**


- Provide a[time spine](https://honeydew.ai/docs/advanced-modeling/time-spines) to ensure consistent date handling.
- Use[time metrics](https://honeydew.ai/docs/advanced-modeling/time-metrics) to establish correct relationships between time and metrics.


1. **Example Questions**


- For extremely complex queries, use dynamic[datasets](https://honeydew.ai/docs/dynamic-datasets) with explicit descriptions. But don’t overload the AI with these—it could lead to confusion.


### **Wrapping Up**


Treating your AI like an inexperienced analyst doesn’t just make the process smoother—it also spares you from countless headaches (and possibly a few existential crises).


With the right tools, a clear approach, and a little patience, you’ll turn your LLM into the most efficient, non-coffee-drinking, non-vacation-taking team member you’ve ever had.


Now go forth, teach your AI, and remember: if all else fails, just blame the “new hire”! 😉
