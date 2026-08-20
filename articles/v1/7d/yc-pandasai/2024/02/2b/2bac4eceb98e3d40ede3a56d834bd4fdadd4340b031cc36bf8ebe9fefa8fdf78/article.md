---
schema_version: "1.0.0"
document_id: "2bac4eceb98e3d40ede3a56d834bd4fdadd4340b031cc36bf8ebe9fefa8fdf78"
company_key: "yc-pandasai"
company: "PandasAI"
source_id: "yc-pandasai-rss-a866addd46fa"
canonical_url: "https://blog.pandas-ai.com/pandasai-v2-0-smarter-more-powerful-and-easier-to-use/"
published_at: "2024-02-29T09:00:00+00:00"
first_seen_at: "2026-07-25T18:21:24.820430+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:09a8632115127f5130ecd261aabb31ad52a2976b0b01f09ece1235aee2ff7987"
---

# PandasAI v2.0: Smarter, more Powerful, and Easier to use

I'm thrilled to announce the launch of **PandasAI's latest major release - version 2.0.** This update unlocks new possibilities for streamlining and enhancing data analysis through natural language conversations.


Packed with customization options, expanded capabilities, and a simplified architecture, PandasAI v2.0 gives you **more control than ever before** over your conversational agents.


In this in-depth guide, we’ll explore some of the key enhancements in PandasAI v2.0. I’ll walk through hands-on examples so you can see just how much you can achieve with a few lines of Python. Let's dive in!


## Custom training


One of the biggest additions in PandasAI v2.0 is the new` train()` method. This provides **advanced options to** **"teach" your agents using custom questions** , ideal responses, key business logic, and more.


There are two main training modes available:


### Instructions Training


Instructions training allows you to **provide overall guidance, context, and business rules** to define your agent's behavior:


```text
sales_df = Agent("sales.csv")


instruction = """
The fiscal year starts on January 1st.
When asked about sales metrics, provide insights into yearly trends.
"""


sales_df.train(docs=instruction)
```


Think of instructions training like mentorship for your agent - you’re sharing knowledge so it understands proper conventions and priorities when doing the analysis.


### Q&A Training


While instructions training covers general principles, **Q&A training allows absolute precision** :


```text
query = "What were total sales last year?"
response = """
import pandas as pd
sales_2021 = sales_df[sales_df['year'] == 2021]['sales'].sum()
result = { "type": "string", "value": f"Total sales in 2021 were {sales_2021}" }
"""


sales_df.train(queries=[query], codes=[response])
```


Here you can directly **define the exact responses you want to specific questions** . This improves consistency exponentially compared to just relying on the agent's natural language capabilities.


**Combining instructional and Q&A training** gives you the best of both worlds - a **knowledgeable, reliable agent** tailored to your use case!


And the beauty is that all training data persists within the agent once complete, so you only need to train once.


## Describing Datasets and Agents


PandasAI v2.0 also introduces options for **adding helpful descriptions** at two levels:


### Dataset descriptions


You can describe individual columns to clarify specifics about the data:


```text
agent = Agent(df, config={
"field_descriptions": {
"sales": "Monthly sales totals aggregated across all product categories",
"month": "The month in which the sales occurred, formatted as YYYY-MM",
}
})
```


This allows the agent to better understand ambiguous column names, ensuring correct interpretation.


### Agent descriptions


You can also set an overview description for the agent's complete purpose and functionality:


```text
description = "Analyze annual sales metrics and KPIs. Enable self-service reporting for the business intelligence team."


biz_agent = Agent(df, description=description)
```


Describing the agent's identity gives crucial context about ideal responses for conversing with users.


Adding relevant, natural-language descriptions ultimately allows your agent to communicate more naturally!


## More New Features in v2.0


On top of advanced training and descriptions, PandasAI v2.0 comes loaded with other useful upgrades including:


- **Multi-Turn Conversations** - Context is now preserved correctly across long, intricate conversations spanning multiple questions.
- **Google BigQuery Connector** - Directly query data in BigQuery databases from within agents.
- **BambooLLM Integration (beta)** - Harness Anthropic's state-of-the-art generative model to power next-level agents.
- **Jinja2 Templating** - Simpler prompt engineering for easier agent creation and maintenance.
- **Spring Cleaning** - Removed non-essential niche features based on user feedback. Leaning into the "do one thing well" philosophy.


Together these make PandasAI more powerful and flexible than ever while keeping complexity to a minimum.


## Ready to Upgrade?


PandasAI v2.0 delivers a heap of enhancements that unlock new potential for streamlining and customizing conversational data analysis. Advanced training, richer descriptions, and leading integrations give you unmatched control to create targeted agents catered to your use cases.


Eager to get started and take your workflows to the next level? Install PandasAI 2.0 now. And please share any feedback on capabilities you want to see in future releases!


**👉 Repo:**[https://github.com/Sinaptik-AI/pandas-ai](https://github.com/Sinaptik-AI/pandas-ai?ref=blog.pandas-ai.com)


##
