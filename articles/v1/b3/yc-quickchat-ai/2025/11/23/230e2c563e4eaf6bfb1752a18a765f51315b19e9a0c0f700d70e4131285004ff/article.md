---
schema_version: "1.0.0"
document_id: "230e2c563e4eaf6bfb1752a18a765f51315b19e9a0c0f700d70e4131285004ff"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/nano-banana-pro-whiteboard-to-research-paper"
published_at: "2025-11-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:25:10.100738+00:00"
content_hash: "sha256:07f2f812afb530e0e084f16008ef069a67e4c10631f5cba86d273f043a9f2654"
---

# People test Nano Banana Pro with "PDF research paper to whiteboard". I did the exact opposite.

## Nano Banana Pro


[Nano Banana Pro](https://deepmind.google/models/gemini-image/pro/) came out[two days ago](https://blog.google/technology/ai/nano-banana-pro/) . Social media is[full](https://x.com/crystalsssup/status/1991773702770552973)[of](https://x.com/skirano/status/1991527921316773931)[examples](https://x.com/tobi/status/1991706720750694601) of turning long research papers into mind-blowing images. Just for fun, I wanted to try **the exact opposite** .


## Whiteboard to PDF research paper


[A while ago](https://www.linkedin.com/posts/piotrgrudzien_if-the-whiteboard-in-your-office-doesnt-activity-7389625251631906816-7FQF) I took this photo of our office whiteboard at the end of a discussion session:


I asked Nano Banana Pro for a **PDF research paper** whose analysis produced the content on the whiteboard.


**Here is what it produced:**


It’s fun to observe how the model tried hard to incorporate all the equations and abbreviations into the content. The paper itself is relatively coherent, although very high-level and blurry on details. Admittedly, the whiteboard is very much so as well.


## Step by step instructions


Here are the exact steps I followed:


1. Go to Nano Banana Pro at[deepmind.google/models/gemini-image/pro](https://deepmind.google/models/gemini-image/pro/) and click on **Try in Gemini**
2. Upload the whiteboard image and use the following prompt:


```text
A team of researchers read a long PDF research paper and spent several hours analysing its content and the implications of the research presented. As a result of their discussion that's what the whiteboard they used look like. Generate the PDF research paper that they most likely used in their discussion.
```


1. If the paper hasn’t been generated in LaTeX format, ask for that in a follow-up message.
2. Use a tool like[overleaf.com](https://www.overleaf.com/) to turn LaTeX into PDF


---


## LaTeX code


The exact LaTeX code produced by Nano Banana:


```text
\documentclass  [11pt, a4paper]{  article  }
\usepackage  [utf8]{  inputenc  }
\usepackage  [T1]{  fontenc  }
\usepackage  {  amsmath, amssymb, amsfonts  }
\usepackage  {  graphicx  }
\usepackage  {  geometry  }
\usepackage  {  hyperref  }


% Basic page setup
\geometry  {
a4paper,
total={170mm,257mm},
left=25mm,
top=25mm,
}


% Title Information
\title  {  \textbf  {  A Unified Framework for Semantic Retrieval and Multi-Stage Ranking using Knowledge-Enhanced Context Chunks (ScCh)  }}
\author  {The Research Team}
\date  {October 26, 2023}


\begin  {document}


\maketitle


\begin  {abstract}
Traditional information retrieval systems often struggle to reconcile diverse retrieval signals, such as dense semantic vectors and structured knowledge graph paths, into a coherent ranking. In this paper, we propose a novel, unified framework that integrates multi-source retrieval with a powerful, knowledge-enhanced re-ranking pipeline. Central to our approach is the introduction of the "Semantic Context Chunk" (ScCh), a flexible data structure that encapsulates retrieved items with their associated semantic scores and contextual subgraphs. Our system employs a "Mapper" to unify diverse retrieval outputs into ScChs, a "Spreader" to augment these chunks with related knowledge from a large-scale knowledge hypergraph (Large Hk), and a "Rescorer" that leverages this enriched context for improved ranking. We introduce a "Reducer" and "Aggregator" to synthesize the processed ScChs into a final result set. Empirical evaluation demonstrates that our framework significantly outperforms strong baselines, particularly in complex queries requiring multi-hop reasoning and semantic disambiguation.
\end  {abstract}


\section  {  Introduction  }
The evolution of search has moved beyond simple keyword matching towards semantic understanding. However, integrating state-of-the-art dense retrieval methods with structured knowledge bases remains a significant challenge. The "vocabulary mismatch" problem is now a "context mismatch" problem, where the semantic nuance of a query is lost during the retrieval and ranking phases.


To address this, we present a unified retrieval and ranking framework designed around a new primitive: the Semantic Context Chunk (  \textbf  {  ScCh  }). An ScCh is not merely a document ID and a score; it is a rich representation containing a score vector (  $  S  $  ), a context embedding (  $  C  $  ), and a set of associated attributes or knowledge graph nodes (  $  A  $  ). Our framework processes these ScChs through a novel pipeline of Mapping, Spreading, Rescoring, and Aggregation, allowing for a more holistic and knowledge-aware ranking process.


\section  {  The Semantic Context Chunk (ScCh) Data Structure  }
The core of our system is the   $  ScCh  $  , which we define formally as a tuple representing the data state at any given point in the pipeline:


\begin  {equation}
ScCh = (S, C, \{A_1, A_2, \dots, A_n\})
\end  {equation}


Where:
\begin  {itemize}
\item   $  S  $   is a vector of ranking scores from different stages (e.g., initial retrieval score, semantic similarity score).
\item   $  C  $   is a dense vector representing the semantic context of the retrieved item.
\item   $  \{A_i\}  $   is a set of attributes or related entities derived from a knowledge base, which we refer to as the "Large Hk".
\end  {itemize}


This structure, denoted as   $  ScCh[]  $   on our architectural diagrams, allows context to be carried and modified throughout the entire ranking pipeline, solving the problem of information loss between stages.


\section  {  System Architecture  }
Our proposed framework consists of five main components, executed in sequence as derived from our whiteboard architecture session.


\subsection  {  Multi-Source Retrieval ("Retrive A")  }
The process begins with a retrieval stage that queries multiple sources in parallel. As shown in our system overview, these sources include:
\begin  {itemize}
\item   \textbf  {  Semantic Retrieval:  } Utilizes dense vector embeddings (e.g., from a BERT-based model) to find semantically similar items.
\item   \textbf  {  Large Hk Retrieval:  } Queries a large-scale knowledge hypergraph to find entities and paths related to the query terms.
\end  {itemize}


\subsection  {  The Mapper  }
The outputs of the diverse retrieval sources are heterogeneous. The "Mapper" component is responsible for unifying these diverse signals. It takes the raw retrieved items and their initial scores and transforms them into the standardized   $  ScCh  $   format. This involves:
\begin  {itemize}
\item   \textbf  {  Normalization:  } Converting different score distributions into a common scale.
\item   \textbf  {  Context Initialization:  } Generating an initial context vector   $  C  $   for each item.
\item   \textbf  {  Handling Defaults:  } Applying default values for missing data points, as indicated by the "Default..." logic in our system design.
\end  {itemize}


The mapping process can be represented formally as:
$$   \text{Mapper}: (\text{raw\_item}, \text{raw\_score}) \rightarrow ScCh_{\text{initial}}   $$


\subsection  {  The Spreader (Knowledge Expansion)  }
The "Spreader" is a key innovation of our framework (highlighted in red in our diagrams). Its purpose is to enrich the initial   $  ScCh  $  s by "spreading" their context into the knowledge graph. For each   $  ScCh  $  , the Spreader queries the "Large Hk" to find related attributes and entities (  $  A_i  $  ) that can provide additional context.


This process is illustrated by the following transformation, where a single chunk generates multiple, enriched variations:


\begin  {equation}
\text{Spreader}: (S, C) \rightarrow \{ (S, C, A_1), (S, C, A_2), \dots, (S, C, A_n) \}
\end  {equation}


This step effectively expands the search space around each retrieved item, bringing in crucial information that was not present in the original document or query.


\subsection  {  The Rescorer  }
Once the   $  ScCh  $  s have been expanded, they are passed to the "Rescorer." This component re-evaluates the ranking scores based on the newly acquired context (  $  C  $  ) and attributes (  $  \{A_i\}  $  ). The rescoring function is a learnable model that takes an expanded   $  ScCh  $   and outputs a new, refined score vector   $  S'  $  .


$$   \text{Rescorer}: (S, C, \{A_i\}) \rightarrow (S', C')   $$


As noted in our design discussions (the green section of the architecture), the Rescorer can apply different strategies, such as "Boost" or "Weighted" adjustments, depending on the nature of the query and the reliability of the knowledge graph paths.


\subsection  {  Aggregation and Reduction ("Aggr", "Reducer")  }
The final stage involves combining the rescored and potentially redundant   $  ScCh  $  s.


\textbf  {  The Reducer:  } Merges multiple expanded   $  ScCh  $  s that correspond to the same underlying item. It applies an aggregation function (e.g., max, average, or a learned weighted sum) to their scores.


$$   \text{Reducer}: \{ (S_1, C), \dots, (S_n, C) \} \rightarrow (S_{\text{final}}, C)   $$


\textbf  {  The Aggregator ("Aggr"):  } Performs the final selection and ranking of the reduced   $  ScCh  $  s to produce the final result list presented to the user.


\section  {  Conclusion  }
We have presented a unified framework for retrieval and ranking that successfully integrates semantic signals with structured knowledge. By introducing the Semantic Context Chunk (  $  ScCh  $  ) and the associated Mapper-Spreader-Rescorer-Reducer pipeline, we provide a robust and scalable solution for next-generation search systems.


\end  {document}
```


## Bonus


Here is the *“PDF to whiteboard”*[I generated](https://www.linkedin.com/posts/piotrgrudzien_people-test-nano-banana-pro-with-100-page-activity-7398096579515006977-NISd) for Immanuel Kant’s[710-page Critique of Pure Reason](https://ia600809.us.archive.org/32/items/immanuelkantscri032379mbp/immanuelkantscri032379mbp.pdf) :
