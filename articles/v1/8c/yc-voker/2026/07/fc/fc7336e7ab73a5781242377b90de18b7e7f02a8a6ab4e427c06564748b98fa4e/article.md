---
schema_version: "1.0.0"
document_id: "fc7336e7ab73a5781242377b90de18b7e7f02a8a6ab4e427c06564748b98fa4e"
company_key: "yc-voker"
company: "Voker"
source_id: "yc-voker-news-import-d9168a6d6fb2"
canonical_url: "https://voker.ai/blog/understanding-how-your-users-use-your-agents-as-a-pm"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-22T19:09:16.557037+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:db790d7e8b49b872ec4957bf240b48d935766ce87c7852dcd7e56f4eac28c31c"
---

# Understanding how your users use your agents as a PM

## **Shipping Production AI is hard**


It's Q3, you just spent the last three months in stakeholder hell trying desperately to get the Head of Ops, Head of Sales, Eng Manager, a fancy Head of Applied AI, and even the CEO all on board with your new in-app conversational agent.


You feel like you finally got through all the one off revisions to the prompt, added the right tools, instrumented some evals (best guesses since for the last 3 mos, this hasn't had a single customer's eye on it) and it's launch day. It's finally here.


You launch and now everyone's asking how the agent is performing. So you go to your engineer who set up your Langfuse instance and you ask if there's any meaningful trends. They say not really so you ask for an export of the logs.


They hand you a CSV with 10,000 rows of conversation data. And this is just one month's worth.


‍


## **How we analyze agent performance today**


If this sounds like you--you're not alone. PM's are being asked the impossible question: how are our AI agents performing?


It's no secret that ROI on LLM spend is top of mind for business execs, both big and small. So how do we measure that?


In a poll I ran across dozens of product people across industries, on this exact question, something we heard with overwhelming consensus was either one of 3 things: (1) an emphatic shrug (2) spot checks with the human eye or (3) dumping aforementioned CSV of logs into Claude or ChatGPT and asking for 'trends, patterns, hair on fire issues'.


While I empathize with the folks doing 1 + 2, the reality is, that's not going to cut it. We are after all the tastemakers of our product--the same applies to an AI product.


That's why I'll focus on #3 for this article (and we can all badmouth stakeholders in my dm's after).


‍


## **Some practical tips and the way forward**


I can't stop you from uploading your logs to your favorite chat--the truth is, for a lot of companies, this is the best we've got, so we may as well do it as best we can. Below are some ways I've seen that greatly improve your odds of getting a more meaningful analysis. ***Note*** : None of these are foolproof methods. AI is faulty by nature and I'm no snake oil salesman.


‍


#### **1. Piecemeal analysis = less context lost**


There's a good chance you know about 'context windows' or 'context limits' and with each model, that limit expands. Even so, the odds that an important pattern or user intent slips throughout the cracks go way up the closer you get to that limit.


In prompting, add: "break this up into n chunks before analyzing"


Voker does this automatically by running analysis on each session individually before bubbling up into larger patterns.


‍


#### **2. Ensure your model can code**


Whichever model you're using needs to have coding functionalities full stop. That's most models these days but it's important to note that llm's are, on their own, pretty bad at math. So a model like Opus latest or Codex that has access to python or pandas to take care of the calculations will be your best bet.


In prompting, add:


```text
"ensure you use pandas or equivalent, code-based calculation tool in your analysis"
```


Voker only uses code-based calculation when putting together the hard-metrics like cost-per-resolution.


‍


#### **3. Guide the LLM to look for what counts**


Surely by now we all know that the more specific we are in our queries, the better the the responses from LLMs. While it's tempting to drop that csv in Claude with nothing more than: "Highlight patterns, trends, and unique insights", it's probably going to perform much better with more specificity.


In prompting, add:


```text
"When running the analysis look for specific user intents (such as [insert_domain_specific_intent]). Generate one of each conversation. Only when each conversation has it's own user intent, run a categorization on top. The output should be specific, evidence based categories.


```


‍


Voker generates a user intent for every conversation, then categorizes them. Each intent and intent category are an actual line item in a database that can be referenced against every new conversation. Context aware and persistent.


Hope this was helpful and if you're using Claude + ChatGPT for llm user analytics, I can't blame you--you probably already have a subscription and it works fine enough for the day to day. If you're feeling the pain of having to do this once a week instead of getting in depth, bespoke analysis automatically on every user-llm interaction, try Voker free today. Tell em Vahan sent ya.
