---
schema_version: "1.0.0"
document_id: "8e924d7a02e68b7ebf96726681589720fea24d33e49085058cfc6b0ce4a8a2d0"
company_key: "innodata-inc-common-stock"
company: "Innodata Inc."
source_id: "innodata-inc-common-stock-rss-09b9ff75e5ec"
canonical_url: "https://innodata.com/ai-aircraft-detection-from-space/"
published_at: "2026-04-08T14:04:54+00:00"
first_seen_at: "2026-07-20T23:17:35.625386+00:00"
fetched_at: "2026-07-28T20:52:48.201959+00:00"
content_hash: "sha256:fc1521c78a9ac322fd06dc80c00af817c64b7398e5f7b675b252becefac6b346"
---

# AI Can Count Aircraft from Space. Understanding Them Requires a World Model

# AI Can Count Aircraft from Space. Understanding Them Requires a World Model


#### Frank Tanner, VP of Computer Vision and Robotics


#### April 14, 2026


A commercial airliner is one of the most recognizable objects on earth — until you look at it from space. From


roughly 383


miles up, WorldView-3 captures nadir imagery looking straight down at the earth’s surface, where that same


aircraft


might occupy fewer than 50 pixels with no familiar profile view, no depth cues, and none of the perspective geometry that ground


level vision systems rely on. It blends into a background of taxiways, shadows, and tarmac clutter with no implicit scale reference. It is a deceptively hard visual problem, and it is exactly the kind of problem that matters most for defense and intelligence


a


pplications.


*Figure 1 – Left: A United Airlines A320 and Emirates aircraft photographed from a lateral perspective (top), and a Cessna 152 in profile view (bottom) — the familiar viewing geometry from which both humans and AI models learn to recognize aircraft . Right: Eight nadir imagery chips from the Maxar WorldView-3 RarePlanes 1 dataset, with large civil transport aircraft (top four) and small civil transport aircraft (bottom four). The dramatic shift in viewing geometry, pixel density, and scale reference between perspective and nadir imagery illustrates why classification that is trivial for a human observer becomes a fundamental challenge for models that lack an internalized physical world model.*


It is also, as our research reveals, a problem that exposes something fundamental about the current state of AI vision


,


and points toward where the field needs to go next.


## What We Did


We recently published


[“Evaluating Multimodal Large Language Models for Geospatial Object Identification and Enumeration in Overhead Imagery”](https://link.springer.com/chapter/10.1007/978-3-032-18474-0_28) in Springer’s Lecture Notes in Computer Science (LNCS) series. We benchmarked nine frontier multimodal LLMs (MLLMs) — GPT-4o, GPT-4o Turbo, GPT-4o Mini, Claude Opus 4.1, Claude Sonnet 4, Gemini 2.5 Flash, Llama 4 Maverick 17B, Grok-4 Fast Reasoning, and Grok-4 Fast Non-Reasoning — against 300 satellite image chips from the publicly available RarePlanes dataset. Each model was asked to count and classify aircraft into seven categories with no task-specific fine-tuning — a pure zero-shot evaluation.


The results split cleanly in two. On counting, the models performed well: Gemini 2.5 Flash reached nearly 88% accuracy, and most models landed in the 74–80% range. On classification, every model fell off a cliff. Gemini again led at around 55%; most others clustered between 25–33%. One of the sharpest failure points was distinguishing Civil Large from Civil Small Transport aircraft — a judgment that is fundamentally about size. Without ground sample distance (GSD) information giving the model a pixel-to-meter conversion, that judgment has no principled basis. The models were pattern-matching without any internalized understanding of physical scale.


That distinction — pattern-matching versus genuine physical understanding — is the crux of the matter.


## The World Model Problem


The AI research community has been converging on a provocative thesis: that the next major capability leap in AI will not come from scaling transformers further, but from equipping models with something closer to a world model — a persistent, structured internal representation of how the physical world works in space, time, and causality. Turing Award winner Yann LeCun has been among the most prominent voices making this argument. After more than a decade as Chief AI Scientist at Meta, he departed in late 2025 to found Advanced Machine Intelligence (AMI) Labs, where he serves as Executive Chairman, with the explicit mission of building world models grounded in physical reality. AMI’s architecture of choice is JEPA — the Joint Embedding Predictive Architecture — designed to learn structured representations of real-world sensory data rather than predicting outputs token by token. The company raised over a billion dollars in seed funding earlier this year, a signal of how seriously the field is taking this research direction.


The world model thesis holds that current large models, including today’s best MLLMs, are fundamentally limited by the nature of their training objective. They learn rich statistical associations over tokens and image patches, but they do not internalize the physical structure of the world. They do not know that a 737 is approximately 40 meters long, that WorldView-3 imagery has a ground sample distance of around 0.3 meters per pixel, and that therefore a 737 should span roughly 130 pixels at nadir. They cannot reason from first principles about what an object should look like given its known dimensions and the sensor’s geometry.


Our benchmark results are, in a sense, a direct empirical demonstration of this limitation. The counting task — is something there? how many? — leverages low-level saliency that general vision training already provides. The classification task — what specifically is this, given its size, shape, and spatial context? — requires exactly the kind of physical grounding that current models lack. The gap between 87% counting accuracy and 33% classification accuracy is not a prompting problem or a data problem. It is a reflection of a deeper architectural gap.


## Prompt Engineering as a Proxy for World Knowledge


This framing shapes how we think about our follow-up research. We are presenting


[“Evaluating Prompt Design Choices for Object Detection, Counting, and Classification in Overhead Imagery”](https://spie.org/defense-security/presentation/Evaluating-prompt-design-choices-for-object-detection-counting-and-classification/14032-16) at SPIE Defense and Security this month. The paper systematically examines how different prompting strategies affect MLLM performance on the same geospatial tasks — spatially guided prompts, multi-step interaction patterns, few-shot conditioning, and crucially, the explicit incorporation of GSD-based spatial grounding.


One way to read that research agenda is as prompt engineering. Another way to read it — and the way I find more intellectually honest — is as an investigation into how much world knowledge we can inject into a model at inference time to compensate for what it was not taught during training. When we provide a GSD value and a pixel-to-meter conversion in a prompt, we are not teaching the model physics; we are supplying, externally, the physical context that a true world model would maintain internally. When we provide few-shot examples of each aircraft class at operational resolution, we are approximating the kind of grounded visual exemplar library that a world model would build from experience.


The question our SPIE paper is probing, then, is: how far can explicit prompting take us in bridging the world model gap — and where does it run out?


## What Comes After Prompting


The honest answer is that prompt engineering is a bridge, not a destination. It can meaningfully improve performance — our early results suggest it can — but it operates within the constraints of what the underlying model architecture can support. There is a ceiling, and it is set by the absence of genuine spatial and physical reasoning.


The research directions that excite me most for this domain are those that begin to close that gap architecturally. Models trained on geospatially grounded data — where imagery is paired not just with labels but with sensor metadata, collection geometry, and known object dimensions — could internalize the pixel-to-meter reasoning that we must currently supply by hand. Agentic frameworks that allow models to query external knowledge sources (GSD tables, aircraft dimension databases, sensor specifications) mid-inference represent a nearer-term path to similar outcomes. And world model architectures that learn structured physical representations from overhead imagery directly could ultimately render much of this scaffolding unnecessary.


Overhead imagery is a demanding test domain precisely because the physics of remote sensing are unforgiving — small objects, unfamiliar geometry, no implicit scale. That same unforgiving quality makes it an unusually informative lens for understanding what today’s AI models actually know about the physical world. Our benchmark suggests the answer is: more than enough to be useful as a first-pass tool, and not yet enough to operate without human oversight or domain-specific grounding.


That gap is where the most interesting research lives right now.


*The full benchmark paper is available via Springer LNCS: [https://link.springer.com/chapter/10.1007/978-3-032-18474-0_28](https://link.springer.com/chapter/10.1007/978-3-032-18474-0_28) . The follow-up study on prompt design will be presented at SPIE Defense and Security: [https://spie.org/defense-security/presentation/Evaluating-prompt-design-choices-for-object-detection-counting-and-classification/14032-16](https://spie.org/defense-security/presentation/Evaluating-prompt-design-choices-for-object-detection-counting-and-classification/14032-16) . The evaluation platform is open source and available on GitHub [https://github.com/timklawa/gen-AI-Image-Eval-Engine](https://github.com/timklawa/gen-AI-Image-Eval-Engine) .*


* *This header image was generated using AI tools*


[Connect with an Expert](https://innodata.com/contact-us)


### Bring Intelligence to Your Enterprise Processes with Generative AI.


Innodata provides high-quality data solutions for developing industry-leading generative AI models, including diverse golden datasets, fine-tuning data, human preference optimization, red teaming, model safety, and evaluation.


[Talk to an Expert](https://innodata.com/contact-us/)


Follow Us


[Linkedin-in](https://www.linkedin.com/company/innodata-inc-)


[Facebook-f](https://www.facebook.com/Innodata.Inc/)


[X-twitter](https://twitter.com/innodata)
