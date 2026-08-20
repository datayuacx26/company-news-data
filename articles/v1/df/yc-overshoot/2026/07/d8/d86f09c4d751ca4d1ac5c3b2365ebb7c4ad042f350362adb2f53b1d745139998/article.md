---
schema_version: "1.0.0"
document_id: "d86f09c4d751ca4d1ac5c3b2365ebb7c4ad042f350362adb2f53b1d745139998"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/when-vlms-answer-without-seeing"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T23:00:17.714410+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:f659512052dc3d326f41143f90557d36dd32d255a72c809ac04168cfd88de3bb"
---

# When VLMs Answer Without Seeing: The Mirage Problem

## What Are Mirages?


Vision-language models (VLMs) are able to answer questions about images and videos: given an image as input, you can ask a question and expect the model to formulate a fluent, confident response.


However, recent research has uncovered a surprising failure mode. VLMs can answer questions about visual content even when the visual input is blank, unrelated, or uninformative.


These responses have been dubbed "mirages"—outputs that create the appearance of understanding visual evidence that is not actually present \[Mirage, arXiv 2026\].


A mirage happens when a vision-language model displays an apparent understanding of what is shown in an image, but the response is not actually supported by the image. Instead, the response may be based on cues in the question, on regularities in the training data, or on other unsupported model priors.


The key issue is not simply whether the answer is correct or incorrect, but whether the model confidently asserts something that is not grounded in the image.


A VLM gives a confident visual answer unsupported by the image. Source: Mirage, arXiv 2026.


## How Do Mirages Happen?


While the exact mechanisms underlying mirages are not entirely understood, researchers have proposed and found evidence for two possible mechanisms behind mirages: textual bias, where the model relies on question text rather than image content, and spurious visual representations, where the model's internal representations behave as though question-relevant visual information were present, even when it is absent \[Mirage Probes, arXiv 2026\].


The first source of error is fairly intuitive: the model answers the question based on the text in the prompt, either implicitly suggested by the prompt itself or learned as a regularity during training, rather than based on the image.


The second source is more interesting: the model acts as if supporting visual information were present, when in reality it is not.


This behavior has been described as a "spurious image"—not an actual generated picture, but a hypothesized internal state in which the model behaves as though supporting visual evidence were present.


## Why Is This A Problem?


Mirages are a problem both from the practical standpoint of deploying vision-language models and from the theoretical standpoint of assessing their ability.


Starting with the practical one: a vision-language model that is able to formulate fluent, confident responses that are visually unsupported can be dangerous in high-stakes settings.


When used in medical, robotic, industrial, or surveillance environments, a VLM must be able to distinguish between what is supported by the visual evidence and what it is inferring from the text. This distinction is especially important for safety-critical applications where an unsupported answer can have serious consequences.


Meanwhile, the other aspect of mirages is connected to the theoretical assessment of a vision-language model's proficiency.


If a model is able to answer visual questions correctly on a benchmark, but some of these questions can be partially answered from text alone, it becomes challenging to determine what exactly the model is good at: processing visual concepts or simply exploiting patterns in captions and questions.


In other words, on many vision-language tasks, the language component can help the model answer questions even when the visual input is unrelated.


## Can Models Know When They Should Not Answer?


The goal of mirage detection is to determine whether a given image-question pair is answerable at all.


This means asking whether the image is relevant and informative—or whether it is unrelated, noisy, or blank. In the latter cases, the safest course of action would be for the model to refuse to answer rather than produce a fluent but unsupported response \[Detect Before You Leap, arXiv 2026\].


At the most basic level, this can be achieved by using heuristics to detect uninformative images: applying deterministic statistics over pixels to identify low-contrast, unusually uniform, or noisy images as unsupported.


Patch variance compares how much image patches vary across the input, helping flag blank or unusually uniform images before a VLM answers.


Beyond these simple image-level heuristics, can we determine whether the image is actually related to the question?


The key lies in analyzing how much the visual content actually supports the question.


A typical way to assess the relevance of an image to a question is to use a CLIP-style score: comparing the embedding of the question to the embedding of the image and measuring how close they are in a shared embedding space.


However, rather than comparing the whole image to the question, we can compare patches of the image to the question across the layers of a pretrained vision encoder.


The intuition is that, if the image contains information relevant to the question, patches representing that content should become more closely aligned with the question in a shared embedding space at later vision layers.


Ultimately, if the image is unrelated to the question, image patches should generally show lower, flatter, or less stable alignment across vision layers.


TC-LIA tracks patch-question alignment across vision layers to test whether the image supports the prompt. Source: Detect Before You Leap, arXiv 2026.


## Can These Alignment Checks Reduce Mirages?


These alignment checks can be surprisingly useful: in *Detect Before You Leap* , baseline vision-language models produced mirages on unrelated or non-informative inputs between 21.7% and 66.6% of the time, depending on the model backbone.


After applying a detection pipeline consisting of blank/noise detection, domain routing, image-question alignment checks, vision-language model self-assessment, and ensemble-based feature fusion, the reported mirage rate ranged between 2.7% and 3.3% across twelve model backbones.


However, this is not a complete solution: image-question alignment checks were overall better at detecting cross-domain mismatches than same-domain mismatches.


A natural image paired with a medical question was much easier to detect as a mismatch than the wrong chest X-ray paired with a chest X-ray question, despite the domain of the image matching the domain of the question.


These mismatches can be much harder to detect because images from the same domain may share high-level visual structure, even if they lack the specific evidence needed to answer the question.


Such checks can identify obvious image-question mismatches, but passing them does not guarantee that the model's answer is correct or visually grounded.


## The Next Frontier In Vision-Language Learning: Visual Grounding Checks For Mirages


The main takeaway is that VLMs should not only make valid claims about image content, but also ground those claims in perceptual evidence.


Mirages reveal a limitation in which VLMs can confidently act as visual reasoners while relying on superficial textual regularities or unsupported internal visual representations. This can lead them to draw conclusions with little grounding in actual perceptual content. This undermines their reliability in real-world visual tasks and can make benchmark performance appear more visually grounded than it really is. The challenge, therefore, is to equip VLMs with perceptual guardrails so that they can better determine when their verbal responses are actually supported by what they perceive.
