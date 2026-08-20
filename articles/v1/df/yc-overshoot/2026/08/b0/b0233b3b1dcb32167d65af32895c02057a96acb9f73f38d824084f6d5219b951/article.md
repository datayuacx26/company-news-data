---
schema_version: "1.0.0"
document_id: "b0233b3b1dcb32167d65af32895c02057a96acb9f73f38d824084f6d5219b951"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/all-you-need-to-know-about-jepa"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T04:53:03.949123+00:00"
fetched_at: "2026-08-14T04:53:04.595812+00:00"
content_hash: "sha256:7d7baa454c98d6c2c207e5e3b1be95617ce11d9630458405f3f5f1d7bdf415b5"
---

# All You Need to Know about JEPA

## Beyond Labels and Pixels


Traditionally, supervised vision systems learn from labeled examples. For instance, an image is paired with a labeled category such as "dog," "car," or "airplane." In training, the model then learns to adjust its parameters until it can predict those classes reliably.


While labeled learning has been crucial to modern AI, it also provides only a narrow description of the visual world. A given photograph can contain information about objects, materials, spatial relationships, motion, lighting, and physical structure. These elements are far more than a single category can convey.


In contrast, self-supervised learning offers a different approach. Instead of merely relying on human-written labels, the model constructs a learning task from the raw data itself. It might hide part of an image, for example, and learn to predict what is missing. Many self-supervised methods either reconstruct the missing input or train different views of the same image to produce similar representations. A Joint-Embedding Predictive Architecture, or JEPA, explores a different goal:


**Instead of predicting the missing pixels, can a model predict what the missing region represents?**


## Learning Without Labels


One useful way to understand visual self-supervised learning is through three broad architectural approaches: generative architectures, invariance-based joint-embedding architectures, and joint-embedding predictive architectures.


### Generative architectures


Reconstruction-based generative methods learn by reconstructing missing or corrupted parts of an input. For example, in a task like masked image modeling, a model receives the visible image patches and tries to recover the missing pixel values.


A Masked Autoencoder, or MAE, follows this approach. While this provides a clear training objective, it can require the model to reproduce details that are difficult to infer and may not be important for understanding the scene. If part of a dog is hidden, the model may need to predict exact colors, textures, shadows, and strands of fur. For many semantic tasks, the more useful information may be that the missing region belongs to the dog.


Masked Autoencoder reconstructing missing image patches from visible context.


### Invariance-based joint-embedding architectures


Invariance-based methods train related views of the same image to produce similar representations. Two versions of the same image might have different colors, scale, or framing, but the model learns to recognize what remains consistent between them.


It is important to point out that these methods do not reconstruct the original pixels. Rather, they are actually learning representations that remain stable across selected transformations.


### Joint-embedding predictive architectures


A JEPA also operates in representation space. Yet its objective is predictive and not purely invariant. At a high level, it uses the representation of one part of an input to predict the representation of another.


In place of reconstructing exact pixels, JEPA learns what information about a missing region can be inferred from the context that surrounds it.


## If the Pixels Are Uncertain, What Should the Model Predict?


For an illustrative example, let's suppose part of an image is hidden.


A pixel-reconstruction system tries to recover the missing RGB values. A JEPA tries to predict the vector that an encoder would produce for that missing region.


A raw image patch contains hundreds of pixel values that describe its precise color and texture. Once these patches go through a vision encoder, each one is represented by a learned vector. Because a Vision Transformer allows patches to interact through attention, the resulting vector can reflect how the patch relates to the rest of the image.


Given this, consider an image of a dog standing on grass. If the dog's torso is hidden, the exact pattern of its fur may be impossible to determine from the surrounding patches. There can be many different pixel arrangements that form a plausible torso.


Thus, the predictable information is more general. This missing region probably contains part of the dog, which connects its head to its legs, and occupies a particular position in the scene.


JEPA is designed to capture this kind of predictable structure, but in a way where the model does not have to resolve every uncertain pixel-level detail. The learned representation space is not given in advance. Rather, this space starts unstructured and develops through training as the encoders and predictor repeatedly solve the prediction task.


## How I-JEPA Learns From an Image


How can a model create a prediction target without being shown the missing content?


I-JEPA applies the JEPA idea to static images. The underlying architecture has three main components: a context encoder, a target encoder, and a predictor, all of which use Transformer-based architectures.


### The target encoder


The target encoder receives the full, unmasked image.


The image is divided into non-overlapping patches. The target encoder then produces a contextualized vector for each patch. Several spatial blocks are selected from these outputs to serve as prediction targets.


This distinction is important here. The target encoder processes the full image, and the target blocks are selected from its output representations. The target encoder does not process each hidden block in isolation, which means its vectors can incorporate information from the rest of the image.


### The context encoder


I-JEPA also samples a large context block from the image. Any patches that overlap with the selected target regions are subsequently removed. This is done to ensure that the context encoder cannot directly observe the content it is supposed to help predict.


The remaining visible patches pass through the context encoder which creates a set of context representations.


### The predictor


The predictor receives the context encoder's output along with mask tokens corresponding to the target patch locations.


Each mask token contains a learned vector plus positional information. It tells the predictor where a missing patch belongs, but it does not reveal what that patch actually contains.


Using the visible context and the target positions, the predictor estimates the vectors that the target encoder produced at those locations.


The process can be summarized as:


I-JEPA predicts target-encoder representations for masked regions from visible context, rather than reconstructing pixels. Source: adapted from Assran et al., I-JEPA.


For training, the loss is calculated using the average squared L2 distance between the predicted patch-level vectors and the corresponding target-encoder vectors. The predictor and context encoder are updated through gradient-based optimization. In contrast, the target encoder is updated differently. It does not receive gradients directly from the prediction loss. Instead, its parameters are updated as an exponential moving average of the context encoder's parameters. As the context encoder learns, the target encoder gradually follows it. Because it changes more slowly, it provides a relatively stable set of representations for the predictor to match.


The vectors produced at the beginning of training are not already meaningful. The target encoder initially begins from the same randomly initialized parameters as the context encoder. Over time, the context encoder, predictor, and moving target encoder jointly develop a more structured representation space.


### Why the Masking Strategy Matters


What makes a missing region useful as a prediction target?


The prediction task must be difficult enough for the model to require meaningful visual understanding, but also not so ambiguous that prediction becomes impossible.


As we have seen, I-JEPA samples several large target blocks and a broad context region, removing any overlap between them. Large targets are more likely to contain meaningful parts of objects or scenes. Predicting a tiny patch might rely mostly on nearby textures or edges. On the other hand, predicting a larger region may require information about object identity, shape, and spatial structure.


The context must also provide enough evidence. For example, if part of a car is hidden behind a tree but its wheels, body, and surroundings remain visible, the model should have enough context to infer the missing region's representation.


The masking strategy therefore helps determine what kind of visual information the model learns.


But does this masking strategy actually produce useful visual representations?


On ImageNet-1K,[I-JEPA](https://arxiv.org/pdf/2301.08243) achieved 79.3% top-1 linear-probe accuracy with a ViT-H/14 encoder after 300 epochs, compared with 77.2% for an MAE model of the same size trained for 1,600 epochs. This shows that competitive visual representations can be learned efficiently without reconstructing raw pixels.


### Why JEPA Matters


JEPA learns by predicting representations rather than reconstructing every observable detail. Unlike generative methods, which predict the original input, or invariance-based methods, which align representations of related views, JEPA predicts higher-level structure within a learned representation space.


But why is this important? The visual world may be uncertain at the pixel level, but it can still be predictable at a more abstract level. A model might not know the exact texture, color, or arrangement of missing pixels. But it can still infer what kind of object or structure belongs there. If machines can learn by predicting abstract structure rather than predefined labels or exact observations, representation-level prediction could be a way toward systems that discover for themselves which aspects of the world are actually worth understanding.
