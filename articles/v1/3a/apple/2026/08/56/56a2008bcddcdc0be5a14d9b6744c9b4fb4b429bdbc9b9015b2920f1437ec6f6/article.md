---
schema_version: "1.0.0"
document_id: "56a2008bcddcdc0be5a14d9b6744c9b4fb4b429bdbc9b9015b2920f1437ec6f6"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/the-p-completeness-of-inverted-index-traversal"
published_at: null
first_seen_at: "2026-08-20T03:26:17.324945+00:00"
fetched_at: "2026-08-20T03:26:20.724183+00:00"
content_hash: "sha256:ae974bb775571e17e6c000e691d1481cf654411b0982dc492844d97be816bfa4"
---

# The P-Completeness of Inverted Index Traversal: On the Complexity of Evaluating Boolean Query DAGs

[View publication](https://arxiv.org/abs/2601.18747)


Modern AI agents increasingly rely on search infrastructure to execute complex, neuro-symbolic reasoning workflows. These workflows often compile into deeply nested, non-monotonic Boolean queries over text fields. However, standard query evaluation strategies over inverted indices face severe theoretical limits when handling these structures. Stateful iterator models (Document-at-a-Time) are structurally bounded by NC^1 formula evaluation, suffering a worst-case O(2^|Q|) exponential blowup in query complexity when unrolling re-convergent logic. Conversely, recursive materialization models (Term-at-a-Time) incur an Ω(|U|) space complexity penalty (the Universal Scan) when evaluating logical negation over the document universe.


In this paper, we establish the theoretical boundaries of executing complex logic natively over an inverted index. We formalize a retrieval language (L_R) based on Directed Acyclic Graphs (DAGs) and prove that its evaluation problem is strictly P-Complete. To make evaluation tractable, we introduce ComputePN, a deterministic, sparsity-aware evaluation algorithm. By decoupling logical negation from universe-scale materialization via a novel Positive-Negative dual representation, and utilizing native DAG memoization, ComputePN strictly bounds evaluation time to O(|Q| · |U_active|). This approach successfully evaluates P-Complete queries natively over the index, avoiding both the combinatorial tree-expansion bottleneck and the universal scan penalty, laying the formal foundation for computational retrieval.
