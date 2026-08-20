---
schema_version: "1.0.0"
document_id: "e59d4ba3cbb250d8ca6019ee2b26022d098a176c4edfa3933f71107d8b3ffc62"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/prompting-best-practices-for-instruction-following-rerankers/"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:0327fb4ba70d3cdbf5deb2a9631085ce4e1bc2f12f7fe3a6695bd4ff17aa0170"
---

# Prompting Best Practices For Instruction-Following Rerankers

TL;DR


This guide focuses on a newer capability: **[instruction-following rerankers](https://www.zeroentropy.dev/concepts/instruction-following-reranker/)** . These models accept natural language instructions that guide the ranking process, letting you encode domain expertise, business logic, and user preferences directly into your retrieval pipeline, without retraining models or building complex rule engines.


We’ll cover what makes instruction-following rerankers different, their main use cases, failure modes to avoid, and how to write effective prompts for[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) .


[Rerankers](https://www.zeroentropy.dev/concepts/reranker/) are critical components in production retrieval systems. They refine initial search results by re-scoring query-document pairs, dramatically improving[precision](https://www.zeroentropy.dev/concepts/precision/) without sacrificing[recall](https://www.zeroentropy.dev/concepts/recall-at-k/) . For a deep dive into how rerankers work and when to use them, check out our[introduction to rerankers](https://www.zeroentropy.dev/articles/what-is-a-reranker-and-do-i-need-one) .


## What is an Instruction-Following Reranker?


Traditional rerankers score documents based purely on query-document similarity. They’re excellent at surface-level and[semantic matching](https://www.zeroentropy.dev/concepts/semantic-search/) , but they can’t adapt to context-specific requirements.


Instruction-following rerankers change this. They accept standing instructions alongside your queries: think of these as persistent configuration that shapes how all queries get ranked for a particular use case or customer.


### How This Differs from Traditional Reranking


With[zerank-2,](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) you can now append specific instructions, lists of abbreviations, business context, or user-specific memories to influence how results get reranked.


Here is an example below.


```text
<query> "Candidates with IMO experience" </query>
<instruction> We're looking for engineering talent for a marine logistics company. </instructions>


Document: "Candidate experience: Worked at the International Marine Organization"
ZeroEntropy zerank-2: 0.3304
ZeroEntropy zerank-2 with instructions: 0.6421
```


The instructions provide persistent context about your domain, priorities, and constraints that inform every ranking decision.


## Main Use Cases Of Instruction-Following Rerankers


Use Cases


- **Domain-specific retrieval** : Encode industry expertise into ranking. A legal tech application might use: “Prioritize statutes and regulations over case law. Documents from .gov sources rank higher than legal blogs or forums.”
- **Multi-tenant systems** : Different customers need different ranking behavior. A document that’s highly relevant for one customer may be noise for another. Instructions let you customize without maintaining separate models.
- **Source and format prioritization** : “Prioritize structured data and tables over prose. Official documentation ranks higher than Slack messages or email threads.” This is especially valuable when your corpus mixes authoritative and informal sources.
- **Temporal and recency preferences** : “Strongly prefer documents from 2024 onwards. Our architecture changed completely in January 2024, making older documentation obsolete.” Unlike hard date filters, this allows flexibility while setting clear preferences.
- **Disambiguation through business context** : Many terms are polysemic. “Apple” means different things for grocery marketplace versus a quant fund. “Jaguar” could be an animal, a car brand, or a legacy software system. Instructions clarify which interpretation matters.


## How to Prompt zerank-2


[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) accepts instructions in flexible natural language. Use whatever formatting makes your[prompts](https://www.zeroentropy.dev/concepts/prompt-engineering/) maintainable—XML tags, markdown, plain text all work. The model is trained to understand various formatting conventions.


Basic structure:


```text
<  query  >Your search query</  query  >
<  instruction  >Your natural language instructions</  instruction  >
```


Or more simply:


```text
Query  : Your search query
Instructions  : Your natural language instructions
```


### Two Types of Instructions


#### Meta Instructions: How to Assess Results


Meta instructions define evaluation criteria: what features matter, what to prioritize, and how to weight different aspects.


**Format and source prioritization:**


```text
Prioritize tables, spreadsheets, and structured data over narrative text.
Official bug tracker issues rank higher than Slack discussions or email threads about the same topic.
PDF reports rank higher than slide decks or meeting notes.
Prioritize contracts from a given jurisdiction.
```


**Temporal preferences:**


```text
Strongly prefer documents from 2024 onwards.
Treat documents from 2023 as secondary sources unless they're exceptionally relevant.
Ignore anything before 2022.
```


**Emphasis on query components:**


```text
When queries mention compliance or regulatory requirements, those constraints
are mandatory. Documents that don't address compliance should rank significantly lower.
```


#### Business Instructions: Domain Context


Business instructions provide the context for *what you’re trying to achieve* , industry knowledge, company-specific terminology, user preferences, and domain expertise.


**Industry context:**


```text
We're a B2B SaaS company selling to enterprise healthcare organizations.
Our customers care about HIPAA compliance, SOC2 certification, and integration
with Epic and Cerner EHR systems. Security and compliance are top priorities.
```


**Terminology disambiguation:**


```text
We're a maritime logistics company. Generally:
-   "Container" refers to shipping containers, not Docker containers
-   "Port" means seaport, not network port
-   "IMO" is the International Maritime Organization, not International Math Olympiad
```


**Company-specific context:**


```text
We're an AI Copilot for VC and PE firms. Our users call the reranker
to gather information about their portfolio, or about industry trends.
Your goal is to find relevant, and helpful context to generate broad reports
given a user query. It is okay to include adjacent context to bring more
depth to the analysis, rather than simply answering the question verbatim.
```


**User preferences and constraints:**


```text
Our customers are non-technical business users who need simple, visual explanations.
Avoid documents with code samples or technical implementation details.
Prioritize documents with clear natural language explanations.
```


**Contextual relevance:**


```text
Include documents that provide important business context even if they don't
directly answer the technical question.
```


### Combining Both Types


The most effective instructions combine meta and business guidance:


```text
instructions   =   """
[BUSINESS CONTEXT]
We're a fintech company offering business credit cards to startups and SMBs.
Our main differentiators are: spend management tools, real-time expense tracking,
and integration with accounting software like QuickBooks and Xero.


Our customers are typically founders, finance managers, and controllers at
companies with 10-500 employees. They care about expense visibility,
accounting integration, and rewards optimized for business spending.


[RANKING PRIORITIES]
Prioritize documents from our Product and Compliance teams over Sales and Marketing.
Recent documentation (2024+) ranks much higher than older material.
Technical integration guides rank higher than general feature overviews.
Documents that mention accounting software integration are highly relevant.


[DOCUMENT TYPES]
Structured data (API docs, integration guides, compliance certifications) >
narrative content (blog posts, case studies) >
informal sources (Slack, email, meeting notes)
"""
```


### Per-Customer Customization with Templates


In multi-tenant systems, you can create base instructions and customize them per customer using[prompt templates](https://www.zeroentropy.dev/concepts/prompt-template/) :


```text
base_instructions   =   """
We are an AI-powered research tool for VC and PE firms.
We help investors analyze companies, markets, and investment opportunities.


Prioritize:
- Financial data, tables, and metrics over qualitative assessments
- Primary sources (10-Ks, earnings calls, company blogs) over news aggregators
- Recent information (last 2 years) for market analysis
- Historical data for company trajectory analysis
"""


# Per-customer memory and preferences
customer_context   =   """
This customer focuses on:   {investment_thesis}
Sectors of interest:   {sector_focus}
"""


# Combine at runtime
full_instructions   =   base_instructions   +   "  \n\n  [CUSTOMER CONTEXT]  \n  "   +   customer_context.format(
investment_thesis  =  user_memory[  'investment_thesis'  ],
sector_focus  =  user_memory[  'sectors'  ]
)
```


This approach lets you maintain consistent base logic while adapting to individual customer needs, preferences, and knowledge.


### Failure Modes


## Impact of Instructions: Concrete Examples


### Example 1: Healthcare Document Retrieval


**Query:** “patient consent requirements”


**Without instructions:**


```text
Top result: General article about medical ethics and informed consent principles
Score: 0.72
```


**With instructions:**


```text
We're building EMR software for US hospitals.
Prioritize HIPAA regulatory documents and state-specific healthcare laws.
Medical journal articles about ethics rank lower than legal/regulatory sources.
```


```text
Top result: HHS guidance on HIPAA consent requirements for electronic health records
Score: 0.89
```


The instruction shifts focus from general medical ethics to the specific regulatory framework that software builders need.


### Example 2: Technical Documentation Search


**Query:** “how to handle rate limiting”


**Without instructions:**


```text
Top result: Blog post about implementing rate limiting in Express.js
Score: 0.68
```


**With instructions:**


```text
We're building a Python-based data pipeline that calls third-party APIs.
Our stack: Python 3.11, asyncio, aiohttp.
Prioritize Python examples and libraries over other languages.
Prefer async/await patterns over synchronous code.
```


```text
Top result: Documentation for aiohttp-retry library with async rate limiting patterns
Score: 0.85
```


### Example 3: Candidate Search with Domain Context


**Query:** “engineers with cloud experience”


**Without instructions:**


```text
Top result: Candidate with AWS certifications and experience deploying web apps
Score: 0.70
```


**With instructions:**


```text
We're a data infrastructure company building analytics platforms.
"Cloud experience" means: data warehouse optimization (Snowflake, BigQuery),
large-scale ETL pipelines, and infrastructure-as-code (Terraform).
Web development and application deployment are less relevant.
```


```text
Top result: Candidate with 4 years optimizing Snowflake warehouses and building
Airflow DAGs for petabyte-scale data processing
Score: 0.88
```


Same query, completely different interpretation based on what “cloud experience” means in your context.


### Example 4: Multi-Source Enterprise Search


**Query:** “Q3 revenue targets”


**Without instructions:**


```text
1.   Slack message: "anyone know if we hit Q3 targets?" (Score: 0.65)
2.   Email thread discussing Q3 planning (Score: 0.63)
3.   Board deck with actual Q3 targets (Score: 0.61)
```


**With instructions:**


```text
Prioritize official financial documents (board decks, finance team spreadsheets,
approved budgets) over informal communication (Slack, email).
Tables and structured data rank higher than narrative summaries.
Documents from the Finance team rank higher than other departments.
```


```text
1.   Board deck with Q3 revenue targets table (Score: 0.87)
2.   Finance team spreadsheet with regional breakdown (Score: 0.84)
3.   Email thread discussing Q3 planning (Score: 0.58)
```


## Conclusion


Instruction-following rerankers are a paradigm shift in how we build retrieval systems. Instead of building separate pipelines for different domains or customers, you can use a single reranker with different standing instructions — useful inside[RAG](https://www.zeroentropy.dev/concepts/rag/) pipelines and[AI agents](https://www.zeroentropy.dev/concepts/agent/) alike.


zerank-2’s native instruction-following turns reranking from a static similarity computation into a context-aware, domain-informed ranking process. Whether you’re building enterprise search, AI agents, or vertical SaaS applications, instruction-following rerankers should be a core component of your stack.
