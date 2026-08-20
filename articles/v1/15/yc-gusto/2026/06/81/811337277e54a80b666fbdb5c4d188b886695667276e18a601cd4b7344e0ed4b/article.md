---
schema_version: "1.0.0"
document_id: "811337277e54a80b666fbdb5c4d188b886695667276e18a601cd4b7344e0ed4b"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/from-prompt-to-classifier-a-production-case-study-8fbcaca9b1eb"
published_at: "2026-06-29T15:16:45+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:248068739c38861f60fe67c00838abebcb7c595bfaca8efd44a61fcc4a258a0f"
---

# From Prompt to Classifier: A Production Case Study

Machine Learning


LLM


Ai Engineering


Customer Experience


# From Prompt to Classifier: A Production Case Study


[Anshumachandak](https://medium.com/@anshumachandak_26477?source=post_page---byline--8fbcaca9b1eb---------------------------------------)


5 min read


·


Jun 12, 2026


--


Press enter or click to view image in full size


At Gusto, one of our core values is being customer obsessed. That means meeting customers where they are and ensuring they get the right support at the right time.


A few months ago, we needed to ship a system that could seamlessly hand customers off from our AI assistant to a human agent, one that could proactively gauge customers’ needs and escalate in a timely manner, helping maintain trust and a good customer experience. There was a clear opportunity to improve the AI-to-human escalation experience, but the rules and criteria were still evolving across teams.


The old playbook for building machine learning (ML) systems is pretty straightforward: collect data, train a model, then deploy. But that process would have taken months, and we didn’t have that kind of time. So we took a different approach: deploy an LLM on day one, treat production traffic as a labeling pipeline, train a smaller specialized model, and swap it in once the problem definition stabilizes.


Here's how the rest of this post is organized:


- **Starting with a prompt** — what we shipped, how we built our eval set, and what production data taught us.
- **Prompt to model** — the signals that told us it was time to move to a classifier, and the technical details of the model.
- **The broader pattern** — where this approach fits, where it doesn't, and where to spot it inside agentic systems.


## Starting with a prompt


On day one, we shipped a prompt. The team had aligned on the rules, we encoded them in a prompt, and shipped it to production.


In parallel, we built an eval set from production conversations, about 3,000 turns drawn from ~500 sessions, randomly sampled across our different user personas so we weren’t optimizing for one segment. Each turn was reviewed and labeled by humans, giving us a stable benchmark to score the prompt against turn by turn.


We ran the prompt against the eval set. That gave us a baseline of ~90% on both precision and recall. From there, every prompt change, even small ones, ran against the eval set as a regression test before going to production, so we weren’t fixing one signal while breaking another.


Over weeks of iteration with app teams, leadership, branding, and CX agents, the rules evolved, grounded in real transcripts rather than guesses. Production data reshaped the rules further. A few of our early signals turned out to generate more noise than signal. For example, we initially escalated when a customer repeated themselves as we assumed repetition meant frustration. In practice, customers who repeat themselves are usually *adding* context by clarifying, supplying details or narrowing the question, not getting frustrated. So we removed the rule from the prompt. This is the kind of pattern that feels obvious in a planning meeting but looks very different across real conversations.


## Prompt to model


Over time, our evaluation metrics became nearly flat, and we stopped seeing meaningful gains from prompt changes. At the same time, the team converged on the core experience principles, i.e., if a customer is frustrated with the AI assistant, or the AI assistant genuinely can’t resolve the issue, make the transition to a human seamless.


So we trained a binary classifier on the labeled production data.


## Get Anshumachandak’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


A few implementation details:


- **Task framing.** Turn-level binary classification: each row is one turn, with the running history as input and a binary label (escalate vs. no_escalation) as the target. The model makes a prediction at every turn rather than once at the end of a session.
- **Data.** About 3,500 labeled turns. Roughly 30% were positive examples, deliberately enriched by sampling more escalated conversations so we were not training on a heavily imbalanced set. Labels came from a mix of LLM outputs reviewed and corrected by CX, along with freshly labeled examples.
- **Model.** A lightweight BERT based binary classifier, standard fine-tuning, optimized for recall and production latency.
- **Threshold selection.** Rather than the default 0.5 cutoff, we tuned the threshold against a recall target (≥0.90).


On the held-out set, the classifier lands at ~94% precision and ~93% recall. It also improves on latency, cost, and predictability, since outputs are bounded and deterministic at fixed weights.


We’ll roll it out to production in shadow mode first, then gradually release it to traffic. The LLM will stay in place for the parts where the rules are still evolving.


## The broader pattern


We have found that the LLM is often the best way to get started. It helps you ship early and fill in the gaps while you figure out the system underneath. Over time, you replace parts of it with smaller, purpose-built models, and keep the LLM only where it’s actually useful: ambiguous inputs, edge cases, and new situations.


### Where this pattern fits


This pattern works best in a specific kind of problem where there is a high volume of similar decisions, humans already in the loop reviewing outputs, and a decision boundary compact enough to encode, such as escalate or not, route to A or B, intent X or Y.


But many problems don’t fit that shape. Open-ended generation, multi-step reasoning, or areas where the rules keep shifting are a different category. Those are probably better left to frontier models.


### Inside agentic systems


Prompting and agentic architectures are absolutely where a lot of the interesting work is going. We are not arguing against them. But inside those systems, there are almost always nodes where a smaller purpose-built model will be faster, cheaper, and more predictable than another LLM call on the path.


### **Spotting the candidates**


The best candidates are usually the most boring ones, narrow decisions made frequently on stable boundaries, like intent classification at the top of an agent flow, tool-selection routers, or retrieval re-rankers. Each is doing one well-defined job, and an LLM is overkill for any of them once you’ve collected enough labeled data to train something smaller.


One heuristic that helped us was walking through the agent flow and asking at each LLM call what it was actually deciding. If the answer was something narrow, stable, and repeated often, it was usually a good candidate for a smaller model.


Start with the LLM, then replace the parts that become well-understood with smaller, purpose-built models.


Gusto is hiring engineers who care about code quality at scale. Learn more at[https://gusto.com/about/careers](https://gusto.com/about/careers)
