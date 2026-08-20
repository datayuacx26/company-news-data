---
schema_version: "1.0.0"
document_id: "02161fa7664e81ef966d30d44ea2a5d8eefe31dae3acdeefdee32f5243b80f24"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-model-prompt-chaining-for-long-legal-document-classification"
published_at: "2023-08-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:bd804a446d7aabea78b316fa5ddee878509aac40a372c3374bf73ac530689afa"
---

# Large Language Model Prompt Chaining for Long Legal Document Classification

Do not index


Original Paper


[https://arxiv.org/abs/2308.04138](https://arxiv.org/abs/2308.04138)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2308.04138](https://arxiv.org/abs/2308.04138)


**By:**[Dietrich Trautmann](https://arxiv.org/search/cs?searchtype=author&query=Trautmann%2C%20D)


**Abstract:**


> Prompting is used to guide or steer a language model in generating an appropriate response that is consistent with the desired outcome. Chaining is a strategy used to decompose complex tasks into smaller, manageable components. In this study, we utilize prompt chaining for extensive legal document classification tasks, which present difficulties due to their intricate domain-specific language and considerable length. Our approach begins with the creation of a concise summary of the original document, followed by a semantic search for related exemplar texts and their corresponding annotations from a training corpus. Finally, we prompt for a label - based on the task - to assign, by leveraging the in-context learning from the few-shot prompt. We demonstrate that through prompt chaining, we can not only enhance the performance over zero-shot, but also surpass the micro-F1 score achieved by larger models, such as ChatGPT zero-shot, using smaller models.


---


###


Summary Notes


##


Simplifying Legal Document Classification with Large Language Model Prompt Chaining


In the complex world of legal document classification, AI engineers are on a constant quest for innovative methods to sift through and categorize dense, domain-specific texts efficiently. One promising strategy that's gaining traction is large language model (LLM) prompt chaining.


This blog post dives into how prompt chaining can be practically applied to classify lengthy legal documents, providing insights and actionable advice for AI professionals in corporate settings.


###


Introduction


####


The Challenge with Legal Documents


Legal documents are notoriously difficult to classify due to their:


- Lengthy nature


- Specialized terminology


- Subtle legal reasoning


Traditional methods often involve labor-intensive annotation and customization of pre-trained language models (PLMs), which can be resource-consuming and sometimes impractical.


####


How Prompt Chaining Offers a Solution


Prompt chaining stands out as an efficient alternative by tapping into LLMs' ability to comprehend complex texts via a sequence of well-thought-out prompts.


This method breaks down the classification task into smaller, more manageable steps, allowing for more precise classification without extensive model tweaking.


###


Background


####


Progress in the Field


The blend of prompt engineering and legal natural language processing (NLP) is gaining momentum, thanks to tools like OpenPrompt and PromptSource that provide standardized frameworks.


These innovations underscore prompt chaining's potential in tackling the distinctive challenges of legal document classification.


###


Data Usage


We focus on datasets from the European Court of Human Rights (ECHR) and the U.S. Supreme Court (SCOTUS), employing binary classification for ECHR and diving into 14 specific issues for SCOTUS. These datasets serve as a solid basis for testing prompt chaining's effectiveness in legal contexts.


###


Models Employed


For text generation, we use two LLMs with 20 billion parameters each, coupled with specialized summarization models such as BRIO and PRIMERA to produce brief document summaries.


We also utilize semantic similarity search to craft effective few-shot prompts, honing the model's focus on the text's most relevant parts.


###


Prompt Chaining Explained


####


Step-by-Step


Prompt chaining includes key steps like:


- **Creating a Document Summary** : Condensing the document to highlight its main themes.


- **Formulating a Few-Shot Prompt** : Using similar summaries to design prompts that direct the model.


- **Classifying with a Generated Label** : Employing the model's response to classify the document.


This approach offers adaptability, allowing AI engineers to tailor the process to their specific needs.


###


Experiments and Outcomes


We tested prompt chaining's impact on legal document classification. The approach significantly improved performance over zero-shot models, especially with the ECHR dataset.


The SCOTUS dataset showed varying effectiveness across different issues.


###


Conclusion


Prompt chaining presents a promising avenue for more effective and efficient legal document classification.


Looking ahead, exploring larger models and additional benchmarks will further validate this method's value in minimizing the need for extensive annotations and model customizations.


###


Implementation Tips


- **Invest in Summaries** : Focus on developing or choosing top-notch summarization models to ensure summaries contain critical details.


- **Refine Your Prompts** : The success of prompt chaining depends on prompt quality. Experiment with various formulations to discover the most effective ones.


- **Iterative Refinement** : Leverage prompt chaining's flexibility to fine-tune your approach, from summary creation to prompt design and label generation.


- **Keep a Close Eye on Performance** : Monitor classification results and confusion matrices to pinpoint improvement areas and tweak your strategy as needed.


Prompt chaining is a cutting-edge strategy for classifying lengthy legal documents, embodying the innovation and agility essential for the future of AI in the legal sector.


For AI engineers at enterprise companies, mastering this technique could pave the way to unprecedented efficiency and accuracy in legal document processing, redefining the standards for legal AI applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
