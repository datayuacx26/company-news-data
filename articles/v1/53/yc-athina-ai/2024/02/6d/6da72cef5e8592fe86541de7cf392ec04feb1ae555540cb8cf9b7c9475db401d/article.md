---
schema_version: "1.0.0"
document_id: "6da72cef5e8592fe86541de7cf392ec04feb1ae555540cb8cf9b7c9475db401d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/llm-evaluation-too-expensive-here-s-how-we-solve-this"
published_at: "2024-02-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:4662874643839f6b5fffe8943b84783f85247fed72292a34c34cf5b664b54ffa"
---

# LLM evaluation too expensive? Here's how we solve this.

Do not index


Original Paper


Blog URL


According to the latest and greatest research, the SoTA eval techniques


use LLMs as evaluators. It's a little meta, but it works better than anything else.


But sometimes, they are too expensive to run in production. Here's how Athina solves this.


####


How to run LLM-graded evals in production


- *Without blowing up your OpenAI budget.*


Here's how we help you solve this so you can still get model performance insights in production!


- **Sampling Percentage:** You can get similar insights by running evals on 100k inferences instead of 500k inferences. We have a configuration setting for this on Athina. Documentation.


- **Max Evals per month:** Set a maximum number of evaluations to run per month as a hard limit. This ensures your costs are always limited.


- **Filters:** When you configure Evals on Athina, you can set filters. These filters will ensure that evals ONLY run on inferences that match the filters. For example, you could set a filter to only run evals on inferences where:


- **user query** contains "refund"


- **language model** is gpt-4


- **prompt slug** is summarization/v3


- **Use a cheaper model:** Many evals will work great with GPT-3.5 (though some will not). That shaves the cost down to 1/10 of GPT-4 Turbo. If you want to run evals on even cheaper models (Llama, Mistral, etc), then reply to this email. We're working on it :)


####


Function Evals


We just shipped a whole new library of function evals


. These evals do NOT use LLMs, which means they are deterministic and FREE. Here are some examples of the function evals we've just shipped:


- **Contains \[All / Any / None\]:** Checks if response contains (or does not contain) some keywords.


- **Regex:** Checks if response contains a regex pattern


- **Contains Valid Link:** Checks that a link exists AND is valid (not a hallucinated link)


- **Contains JSON:** Checks if response contains JSON


- **Contains Email:** Checks if response contains JSON


- **Answer similarity:** Similarity between the response and expected_response.


- **API Call:** Bring your own eval. We'll hit your endpoint to run an eval.


You can set these evals up on our UI


or through our open-source SDK


.


View the documentation here


.


####


Want Early Access to Our Latest Features?


We're working on some exciting new things.


Hint: It rhymes with *shine-tuning.*


If you want early access, or want to see what we're doing, email me atshiv@athina.ai .


Cheers,


Shiv Sakhuja
