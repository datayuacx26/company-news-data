---
schema_version: "1.0.0"
document_id: "c30bfd37cab011727a9bf68f9a7291f70eb0d7adecc0ac76f07862ea920954a0"
company_key: "yc-sid"
company: "SID"
source_id: "yc-sid-news-import-30f4e157198f"
canonical_url: "https://www.sid.ai/research/sid-1"
published_at: "2025-12-04T00:00:00+00:00"
first_seen_at: "2026-07-25T23:59:57.623768+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:5ed1f0b4ce4ce0f9dd0eec2e596dd06e821e212fef2953a91defb2cd4625b4c2"
---

# Introducing SID-1

# Introducing SID-1


SID


Research


|


December 4, 2025


SID-1 is our first model for agentic retrieval: 1.9x more likely to surface the right results than embedding-only search across general knowledge, finance, science, legal, and email. It is more accurate than agentic retrieval based on Gemini 3 Pro, Sonnet 4.5, and GPT-5.1 at its highest compute setting, while being 24x faster (144 vs 5.7 seconds).


SID-1 (4x)


0.84


5.5s


$0.0014


GPT-5.1 (high)


0.78


131s


$0.24


SID-1


0.77


5.5s


$0.00062


Gemini 3 Pro


0.66


156s


$0.12


Sonnet 4.5


0.64


35s


$0.54


Reranker @10


0.45


0.78s


$0.00061


Vector only @10


0.44


0.15s


$0.0000098


0.2


0.45


0.65


0.85


Retrieval Performance


Recall


0


60


120


180


Latency


Seconds per Question


0


0.2


0.4


0.6


Cost


USD per Question


Traditional retrieval pipelines pre-program each step: query rewriting, searching, reranking. SID-1 retrieves like a human would: it searches, reads results, and refines its queries, taking as many steps as needed. We trained SID-1 on questions that take human researchers over an hour to solve.


SID-1 is drop-in compatible with existing retrieval systems. It is designed to work as a subagent with larger LLMs like GPT-5 or Sonnet. Teams running embedding-only retrieval or RAG can typically integrate it in an afternoon.


Step-change improvements in retrieval create new winners. Claude Code was better at finding the right file, unlocking larger code bases and driving massive adoption for Anthropic. Open Evidence made retrieving over medical research easier and took doctors by storm. Looking back further, relational was better than hierarchical for databases, creating Oracle. Better web search created Google. *[We're working closely with a small group of companies who want that edge now for their domains.](https://www.sid.ai/waitlist)*


Given we're compute-limited, we're rolling SID-1 out slowly. We're offering SID-1 through our API, AWS Bedrock, and as a self-hosted model. *If you're interested in SID-1, please sign up to our[waitlist](https://www.sid.ai/waitlist) or read the[technical report](https://www.sid.ai/research/sid-1-technical-report) for more details.*


We thank NVIDIA for their compute support.
