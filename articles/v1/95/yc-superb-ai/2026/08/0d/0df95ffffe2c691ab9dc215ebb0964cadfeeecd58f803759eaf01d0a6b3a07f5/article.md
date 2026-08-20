---
schema_version: "1.0.0"
document_id: "0df95ffffe2c691ab9dc215ebb0964cadfeeecd58f803759eaf01d0a6b3a07f5"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/interactive-object-assets-robot-manipulation-en"
published_at: null
first_seen_at: "2026-08-19T12:47:31.411698+00:00"
fetched_at: "2026-08-19T12:47:33.093131+00:00"
content_hash: "sha256:1a149b3ee91a45291aa0a4c2059581e054603ef2ed88c43abde7a636c320ee7b"
---

# For a Robot, a Drawer Must Open, Not Just Be Seen: Building 10,000 Interactive Object Assets

Tech


For a Robot, a Drawer Must Open, Not Just Be Seen: Building 10,000 Interactive Object Assets


Hyun Kim


Co-Founder & CEO | 2026/08/19 | 22 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


Even if a robot perfectly perceives the room around it, there is not much it can do if it cannot handle objects.


Manipulation — picking up a cup, opening a door, pulling out a drawer — is the very reason home robots exist. To learn manipulation, robots need data at the level of individual objects.


In Phase 2 of **Korea’s Sovereign AI Foundation Model Project** , known in Korean as **Dokpamo** , we built 10,000 object assets from household items that robots are likely to manipulate frequently.


In the structure introduced in Part 2, these are the objects placed on top of the 3DGS background and responsible for actual interaction.


****


### **What Is an Interactive Object Asset?**


An interactive object asset is object data that goes beyond separating an object at the pixel level through instance segmentation. For objects that move, such as doors and drawers, it also includes articulation and mechanical properties — such as the axis and range of motion — so the object can operate realistically inside a simulator.


# **Utilizing Phase 1 Assets Instead of Capturing New Data**


This task did not begin with new data capture.


The 300,000 frames processed in Phase 1 already contained household objects in real Korean home environments, preserved in their actual usage contexts.


We uniformly sampled frames, used object detection to filter for valid segments where people were performing actions, narrowed down the source data, and extracted individual objects into object-level assets.


This is how a data factory works: raw material that is collected well once becomes the source for the next task.


# **Different Tasks for Humans and AI**


Labeling was not done by humans manually painting every pixel. Foundation models such as SAM, along with computer vision algorithms for refining boundaries, performed the first pass. A VLM conducted the initial review. **Humans focused on the final quality check.**


This structure lets AI handle volume while humans take responsibility for final quality. The AI-assisted labeling workflow refined since Phase 1 was applied directly to object asset creation.


# **How to Interpret the Quality Bar**


The quality bar was an IoU of 0.7 or higher and 90% semantic accuracy. Two points are important.


First, **0.7 is not the achieved average. It is the passing threshold applied to the entire dataset.** Data below the bar was rejected during review and sent back for rework.


Second, the difficulty of the targets matters. These were not clean objects captured from the front in controlled settings. They were objects in real usage contexts inside actual homes, and the requirement was applied at the pixel level.


The quality of training data is not determined by the best-performing sample. It is determined by the lower bound across the full dataset. That lower bound is what we managed.


# **The Hardest Problem Was Occlusion**


The hardest challenge in segmentation was not transparent or reflective materials, as we initially expected. It was **occlusion** .


The strength of this dataset — that it captures real usage contexts — also made segmentation difficult. In a natural home environment, chairs are often tucked under tables or occupied by people, so scenes where the full object shape is clearly visible are actually rare.


# **What Makes the Assets Interactive: Eight Articulated Object Types**


Many household objects are not rigid bodies.


In this phase, we applied articulation modeling to eight types of household objects with opening and closing mechanisms: **cabinets, refrigerators, washing machines, drawer units, curtains, storage cabinets, wardrobes, and sink cabinets.**


This allows them to open and close realistically inside a simulator.


Objects without joints, such as cups or plates, are built as rigid-body objects.


The most difficult object, unexpectedly, was the curtain.


Reproducing the fluttering movement of fabric in a simulator is not impossible, but it is computationally expensive. So we simplified curtains as objects that slide sideways.


This kind of judgment — balancing physical realism against simulation cost — is required throughout the asset creation process.


Concept diagram — opening and closing an object with articulation properties, including axis and range of motion


# **What Is Happening Globally**


The approach of turning objects into 3D assets is moving in the same direction as the global frontier.


Meta’s SAM 3D is a research effort around segmentation-based 3D object asset creation, while the AgiBot World dataset has been released with more than 3,000 types of real-world objects.


What makes our work different is that we executed this at scale for the specific domain of Korean home environments, including articulation properties.


# **Frequently Asked Questions**


### **Q. Is an IoU of 0.7 considered high?**


The number cannot be evaluated in isolation. For easy classes in a controlled benchmark, it may not be a high threshold. But as a lower-bound passing threshold applied to the entire dataset of real-world household objects, where occlusion is common, it carries a different meaning.


### **Q. Was all labeling done manually by humans?**


No. The workflow was divided into three stages: foundation models such as SAM and computer vision algorithms performed the first pass, a VLM conducted the initial review, and humans handled the final quality check.


### **Q. What kinds of objects are included?**


The assets focus on household objects that robots are likely to manipulate frequently. Eight articulated object types — cabinets, refrigerators, washing machines, drawer units, curtains, storage cabinets, wardrobes, and sink cabinets — were built in an interactive form.


The next article will be the final part of the series. It will cover the synthetic data pipeline that combines spatial, object, and behavior assets to automatically generate training data.


*Superb AI is a Vision Intelligence company that transforms visual data from industrial environments into intelligence enterprises can use.*


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Tech Not a Photo, but a Space You Can Walk Into: Reconstructing 50 Korean Homes with 3DGS Hyun Kim Co-Founder & CEO | 5 min read](https://superb-ai.com/en/resources/blog/3d-gaussian-splatting-korean-home-digital-twin-en)[Tech How Robots Learn Human Behavior: Building 5,000 4D Behavior Data Assets Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/4d-human-action-data-robot-learning-smpl-en)[Tech 50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/physical-ai-data-factory-dokpamo-phase-2-results-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
