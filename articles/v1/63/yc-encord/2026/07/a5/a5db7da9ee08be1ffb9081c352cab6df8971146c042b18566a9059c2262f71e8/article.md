---
schema_version: "1.0.0"
document_id: "a5db7da9ee08be1ffb9081c352cab6df8971146c042b18566a9059c2262f71e8"
company_key: "yc-encord"
company: "Encord"
source_id: "yc-encord-news-import-59af355da1b0"
canonical_url: "https://encord.com/blog/webinar-recap-nvidia-cosmos-3/"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-21T18:06:14.355617+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:a3d2701e358a119d6a467f0709186ea3c603d81b6dd98836e5b61ba1dbca9a12"
---

# [Webinar Recap] How NVIDIA Cosmos 3 Is Changing the Way Physical AI Teams Build Training Data

# \[Webinar Recap\] How NVIDIA Cosmos 3 Is Changing the Way Physical AI Teams Build Training Data


[Eric Landau](https://encord.com/author/eric-landau/)


Co-Founder & CEO at Encord


July 7, 2026


|


5 min read


Summarize with AI


-
-
-


*A recap of our joint webinar with NVIDIA on accelerating robotics training with world foundation models.*


Encord went live with NVIDIA to demo something we've been genuinely excited about: NVIDIA Cosmos 3 running natively inside the Encord platform to automatically pre-label egocentric robot video.


In 30 minutes, Nick Rajpal (VP of Physical AI at Encord), Chhavi Nijhawan (Senior Product Marketing Manager, NVIDIA Metropolis), and Kai Kato (Founding US Engineer, Encord) covered the physical AI landscape, what Cosmos 3 actually is under the hood, and a live end-to-end demo of it running inside a real annotation pipeline.


Here's what we covered.


## Why multimodal reasoning matters for physical AI


Nick opened by zooming out to where physical AI has come from.


A few years ago, robots operated on classical perception stacks, detecting whether something was present and executing a pre-coded response. Simple, brittle, and limited. What's changed is the emergence of vision language models, VLAs (vision language action models), and world models that give robots something closer to contextual understanding.


Annotation pipelines for robotics used to be largely manual or semi-automated. With reasoning models in the loop, you can have AI auto-generate prelabels as a first pass, turning a slow, human-only process into a collaborative one between humans and models.


As Nick put it: the developments over the last 12 months have had a real, measurable impact on what's possible for physical AI teams building at scale.


## What is NVIDIA Cosmos 3?


Chhavi walked through the backstory before getting into the model itself.


Training physical AI models is genuinely hard. The real world is infinite and unpredictable. Even with teleop data, simulation data, and internet video, most teams find it's still not enough to cover the long tail of edge cases their models need to handle. World foundation models exist to close that gap, learning the dynamics of the physical world from diverse multimodal data so you can generate training scenarios at scale rather than capturing them all in the real world.


Cosmos 3 is NVIDIA's latest release in that lineage, and it represents a significant step up. It's an omni model, meaning it natively takes in text, image, video, audio, and action as inputs, and generates video, audio, and action as outputs. Previous models handled some of these; Cosmos 3 handles all of them in a single architecture.


The key architectural innovation is a mixture of transformer design with two towers working together:


- An autoregressive reasoning tower: responsible for understanding inputs: video, text, audio, and prompts
- A diffusion-based generator tower: responsible for generating outputs: images, video, actions


These two towers are connected through shared attention, which means the model reasons through the input before generating the output, rather than treating them as separate tasks.


The practical implication: teams that previously ran fragmented pipelines (one model for reasoning, one for generation, one for action) can now consolidate into a single model. Cosmos 3 leads across 9+ leaderboards for physical AI capabilities, and it's fully open, available to fine-tune and post-train for specific use cases.


For teams focused specifically on reasoning and vision understanding, Chhavi clarified that you can use just the reasoning tower as a standalone VLM. A 64B parameter Cosmos 3 Super becomes a 32B VLM when used this way.


## The live demo: Cosmos 3 inside Encord


The scenario: a robotics team has collected egocentric video (footage from the robot's perspective during manipulation tasks) and needs to generate structured training data for a VLA model. Historically, that means annotators watching every clip and manually writing descriptions from scratch. With Cosmos 3 in the loop, the process looks different.


### Step 1: Create a project with a pre-labeling agent


Inside Encord, Kai created a new annotation project and added the Cosmos 3 segment videos agent from the platform's agent catalog. The setup is straightforward: you configure the agent to match your ontology (the schema of labels you want to produce) and write a prompt that tells Cosmos exactly how you want the captions structured.


Teams have very specific requirements for how their labels should read. For example, which hand is doing what, how granular the action segments should be, whether you want individual micro-actions or grouped sequences. The prompt is where you specify all of that. Cosmos will follow it consistently across every clip in the dataset.


### Step 2: Run the pipeline


Once the agent is configured, hitting go live sends every video in the dataset through Cosmos 3 for pre-labeling. Kai had already run this on a prepared dataset, so the demo jumped straight to the results.


### Step 3: Review the outputs


On a clip of someone picking up a water bottle, Cosmos had correctly segmented the video and generated captions at the segment level, describing what the left hand was doing, what the right hand was doing, the object being handled, and the action taking place. On a more complex clip involving shirt folding, the segmentation held up across multiple distinct actions.


The key point Kai made: these outputs aren't meant to be perfect. They're meant to be a strong starting point. Annotators review, refine, and correct where needed.


The faster you move through that loop, the faster you can iterate on your VLA model. And when you find new edge cases, you send them back through the same pipeline.


## Three things to take away


1. **The annotator's job is changing.** With Cosmos doing the first pass, annotators shift from raw labeling to review and refinement. That's not a reduction in the importance of human judgment, it's a reallocation of it toward the decisions that actually require expertise.
2. **One model is better than four fragmented pipelines.** Cosmos 3's unified architecture means teams can stop stitching together separate models for reasoning, generation, simulation, and action. Fewer handoffs, fewer integration points, fewer things to break.
3. **Iteration speed compounds.** The faster you can move data through a pipeline and get it back into training, the faster your models improve. Cosmos doesn't just speed up a single annotation run, it accelerates every subsequent one too.


## Watch the recording


[< Previous What Is AI Data Labeling? Definition, Types and the Process](https://encord.com/blog/what-is-ai-data-labeling/)[Next > Build vs buy: A decision framework for data labeling tools](https://encord.com/blog/build-vs-buy-data-labeling-tools/)


## Get the data right.


300+ of the best AI teams in the world use Encord.


[Take a tour](https://encord.com/explore-product/)[Book a demo](https://encord.com/book-demo/)
