---
schema_version: "1.0.0"
document_id: "0d35d4d6d7889cc836263a0d5353733cd470b4bbd95ff80ddee125ad6cbf8f35"
company_key: "yc-encord"
company: "Encord"
source_id: "yc-encord-news-import-59af355da1b0"
canonical_url: "https://encord.com/blog/encord-integrates-nvidia-cosmos-reason-and-embed/"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-24T06:09:22.733178+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:fc0591ce53aee794190e9e1a596ed29b020dbc73c403ad833079acb15a66be31"
---

# Encord Integrates NVIDIA Cosmos Reason and Embed

# Encord Integrates NVIDIA Cosmos Reason and Embed


[Eric Landau](https://encord.com/author/eric-landau/)


Co-Founder & CEO at Encord


June 3, 2026


|


5 min read


Summarize with AI


-
-
-


We're excited to announce the integration of two[NVIDIA Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) models - Cosmos Reason 2 and Embed - directly into the Encord platform. Both run natively on Encord's own infrastructure, so customers can use them in production from day one.


The Cosmos Reason agent automates prelabels on physical AI video. Cosmos Embed makes that video searchable by behaviour, not just by scene. Together, they turn raw video into structured, searchable training data.


*Cosmos Reason 2 in the Agents Catalog*


## Cosmos Reason: Automated Captioning And Pre-Labelling


Cosmos Reason is a vision-language world model. Given a video clip as input, it returns natural-language descriptions of the actions, objects, and scene context in the footage. Inside Encord, those descriptions arrive as prelabels attached to the right video segments. Annotators review and refine instead of starting from a blank canvas. Approved labels stream straight to the customer's training pipeline.


What this means in practice:


- A robotics team labelling dexterous manipulation no longer hand-types every grasp or release.
- An AV team captioning camera footage gets a usable starting point on every clip instead of writing each one from scratch.
- Industrial inspection teams get descriptions of anomalies, object states, and conditions without anyone watching the full reel first.


This means less time describing video, and more time improving the model.


## Cosmos Embed: Action-Aware Embeddings And Behaviour Search


Cosmos Embed is a video embedding model. Each embedding is calculated over an eight-frame window, capturing the action that happens across those frames. That makes a video dataset searchable by behaviour, using natural-language queries.


In practice, you can query for what's actually happening in a clip - not "car on a snowy road" but "car turning in a snow storm," not "highway scene" but "car overtaking another vehicle." That makes edge cases easier to find - the rare scenarios that decide whether a model ships safely.


### How They Work Together


- **Robotic manipulation.** Reason generates action and state descriptions for dexterous manipulation footage. Embed lets teams find the specific behaviours that need more training examples.
- **Autonomous vehicles.** Reason captions camera data at scale. Embed lets teams pull the exact driving scenarios that matter for their perception models - lane changes in rain, unprotected lefts, occluded pedestrians.
- **Industrial inspection.** Reason describes anomalies and object states in operational footage. Embed surfaces every clip where a defect occurs in a specific context.
- **Vision-Language-Action models.** Reason produces the grounded captions VLA training depends on. Embed makes the underlying dataset queryable by the behaviours those models need to learn.


For more information on Encord's Physical AI suite, click[here](https://encord.com/lidar/) .


[< Previous The Complete Guide to Security Video Annotation](https://encord.com/blog/security-video-annotation/)[Next > The Complete Guide to Data Labeling for Robotics](https://encord.com/blog/data-labeling-for-robotics/)


## Get the data right.


300+ of the best AI teams in the world use Encord.


[Take a tour](https://encord.com/explore-product/)[Book a demo](https://encord.com/book-demo/)
