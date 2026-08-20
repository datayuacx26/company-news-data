---
schema_version: "1.0.0"
document_id: "7114651ac2b2aab1687046e525169b65ebc6749b1070fb0785d6234b562ac278"
company_key: "yc-guide-labs"
company: "Guide Labs"
source_id: "yc-guide-labs-news-import-41fe5d9b8b28"
canonical_url: "https://www.guidelabs.ai/post/the-fineweb-concept-atlas/"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-07-25T07:19:32.933709+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:0ba31a8c9a4806c0860b93dd01c12b1514a2310f25369c074114bc78c643b0a4"
---

# The FineWeb Concept Atlas

We are releasing[FineWeb Atlas](https://huggingface.co/datasets/guidelabs/fineweb-atlas) , a concept-annotated version of the[FineWeb-Edu dataset](https://huggingface.co/datasets/HuggingFaceFW/fineweb-edu) : a 10.18B-token corpus with sub-document-level, human-understandable topic annotations. Each document is broken into chunks, and each chunk is annotated with 4 types of concepts that capture the chunk’s primary content, tone, key entities, and purpose. Built using an improved version of our[ATLAS](https://www.guidelabs.ai/post/atlas-concept-annotated-pretraining-release/) pipeline, this dataset should enable new directions in LLM model training, steering, and auditing.


FineWeb Atlas annotates 14,868,862 documents (95,486,049 chunks, 10,183,028,973 tokens) with 16,790 human-understandable concepts. The full release is available on HuggingFace and consists of four core artifacts:


- ` chunks` : Chunk-level text with concept annotations. Across all 95,486,049 chunks, the average label count is 14.73 (spanning content, tone, document, and entity concepts).
- ` concepts` : The concept inventory: 16,790 concepts, each with a human-readable name, description, and type metadata.
- ` field_guide` : A reverse index mapping each concept to matching documents/chunks, enabling concept-first retrieval.
- ` co-occurrence` : A concept co-occurrence matrix capturing which concepts appear together and how often.


Together, these artifacts let anyone explore pretraining data at the concept level, querying, filtering, and analyzing a 10.18B-token corpus with human-understandable concepts.


Below are five examples from the corpus, showing how documents break into chunks and the concept annotations our pipeline assigns to each. These examples span technical how-to content, reviews, fan/community writing, and pop-culture discussion.


Curated webtext example | WordPress to Salesforce integration


Curated webtext example | Master Data Services learning guide


Curated webtext example | indie film review and reflection


Curated webtext example | community wiki maintenance update


Curated webtext example | pop-culture discussion prompt


Figure: Five examples from different corners of webtext. Each panel shows raw text with content, document, tone, and entity labels, illustrating how the annotation stack captures both topic and style across varied writing formats.


## The FineWeb Concept Atlas


The overall concept library spans topics like NASCAR to Hepatitis C, from specific entities like Fort Wayne, Indiana to Stockholm, from blood transfusion protocols to GPS caching. The library includes 16,790 concepts reflecting the full breadth of what appears in web-scale text corpora.


Loading visualization...


Figure: 2D UMAP projection of roughly 3,000 sampled concepts. Nearby points are semantically related concepts, and colors show taxonomy groups, making it easier to see where domains form tight clusters versus broad overlap.


Colors indicate taxonomy groups; use the legend to isolate or hide groups.


At the annotation stage we seek to annotate text along 4 dimensions:


- **Content** : concepts that capture the primary topic of a chunk of text, e.g., Christianity, orbital mechanics, interior design, Python debugging. 12,786 concepts (76.15% of the library) are content concepts.
- **Entity** : concepts that identify proper nouns, i.e., specific named things: people, places, organizations, texts. Examples of this type of concept include cities like Fort Wayne, countries like Zambia, and religious text like The Bible. This is a new addition compared to the original ATLAS release, reflecting the importance of named entities in web text.
- **Tone** : concepts that describe the style of a chunk, i.e., persuasive, funny, tongue-in-cheek, conversational, etc. This class of concepts represents 587 in total, accounting for 3.50 percent of the overall library. There are far fewer tone concepts than content concepts, but tone accounts for 46.82% of all concept assignments, because concepts like “matter-of-fact” and “Informational” apply to the vast majority of chunks.
- **Document** : concepts that capture the purpose or format of the text: news report, tutorial, short announcement. We have 31 (0.18 percent) document concepts in the entire library.


Figure: Concept frequency vs mean LLM-judge score. Horizontal position reflects concept prevalence, vertical position reflects label quality, and color highlights where quality failures concentrate across the frequency spectrum (yellow: <= 2, blue: > 2). This is a floor estimate from the shared sampled set (>=10 LLM ratings per concept): we also apply additional quality-improving curation steps (for example, dropping clearly worst concepts), but we do not yet have a robust measurement of how much those steps improve these metrics.


On average, each chunk of text receives 14.73 concept labels: 5.68 content, 6.90 tone, 1.52 document, and 0.63 entity. The density is intentional; a single chunk about GPS caching in a tutorial might carry content labels for the technology, tone labels for its instructional style, a document label marking it as a tutorial, and an entity label for a specific platform or standard. The concept frequency distribution follows a familiar power law pattern: a small number of high-coverage concepts dominate, while a long tail of specific concepts each appear in a small fraction of chunks. For example, “matter-of-fact” covers 83.58% of chunks and “Informational” covers 79.82%. At the tail, thousands of concepts capture niche subjects that matter for specific domains.


The co-occurrence matrix reveals which concepts tend to appear together. Some pairings are unsurprising but confirm expected correlations. The strongest overall pairing, “Informational” and “matter-of-fact”, co-occurs 72 million times covering 75.53% of all chunks. This reflects the dominant educational tone of FineWeb-Edu’s 10.18B-token corpus. In another case, “Christianity” and “Religion” co-occur in 2,197,944 chunks.


### How FineWeb Atlas Was Produced


FineWeb Atlas was built using an improved version of the ATLAS pipeline, which produces concept annotations in three stages:


- **Stage 1: Documents → Tags** . Documents are split into chunks and each chunk is annotated with structured tags by a language model. For FineWeb Atlas, this produced 95,486,049 chunks from 14,868,862 documents.
- **Stage 2: Tags → Concepts** . The raw tags are embedded, clustered, and deduplicated into a canonical concept library. Each concept receives a human-readable name and description. For FineWeb Atlas, we restructured the concept categories, adding entity and document types, truncated rare tags more aggressively, and used alternate deduplication strategies, arriving at 16,790 concepts compared to the original 33,000.
- **Stage 3: Concept Annotator** . A trained model predicts concepts for arbitrary text, enabling annotation at corpus scale. For FineWeb Atlas, we evaluate this model’s predictions against the LLM labels in the Label Quality section below.


## Annotation Label Quality


We evaluate annotation quality using the same framework from the original ATLAS release: an LLM judge scores each chunk-concept assignment on a 1–5 scale, where a score of 2 or higher counts as a successful annotation. We report quality for both the ground-truth labels and the trained annotator’s predictions.


Ground truth vs model quality distributions (yellow bins are scores` <= 2` , blue bins are` > 2` ). This single 4-panel figure shows:


- ground truth averaged over chunks
- ground truth averaged over concepts
- model averaged over chunks
- model averaged over concepts


Figure: Score-distribution comparison between ground truth and the final KNN annotation model, each aggregated two ways (by chunks and by concepts). Panels are normalized to proportions with matched y-axes so distribution-shape differences are directly comparable.


## Research We Are Excited to See


We built FineWeb Atlas because we needed concept-level annotations for interpretable model development. But the release is designed to be useful well beyond our own work. Here are some directions we’re excited to see the community explore:


- **Concept taxonomies** : The concept library is flat by design, but it doesn’t have to stay that way. The concept metadata, co-occurrence structure, and Library of Congress taxonomy mappings provide natural starting points for organizing concepts into hierarchies, discovering implicit parent-child relationships, or building domain-specific subtrees for targeted analysis.
- **Interpretability research** : Concept annotations create a bridge between model internals and human-understandable meaning. If you can label what concepts a model was trained on, you can ask sharper questions: which concepts does a model represent well and which does it collapse? How do concept mixtures in training data relate to the features that form in a model’s representations? FineWeb Atlas provides the labeled substrate that makes these questions empirically testable.
- **Training on concept mixtures** : Rather than controlling your pretraining data mix at the source domain level (more web text, less code), concept annotations let you control it at the semantic level. We might want a corpus with more physics and less celebrity gossip, more formal reasoning and less conversational filler? The concept labels make this kind of fine-grained curation straightforward.


The full dataset is available on HuggingFace. If you’re interested in our broader work on interpretable model development, read our other blog posts.


[← Previous blog Discovering Human-understandable Concepts in Steerling-8B](https://www.guidelabs.ai/post/concept-discovery-in-steerling-8b/)[Next blog Alignment Without Retraining: Auditing and Controlling Steerling-8B →](https://www.guidelabs.ai/post/steerling-8b-alignment-without-retraining/)
