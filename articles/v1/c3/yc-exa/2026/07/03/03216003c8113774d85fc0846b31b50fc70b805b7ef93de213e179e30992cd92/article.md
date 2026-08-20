---
schema_version: "1.0.0"
document_id: "03216003c8113774d85fc0846b31b50fc70b805b7ef93de213e179e30992cd92"
company_key: "yc-exa"
company: "Exa"
source_id: "yc-exa-news-import-e15579f9a79b"
canonical_url: "https://exa.ai/blog/publications-search"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T04:05:34.244049+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:ca2497f6a1d0ae0dcecd3d4c6521eb1c67d5fdba186906649ef5fda8c5046d19"
---

# SOTA Search Over Academic Publications

[The Exa Team](https://x.com/ExaAILabs) Jul 23, 2026


# SOTA Search Over Academic Publications


Today we're launching state of the art search over research papers and technical publications at Exa. We built a dedicated index of roughly 350 million publications and added around 30 million authors to our people index. Exa can now search across scientific literature using natural language, even when a query is vague, highly specific, or based on an imperfect memory of a paper.


Research paper search is available through the Exa API using the Publication category.


### Find papers semantically (the way people actually remember them)


Researchers do not always search using a paper's exact title. Sometimes they remember a specific result:


In a German study of 145 cosmetic and personal care samples, what percentage contained NDMA and what was the maximum concentration found?


Other times, they remember only the broad outline:


Wasn't there a paper from the mid 70s looking for a threshold below which dietary cholesterol wouldn't harm arteries in primates, and they never found one? I think HDL dropped and LDL rose even though total cholesterol looked normal.


Traditional academic search works well when you already know the title, author, or exact keywords. It is much less useful when all you have is a result, an experimental setup, or a half-remembered description.


Exa searches over the meaning of the query and the contents of the paper. This makes it possible to retrieve a specific publication from factual clues or an incomplete recollection, without requiring the user to translate the question into the right academic keywords.


### Evals


We created two new benchmarks reflecting common ways people search for research publications. The first tests known-item retrieval, where the searcher provides precise factual clues and the system must identify the paper they came from. The second tests tip-of-the-tongue retrieval, where the searcher describes a paper using vague, incomplete, or slightly incorrect details, closer to how someone recalls work encountered months or years ago.


Across this benchmark, Exa retrieved the correct paper for 82.8% of queries.


Searcher Recall MRR Mean latency


Exa 86.4% 0.726 0.578 ± 0.017 s


Perplexity 66.8% 0.568 1.277 ± 0.016 s


Parallel Advanced 50.0% 0.312 3.118 ± 0.082 s


Parallel Turbo 39.2% 0.278 0.403 ± 0.013 s


Google Scholar 28.0% 0.179 1.098 ± 0.053 s


Exa had the highest recall and MRR of the systems tested while returning results in under 400 milliseconds on average.


We are also developing an evaluation for topical completeness. Instead of finding one specific paper, it measures how completely a search system recovers the important body of work around a topic, such as:


Floquet engineering methods for topological phase simulation in ultracold atoms


Finding a single known paper is useful. Understanding a field requires finding the surrounding work too.


### How we built it


Research papers are unusually difficult documents to search. Important information may be buried in a long PDF, a table, an appendix, or a scanned document. The same paper may also appear at several URLs with different titles and metadata.


Our ingestion pipeline uses OCR and document-parsing models to turn research PDFs into high-quality searchable text. We combine this text with information about authors, institutions, publication histories, citations, and collaborators.


At query time, Exa searches both its web index and the dedicated publication index, then combines and reranks the results. This gives researchers the coverage of the open web with the structure of a purpose-built scientific index.


### Try it


You can use it through the Search API today:


` from exa_py import Exa exa = Exa() results = exa.search( "papers proposing alternatives to attention for long-sequence modeling", category="publication", num_results=10 )`


Try it in the[Exa dashboard](https://dashboard.exa.ai/) or read the[Search API documentation](https://exa.ai/docs/reference/search) .


---


#### Cheers,


#### [The Exa Team](https://x.com/ExaAILabs)


SEE MORE


[Introducing Exa Agent The Exa Team June 16, 2026](https://exa.ai/blog/exa-agent)[Exa raises $250M Series C to build the search engine for AIs Will Bryk May 20, 2026](https://exa.ai/blog/announcing-series-c)[Exa and Google Partnership The Exa Team April 28, 2026](https://exa.ai/blog/exa-google-cloud)
