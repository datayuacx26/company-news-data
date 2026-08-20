---
schema_version: "1.0.0"
document_id: "e966d43c5a5b2647b26f69012e8850bc6a35134acef0999d23bb57a8f520df91"
company_key: "yc-nyckel"
company: "Nyckel"
source_id: "yc-nyckel-news-import-fbb975919637"
canonical_url: "https://www.nyckel.com/blog/linkedclient-intent-engine-classification/"
published_at: null
first_seen_at: "2026-07-22T06:43:30.013332+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:d053af8579a307280421f340beed922a7de06903db40eb15fab30281f9fd90ec"
---

# LinkedClient builds highly-accurate sales intent engine in just minutes

"Using machine learning for classification is a competitive edge for us. It’s more accurate and predictable than LLMs like ChatGPT."


Henric Otterberg


CTO, LinkedClient


LinkedClient used Nyckel to quickly build a feature that analyzes sales intent in emails and Linkedin messages. Built this model with as few as 10 training samples per class Model took ~30s to build and deploy Many millions of LinkedClient messages have gone through Nyckel Saved hundreds of thousands of dollars not having to hire an in-house ML expert


## About LinkedClient


[LinkedClient](https://linkedclient.com/) is the world’s first AI sales assistant for B2B sales teams. Unlike the traditional outbound process - cold calling, intense admin, responding to emails, etc. - LinkedClient automates it all, allowing sales teams to do more with less.


Their secret is Elsie, an AI assistant that will build prospect lists, reach out to those contacts, interact with them as though they were you, and then continue until a meeting is booked.


## The Challenge


LinkedClient relies on real-time sentiment analysis of LinkedIn messages and email responses. Knowing whether a prospect responded positively, negatively, or neutrally is key in determining how the AI should respond.


While many pretrained sentiment classifiers do exist, none were focused on their exact use case: responses to cold sales inquiries. For instance, LLMs had trouble with nuance replies like below:


Such mistakes could be devastating for LinkedClient. A model that didn’t classify intent correctly could lead to unhappy customers and high churn. It was imperative, then, for LinkedClient to build a custom, highly-accurate model that could classify email response intent.


But, at the same time, it didn’t really make sense for LinkedClient to build and host such a model themselves either.


## The Solution


LinkedClient turned to Nyckel to build this custom sentiment classifier in just minutes. Even with just 10-20 samples per class (included ‘interested’, ‘not interested’, and so on), they were able to create a production-ready model that was already more accurate than their LLM tests.


"We were very impressed with Nyckel’s API-based machine learning approach. It was simple to turn our idea into a reality."


Henric Otterberg


CTO, LinkedClient


And given that Nyckel models train and deploy within 30 seconds, LinkedClient was able to start integrating immediately. Within a few days they had connected Nyckel’s API to their backend.


Now, text from emails and Linkedin messages are routed through Nyckel, classified, and then tagged appropriately within LinkedClient. This data is then used by the AI assistant, Elsie, to determine how to respond in real-time.


LinkedClient has also taken advantage of Nyckel’s continuous improvement features. These tools allow LinkedClient to add more training samples over time without needing to manually redeploy. This has made it easy to fine-tune and iterate the model over time.


"We were blown away by how few samples it took to build an accurate model. It took us just 10-20 samples per class to build a production-ready model."


Henric Otterberg


CTO, LinkedClient


## The Results


By outsourcing this sentiment classification to Nyckel, LinkedClient has saved themselves hundreds of thousands of dollars in salaries and server costs. By releasing the feature in days instead of months, they also quickly delighted customers and improved their product.


There was also just a peace-of-mind letting a classification tool optimize their discriminative AI tasks, while they focused on building the best AI-powered outbound tool in the market.


"The alternative to Nyckel was hiring an ML-expert and spinning up our own model. We didn’t have the time or money for that. With Nyckel, we got everything we needed at a fraction of the cost and time."


Henric Otterberg


CTO, LinkedClient


Interested in exploring how Nyckel can support your business?[Try Nyckel for free](https://www.nyckel.com/contact) , orreach out to us with any questions.
