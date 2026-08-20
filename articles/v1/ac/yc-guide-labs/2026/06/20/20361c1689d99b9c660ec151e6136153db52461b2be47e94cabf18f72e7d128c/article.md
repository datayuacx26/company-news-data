---
schema_version: "1.0.0"
document_id: "20361c1689d99b9c660ec151e6136153db52461b2be47e94cabf18f72e7d128c"
company_key: "yc-guide-labs"
company: "Guide Labs"
source_id: "yc-guide-labs-news-import-41fe5d9b8b28"
canonical_url: "https://www.guidelabs.ai/post/single-cell-concept-bottleneck-generative-models/"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-25T07:19:32.933709+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:0f51fe2eef82818fda5d02a05fbc817c71c536f839cae17e7fea57b41a00e087"
---

# Cell Editing with Interpretable Generative Models

We show how a new type of interpretable generative model, single-cell Concept Bottleneck Generative Model (scCBGM), extracts the mechanism that mediates a cell’s response to a drug, then performs counterfactual edits on non-responding cells to turn them into responders. Together, these form an explain-and-edit framework: scCBGM decomposes a cell into known concepts (cell type, drug dose, pathway activity) and an unknown remainder, so you can read off the concepts behind a cell’s behavior (explain) and change them to generate a counterfactual cell (edit). Combined with flow matching, it achieves the best counterfactual accuracy across three real-world datasets, and we use it to generate testable hypotheses for enhancing drug response.


[Read the full paper.](https://arxiv.org/pdf/2606.07760)


*This work was a collaboration between Alma Andersson, Edward De Brouwer, and Doron Haviv (Braid at Genentech) and Aya Abdelsalam Ismail (Guide Labs).*


Editing low-dose stellate cells in silico. The model never saw a stellate cell respond during training, yet the edited cells ( red


) move out of the low-dose population and land on the true high-dose responders ( green


), with an rMMD of 0.678.


An important part of drug discovery is understanding how a cell’s gene expression profile would change under a particular treatment. We dose a liver with a drug, and find a split among the stellate cells: some respond, others don’t. This raises two questions: what causes some cells to respond, and how do we make the non-responders respond too?


In this post, we show how a new kind of interpretable generative model, the single-cell Concept Bottleneck Generative Model (scCBGM), identifies the pathway signature behind the responders, then lets us intervene on those pathways in the non-responding cells and turn them into responders. This makes cellular editing a practical procedure rather than a guessing game.


The key is that scCBGM exposes the mechanism and concepts that it uses to generate its output, so we can perturb the model directly in a direction of interest. In the wet lab, getting the same result means running several experiments, and hoping we hit the right pathways. Standard generative models aren’t much help either: they don’t expose the factors driving a given cell, so they can’t produce a counterfactual for an individual one.


In the rest of this post, we’ll introduce concept bottleneck generative models and the new single-cell version we built on top of them. We then show how their explain-and-edit structure lets you read the mechanism behind a cell’s behavior and rewrite it.


## Single-Cell Concept Bottleneck Generative Models


Our starting point is a class of interpretable generative models called[Concept Bottleneck Generative Models](https://openreview.net/pdf?id=L9U5MJJleF) (CBGMs). A CBGM first maps its input to concepts, properties of the input that a human can understand and recognize. For a gene expression profile, these are things like pathway signatures or which treatment a cell received. The concepts are then transformed into the output. In a generative setting it’s almost impossible to enumerate every concept, so the model usually adds a second component to capture whatever it didn’t explicitly track but still needs in order to reproduce the output. These are the unknown concepts. A key requirement is that the unknown concepts don’t redundantly re-encode the known ones.


But the original CBGMs were designed for images, a very different kind of data from gene expression. Single-cell data is higher dimensional, highly heterogeneous, and carries substantial technical noise and unreliable concept annotations. It turns out a few small changes are enough to adapt CBGMs to this setting, which is what we do in single-cell Concept Bottleneck Generative Models (scCBGM).


-


**Skip connections in the decoder.** We feed a copy of the extracted concepts into every decoder layer rather than only the first. This keeps the conditioning strong even when the biological annotations are noisy.


-


**A different disentanglement loss.** CBGMs had the right instinct, keep the known and unknown parts from overlapping. But they enforced it with a cosine similarity, which forces the two embeddings to be the same size. For single-cell data that is the wrong constraint, because the unknown part has to capture far more than the known part, and shrinking it to match would harm generation quality. We replace the cosine loss with a normalized cross-covariance penalty, in the spirit of the[Hilbert-Schmidt Independence Criterion with a linear kernel](https://arxiv.org/abs/1910.00270) , which penalizes statistical dependence between the known and unknown representations without tying their dimensions together.


-


**A simpler bottleneck.** CBGMs used a[concept embedding model](https://arxiv.org/abs/2209.09056) , which represents each concept as a pair of embeddings, one for its presence and one for its absence. That does not fit single-cell data well, where many concepts are continuous rather than binary, like a dose or a pathway score. So we use a standard concept bottleneck instead, encoding each concept as a single real value.


We’ll now describe the explain-and-edit framework that scCBGM enables.


## Explain


Now that we have the model, the first thing it gives us is an explanation: why does one cell behave one way and its neighbor another? scCBGM answers in terms of concepts. A concept is a property of a cell. Some we can name, measure, and set on purpose, e.g., its cell type, an active pathway, whether it’s seen a drug and at what dose. Others we can’t fully pin down: the stuff that encodes the cell’s identity, or biology nobody understands yet. These are exactly the known and unknown channels the model splits a cell into.


## Edit


The second step is editing, which is where you ask the counterfactual question. In this phase, we make one change to the cell while preserving everything else. With the scCBGM, this translates to interventions on the concepts that are in the model.


## Does it work?


Verifying whether a counterfactual is actually correct on a real cell is impossible, because you would have to measure the same cell twice, once treated and once not. So to stress test different methods, we built a synthetic benchmark where we can control the noise level and the true counterfactual is known by construction. There, scCBGM has the lowest cell-level error of any method we compared.


We can also hold a state out entirely and see whether the model finds it on its own. We removed stimulated Naive CD4 T cells from training, then predicted that state from the matching control cells. scCBGM lands right on the true held-out population, while the baselines drift off.


Predicting a held-out cell state. Stimulated Naive CD4 T cells were removed from training, then predicted from control cells. scCBGM (second panel) lands on the true held-out population, while the other methods drift away.


Across the full benchmark, we use a population-level proxy called rMMD, where anything below 1 means the prediction beats the trivial baseline of mapping a cell to the nearest existing population. Only the scCBGM family clears that line, and across three datasets it gives the best counterfactual accuracy on 10 of 14 cell types (full results in the paper).


Mean rMMD across the real-data benchmark, lower is better. The three scCBGM variants in blue are the only methods below the rMMD = 1 line, where a prediction starts to beat simply reusing the most similar existing population.


## Enhancing drug response in stellate cells


Let’s now return to our opening motivating problem. We find that some stellate cells respond to a drug and others do not, and we want to know why, and how to fix it. We will now demonstrate how the explain and edit framework that we discussed above allows us to solve this problem.


**First, Explain.** Among the stellate cells, some responded to the drug and some did not. Comparing the two groups, the concept module reads out what separates them, here a pathway signature of ↑ TGFβ, ↑ PI3K, ↑ p53, ↓ TRAIL, and ↓ VEGF.


**Then, Edit.** We first remove every responding stellate cell from training, so the model has never seen a stellate cell respond. We take low-dose control stellate cells, set those five pathway concepts to the responder profile, and turn the dose concept up.


The edited cells become responders. They leave the low-dose population and land on the true high-dose responders, a state the model was never trained to produce.


We note that the edit above does not imply that the pathways do indeed cause the response in real life. The model could have learned spurious associations; however, the scCBGM gives us a testable mechanistic hypothesis from non-responder to responder, generated in silico and ready for wet-lab testing.


As AI models and agents take on more of drug discovery, it is valuable to devise approaches that allow us to extract the mechanism behind their answers. Scientific work is not only about prediction, which insight is also paramount.


[← Previous blog Making a Dataloader for Scale and Flexibility Without Compromises](https://www.guidelabs.ai/post/dataloader-how-we-built/)
