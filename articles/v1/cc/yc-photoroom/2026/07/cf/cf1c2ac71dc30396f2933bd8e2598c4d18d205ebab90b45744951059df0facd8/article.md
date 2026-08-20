---
schema_version: "1.0.0"
document_id: "cf1c2ac71dc30396f2933bd8e2598c4d18d205ebab90b45744951059df0facd8"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-16e4c55287d6"
canonical_url: "https://www.photoroom.com/customer-stories/hypercinema"
published_at: null
first_seen_at: "2026-07-23T22:00:21.424777+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:8d477f5b936d36d65626f726eaee7cc7f28e34b8471d1b981414c4c799841683"
---

# How HyperCinema achieved 100% image accuracy with Photoroom's API

*HyperCinema tested multiple background removal APIs before choosing Photoroom. Competing solutions failed approximately 30% of the time, producing errors such as cutting off parts of a subject's face. Photoroom's Remove Background API delivers accurate results for HyperCinema 100% of the time, with consistent handling of hair, posture, and lighting conditions.*


### **Life before Photoroom**


-


Limited image variety for AI model training


-


No reliable automated background replacement


-


Other tools failed 30% of the time, clipping faces and losing hair detail


### **Life with Photoroom**


-


100% background removal accuracy


-


API integrated in approximately five minutes


-


Clean handling of hair, posture, and lighting


-


Fully automated image preprocessing


> "Using Photoroom has allowed us to deliver a truly unique, hyper-personalized experience for our customers. Without it, we wouldn't be able to get the flexibility into our models that allow us to create these one-of-a-kind creative experiences."
>
>
> — Gareth Hordyk, Co-founder and CTO, HyperCinema


[HyperCinema](https://www.hypercinema.ai/) builds personalized AI cinematic experiences from Auckland, New Zealand.


**Here’s how it works:** You step into the venue, have around 15 photos taken, then the photos are turned into an instant album used to mimic the last five years of your life through a variety of situations. Next, you answer a short personality quiz about your likes, hopes, and dreams.


HyperCinema uses the album and quiz answers to train a highly-specialized AI image and language generating model that ultimately creates your likeness. From there, the company’s AI generates a unique 15-minute film where you’re the hero of your own story, climbing mountains, winning boxing matches, working as a chef, or crossing seas as a Viking warrior. Every film is different, generated once, and never repeated.


Making that experience truly inspiring, convincing, and memorable required solving a specific image problem that turned out to be harder than the team expected.


*A personalized cinematic moment created by HyperCinema. Source: HyperCinema*


## **The challenge: building flexible AI training data at scale**


HyperCinema's team faced two connected challenges: their AI needed far more image variety than raw customer photos could provide, and the tools available to create that variety kept failing.


### **1. HyperCinema’s AI model needed training images for various scenes**


For HyperCinema's films to look convincing, the AI model has to place each customer into dozens of different situations. For example, a character on a mountaintop needs a puffer jacket, not a business suit, and the background has to match the clothing. Without variety in its training data, the AI struggles to produce the right images.


Co-founder and CTO Gareth Hordyk realized the solution: swap out both the character’s clothing *and* the image background *before* training the AI models. This way, they could turn a small set of real customer photos into many different realistic scenes before training.


The company’s technology could already handle clothing swaps. But replacing backgrounds realistically was a different problem entirely. It meant tracing cleanly around every strand of hair without clipping the face, understanding where the person ends and the background begins, and adjusting for different lighting conditions across hundreds of photos per session.


### **2. Other background removal tools failed on complex images**


Hordyk and his team tested several background removal solutions. The results were consistent in the wrong way: the tools worked about 70% of the time, according to Hordyk, who shares that “the other 30% of the time, they did unsettling and unacceptable things like half chopping off a person's face" as part of the background replacement.


For a product built on hyper-personalization, where every image feeds directly into AI training, even occasional failures created unusable outputs. A botched cutout didn't just look bad. It meant the AI model learned from corrupted data, which degraded the entire experience downstream.


## **Why HyperCinema chose Photoroom's background removal API**


Hordyk tried Photoroom and saw the difference immediately. Where other tools struggled with hair, posture, and complex lighting, Photoroom's[Remove Background API](https://www.photoroom.com/api/remove-background) completed accurate cutouts across every test image. The replacement backgrounds matched what each character was wearing, and the results were consistent regardless of lighting conditions.


> "Photoroom is rock-solid. The best part is we don't have to think about it that much, because it's that reliable and bullet-proof for us to use."
>
>
> — Gareth Hordyk, Co-founder and CTO, HyperCinema


The team set up the API in a few minutes, which enabled a fully automated workflow:


-


**The five-minute setup:** It took approximately five minutes to integrate the Photoroom API into HyperCinema's production system.


-


**A fully automated workflow:** With the API connected, each batch of customer photos goes through automated background removal and[addition of new backgrounds](https://www.photoroom.com/batch/ai-backgrounds) before the training step, with no manual intervention needed.


## **Results: 100% reliable image processing, fully automated**


Since integrating the Photoroom API, HyperCinema has seen:


-


**100% background removal accuracy** compared to roughly 70% with competing tools, meaning zero corrupted training images reaching the AI model.


-


**Consistent handling of complex images** , including hair detail, varied postures, and different lighting conditions across hundreds of photos per session.


-


**A five-minute API integration** from first API call to production-ready workflow, with no heavy engineering required.


-


**Fully automated preprocessing** , freeing the team to focus on the AI engine and business growth rather than fixing image quality issues.


Beyond quality workflows and results, one thing that has impressed Hordyk most is Photoroom’s reliability.


> "It's been great to engage with Photoroom’s team, who really care about our success and are always actively working to help us improve our system and make it more efficient."
>
>
> — Gareth Hordyk, Co-founder and CTO, HyperCinema


After a successful initial seven-week run at its Auckland venue, HyperCinema is expanding the technology into B2B and B2C marketing, with Photoroom continuing to handle the image preprocessing that underpins every personalized experience.


*An AI-generated scene from a HyperCinema project, placing the customer into a personalized action sequence. Source: HyperCinema*


Photoroom's API gives developers and businesses programmatic access to best-in-class AI background removal and replacement. It processes images in seconds, integrates in minutes, and is trusted by[tech companies](https://www.photoroom.com/industry/tech) like HyperCinema that are building at the cutting edge of generative AI.
