---
schema_version: "1.0.0"
document_id: "b3d064f8a88ebd3a5bb682dbb178f595d22b3189e3d6bd22aa1631493dcb0a54"
company_key: "yc-guide-labs"
company: "Guide Labs"
source_id: "yc-guide-labs-news-import-41fe5d9b8b28"
canonical_url: "https://www.guidelabs.ai/post/concept-discovery-in-steerling-8b/"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-07-25T07:19:32.933709+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:5148409f5c49f1b6d3dbda36a59d99c0e5cca00dbfaccf608038a63dabead227"
---

# Discovering Human-understandable Concepts in Steerling-8B

We examine the universe of concepts that the recently released[Steerling-8B](https://huggingface.co/guidelabs/steerling-8b) model learned in its representations. Here we show that we can easily discover thousands of novel concepts from the model; concepts it was never explicitly trained to learn. The model learned to distinguish British English spelling, unified “you” across six languages, separated spelled-out numbers from digits, learned to recognize typographic errors, and even developed a dedicated concept for broken Unicode.


## Concept discovery in standard models is challenging


Extracting knowledge from the representations of an AI system remains a longstanding difficulty. As these systems approach superhuman capabilities, understanding what drives their performance is urgent. What latent knowledge do LLMs possess that enables them to resolve open problems in mathematics? Where in AlphaGo’s representations did it acquire the insight behind move 37?


To illustrate the difficulty, we sample random neuron directions from three open-weight models and project each direction into the vocabulary space to see which tokens it promotes most strongly.


Qwen 3 8B: Direction #42


1 / 6


'll


Warm


的话


Morrison


hte


Cy


Cyber


ords


.www


撸


These results are difficult to decipher. There seems to be no clear pattern to these random neuron directions in the model. The model’s knowledge is distributed across directions that bear no correspondence to human-interpretable concepts. This is the fundamental nature of the problem: the representations of standard AI systems are, by default, entangled.


Steerling-8B takes a different path; it learns disentangled representations by construction through architectural and training-time constraints. Consequently, we shift the question from “Can we reverse-engineer what this model knows?” to simply: “What did this model learn?”


## The Concept Unmasking Game


To make the difference between entangled and disentangled representations concrete, try the matching game below. We took five concepts from Steerling-8B and projected each one into the model’s vocabulary, extracting the tokens it weights most heavily. On the left you’ll see concept labels like “SQL Query Keywords” or “Wide-Angle Photography.” On the right, we show groups of tokens. The pairings have been shuffled. Click a concept, then click the token group you think belongs to it. After matching all 5 pairings, you can click unmask to see how well you did.


Interactive


Round 1


## Unmask the Concept


Match each concept to its vocabulary projection.


❶ Click a concept


→


❷ Click its token projection


→


❸ Unmask


Concepts


Vocabulary Projections


0 / 5 matched


If the matches feel obvious, that’s the point: in standard models such a matching game is challenging because the model learns distributed representations. However, because Steerling’s concepts are trained to be interpretable, it is easier to understand what the model’s representations mean.


In the sections that follow, we show that Steerling-8B has learned thousands of novel concepts that it was never explicitly trained to acquire. Before we catalog these discoveries, it is worth understanding why this has not been possible before. The interpretability field has developed post hoc methods to extract knowledge from black-box models.[Probing](https://arxiv.org/abs/1610.01644) classifiers test whether specific concepts are decodable from a model’s activations.[Sparse autoencoders (SAEs)](https://transformer-circuits.pub/2024/scaling-monosemanticity/) attempt unsupervised decomposition of representations into interpretable directions. These methods have yielded real insights, but they face an inherent limitation: they are attempting to recover a unique decomposition of a space that admits many. A model’s activations can be decomposed along infinitely many directions, and there is no ground truth that privileges one decomposition over another.[Post-hoc disentanglement of entangled representations](https://arxiv.org/abs/1811.12359) faces irreducible ambiguity that no post hoc method can fully resolve without strong assumptions.


## Discovering new concepts in Steerling-8B


To start, we show a sample of 50 new concepts that Steerling-8B discovered without any constraints.


Second-Person Pronouns Across Languages


1 / 50


you


YOU


vous


你


você


thee


ya


Ihnen


ye


您


вы


bạn


Sie


usted


thou


Вы


yourself


yourselves


The model unified 'you' across 6+ languages, discovering that these words serve the same function with no explicit multilingual signal.


Steerling-8B’s design directly decomposes the model’s embeddings into three explicit pathways: ~33K supervised “known” concepts, ~100K “discovered” concepts the model learns on its own, and a residual that captures whatever remains. For the full architecture, training objectives, and scaling analysis, see[Scaling Interpretable Models to 8B](https://www.guidelabs.ai/post/scaling-interpretable-models-8b/) .


Schematic of Steerling-8B’s embedding decomposition process.


To identify what a discovered concept represents, we project its embedding into vocabulary space. The highest-weighted tokens reveal the concept’s semantic content. If those tokens cluster around a recognizable theme - SQL keywords, British spellings, second-person pronouns - we can assign the concept a human-readable label. The method is deliberately simple: the architecture and Steerling’s design do the heavy lifting, and the projection merely reads off what the model already organized.


## How Different are Known and Discovered Concepts?


Steerling-8B has ~33K known concepts it was explicitly trained on and ~100K discovered concepts it learned on its own. A natural question is whether these discovered concepts are as coherent and useful as the known ones; or whether the model simply filled those extra dimensions with noise.


To visualize the relationship between known and discovered concepts, we project each concept’s embedding vector into two dimensions using UMAP. Each point represents a single concept. Known concepts (blue) and discovered concepts (gold) occupy distinct but adjacent regions of the space, confirming that the model’s unsupervised concepts are structurally different from the supervised ones.


The visual separation confirms the two groups are structurally distinct. But we might wonder whether both groups of concepts are equally high quality. To answer this, we evaluate every concept along multiple dimensions.


We define an **analytical coherence score** as the harmonic mean of three metrics computed directly from the model’s representations:


- **Separation** measures whether a concept strongly promotes specific tokens over the rest of the vocabulary.
- **Concentration** measures whether that preference is sharply peaked or diffusely spread across many tokens.
- **Coherence** measures whether the promoted tokens actually mean similar things, forming a genuine semantic cluster.


The remaining three dimensions are assessed by an LLM (scored in the range 0-10) examining each concept’s top tokens and assigned labels:


- **Crispness** captures whether the concept picks out a specific, well-bounded idea rather than a vague or fuzzy one.
- **Usefulness** estimates whether a person would actually care about controlling the concept at inference time.
- **Controversiality** indicates whether the concept touches on sensitive or divisive subject matter.


These criteria probe complementary aspects of concept quality.


Distribution of concept quality metrics across known (blue) and discovered (gold) concepts. Top left: Analytical coherence, the harmonic mean of separation, concentration, and coherence scores computed from the model’s representations. Top right: Crispness. Bottom left: Usefulness. Bottom right: Controversiality. The latter three are assessed by an LLM examining each concept’s top tokens.


The distributions largely overlap. For the analytical coherence score (top left), known concepts skew higher, centering around 0.35–0.45 compared to 0.20–0.30 for discovered concepts, but the two distributions share substantial overlap. The modest shift might reflect the absence of curated supervision rather than a fundamental quality gap. For the three LLM-assessed metrics, the picture is even more striking. Crispness scores (top right) cluster tightly around 7–8 for both groups. Usefulness (bottom left) shows a similar pattern, with both groups peaking around 5–7. Controversiality (bottom right) is low for both, concentrated near 0–2, meaning neither group disproportionately captures sensitive topics. The implication is that the ~100K discovered concepts are not filler. The model’s unsupervised concept formation produces representations that are, indeed, less analytically coherent than the supervised ones, but maybe similarly crisp, useful, and non-controversial.


## Looking Forward


The standard approach to understanding neural networks is to train a black box and then try to pry it open. Steerling suggests an alternative: build the model so that its knowledge is accessible from the start. The ~100K concepts discovered here are a first demonstration of what becomes possible when interpretability is a design choice rather than a retrofit.


To explore Steerling-8B yourself:


- 🤗[Steerling-8B on huggingface](https://huggingface.co/guidelabs/steerling-8b)
- 💻[Code on GitHub](https://github.com/guidelabs/steerling)


[← Previous blog Steering Interpretable Language Models](https://www.guidelabs.ai/post/steerling-steering-8b/)[Next blog The FineWeb Concept Atlas →](https://www.guidelabs.ai/post/the-fineweb-concept-atlas/)
