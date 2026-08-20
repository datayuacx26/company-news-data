---
schema_version: "1.0.0"
document_id: "87b321fb84ae6eb595dc8ab8fa19e656603475e268ced6a1a9f7f9828408e367"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/launching-serifs-ai-assistant"
published_at: "2024-11-19T21:31:00+00:00"
first_seen_at: "2026-07-22T13:10:17.126339+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:f2b28a5b70abed627bcf7362a3e7d6aa9ef647a9f3f4d20e266a5fe440c53ad8"
---

# Launching Serif’s AI Assistant

At Serif, we are focused on providing the latest and greatest innovations in AI, ML, + big data & analytics tools to transform the 300 billion + rows of price transparency data we ingest monthly into actionable insights for our customers.


We are excited to announce that our web platform Signal now includes an amazing generative AI integration for working with price transparency data. If you are a current Signal subscriber, you now have a personal copilot to help you parse through the searches and reports you generate for new insights, visualizations, and much more!


Already, we have seen alpha users use AI to help with some of their most pressing data questions such as:


### – **Identifying the highest reimbursed providers for a given service line and geography** :


***Example Prompt*** *: “What are some of the highest reimbursed ABA groups in Atlanta?”*


### – **Graphing data for easy visualization**


***Example prompt*** : *“Can you graph this data to show how rates differ by practitioner type?”*


### – **Clarifying questions about the data**


*Example prompt* : “ *A typical rate for a 97802 done by a Registered Dietitian is ~$25. Any reason why we’re seeing a median rate of close to ~$100 for this payer?”* ‍


As shown in the examples above, Signal’s AI assistant can help with a wide variety of tasks from creating charts, clarifying odd data points, and finding the nuggets of information you need most.


We’ve been absolutely amazed by some of the creative custom analyses and reports customers have already pulled together and cannot wait to see how this improves over time with more model iteration and learning.


## Technical Notes


Getting here required careful calibration, testing, limits, and feedback.


First, we had to find the right large language model to integrate into our stack. We tested several, including heavy-hitters such as ChatGPT, Gemini, and Claude, to find which ones had the best data analysis capabilities for price transparency.


Second, after choosing one to integrate with, we had to provide context, history, and precise instructions on how to parse Serif’s price transparency data. For example, we assist the model with choosing the correct rate when multiple are present with the following prompt:
` 1. Primary Aggregation Logic:`


` - For each unique combination of EIN, payer, and code:`


` * Select the record with the maximum NPIListLength`


Lastly, we had to set limits to make sure the models would perform. Generally, we found smaller data-sets worked best for AI-driven analysis (e.g., single code, single-specialty searches) whereas larger datasets with nuances around different codes, contracting types, billing classes and baseline rate values caused more confusion and less insightful details in the response.


After applying these learnings from our research, Signal’s AI assistant provides extra leverage for those looking to understand the price transparency data at a deeper level. Just like any human, we encourage our subscribers to sense check the responses and be cognizant of what the limits are but overall we look forward to seeing how our users incorporate AI into their price transparency research.


If you are curious or want to learn more, please reach out tohello@serifhealth.com !


‍
