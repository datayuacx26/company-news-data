---
schema_version: "1.0.0"
document_id: "d0591e80bc370053a12dc02f8e4ab77c95ae99c7a580301293cb6c56e855a4b0"
company_key: "yc-altrina"
company: "Altrina"
source_id: "yc-altrina-news-import-a8da914aea6c"
canonical_url: "https://www.altrina.com/blog/introducing-large-neurosymbolic-cognitive-models"
published_at: "2025-01-04T00:00:00+00:00"
first_seen_at: "2026-07-21T06:06:15.390227+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:13a44a83be5953228dee1e15b0baef3e73e83aeaa9edafff02dcf1a580bcfca3"
---

# Introducing Large Neurosymbolic Cognitive Models

**Note: This blog was published prior to our[rebrand from Tessa AI to Altrina](https://www.altrina.com/blog/tessa-ai-is-now-altrina)**


The future of AI isn't just about automating individual tasks – it's about creating systems that learn, adapt, and master new skills just like humans do. Today, we're excited to introduce a novel architecture for self-improving AI: Large Neurosymbolic Cognitive Models (LNCMs), representing the next evolution beyond reasoning LLMs.


LNCMs possess the ability to understand and reflect on their own capabilities and limitations while autonomously acquiring new skills. This enables LNCMs to learn and adapt based on feedback received through experiences and human interactions.


## A Breakthrough in Real-World Performance


We're proud to announce that the LNCM architecture achieves a new **state-of-the-art result of 93% on the WebVoyager benchmark.**


**Comparison between publicly advertised benchmark results**


The WebVoyager benchmark comprises 640 scenarios involving real, live websites. There were a number of time-dependent scenarios which were minimally updated manually. For example, if a scenario contained a date in March 2023, it was updated to the same date in March 2025. The "minimal" difference was the year, ensuring the integrity of the benchmark.


The evaluation of the agent utilized multiple human evaluators instead of the auto-evaluation method outlined in the original paper. This was to ensure that we were only evaluating the agent's performance, and not the evaluator's performance jointly with the agent.


Impressively, this state-of-the-art 93% success rate was achieved without the self-optimization feature. We expect to reach a 100% success rate with the self-optimization, as the system autonomously masters web browsing tasks.


Tessa outperforms Convergence's Proxy at 82%, Emergence's Agent-E at 73%, and H Company's Runner H at 67%.


‍


## Tesseract: The Brain Powering Tessa


Tesseract takes inspiration from how the human brain processes information and learns from experience. It employs three interconnected graph structures, wrapped in a neurosymbolic metacognitive layer. Graphs are powerful as they allow us to structure data and operations while allowing for incremental improvements to the ontology—the system can reconfigure and optimize the graphs as necessary.


The components of Tesseract


- **Compute Graph:** This is Tesseract's executive function center, handling complex reasoning tasks, from planning to self-critique and verification. This is equivalent to the human brain's prefrontal cortex, orchestrating executive function and ensuring coherent decision-making.
- **Procedure Graph:** This is Tesseract's experiential memory. This structure maps out past states and the actions that led between them, allowing for skill acquisition and recall. Rather than approaching each new situation from scratch, Tesseract can draw upon this graph as a planning heuristic, much like how humans rely on pattern recognition from past experiences to navigate novel challenges.
- **Knowledge Graph:** This is where factual information is stored, interconnected, and recalled. Unlike traditional databases with rigid schemas, the graph structure allows Tesseract to determine its own connections between concepts, discovering relationships and implications that might not be immediately obvious.


The three graph structures are encapsulated by a neurosymbolic self-model. This self-model gives Tesseract something akin to self-awareness – a comprehensive understanding of its own capabilities, limitations, and internal structures. Rather than relying on single-shot language model calls, as many current AI agents do, Tesseract actively monitors its knowledge and skillset, ensuring more reliable and grounded responses.


This self-model goes beyond mere monitoring: it enables Tesseract to reconfigure itself when faced with novel challenges that lie outside its existing capabilities. While tree search has become a popular mechanism for allowing AI agents to explore different actions and learn from their outcomes, Tesseract extends this concept to metacognition. It can search through different configurations of its cognitive graphs, effectively learning how to think differently when faced with new types of problems. This autonomous self-optimization, we believe, is a significant step towards fully generalizable superintelligence.


## Looking Ahead


We're now putting this powerful technology to work across various industries, from financial analysis and consulting to real estate. If you have any suggested use cases in mind, sign up for our waitlist and let us know!
