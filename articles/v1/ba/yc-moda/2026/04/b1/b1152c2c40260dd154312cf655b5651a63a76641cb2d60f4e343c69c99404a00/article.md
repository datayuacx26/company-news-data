---
schema_version: "1.0.0"
document_id: "b1152c2c40260dd154312cf655b5651a63a76641cb2d60f4e343c69c99404a00"
company_key: "yc-moda"
company: "Moda"
source_id: "yc-moda-news-import-ace8c1d7597a"
canonical_url: "https://moda.dev/blog/clustering"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-24T04:44:21.515931+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:e8da743f4dc8fbf40b7cff2e219e64fe3dd63b242cdcf4242036672f60ceea86"
---

# How We Clustered 150 Million Conversation Segments Without a Single Label

A single conversation with a consumer AI agent can contain five different user intents. A user asks about pricing, pivots to a feature question, gets frustrated about a bug, and ends by asking for a refund. Embed the full conversation, assign it to one cluster, and you've averaged over all of them.


That's where we started. It didn't work.


We process over 50 million conversations across 30+ customers. This post walks through the pipeline that turns those conversations into navigable structure, automatically, with no labels and no predefined taxonomy.


01


## Conversations are the wrong unit of analysis


Consumer agents produce long, multi-topic conversations that routinely exceed embedding


model context windows. Even if they didn't, embedding a 200-message transcript collapses distinct intents into one vector. Clusters become "people who talked about a lot of stuff." Useless.


The fix: stop clustering conversations. Cluster *segments* -- contiguous, topic-coherent slices. A conversation about onboarding, then billing, then a product bug becomes three segments. Each is routed independently. "Onboarding confusion" and "billing disputes" can now be separate behaviors even when they co-occur in the same chat.


For 50 million conversations, this produced over 150 million segments.


02


## Summaries are the primitive


Raw conversation text is noisy. Filler ("got it, one moment") eats the same token budget as intent ("the checkout flow is double-charging me"). Embed the raw text and the filler dominates. Truncate to fit a context window and you lose structure.


So we don't embed raw text. We summarize first, then embed. Three layers feed each other:


1. **Segment summary** -- one or two sentences per segment, capturing what the user was trying to do.
2. **Conversation summary** -- stitched from segment summaries; the artifact dashboards render and search indexes use.
3. **Cluster summary** -- the label and description for each hierarchical cluster, generated from its member summaries.


Everything downstream -- clustering, search, labeling, online assignment, lineage -- operates on the same 4096-D summary embeddings


. One uniform representation, no parallel storage. Intent survives; noise doesn't.


03


## Finding topic boundaries with cosine drift


To cluster segments, you first need to find them. We detect topic shifts geometrically, no classifiers involved:


1. Group consecutive same-speaker messages into turns.
2. Pair adjacent turns (user turn + assistant response).
3. Embed each turn pair at 4096 dimensions.
4. Split wherever the local drop in similarity exceeds an adaptive, per-conversation gate.


The adaptive part is the unlock. Any global cutoff fails across tenants with different baseline similarities -- a flat-tone bot runs systematically lower than a high-empathy bot, and a single fixed value can't serve both. Our gate is anchored on each conversation's own similarity distribution and continuously recalibrated by an offline tuning loop running against a held-out, human-segmented evaluation set. The system owns the dial; we own the loss function.


Cosine drift segmentation


0 segments · threshold 0.6


user


can i upgrade to the team plan


assistant


yes, you can upgrade from the settings page under Billing.


user


will my data transfer over?


assistant


all your workspaces, members, and settings stay intact.


user


hmm actually the export isn't working


assistant


can you share the error you're seeing?


user


it just spins forever on CSV downloads


assistant


got it, that's a known issue with large exports. try Parquet.


user


also who should i invite to review invoices


assistant


members with Admin or Billing role can view invoices.


user


is there sso for the team plan?


turn-pair similarity


Adjacent turn pairs are embedded, then compared. When similarity falls below 0.6


, a new segment begins.


04


## Hierarchical clustering on summary embeddings


Flat clustering worked until it didn't. A single "billing" cluster ends up covering refunds, upgrades, payment failures, and invoice issues all at once. Useless.


We needed three levels: Category, Subcategory, Cluster. Here's what makes it actually work at 150M points.


### BIRCH coreset, then HDBSCAN


HDBSCAN


's mutual-reachability graph


is roughly O(n²) in memory. Single-node exact HDBSCAN caps out around 30M points. So we don't run it on the full corpus.


Instead, we run BIRCH


in a single streaming pass to build a coreset


of ~1M radius-bounded micro-clusters that preserve local density. HDBSCAN runs on the coreset. The remaining points map back via a FAISS IVF


index. A full run -- BIRCH, UMAP


, recursive HDBSCAN, labeling, and centroid


persistence -- takes about 90 minutes on a single node.


### UMAP only inside the clustering step


HDBSCAN doesn't work well in 4096 dimensions even on a coreset. We project to a small number of dimensions via UMAP, configured for tightness rather than legibility -- we want clusters HDBSCAN can find, not a chart anyone can read.


UMAP is stochastic and non-deterministic. So we never persist it. Centroids, online assignment, and lineage tracking all anchor on the original 4096-D embeddings. UMAP is an internal detail of one clustering run and never leaves it.


### Depth-aware parameters


HDBSCAN parameters that work at the root don't work at the leaves. We descend a per-depth schedule that pushes the system toward broad partitions near the top and finer resolution near the bottom. The schedule itself is the output of an inner optimization loop -- a Bayesian search


over the joint parameter space, with each trial scored against a held-out human-rated panel on a weighted objective of DBCV


, silhouette


, and downstream labeling coherence. We re-run the loop on a rolling cadence; nothing here is hand-picked, and nothing stays static for longer than a quarter.


### The mega-cluster rule


HDBSCAN sometimes produces mega-clusters -- a single blob soaking up a substantial fraction of all segments at a level. Product teams open them, see thousands of members, and close them.


Our rule: anything that exceeds a navigability budget at its depth recurses using the next depth's parameters. Anything that survives recursion at max depth gets a forced k-means


shard split, with each shard relabeled independently. The exact tipping points -- the budget, the recursion gate, the shard target -- are part of the schedule the optimization loop owns. They've shifted twice in the last year.


### Noise is not noise


HDBSCAN labels low-density points as noise. We don't drop them. When the noise share at a level grows past a tolerance the optimization loop sets, we recurse into the noise branch with tighter parameters. Emerging user behaviors -- new features, viral use cases, breaking changes -- almost always show up here first, before they're big enough to displace an existing cluster.


Recursive HDBSCAN hierarchy


click to drill in · 75M segments


Mega-cluster > 5% → recurse


Noise branch (clustered at finer scale)


Normal cluster


05


## Labeling clusters at scale


5,000 leaves per tenant. Each needs a label a product manager can scan in one second and know what's inside.


The naive approach (send all segment summaries to an LLM) breaks on context windows and produces vague hedge-everything labels well before the limit. Random sampling over-represents the dense center of the cluster, missing its edges -- which is often where the actionable variants live.


We use farthest-point sampling


seeded at the centroid. Start with the most-central segment (one canonical example), then iteratively pick the segment sitting furthest from everything already selected. The sample budget is a function of cluster size and intrinsic spread -- we sample enough to span the cluster's semantic frontier and stop where additional samples stop adding coverage. On a blind reviewer panel, labels generated from FPS


-sampled summaries were rated "specific enough to be actionable" 78% of the time -- versus 41% for random sampling of the same size.


Those samples run through a map-reduce


: small parallel batches each produce candidate themes, and a final reduce pass synthesizes one label and description. Relabeling the full hierarchy is a weekly job; changed leaves -- splits, merges, births -- get relabeled within 60 minutes of a clustering run completing.


06


## Real-time assignment without re-clustering


A full run is 90 minutes. Between runs, segments are arriving constantly. Every one needs to land somewhere *now* .


Leaf cluster centroids


are stored as the L2-normalized


mean of the original 4096-D summary embeddings of members. When a new segment arrives:


1. L2-normalize


the segment summary embedding.
2. Query the tenant's FAISS IVF index for the top centroids.
3. Apply a temperature-scaled softmax


over their cosine similarities.
4. Accept only if both gates -- a relative one over the softmax distribution and an absolute one over cosine similarity


-- hold simultaneously.


The softmax works because the centroid space is well-separated by construction: the UMAP, recursion, and noise-handling stages of clustering all push centroids apart in the original embedding space. Top similarities typically span a wide range; the temperature is tuned to produce a sharp distribution the gates can read. In degenerate cases where the top results are near-equidistant, the softmax correctly collapses to near-uniform and the segment is routed to noise.


Both gates matter. The relative one rejects ambiguous-winner cases; the absolute one rejects far-but-relatively-dominant cases. Either gate alone runs around a ~5% false-assignment rate on our validation set. Combined, it drops to 2.3%. The temperature, the relative cutoff, and the absolute cutoff are jointly fit by the same offline Bayesian search we use for clustering parameters, against a continuously-growing labeled validation set, and re-fit on every clustering run. p95 online assignment latency is 7.3ms.


Segments that fail both gates land in a per-tenant noise bucket. Every hour we run a mini-HDBSCAN over a rolling window of that bucket with tighter parameters. Any cluster that forms is either merged back if it sits within the absorption radius of an existing leaf, or surfaced as an emerging cluster in the customer's dashboard. This is where behavior shifts show up first.


Online FAISS assignment


0 routed


·


0 → noise


Refunds


Plan upgrade


Onboarding


API keys


Clustering


Alerts


incoming segment


Segments arrive, compare to stored centroids via FAISS, then lock to the top match when both thresholds hold. Misses collect in the noise bucket, where emerging patterns first appear.


score ≥ 0.30


distance < 0.70


T = 0.5


07


## Tracking clusters across time


Every full run can produce a different set of clusters. Without lineage, you lose the ability to say "this failure mode resolved after the last deploy."


We compute lineage in three passes over the old-vs-new centroid cosine similarity matrix:


1. **Hungarian 1:1 matching.** Pairs above the *continuation threshold* are **Continuations** -- same cluster, updated membership.
2. **Row scan for splits.** For each matched old cluster, look for *additional* new clusters above the looser *secondary-link threshold* in its row. Extra matches mean the old cluster fractured -- this is a **Split** .
3. **Column scan for merges.** For each matched new cluster, look for additional old clusters above the same secondary-link threshold in its column. Extra matches mean multiple old clusters converged -- this is a **Merge** .


Anything unmatched with nothing above threshold is a **Birth** (new) or **Death** (disappeared).


Lineage · Old run → New run


Continuations


1


Splits


1


Merges


1


Births


1


Deaths


1


Hungarian alone is strict 1:1 and can't express many-to-many lineage. The two residual passes are what recover splits and merges from the same similarity matrix.


Both thresholds -- the tighter one for primary continuation, the looser one for secondary lineage edges -- are outputs of the cross-tenant calibration loop. Like every gate in the system, they don't stay constant; we re-fit them every clustering run against a held-out set of human-confirmed cluster transitions.


Splits and merges are the most useful signal. A "refund" cluster splitting into "refund -- billing error" and "refund -- feature gap" after a pricing change is a direct readout of that change's impact. Two "onboarding confusion" clusters merging after a UX redesign tells you the redesign unified a fragmented experience.


08


## The hard part


Every algorithm in this pipeline -- BIRCH, UMAP, HDBSCAN, FAISS


, Hungarian -- is standard. We didn't invent any of them.


The hard parts came from making them cooperate on 150M segments across 30+ tenants with wildly different conversation patterns:


- **The BIRCH coreset unlocked scale.** Before it, we capped at 30M segments per tenant. A 1M coreset is the only reason a full run fits in 90 minutes on one node.
- **UMAP never leaves clustering.** Anchoring everything downstream in the 4096-D originals is why online assignment and lineage survive across runs.
- **No threshold is held constant.** Every gate, every cutoff, every recursion budget in the system is the output of an offline optimization loop running against a continuously-growing human-labeled evaluation set. Hand-picked numbers don't survive contact with a fintech support bot and a developer-tool agent in the same week. The only durable thing is the loss function.
- **Noise is the feature.** The most valuable things our customers surface first appear in noise branches, never in the main hierarchy.


Each of these came from a specific failure. The mega-cluster rule exists because mega-clusters made the product unusable. FPS exists because random sampling produced labels that missed half the cluster. The BIRCH coreset exists because we tried exact HDBSCAN on 40M segments and killed our clustering node. The continuous-calibration loop exists because every threshold we ever hand-picked turned wrong inside a quarter.


Traces tell you what happened. Clustering, done right, tells you why it matters.


See what your users are actually doing, across every conversation.


[Talk to the team](https://cal.com/team/moda/demo-meeting)
