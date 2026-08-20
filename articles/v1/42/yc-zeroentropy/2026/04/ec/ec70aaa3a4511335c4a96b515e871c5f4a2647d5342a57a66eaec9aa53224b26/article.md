---
schema_version: "1.0.0"
document_id: "ec70aaa3a4511335c4a96b515e871c5f4a2647d5342a57a66eaec9aa53224b26"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/auto-optimize-embedding-models-in-agentic-workflows/"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:15:54.285032+00:00"
content_hash: "sha256:9066500c42ecc2526a93fe73d473a8949fd7c69aeb1422d272f0ff5230be148c"
---

# AutoOptimize: Why Your Embedding Model Is the Bottleneck in Agentic AI

TL;DR


- We open-sourced **[AutoOptimize](https://github.com/zeroentropy-ai/auto-optimize)** , a live arena where 9 AI[agents](https://www.zeroentropy.dev/concepts/agent/) race to approximate a mystery function — each team searching 1,468 ArXiv papers with a different[embedding](https://www.zeroentropy.dev/concepts/embedding/) model
- Same LLM, same corpus, same tools, same iteration budget — the **only variable is the embedding model**
- The zembed-1 team finishes with a best score of **** vs **** (OpenAI) and **** (Cohere) — a gap that grows, not shrinks, over 64 iterations
- In agentic workflows, **embedding quality compounds** : better retrieval surfaces better techniques, which unlock better solutions, which get refined further — each iteration amplifies the gap
- Clone the repo, add your API keys, watch the race:[github.com/zeroentropy-ai/auto-optimize](https://github.com/zeroentropy-ai/auto-optimize)


# Your Embedding Model Decides What Your Agent Can Think


Most evaluations of embedding models measure retrieval in isolation: given a query, how many relevant documents land in the top 10? That matters. But it misses something important about how embeddings actually get used in production.


In agentic workflows — where an[LLM](https://www.zeroentropy.dev/concepts/large-language-model/) searches, reasons, acts, and iterates — the embedding model isn’t just a retrieval component. It’s the agent’s *perception* . It determines which techniques the agent discovers, which approaches it considers, and ultimately, what solutions it can build. A few percentage points of retrieval accuracy don’t just mean a few more relevant documents. They mean entirely different trajectories of reasoning.


We wanted to measure that compounding effect directly. So we built AutoOptimize.


## The Result


Over 64 iterations, the three teams diverge — not gradually, but compoundingly. Team[zembed-1](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model) finds sharper techniques earlier, refines them further, and closes the race with a best-so-far score well ahead of teams using[OpenAI text-embedding-3-small](https://openai.com/blog/new-embedding-models-and-api-updates) or[Cohere embed-english-v3](https://cohere.com/blog/introducing-embed-v3) .


Best-so-far score (solid) and per-iteration score (dotted) for each team across a representative run. The zembed-1 curve separates from the baselines after roughly iteration 10 and continues to pull away.


**Team zembed-1 finishes at digits · √speed. Team OpenAI: . Team Cohere: . Same corpus, same LLM, same 64-iteration budget.**


The pattern is stable across runs: zembed-1 retrieves papers on Chebyshev node placement, barycentric interpolation, and rational approximation when those are the techniques that matter, while the baselines tend to return adjacent-but-weaker material — general interpolation theory, introductory surveys, polynomial methods. By iteration 10, zembed-1’s team is often already refining barycentric interpolation with optimal node placement; the other teams are still iterating on techniques that cannot, in principle, handle the target function’s Runge-like spike or high-frequency burst.


## The Setup: A Controlled Race


Three teams of AI agents compete to approximate an unknown function on . Each team gets 64 sample points and must build the best approximation — scored on accuracy multiplied by speed. The teams search the same corpus of 1,468 ArXiv papers on numerical methods, but each uses a different[embedding model for retrieval](https://www.zeroentropy.dev/concepts/dense-retrieval/) :


Team Embedding Model Dimensions Agents


ZeroEntropy zembed-1 2560 3


OpenAI text-embedding-3-small 1536 3


Cohere embed-english-v3.0 1024 3


**Same corpus. Same LLM (Gemini 3 Flash). Same tools. Same iteration budget. The only variable is which embedding model powers the search.**


Each team runs 3 agents in parallel that share a notebook — they can see each other’s best code, scores, and findings. Every iteration, each agent decides whether to refine the current approach or search for something fundamentally different. The dashboard shows the race live, with score charts and function approximation plots updating in real time.


## The Mystery Function


The target on is deliberately adversarial: a chirp (frequency sweeping with ), a Runge-like spike near , non-differentiable kinks from , a localized high-frequency burst near , and a steep sigmoid step near . Uniform polynomial interpolation fails catastrophically here. To score well, agents have to *discover* — from the papers — techniques like Chebyshev node placement, barycentric interpolation, rational approximation, or piecewise methods. The quality of what they discover depends entirely on the quality of their search.


```text
f(x) = sin(x²·2 + 3x) · exp(-0.08x²)       // chirp — frequency sweeps with x
+ 2/(1 + 100·(x-1.7)²)                  // sharp spike — Runge-like pole
+ 0.4·|sin(3x)|                          // periodic kinks — non-differentiable
+ 1.5·exp(-8·(x+3)²)·cos(25x)           // localized burst — high-freq near x=-3
+ 0.6·tanh(15·(x-3.5))                   // steep step — sigmoid transition
```


Each term is chosen to defeat a different naive approach: the chirp resists fixed-frequency bases, the Runge spike breaks equispaced polynomial interpolation, the absolute-value kinks violate smoothness assumptions, the localized burst punishes uniform sampling, and the sigmoid step creates a region that any global polynomial has to “spend” degrees of freedom on.


## Why This Is a Fair Test


It’s worth emphasizing what is *not* varying:


Controlled Variables


- **LLM** : All 9 agents use Gemini 3 Flash Preview — same model, same temperature, same max steps
- **Corpus** : All teams search the same 69,602 chunks from the same 1,468 papers
- **Tools** : Identical` searchPapers` and` evalCode` tools, same API signatures
- **Budget** : 64 iterations per team, 64 sample points per evaluation
- **Scoring** :` score = digits × √speed` , where` digits = -log₁₀(mean_absolute_error)` over 10,001 test points


The embedding model is the only degree of freedom. If one team consistently outperforms, it’s because their retrieval surfaces better techniques — and that’s a direct measurement of embedding quality in an agentic context.


## Architecture: How It Works


AutoOptimize is built with[Mastra](https://mastra.ai/) (TypeScript AI agent framework), the Vercel AI SDK, and Next.js.


#### Data Pipeline


1,468 ArXiv papers on numerical methods are downloaded,[chunked](https://www.zeroentropy.dev/concepts/chunking/) at newline boundaries into ~1,500-character segments (69,602 chunks total), and pre-embedded with all three providers. Embeddings are stored as binary files on disk (~1.4 GB total).


#### Race Initialization


The server loads all pre-computed embeddings into memory. When the user clicks “Start Race,” 9 Mastra agents launch in parallel — 3 per team. Each agent gets a system prompt, access to` searchPapers` and` evalCode` tools, and a shared team notebook.


#### Agent Loop


Each iteration: the agent calls` generate()` with up to 5[tool-use](https://www.zeroentropy.dev/concepts/tool-use/) steps. It can search papers (embed query →[cosine similarity](https://www.zeroentropy.dev/concepts/cosine-similarity/) → top-5 chunks) and evaluate code (run the approximation function, measure accuracy and speed). Agents within a team share their best code, scores, and search findings via a shared notebook.


#### Live Dashboard


The dashboard polls the server every second for score updates and every 3 seconds for function plot data. Score charts show best-so-far (solid lines) and raw iteration scores (dotted lines). Function plots overlay each team’s best approximation against ground truth.


## Run It Yourself


AutoOptimize is fully open-source under Apache-2.0. Clone it, add your API keys, and watch the race live:


```text
git   clone   https://github.com/zeroentropy-ai/auto-optimize
cd   auto-optimize


# Download corpus data and pre-computed embeddings (~1.5 GB)
./download.sh


npm   install
npm   run   build
NODE_OPTIONS  =  "--max-old-space-size=8192"   npx   next   start   -p   3000   -H   0.0.0.0
```


Then open` http://localhost:3000` and click “Start Race.”


The only thing you need is API keys for the three embedding providers and Gemini. The embedding scripts are idempotent — if you want to swap in a different model or re-chunk the corpus, they’ll resume from where they left off.


## What This Means for Production


If your agents search a knowledge base, the embedding model isn’t a commodity input — it’s the bottleneck on what your agent can learn. A few points of retrieval accuracy, compounded over dozens of iterations, produce fundamentally different outcomes.


**In agentic workflows, embedding quality doesn’t add — it multiplies.**


This is why[zembed-1](https://www.zeroentropy.dev/articles/introducing-zembed-1-the-worlds-best-multilingual-text-embedding-model) is built for accuracy first — the same property that puts it[ahead of voyage-4](https://www.zeroentropy.dev/articles/zembed-1-overperforms-voyage-4) and[harrier-27b](https://www.zeroentropy.dev/articles/zembed-1-vs-harrier) on static retrieval benchmarks is the property that makes it compound through an agent loop. For the reranking side of the same thesis, see[zerank-2](https://www.zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker) . AutoOptimize is one way to see all of this end-to-end in a system you can run yourself.


### Get Started


zembed-1 is available today through multiple deployment options:


[→ AutoOptimize clone the repo and run the race yourself](https://github.com/zeroentropy-ai/auto-optimize)[→ ZeroEntropy API fully managed, lowest-friction path to production](https://docs.zeroentropy.dev/models)[→ HuggingFace open weights, run it on your own infrastructure](https://huggingface.co/zeroentropy/)[→ AWS Marketplace deploy within your existing AWS environment](https://aws.amazon.com/marketplace/seller-profile?id=seller-nurj4uavxb4z2)


```text
from   zeroentropy   import   ZeroEntropy
zclient   =   ZeroEntropy()
response   =   zclient.models.embed(
model  =  "zembed-1"  ,
input_type  =  "query"  ,
input  =  "Chebyshev node placement for function approximation"  ,
dimensions  =  2560  ,
encoding_format  =  "float"  ,
latency  =  "fast"  ,
)
```


**Documentation** :[docs.zeroentropy.dev](https://docs.zeroentropy.dev/)


**GitHub** :[github.com/zeroentropy-ai/auto-optimize](https://github.com/zeroentropy-ai/auto-optimize)


**Get in touch** :[Discord community](https://discord.gg/5mkQCTnmY9) orcontact@zeroentropy.dev
