---
schema_version: "1.0.0"
document_id: "2cc746c7157db66703452172240d9a146ef01967e766120a5d9031ab246b92d6"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/rag-benchmarks"
published_at: "2025-05-06T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:4e4a6e1b483b47699f96ce972d48b6e7813783a11b0df97593b7debbf0985e4e"
---

# 7 RAG benchmarks

Retrieval-Augmented Generation (RAG) is a popular technique for grounding the outputs of large language models (LLMs) in reliable, context-specific data. By pulling in relevant information from trusted data sources, RAG helps reduce[hallucinations](https://www.evidentlyai.com/blog/llm-hallucination-examples) , improve response accuracy, and enable source-backed and personalized answers.


RAG is already powering a wide range of[real-world systems](https://www.evidentlyai.com/blog/rag-examples) , from customer support bots to fraud investigation tools. As adoption grows, so does the need to evaluate these systems effectively.


In this blog, we highlight seven RAG benchmarks that help measure and compare how well different LLMs handle core RAG challenges like large context windows, grounded reasoning, and using retrieved evidence effectively.


> **Using RAG?** Check out this in-depth[guide on RAG evaluation](https://www.evidentlyai.com/llm-guide/rag-evaluation) .


**Want more examples of LLM benchmarks?** We put together[database](https://www.evidentlyai.com/llm-evaluation-benchmarks-datasets) of 250+ LLM benchmarks and datasets you can use to evaluate the performance of language models.


## NeedleInAHaystack (NIAH)


The[Needle-in-a-Haystack (NIAH)](https://github.com/gkamradt/LLMTest_NeedleInAHaystack) test, first proposed by[Greg Kamradt](https://gregkamradt.com/) , is a simple yet powerful method for testing the in-context retrieval ability of long context LLMs. It tests whether a model can successfully retrieve a small, planted piece of information (the “needle”) hidden in an extensive collection of irrelevant data (the “haystack”). The model is asked a question that can only be answered correctly by finding and using that specific information. Evaluation is straightforward: whether the model can retrieve it accurately. You can also iterate over various document depths, where the needle is placed, and context lengths to measure performance.


Initially, Paul Graham's essays were used as a “hay” where a random statement – “The best thing to do in San Francisco is to go to Dolores Park and eat a sandwich on a sunny day” – was placed to perform the test. However, you can use any data corpus as a long context window to run the test.


> **Video explainer:**[Needle In A Haystack](https://www.youtube.com/watch?v=KwRRuiCCdmc) , by Greg Kamradt **Test repo:**[Needle In A Haystack on GitHub](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)


*Needle In A Haystack test results for GPT-4 128K. Source:*[Needle In A Haystack GitHub repo](https://github.com/gkamradt/LLMTest_NeedleInAHaystack)


##### **\[fs-toc-omit\]200 LLM benchmarks**


> Want more examples of LLM benchmarks? We put together a[database](https://www.evidentlyai.com/llm-evaluation-benchmarks-datasets) of 200 LLM benchmarks and datasets you can use to evaluate the performance of language models.[Bookmark the list ⟶](https://www.evidentlyai.com/llm-evaluation-benchmarks-datasets)


## BeIR


[BeIR (Benchmarking Information Retrieval)](https://github.com/beir-cellar/beir) evaluates retrieval models across diverse datasets and tasks. It tests the ability of models to find relevant documents, focusing on zero-shot and domain-agnostic evaluation.


BeIR includes 18 datasets across 9 task types, including fact checking, duplicate detection, question answering, augment retrieval, and retrieval from forums. The benchmark also allows testing retrieval abilities across diverse domains, from generic ones like news or Wikipedia to highly specialized ones such as biomedical scientific publications. BeIR can evaluate various retrieval systems, such as dense and sparse retrievers, hybrid models, and re-ranking systems.


**Example questions:**


- Who plays for VA Tech?
- Are neurochemical drugs based on antidepressants?
- Which dissolves in water quickly: sugar, salt, methane, or carbon dioxide?


> **Paper:**[BeIR: A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models](https://arxiv.org/abs/2104.08663) **Dataset:**[BeIR dataset](https://huggingface.co/BeIR)


*Tasks and datasets in the BeIR benchmark. Source:*[BeIR: A Heterogeneous Benchmark for Zero-shot Evaluation of Information Retrieval Models](https://arxiv.org/abs/2104.08663)


## FRAMES


[FRAMES (Factuality, Retrieval, And reasoning MEasurement Set)](https://huggingface.co/datasets/google/frames-benchmark) offers a unified framework for assessing LLM performance in end-to-end RAG scenarios. It tests RAG systems across three dimensions: factuality, retrieval accuracy, and reasoning.


The dataset comprises over 800 test samples with challenging multi-hop questions that require the integration of information from 2-15 Wikipedia articles to answer. FRAMES questions also cover different reasoning types needed to answer the question, including numerical, tabular, and temporal reasoning, multiple constraints, and post-processing.


**Example questions:**


- "How many times faster is the second fastest bird in the Americas compared to the fastest human in the world? Round to the nearest integer."
- "I’m thinking of an airport near the intersection of US-52 and I-95. Can you remind me which one it is?"
- "Leonardo DiCaprio once won an Oscar for Best Actor. Who won the award for best costume design sixteen years earlier?"


> **Paper:**[Fact, Fetch, and Reason: A Unified Evaluation of Retrieval-Augmented Generation](https://arxiv.org/abs/2409.12941) **Dataset:**[FRAMES dataset](https://huggingface.co/datasets/google/frames-benchmark)


*Example question from FRAMES benchmark. Source:*[Fact, Fetch, and Reason: A Unified Evaluation of Retrieval-Augmented Generation](https://arxiv.org/abs/2409.12941)


## RAGTruth


[RAGTruth](https://github.com/ParticleMedia/RAGTruth) is a benchmark that helps to evaluate the extent of hallucination in RAG systems. It is tailored for analyzing word-level hallucinations and comprises 18,000 naturally generated responses from diverse LLMs using RAG. The benchmark distinguishes between four types of hallucinations:


- **Evident conflict** , where generated content contains clear factual errors, misspelled names, or incorrect numbers.
- **Subtle conflict** , where generated content diverges from the provided information, altering the contextual meaning.
- **Evident introduction of baseless information** that involves hypothetical, fabricated, or hallucinatory details.
- **Subtle introduction of baseless information** that involves subjective assumptions or sentiment.


RAGTruth can be used to assess both hallucination frequencies in different models and the effectiveness of hallucination detection methodologies.


> **Paper:**[RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models](https://arxiv.org/abs/2401.00396) **Dataset:**[RAGTruth dataset](https://huggingface.co/datasets/wandb/RAGTruth-processed)


*RAGTruth data gathering pipeline. Source:*[RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models](https://arxiv.org/abs/2401.00396)


## RULER


[RULER](https://github.com/NVIDIA/RULER) extends the original NeedleInAHaystack (NIAH) test by varying the number and types of "needles" (target information) within large contexts. It tests LLMs across four task categories: retrieval, multi-hop tracing, aggregation, and question answering.


RULER is a synthetic benchmark. It automatically generates evaluation examples based on input configurations of sequence length and task complexity.


> **Paper:**[RULER: What's the Real Context Size of Your Long-Context Language Models?](https://arxiv.org/abs/2404.06654) **Dataset:**[RULER dataset](https://github.com/NVIDIA/RULER)


*Task examples with flexible configurations in RULER. Source:*[RULER: What's the Real Context Size of Your Long-Context Language Models?](https://arxiv.org/abs/2404.06654)


## MMNeedle


[Multimodal Needle in a Haystack (MMNeedle)](https://github.com/Wang-ML-Lab/multimodal-needle-in-a-haystack) evaluates the long-context capabilities of Multimodal Large Language Models (MLLMs). It tests the ability of MLLMs to locate a target sub-image (the “needle”) within a set of images (the “haystack”) based on textual descriptions.


The benchmark covers diverse settings with varying context lengths and single and multiple needles. It includes 40,000 images, 560,000 captions, and 280,000 needle-haystack pairs. MMNeedle also employs image stitching to further increase the input context length.


> **Paper:**[Multimodal Needle in a Haystack: Benchmarking Long-Context Capability of Multimodal Large Language Models](https://arxiv.org/abs/2406.11230) **Dataset:**[MMNeedle dataset](https://github.com/Wang-ML-Lab/multimodal-needle-in-a-haystack)


*MMNeedle evaluation overview. Source:*[Multimodal Needle in a Haystack: Benchmarking Long-Context Capability of Multimodal Large Language Models](https://arxiv.org/abs/2406.11230)


## FEVER


[FEVER (Fact Extraction and VERification)](https://github.com/awslabs/fever) is a publicly available dataset for verification against textual sources. It is designed to test a system’s ability to fact-check claims using evidence from Wikipedia. It includes over 185,000 human-generated claims based on Wikipedia articles and labeled as Supported, Refuted, or Not Enough Info. A model is given a claim (e.g., “The Eiffel Tower is in Berlin”), and must then retrieve Wikipedia sentences relevant to this claim and determine the correct label.


The benchmark evaluates information retrieval, evidence selection, and reasoning capabilities. FEVER can be used to assess RAG pipelines and LLMs' ability to avoid hallucination and reason with evidence.


**Example claims:**


- "Henri Christophe is recognized for building a palace in Milot."
- "Lorelai Gilmore's father is named Robert."
- "Oliver Reed was a film actor."


> **Paper:**[FEVER: a large-scale dataset for Fact Extraction and VERification](https://arxiv.org/abs/1803.05355) **Dataset:**[FEVER dataset](https://fever.ai/dataset/fever.html)


*Example claim from the FEVER benchmark. Source:*[FEVER: a large-scale dataset for Fact Extraction and VERification](https://arxiv.org/abs/1803.05355) **


## Test your RAG system with Evidently


While benchmarks help compare models, your RAG system needs[custom evaluations](https://www.evidentlyai.com/blog/open-source-rag-evaluation-tool) on your own data to test it during development and production.


That’s why we built[Evidently](https://www.evidentlyai.com/) . Our open-source library, with over 25 million downloads, makes it easy to test and evaluate LLM-powered applications, from chatbots to RAG. It simplifies evaluation workflows, offering 100+ built-in checks and easy configuration of custom LLM judges for every use case.


We also provide[Evidently Cloud](https://www.evidentlyai.com/register) , a no-code workspace for teams to collaborate on AI quality, testing, and monitoring and run complex evaluation workflows.


Ready to test your RAG?[Sign up for free](https://www.evidentlyai.com/register) or[schedule a demo](https://www.evidentlyai.com/get-demo) to see Evidently Cloud in action. We're here to help you build with confidence!
