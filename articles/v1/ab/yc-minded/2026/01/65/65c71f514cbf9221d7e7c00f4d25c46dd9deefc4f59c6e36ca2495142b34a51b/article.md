---
schema_version: "1.0.0"
document_id: "65c71f514cbf9221d7e7c00f4d25c46dd9deefc4f59c6e36ca2495142b34a51b"
company_key: "yc-minded"
company: "Minded"
source_id: "yc-minded-news-import-42e7450e1631"
canonical_url: "https://www.minded.com/blog/our-ai-agent-journey-to-99-8-accuracy-in-production"
published_at: "2026-01-13T12:00:00+00:00"
first_seen_at: "2026-07-22T04:25:07.261888+00:00"
fetched_at: "2026-07-28T22:23:40.402133+00:00"
content_hash: "sha256:27db8c516f401795439972f548cf7eeb6e67de9460ef661ff1401ef6854c466e"
---

# Our AI Agent journey to 99.8% accuracy in production

The process of building LLM agents can be misleading. You take an extremely powerful underlying LLM model, wrap it in a few lines of code, throw in some function calls, and think you are ready to go.


The business opportunities and ideas are almost endless. You can build a customer support agent, a sales representative, a language tutor, or a local tour guide.


However, sooner rather than later, you'll realize the harsh reality: building LLM agents is extremely challenging. Mistakes in your new agent could potentially lead to catastrophic events in certain cases. Recently, Air Canada was held liable when it's chatbot promised a discount that wasn't available.


Here are a few problems that might sound familiar:


-


Hallucinations: Your shiny new Sales representative LLM agent can invent non-existing products and promotions in production. Ouch.


-


Inconsistency: Every LLM model has a statistical component; the test case that just worked can break on the next run.


-


Edge cases: The agent can behave perfectly in a lab environment, but a few real-world customer interactions might reveal how unstable the agent actually is.


-


Reproducibility: The bug you just experienced in production is not easily testable in your development environment.


-


Quality assurance: Does my agent actually work the way I expect?


## Dealing with the challenge


Here at Agentsforce we are building the first LLM agents platform, addressing the daily challenges of building a production grade agents at scale to serve our customers.


We had a clear mission from early on to focus our efforts on crafting a comprehensive suite of tools for enhancing agent quality.


## LLM Tests


Understanding begins with recognizing the fundamental distinction between a conventional unit test and an LLM test.


Unit tests test code, while LLM tests prompts.


LLM tests consist of two testing phases:


Validation phase - In this phase, we will usually run a few tests on the raw prompt to ensure it is concise and doesn't contain any contradictions or vagueness. As the prompt gets bigger, the risk of contradictions between different parts of the prompts is increasingly common.


Running phase - In this phase, we actually run the prompt against the model in an automated test. Usually, we would want to test critical paths, such as function calls, given a certain ticket scenario.


it is important to note that because the model may act randomly, it is important to run the same automated tests more then once for each release.


> LLM Tests can be complicated and require the right infrastructure


## CustomerGPT


A key component of Quality Assurance is making sure that we handle as many edge cases as possible. Searching for every edge case can be lengthy and daunting task. Luckily, there is a nice solution.


When LLM talks with LLM...


Manual tickets creation can be easily automated by utilizing CustomerGPT.


CustomerGPT talks with your agent as though it were a real customer! It effectively simulate 100 tickets in a minute!


## QA Agent


In Agentsforce, in addition to testing our agents in the development stage, we also wish to monitor our production environment to ensure that the agent doesn't make any mistakes.


The QA agent is a special type of LLM agent for testing existing tickets and measuring agent performance in real time.


By analyzing user sentiment, tone, and confusion, we are able to detect hard-to-find bugs through a pile of tens of thousands of tickets.


In addition, the QA agent accesses a vector database to compare similar tickets and detect anomalies and bugs in real-time.


## Analytics dashboard


QA agents provide valuable insights into individual problematic tickets. However, for higher-level insights: utilize an analytics dashboard.


Analyzing tens of thousands of tickets through a single dashboard can help answer various questions, such as:


-


What is my agent resolution rate?


-


What categories of tickets do I receive?


-


What is the total amount of refunds issued to customers this month?


-


How is my agent performing compared to human agents?


-


Where is the biggest drop-off in my sales tickets that I might tackle?


It's crucial to track agent performance at least monthly to identify and address areas for improvement.


## Turning theory into action


It's important to note that automated tests should not be viewed as a substitute for manual tests, and sophisticated monitoring tools should not replace testing during the development stage. These are all tools in your arsenal that you should utilize as needed.
