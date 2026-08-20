---
schema_version: "1.0.0"
document_id: "4024c11aefd79d5bcdd71441e0f2e4a8bbe1bc8115a1724155abb0b702dc3ffd"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/the-real-world-is-not-a-dropdown-menu"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T20:34:11.571376+00:00"
fetched_at: "2026-08-11T20:34:12.740280+00:00"
content_hash: "sha256:f5714848bdaa6971a7df08936aea2d2780ad196eb26bf540b79c5c810f1d8dd3"
---

# The Real World Is Not a Dropdown Menu: Why Fixed-Label Vision Models Break Down

## A Simple Assumption With Big Limits


Computer vision has seen tremendous progress in the last decade. Vision models can perform tasks such as image classification, object detection, pixel-level segmentation, and visual retrieval. In the right domain, today’s systems can provide very strong results.


But many traditional visual recognition systems operate under a simple constraint: they recognize visual data using a fixed set of predefined categories.


This approach works well for constrained tasks. A model trained on digits only has to distinguish between ten possible digits. Similarly, a model trained on cats, dogs, cars, and airplanes only has to choose between those four categories. However, this approach fails to capture many open-ended applications, where the model has to reason about a wide range of concepts.


Fixed-label systems become limiting when a task requires:


- Understanding or describing objects that the model has not seen before.
- Describing rare or anomalous phenomena.
- Matching a visual observation against a description, such as “the image shows X.”
- Answering unusual questions that the model was not trained on.


## The Fixed-Label Era


Traditional image classification systems are often built on the following setup: acquire and curate a labeled dataset, train a vision model to recognize images as belonging to a fixed set of labels, and use that model to perform classification.


Two major types of models used for these computer vision systems are Convolutional Neural Networks, or CNNs, and Vision Transformers, or ViTs. Both can be used for vision classification tasks, with CNNs learning hierarchical patterns in images through learned convolutional filters and ViTs dividing images into patches before analyzing context with self-attention.


**Image → CNN / ViT → fixed class label prediction**


A traditional CNN classifier extracts visual features from an image, passes them through a classifier, and outputs probabilities over a fixed set of classes. Source: adapted from IONOS Digital Guide.


The main limitation is not the architecture itself.


It is the fixed set of output labels. While CNNs and ViTs can learn rich visual patterns, many supervised classification systems are still constrained to differentiate between a fixed set of classes.


## Where Fixed Labels Fail


Open-ended environments, however, contain tasks that go beyond distinguishing between a handful of classes.


The real world often contains new objects, rare corner cases, and anomalous observations that may not be represented by a system’s predefined categories or task-specific training data. It also contains questions and descriptions that a vision system was not explicitly trained to answer. For example, a robot may need to follow an instruction like “pick up the red cup next to the laptop,” which requires reasoning about objects, attributes, and spatial relationships rather than choosing from a single fixed category. Similarly, a manufacturing application might require a model to analyze defects. A binary “good” or “bad” label may not be sufficient to describe the type, location, or severity of the defect.


## What Real-World Vision Systems Need


More flexible vision systems need to perform open-ended reasoning to answer questions such as: What is happening here? How should this scene be described? What changed from the last image to this image? Does this image depict X or not? Does this image provide evidence for claim X? This requires systems that move from a closed set of possible answers, such as class labels, to more flexible answers that are grounded in what the image shows.


Traditional image classification tasks have a fixed set of answers. Newer tasks move toward more open-ended outputs, such as captions, descriptions, retrieval results, or natural language answers. This changes the interface from one where the user asks “what is this image?” and the system answers with a label, to one where the user asks “does this image depict X?” and the system answers using natural language.


One way to relax this constraint is through zero-shot or few-shot methods. These approaches allow models to recognize or adapt to concepts without requiring a large task-specific labeled dataset, using broader sources of supervision such as natural language or a small number of examples.


## Zero-Shot Image Classification Using Natural Language Supervision


CLIP, or Contrastive Language-Image Pre-training, uses a large-scale dataset of 400 million image-text pairs collected from the internet. The model uses an image encoder and a text encoder, both projecting their outputs into a shared embedding space (Radford et al., 2021).


CLIP learns a shared embedding space for images and text by contrastively training image and text encoders to align matching pairs and separate mismatched pairs. Source: OpenAI.


During training, for every batch of image-text pairs, the model compares image and text embeddings using cosine similarity. It learns to bring the embeddings of correct image-text pairs closer together while pushing apart the embeddings of incorrect image-text pairs (Radford et al., 2021). Essentially, the model learns associations between visual concepts and language without being explicitly trained for one specific computer vision task.


A key property of CLIP is that it can perform zero-shot classification. Given an image, the model can compare natural language prompt options and select the one that is the best match. One can use prompts such as “a photo of a cat” or “a photo of a dog” as candidate descriptions and select the one with the highest cosine similarity to the image embedding.


Zero-shot learning allows a model to classify an unseen category by using semantic information, such as attributes or natural language descriptions, rather than relying only on examples of that exact class. Source: ModulAI.


The candidate classes do not need to be encoded in a task-specific classification head. Instead, class labels can be provided as natural language prompts to the text encoder.


It is important to note that the usefulness of such approaches is highly contextual and depends on the exact task, prompt design, and distance of test examples from the training distribution. Nevertheless, such methods provide a valuable shift from fixed output classes toward matching images with flexible natural-language descriptions.


## From CLIP to Modern VLMs


While CLIP is a powerful tool for zero-shot image classification and image-text retrieval, it is not a fully fledged vision-language assistant. Given an image, CLIP does not directly generate free-form natural language captions or answers. Modern vision-language models, or VLMs, extend this approach by generating language from visual input.


One approach, used by LLaVA (Liu et al., 2023), is to connect a vision encoder with a large language model using a projection layer. The vision encoder generates embeddings of the input image, and the projection layer, usually a single linear layer or sometimes a small neural network, maps these embeddings into a space the language model can process alongside text tokens. Another approach, used by Flamingo (Alayrac et al., 2022), connects vision and language through cross-attention layers. The general idea behind these models is to allow the language model to use visual information while performing downstream tasks such as captioning, visual question answering, and multimodal dialogue.


These models provide a different interface. Rather than only retrieving existing captions or matching an image to candidate prompts, they can generate natural language responses given an image input.


**Traditional classifier: image in, predefined class out.**


**CLIP classifier: image and candidate descriptions in, best match out.**


**VLM: image and prompt in, generated response out.**


This provides a more flexible way to request visual information. Instead of being limited to existing categories, VLMs can respond to questions about what an image contains or might mean. They can be used for a variety of tasks, from captioning to complex visual dialogues.


## Where Do We Go from Here?


Fixed-label vision systems are unlikely to disappear. For narrow use cases, a carefully designed CNN or ViT can provide excellent results with minimal resources. However, for many open-ended real-world use cases, such approaches are insufficient.


Human oversight is also likely to remain important, especially in cases where a vision system needs to make a judgment call about ambiguous evidence. Future research will also involve finding better ways to ground vision systems in reality, ensuring that captions, labels, or answers generated by vision systems actually correspond to what is present in the image.


The real world is rarely confined to a small set of categories. Zero-shot approaches using natural language supervision, along with modern vision-language models, allow us to think about new ways of interacting with vision systems beyond choosing between a few fixed options.
