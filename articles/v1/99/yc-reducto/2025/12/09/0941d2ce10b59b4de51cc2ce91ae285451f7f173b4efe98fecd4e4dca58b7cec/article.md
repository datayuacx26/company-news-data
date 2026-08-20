---
schema_version: "1.0.0"
document_id: "0941d2ce10b59b4de51cc2ce91ae285451f7f173b4efe98fecd4e4dca58b7cec"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-chart-extraction"
published_at: "2025-12-03T12:00:00+00:00"
first_seen_at: "2026-07-22T11:10:28.155558+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:014ddbb6a6c157ee4cada0849afd109ddd3b33b2562cbd75ce0414a025f8b236"
---

# How Reducto is building SOTA chart extractions

*This write-up is part of Reducto’s Applied Research arm, working on the hardest problems in the document processing, OCR, and VLM space. If you want to solve problems like this,*[we’re always looking](https://reducto.ai/careers) *for incredible ML and engineering talent.*


*Engineering by Arth Bohra, Ray Wynne, Evan Vogelbaum. Written by Adel Wu.*


Observe these two charts:


Can you see any differences?


The left is the original chart image from a report. The right is plotted using data points that Reducto extracted from the original image.


Our ML research team developed a new approach to **chart extraction** (extracting numerical data from charts and graphs), built entirely in-house. The above demonstration is just one example of thousands of difficult sample tests, comparing our outputs to the original plots.


AtReducto , we’re focused on how to unlock the most value from unstructured documents for our users, and charts are an extremely prevalent figure type. It’s common across all industries and document types: financial reports, academic papers, and infographic presentations are just a few types of documents where charts in particular contain critical data that enrich the context that surrounds it.


Accurate chart extraction is a game-changer for our biggest customers in finance, banking, and beyond, where reliable document ingestion underpins all knowledge work. This new version of chart extraction is now available for use on our API as well as Studio, enabled with advanced chart extraction in your[agentic figure configurations](https://docs.reducto.ai/api-reference/parse#figureagentic) .


## Why are charts a “hard problem”?


[Eighty percent](https://www.box.com/resources/unstructured-data-paper) of enterprise knowledge is trapped in unstructured forms – PDFs, slide decks, long-form reports – many of which contain charts as a visual aid for data. When a company wants to ask a question across its huge corpus of dense documents to find a metric, it’s sometimes buried in visual information like charts and diagrams. An image can speak a thousand words, and charts are prevalent especially in data-rich sources like presentations that contain business-critical information.


Charts are one of the *most challenging* types of images to interpret because they lack standard formats, often use custom designs, and can represent extremely dense information. To handle them correctly, it’s critical to capture the data with numeric accuracy, preserve the relationships between data points, and interpret the overall meaning of the chart.


### Current work in chart understanding


Prior work has focused on Visual Question Answering (VQA)—models that take a chart image plus a natural-language question and return a short text answer (e.g., "What is the peak value?"). While VQA works well for targeted queries on individual charts, it processes each chart independently without retrieval, making it prohibitively slow for large document collections. More fundamentally, VQA doesn't expose the underlying data, struggles with dense multi-series charts, and offers limited auditability of results.


Multimodal-embedding pipelines (e.g.,[ColPali](https://arxiv.org/abs/2407.01449) -style RAG) address the scalability challenge through a two-stage architecture: they first retrieve relevant chart pages from large collections using vision-based embeddings, then generate answers from only the most relevant results. This retrieval-then-generation approach is ideal for semantic search across thousands of documents. We leverage this pattern and also support cropping charts for direct visual reasoning when appropriate.


Specific use cases demand techniques that recover fine-grained, numerically accurate data from real-world charts at enterprise scale – feeding downstream analytics/forecasting, auditing and lineage for compliance, aggregating metrics across thousands of charts, and generating accessible alt-text.


We use both patterns, but we’ve heard from enterprise customers, especially in extremely chart-heavy industries, who need structured chart data they can search, audit, and trust.


### Why do you need to represent the chart as text?


In most of today’s AI applications, the main method of communication is **text** . Large language models (LLMs) have matured earlier than multi-modal vision language models (VLMs), with many AI agent flows relying on techniques such as retrieval augmented generation (RAG) or context engineering to synthesize data to return to the user.


A common Reducto use case is large-scale **knowledge base construction** , where hundreds of thousands of documents are parsed and indexed for retrieval. Since these documents often mix text with figures, tables, and charts, relying solely on a VLM would constrain downstream reasoning—VLMs are slower, less scalable, and less mature than text-only LLMs.


Recent research has shown that document ingestion quality fundamentally determines RAG system performance. Studies like[OHRBench](https://arxiv.org/abs/2412.02592) and[ChatDOC](https://arxiv.org/abs/2401.12599) demonstrate that OCR errors, poor PDF parsing, and inadequate structure recognition create a 'quality ceiling' that limits downstream accuracy—even when using the most advanced LLMs for retrieval and generation.


By representing chart content as structured text, we enable LLMs to operate over the full corpus with higher throughput and broader reasoning capabilities.


**The problem is: How do we extract the most accurate data from a chart?**


## Breaking down Reducto’s chart extraction journey


First, we scoped the types of charts we wanted to focus on first like line, area, bar, and scatter plots–the most commonly seen types of graphs in documents. We aimed to return the parsed graph output in a tabular format with each data point.


We started with the most straightforward approach: prompting frontier models to extract chart data directly. While state-of-the-art models excel at many visual reasoning tasks, they fall short on numerical precision for this use-case. Models frequently misread axis intersections, missed data points, and generated errors obvious even to manual review.


The core issue was verifiability. We couldn’t be sure models would produce accurate results, but we also couldn’t sit for hours combing through tabular data. In order to make the process more verifiable, we decided to use a model to purely replot the graph for us, to use as a way to more easily check the values.


We decided to train a separate verifier model that would look at this replotted representation, and determine general accuracy. This became a core part of an **agentic loop** that alternated between the chart extraction model and our verifier model, until the outputs “looked correct”.


But visual verification had limits. The verifier caught major errors like wrong trends or misaligned series, but individual data points could still be off by 5-10%. We also faced a stopping problem: strict thresholds caused endless iterations over imperceptible differences, while loose thresholds let significant errors through. A line can have infinite points—at what granularity do you declare " *correct enough* "?


### Our final approach


Because of our goal of point-level accuracy, we trained multiple specialized lightweight models—each focused on detecting and interpreting a specific chart component: axes, tick marks, legends, data series, bars, and lines.


This lets us extract complete, high-resolution data while intelligently identifying which points correspond to major axis ticks. By default, we return only tick-aligned values to keep outputs clean and actionable, but we can provide a pixel-perfect extract for users who require additional granularity.


The system was designed for adaptability. When a large enterprise client encountered charts without Y-axes, they needed each bar's length expressed relative to the maximum. We easily modified the extraction to output normalized ratios, turning axis-less charts into usable comparative data.


## How does Reducto perform compared to current solutions?


Through our research, we’ve identified some of the top types of charts that are difficult for models (and humans!) to understand. To demonstrate, we ran* a couple of representative charts through GPT 5.1 Thinking, Opus 4, DeepSeek OCR, Gemini 3.0 Pro, LlamaParse, and Reducto**, and replotted the data to highlight the differences. The inputs were images of the graphs with the same prompt and markdown outputs, then replotted in a new context with only simple instructions on a color palette:


### 1/ Dense line charts with many small inflections


Charts that have many highly detailed line work often lack annotated data, which makes it hard for models to pick up details and infer the correct values.


The following comparison is using a line graph containing 8 soccer players and plotting their goals per game ratio over time. GPT 5.1's output looks good from afar until you check the details. Many data points and trends are simply wrong. Models struggled to understand the overlaps of the many lines as well.


### 2/ Stacked bar charts with overlapping lines


Bar charts are a common type where density can come from the pure number of categories that must be represented and labeled.


The following comparison is using a stacked bar chart with a line on top, representing a breakdown of Consumer Price Index (CPI) Inflation over a five-year period (from 2021 to 2025).


Many models simplified the number of bars (Sonnet), missed the negative values (LlamaParse/DeepSeek), or read the line graph component incorrectly (GPT 5.1/Gemini).


### 3/ Complex area & line charts


Some charts contain many different lines or bars differentiated only by color, and overlap a ton (making it hard to read).


The following comparison is using a line plus area graph displaying a bank’s liquidity demand and SDF holding.


As you can see, many models still approximate when it comes to small inflections, intersections, and dense data within complex charts. Incorrect data points could be the cause of a critical analysis or assertion mistake, leading to downstream consequences for the customer.


During our representation test, several notable patterns emerged across the models evaluated. GPT 5.1 Thinking/Pro regularly timed out after 20+ minutes during processing. Most models explicitly caveated their output data, acknowledging their own limitations, mentioning that values were "approximate" or describing their use of gaussian curves to interpolate between data points. Many outputs—though appearing accurate at first glance—revealed numerous data discrepancies upon closer examination.


## The future steps


While document parsing inevitably has a long tail of variations, our approach to chart extraction builds a solid foundation for future steer-ability with custom workflows. We’re fast following on things like additional latency improvements, rotation cases, and much more!


At Reducto, we’re experimenting with new ways of generating training data that set a clearer benchmark for what 100% accuracy means in practice. By creating verifiers, benchmarks, and training pipelines that define correctness more rigorously than ever before, we’re building production-grade reliability at scale.


[Contact our team](https://reducto.ai/contact) if you have a use case bottlenecked by difficult ingestion - we’d love to see how we can help. This is just the beginning, and we’re excited to share more as we continue to make messy, visual data AI-ready.


**Chart extraction was done using the prompt "Extract the data for all of the components of this chart. Be very careful about accuracy and try to capture as many data points as possible." While this prompt is not extensively optimized, testing shows that even sophisticated prompt engineering yields only marginal gains for complex chart extraction tasks.*


***Reducto chart extraction is out-of-the-box with* *advanced chart extraction enabled in your*[agentic figure configurations](https://docs.reducto.ai/api-reference/parse#figureagentic) *and does not require a prompt.*


---


*This work couldn’t have been possible without the hard work from our ML and engineering team. If these types of problems excite you, apply to join our team on*[our careers page](https://reducto.ai/careers) *.*


*If you want to try Reducto on your own documents, either visit*[Studio](https://studio.reducto.ai/) *to sign up for free, or*[request a demo](https://reducto.ai/contact) *.*
