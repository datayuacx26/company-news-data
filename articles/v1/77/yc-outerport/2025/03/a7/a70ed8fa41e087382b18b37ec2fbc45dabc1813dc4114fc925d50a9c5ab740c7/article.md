---
schema_version: "1.0.0"
document_id: "a70ed8fa41e087382b18b37ec2fbc45dabc1813dc4114fc925d50a9c5ab740c7"
company_key: "yc-outerport"
company: "Outerport"
source_id: "yc-outerport-news-import-3c04a81f3c8c"
canonical_url: "https://www.outerport.com/blog/agentic-search"
published_at: "2025-03-27T00:00:00+00:00"
first_seen_at: "2026-07-22T07:54:25.111667+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:c3dabfc0698ae4983b0a1399c98fccda5f681e6ba5c79fbb5dd22c6343ed377d"
---

# How agentic search helps AI understand long documents

We’ve recently been working on an agentic search system that can read and understand complex, long documents like a researcher or lawyer would- iteratively reasoning through the document and citing evidences for claims. We've deployed this system in production for our customers, including a bank run by a Fortune 150 company to help employees make sense of complex internal policies and regulations.


Our agentic search system improves upon RAG and naive long context, allowing us to perform more accurate and faster retrieval. Under the hood, we use cool techniques like KV cache management, and wanted to share our learnings with the community.


## What's the problem with RAG?


Lots of people use vector RAG, but they also often suffer from accuracy issues in mission critical applications (like compliance or legal).


But first of all, what exactly is RAG?


RAG stands for "Retrieval-Augmented Generation", and it works by first retrieving information from a knowledge base before the LLM (or any AI model or agent) answers the question. In most cases, this retrieval system is implemented using a vector database. We will refer to this approach as *Vector RAG* .


Vector RAG works by embedding sentences or chunks into vectors, and using an embedding of the query to find relevant snippets. This has a couple of challenges:


- **Cannot handle indirect causations.** For example, the answer to the question: "What's the cause of rising death rates in Mississippi?"might be that “there is an increase in drunk driving, which leads to more car accidents, and rising death rates”. The embedding for “increase in drunk driving” is not necessarily similar to the embedding for “death rates in Mississippi”.
- **Embeddings are unreliable and hard to improve.** Even for queries that are related, the embeddings may not be in the same space due to lack of training data. While fine-tuning is possible, it’s hard to debug / improve the system if this happens.
- **There are lots of decision parameters.** The quality of RAG systems relies on a number of design decisions (window size, overlap, metadata, query translation, reranking, etc) which are all hard to tune. A common pain-point we hear is that it's hard to know how much of the inaccuracies stem from not knowing how to properly tune the system, or if the system is just fundamentally flawed.
- **Poor user experience.** Since the retrieval will return the top K results, it's hard to understand the logical ordering of the retrievals, and often times they will be random text blobs that are not human readable.


Perhaps most importantly, it is an inflexible system. Maybe what you want the system to do is a task like "find all section reference numbers for inventory management policies", which requires you to not only retrieve the relevant sections, but also understand that it needs section numbers which might not be present in the retrieved sections.


## Is Long-Context the Solution?


An alternative to Vector RAG is to feed the documents into the LLM as a prompt and ask the LLM to retrieve relevant sections.


Various benchmarks show that long context retrieval has higher accuracy than vector RAG. LLMs can holistically reason over the document and retrieve relevant information based on the broader context. But long context retrieval also has a number of challenges:


- **Computationally expensive.** The attention operation is O(n^2) in number of tokens, and pre-filling latency scales super linearly with respect to the context length. In practice, this means it takes 3 minutes to process 128k tokens when running a 70B model (quantized) on 2 A100 GPUs.
- **Accuracy decreases as context length increases.** The accuracy of long context retrieval starts to gradually decrease after 40k+ tokens. The drop-off is even more significant for reasoning models like OpenAI o1 and DeepSeek R1.
- **Hard limit on the context length.** Most popular models are limited to a context size of 128k. For example, 10K reports or internal procedure documents can easily exceed 200k+ tokens.
- **Hallucinations.** Since the LLM is generating the retrievals, it can make up quotes and fake references that don't exist.


In practice, both Vector RAG and long-context retrieval hallucinate, but just in different ways. Vector RAG hallucinates the answer because the retrievals are not relevant or missing information, while long-context retrieval hallucinates by making up retrievals or missing information in the long-context.


## Our Solution: Chain-of-Memory Retrieval


Unsatisfied with vector RAG or long-context retrieval, we had to develop a new technique, which we call **chain-of-memory retrieval** which uses an agentic search approach for retrieval.


This technique consists of *breaking down* the question & answering process into steps:


- **Planning:** An initial planning step reasons over user query to create a filter for later retrievals.
- **Hotswapping the KV Cache:** We preprocess the KV cache of chunks, and leverage KV cache offloading and hot-swapping to bypass pre-fill latency almost entirely.
- **Step-by-step Retrieval:** We automatically determine the semantic chunking lengths that maximize retrieval accuracy. We can employ this in either a parallel, iterative, or tree-based manner to control the throughput-accuracy curve. We also employ a smaller model specialized for retrieval in this step for maximum performance.
- **Generating the final answer:** A final reasoning step synthesizes the retrieved information into a final answer. Let’s dive in deeper what each of these entail.


We will explain each of these components in detail. One key benefit of decomposing into steps like above is that it allows us to use different models for different steps to both manage cost and improve accuracy.


There are also a few prerequisites, like OCR and vision models that extract tables and figures as well as a system to pick the relevant documents in the first place. We will not cover these in this blog post, and instead dive deep in them in future blog posts as they have a lot of nuances on their own.


**Disclaimer:** The prompts that we will write in this blog post are not the prompts we use in production. In practice, the prompts are much more complex, and the amount of work you put in writing prompts and running rigorous evaluations on them matters a lot for quality. Some of the technical details are also different in production (and honestly, constantly evolving as we improve quality further!)


### Planning


The initial planning step allows us to attempt to clarify the user query and enhance the retrieval step. This is similar to the *reasoning* that models like OpenAI o1 and DeepSeek R1 do. This planning is based only on the user query and a short summary of the relevant documents (and hence is low-cost).


We first have a prompt that can elaborate on the user query as follows:


As an example, this would have the following outputs for a given question:


This step is analogous to a human expert who first decides what they are looking in the documents before diving into them.


Note that this step has a short input context and output context (~500 tokens in total), making it relatively quick and cheap. Optionally, a human user can provide feedback on the planning step to refine the plan.


### Hotswapping the KV Cache


When LLMs process input tokens (the prompt, in a process known as pre-filling), it produces the *KV cache* .


The KV cache contains vectors that represents the semantic meaning of each token in reference to other tokens. The mathematics aside, they are analogous to the short term memory of a human - the cognitive context we develop after we read something.


The key observations are that:


-


As long as the prefix for the prompt doesn’t change, we can reuse the KV cache by saving the KV cache to either CPU or disk, and reload instead of recomputing it. This saves an enormous amount of redundant compute.


-


We often ask many questions against the same context (i.e. a financial report you are analyzing, or a company’s internal policy documents).


This means that we can preprocess the KV cache for the documents once and reuse them, cutting the time-to-first-token (TTFT) from 10s of seconds to 1-2 seconds. This is analogous to reading the documents once, and being able to bring back the short term memory at will instead of having to reread them again.


More concretely, we prompt the LLM with something like:


The reusable prefix section is saved as a KV cache, and is recycled for future questions. OpenAI and Anthropic both seem to leverage some sort of KV cache re-use under the hood, but they only retain the KV cache for 5-10 minutes and are[“always cleared after 1 hour”](https://openai.com/index/api-prompt-caching/) .


By using our own self-hosted open source models with our custom inference engine, we can keep the KV cache for an indefinite period of time. Self hosting on your infrastructure creates rooms for a completely different allocation of compute resources like CPU memory, compared to serverless APIs that don’t let you have control over this.


### Retrieval


Separating the reasoning (planning) step from the retrieval step allows us to use a smaller model for retrieval, which helps manage cost and latency because this is the step that is the most compute / token intensive but does *not* require a sophisticated model. As a matter of fact, we find that using advanced reasoning models for retrieval is actually detrimental to retrieval accuracy.


Based on benchmarks on our internal datasets, we find that retrieval accuracy for long context LLMs generally drop-off when the documents get long, and *especially so* for reasoning models like OpenAI o1, DeepSeek R1.


The graph also shows a tradeoff between retrieval accuracy and context length. While it *is* possible to simply dump the entire context (documents) to the LLM to find relevant passages, we found that this is not reliable for larger inputs.


This motivates chunking the documents into medium size contexts, so that retrieval rate remains high. This is generally still enough for the model to look at the content holistically without missing cross-chunk relations. In rare cases where cross-chunk relations *are* important, we can take multiple passes over the chunks.


There are a few ways to further make sure we don’t miss cross-chunk relations:


-


Overlapping windows between chunks


-


Using semantic boundaries (i.e breaking up by chapters)


-


Prompt the LLM to focus on retrieving relevant information from the source, and avoid making conclusions.


Once we chunk the documents, we can use several techniques to retrieve relevant information from the chunks.


#### Adding context and reasoning to the retrieval


A small technique that we find is very useful and lends itself to good user experience is to have the model output structured reasoning for each retrieval. This can also be a place to add more metadata about the retrieval, which can help answer questions like where the information came from, what chapter it is from, etc.


#### Getting rid of hallucinations


All this helps with not missing critical information, but it does not prevent the LLM from hallucinating and making up information that is not in the documents. It is also rather slow, as the LLM has to repeat the sentences from the source documents, spending precious output tokens.


As a result, we also retrieve sentences from the source documents in the same way computers efficiently store and read information: through addresses. Instead of repeating entire sentences, we retrieve IDs for sentences, figures, and tables (that are populated in preprocessing). This also gets rid of hallucinations entirely, since the model can only retrieve source material that actually exist in the source documents.


### Generating the final answer


Given the relevant information from each chunk, we can aggregate them to generate the final answer.


This part is simple. Crucially, however, we can use a powerful reasoning model without worrying too much about latency since the input context will have been summarized to be relatively short.


### Showcase


We show that our technique can indeed improve complex document Q&A accuracy.


We compare against a naive long-context retrieval method using OpenAI 4o. Note that we did not use o1, because we’ve found that its long context retrieval capability are weak. We run our technique with open source models that can be self hosted. We show that even while using a slightly weaker model, our technique ends up performing much better.


Using the 3M’s 10K report as the source document, we ask the following the question:


A naive way to handle this with long context LLM is to split the 200k tokens into chunks of 100k tokens, and ask the question. Then have another step that aggregates the answers.


We get the following answers from OpenAI 4o, depending how we prompt it.


In both cases the answers are wrong- prompt engineering does not save us. Our chain-of-memory technique, on the other hand, returns the correct answer.


And this successfully returns the correct answer!


## How do we get in on this?


If you're interested in using our agentic search platform, please reach out! There is a lot of small details that matter for quality, and we're happy to provide white-glove services to help you get the most out of it.


We offer our service as:


- A hosted SaaS / API service
- License to self-host the API service on-prem on VPC
- Customized deployments on a case-by-case basis


If interested, feel free toemail us or sign-up on our website.


## What's next?


We will also write blog posts on other topics, like OCR / vision models for document processing, document retrieval, how to integrate this with other services, and more. We're also working on some exciting new features that let you use this advanced retrieval technology to do things like cross-document comparisons and workflow mining- our goal is to make it easy to find usecases where this technology can be applied.


Follow us on[X](https://twitter.com/outerport) or on[LinkedIn](https://www.linkedin.com/company/outerport) to stay in touch with more to come! Or email us atinfo@outerport.com to get in touch.
