---
schema_version: "1.0.0"
document_id: "34e9dc41163318acbbaa48c72a48c44be2c03c5a7f5a0e25268220d3941c313d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/ragar-your-falsehood-radar-rag-augmented-reasoning-for-political-fact-checking-using-multimodal-large-language-models"
published_at: "2024-04-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:d9f84685f56dd18e2e822518b83379370689966946d04506f228bf5c363aaed8"
---

# RAGAR, Your Falsehood RADAR: RAG-Augmented Reasoning for Political Fact-Checking using Multimodal Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2404.12065](https://arxiv.org/abs/2404.12065)


Blog URL


[blog.athina.ai /ragar-y...e-models](https://blog.athina.ai/ragar-your-falsehood-radar-rag-augmented-reasoning-for-political-fact-checking-using-multimodal-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2404.12065](https://arxiv.org/abs/2404.12065)


**By:**[M. Abdul Khaliq](https://arxiv.org/search/cs?searchtype=author&query=Khaliq%2C%20M%20A) ,[P. Chang](https://arxiv.org/search/cs?searchtype=author&query=Chang%2C%20P) ,[M. Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20M) ,[B. Pflugfelder](https://arxiv.org/search/cs?searchtype=author&query=Pflugfelder%2C%20B) ,[F. Miletić](https://arxiv.org/search/cs?searchtype=author&query=Mileti%C4%87%2C%20F)


**Abstract:**


> The escalating challenge of misinformation, particularly in the context of political discourse, necessitates advanced solutions for fact-checking. We introduce innovative approaches to enhance the reliability and efficiency of multimodal fact-checking through the integration of Large Language Models (LLMs) with Retrieval-augmented Generation (RAG)- based advanced reasoning techniques. This work proposes two novel methodologies, Chain of RAG (CoRAG) and Tree of RAG (ToRAG). The approaches are designed to handle multimodal claims by reasoning the next questions that need to be answered based on previous evidence. Our approaches improve the accuracy of veracity predictions and the generation of explanations over the traditional fact-checking approach of sub-question generation with chain of thought veracity prediction. By employing multimodal LLMs adept at analyzing both text and images, this research advances the capability of automated systems in identifying and countering misinformation.


---


###


Summary Notes


##


Advanced AI in Combatting Political Misinformation: Exploring RAGAR


In the era of digital communication, the spread of misinformation, particularly in political discussions, poses a significant challenge. Social media's broad reach, while beneficial for access to information, also facilitates the rapid dissemination of inaccuracies.


Research indicates that on platforms like Twitter, false information circulates more quickly than factual content, making it difficult to keep the public well-informed.


###


**RAGAR: A New Frontier in Fact-Checking**


The RAGAR research introduces innovative methods to improve the precision of fact-checking tools.


By blending sophisticated reasoning with Large Language Models (LLMs), the Chain of RAG (CoRAG) and Tree of RAG (ToRAG) approaches offer effective strategies against misinformation.


These techniques are crucial for developing more advanced fact-checking processes, especially for evaluating complex multimodal claims.


###


**Background Studies**


####


**Enhancing Fact-Checking with Retrieval-Augmented Generation (RAG)**


- **Preventing Mistakes:** Uses external data to improve LLM responses, tackling the problem of inaccuracies in generated text.


- **Incorporating External Knowledge:** Examines how integrating external knowledge and automated feedback can boost LLM efficiency.


####


**Checking Multimodal Claims with LLMs**


- **Analyzing Text and Image Claims:** Uses the latest instruction-finetuned multimodal LLMs to check the alignment between text and images in claims.


###


**Methods Used**


####


**Chain of RAG (CoRAG)**


- **Asking Follow-up Questions:** Generates additional questions using RAG responses to gather more evidence, enhancing the accuracy of predictions.


####


**Tree of RAG (ToRAG)**


- **Exploring Different Perspectives:** Uses branching questions to explore various aspects of a claim, then narrows down the evidence to the most relevant.


###


**Data Source**


The study uses the MOCHEG dataset, which includes 21,184 multimodal claims from PolitiFact and Snopes, categorized into three simple labels: supported, refuted, and not enough information (NEI). This simplification aids in streamlining the fact-checking process.


###


**Fact-Checking Steps**


1. **Generating Multimodal Claims:** Uses GPT-4V to produce responses that cover both the textual and visual elements of claims.


1. **Finding Relevant Evidence:** Searches for text snippets and images relevant to the claims.


1. **Refining Reasoning with RAG:** Applies CoRAG and ToRAG methods to improve the accuracy of question-answering mechanisms for claim verification.


1. **Predicting and Explaining Veracity:** Assigns a veracity label (supported, refuted, or NEI) based on the evidence, with detailed explanations.


###


**Performance and Outcomes**


Testing focused on the accuracy of veracity predictions and the quality of explanations highlighted the superior performance of RAGAR methods compared to basic models. This confirms their effectiveness in improving fact-checking.


###


**Wrapping Up**


RAGAR introduces significant advancements in the field of fact-checking within political discussions. By harnessing LLMs enhanced with RAG techniques, it presents a promising avenue for tackling misinformation.


Despite its achievements, the study acknowledges certain limitations, like the inconsistency of search results, and suggests areas for future research.


As misinformation evolves, so must our strategies to counter it. RAGAR marks a substantial step forward, offering new tools for AI engineers to boost fact-checking accuracy and reliability.


Continuing to innovate is key to maintaining an informed public and supporting healthy political discourse.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
