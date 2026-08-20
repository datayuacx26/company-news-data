---
schema_version: "1.0.0"
document_id: "2aa5be7ff0fb8035e84c133b317f5ba6e8bee75a342b8f8fc82f076a266a6d5e"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/from-pixels-to-tokens"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-29T03:44:15.654746+00:00"
fetched_at: "2026-07-29T03:44:17.208372+00:00"
content_hash: "sha256:b6ee8bf5dabfa6e372034a809c436da9e4a80d544997d3453587d1155dd94b99"
---

# From Pixels to Tokens: How Vision Transformers Read Images

## Text Is Sequential. Images Are Spatial.


Transformers were first developed for tasks such as machine translation. They differ from traditional recurrent models in that they use attention to process a sequence of vector representations, allowing each representation to incorporate information from other positions in the sequence (Vaswani et al., 2017).


Text inherently has an ordered structure: words or subwords can be thought of as units that make up a sentence, and the position of each unit affects how it relates to the others.


An image, by contrast, is usually represented as a two-dimensional grid of pixel values. A still image is organized spatially rather than temporally.


To adapt the Transformer architecture to vision tasks, Vision Transformers split an image into patches, which are then represented as vectors. These vectors can be thought of as tokens in the visual domain.


But why do Vision Transformers turn images into tokens in the first place?


## Transformers Process Sequences of Vectors


Modern large language models, or LLMs, are built on the Transformer architecture. In language models, text is split into tokens, such as words or subwords, which are then converted into embeddings. More generally, a Transformer is not inherently tied to language and can process many kinds of data once they have been encoded as sequences of vectors.


Self-attention then allows each element in the sequence to weigh information from the other elements, which helps the model capture relationships across the input.


But when it comes to vision, what should each element in this sequence represent?


Only using one token would prevent self-attention from directly modeling relationships between different local regions. Conversely, encoding each individual pixel as a token would produce a much longer sequence and substantially increase the computational cost of training.


## Dividing an Image Into Patches


For a practical example, let's look at an RGB image that is 224 × 224 pixels. It can be represented as a three-channel tensor with dimensions of 224 × 224 × 3.


The final dimension contains the red, green, and blue values associated with each pixel.


The first step in processing such an image with ViT is to split it into square patches of a chosen size. For example, it is common for a ViT to divide the image into 16 × 16-pixel patches (Dosovitskiy et al., 2021).


A 224 × 224 image is then split into 14 patches across and 14 patches down. This produces 14 × 14, or 196, patches.


A 224 × 224 image divided into 196 non-overlapping 16 × 16 patches.


The image is now represented as a sequence of 196 local image regions.


For image classification, the original ViT adds a special token, called the` \[CLS\]` token, to the beginning of the sequence. As the tokens move through the Transformer, the` \[CLS\]` token gathers information from the image patches. Its final representation is then used to classify the image.


## How Is an Image Patch Converted Into a Token?


Now that we have these patches, each 16 × 16 RGB patch contains 16 × 16 × 3, or 768, pixel values.


In the original ViT framework, each patch is flattened into a one-dimensional vector and then passed through a learned linear projection into the embedding dimension used by the Transformer (Dosovitskiy et al., 2021).


At a high level, the transformation goes like this:


**Image → Patches → Flattened patch vectors → Patch embeddings**


Vision Transformer architecture (Dosovitskiy et al., 2021), from An Image Is Worth 16×16 Words: Transformers for Image Recognition at Scale.


The resulting patch embeddings are the visual analog of token embeddings in a natural language processing task. In language, a token embedding represents a word or subword token, while in vision, a patch embedding represents a local region of the image.


An important distinction is that the patch embedding does not necessarily represent a meaningful object or concept. A patch may contain part of an edge, a texture, a background region, or pieces of multiple objects. As the patch embedding passes through the Transformer, its representation becomes contextualized through interactions with tokens from other parts of the image.


## How Does the Model Know Where a Patch Is Located?


Converting an image into a sequence of patches creates another problem. The model needs information about where each patch appeared in the original image because spatial arrangement is important for understanding shapes, objects, and where they are located.


Standard self-attention does not inherently encode the original position of each element in a sequence.


To account for this, ViTs add a learned positional embedding to each patch embedding before feeding the sequence into the Transformer (Dosovitskiy et al., 2021).


These positional embeddings allow the model to distinguish between patches partly based on where they appeared in the image.


The learned positional embeddings reflect aspects of the image's two-dimensional structure: patches that are closer together tend to have more similar positional embeddings, and row-and-column patterns emerge among them.


## Why Not Split an Image Into Individual Pixels?


But why not avoid patches altogether and treat every pixel as its own token?


The example image described above has a width and height of 224 pixels, meaning that it contains 50,176 pixel locations in total.


If each pixel became a separate token, the resulting sequence would be 256 times longer than the sequence produced by 16 × 16 patches: 50,176 tokens instead of 196.


Because self-attention scales quadratically with sequence length, this corresponds to 65,536 times as many pairwise token relationships.


Naturally, this would require substantially more computation and memory.


Therefore, the main reason for using patches is that they give ViT a manageable sequence of local image regions while preserving useful spatial detail.


Patching also introduces a trade-off involving patch size.


Larger patches produce fewer tokens, and as a result reduce computation. Yet, each token also covers a greater portion of the image and provides a coarser representation.


Smaller patches produce more tokens and provide a finer-grained representation, which may help the model capture small objects or subtle visual patterns. At the same time, they also require more computation.


## From Visual Tokens to Vision-Language Models


The ability to convert images into tokens also helps connect ViTs with language models.


Many vision-language models use a vision encoder to process an image and then pass the resulting visual representations through a projection layer. This layer maps the visual embeddings into the dimensional space expected by the language model, allowing the model to use them for tasks such as image captioning and visual question answering.


In this way, visual tokens act as an interface between the image encoder and the language model. They allow visual information to be represented in a form that can be combined with, attended to, or otherwise used alongside text tokens.


This token-based interface is one of the key ideas underlying modern vision-language models.
