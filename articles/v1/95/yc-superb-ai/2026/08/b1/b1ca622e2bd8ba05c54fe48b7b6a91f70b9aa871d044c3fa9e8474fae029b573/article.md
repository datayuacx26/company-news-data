---
schema_version: "1.0.0"
document_id: "b1ca622e2bd8ba05c54fe48b7b6a91f70b9aa871d044c3fa9e8474fae029b573"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/4d-human-action-data-robot-learning-smpl-en"
published_at: null
first_seen_at: "2026-08-11T08:13:22.531448+00:00"
fetched_at: "2026-08-11T08:13:24.354454+00:00"
content_hash: "sha256:ade9f1a7a9d26802bc2d5c20f0d6e316c0e75cc0151c60016226fc86635e421a"
---

# How Robots Learn Human Behavior: Building 5,000 4D Behavior Data Assets

Tech


How Robots Learn Human Behavior: Building 5,000 4D Behavior Data Assets


Hyun Kim


Co-Founder & CEO | 2026/08/11 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


“Human behavior and movement need to be converted into data so robots can learn from them.”


This is one of the most common statements we hear today in conversations about robotics and Physical AI. Most people agree with the direction. The real question comes next: How can human movement be converted into a form that robots can actually learn from?


This article is a record of how we answered that question while building 5,000 4D behavior assets in Phase 2 of Korea’s Sovereign AI Foundation Model Project, known in Korean as Dokpamo.


**🔗Related post:**[50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets](https://superb-ai.com/en/resources/blog/physical-ai-data-factory-dokpamo-phase-2-results-en)


**What Is 4D Behavior Data?**


4D behavior data records human movement across both 3D space and time. It is not simply a sequence of joint coordinates. Because body shape and pose are separated as parameters, the motion can be transferred to digital humans or robots with different body types.


# **Teach Robots by Teleoperation, or Let Them Learn by Watching Humans?**


There are two major approaches in the global robot learning data ecosystem.


One is **teleoperation** , which collects trajectories by having humans directly operate robots. China’s AgiBot demonstrated the scale of this approach by open-sourcing more than one million robot trajectory data points through[AgiBot World](https://agibot-world.com/) .


The other is **human behavior observation** , which captures people performing tasks from multiple viewpoints and converts their movements into data.


**▶ YouTube video:**[https://www.youtube.com/watch?v=3lRNe7hJ5Dk](https://www.youtube.com/watch?v=3lRNe7hJ5Dk)


These two approaches are often framed as competing methods. But based on what we have learned from operating the pipeline in practice, our view is slightly different. They are not a matter of which is better. They serve different roles.


Teleoperation data captures actions directly in the robot’s coordinate system, making it immediately useful for learning precise manipulation. However, robots, operators, and equipment all become bottlenecks, which means the amount of data can only grow linearly.


Human behavior data captures tool use, whole-body coordination, and prior knowledge about what people are doing and why, at much larger scale and with far greater diversity.


In practice, the two approaches are used together. **Human behavior data supports pretraining. Teleoperation data supports precise fine-tuning.** Simulation and retargeting connect the two.


There is one more important point. For robots such as quadrupeds or humanoids that may not yet be able to stand on their own in early development, teleoperation itself may not be possible. For these robots, human behavior data is essential for early-stage learning.


# **How We Counted 5,000 Behavior Assets**


In Phase 1, we structured household behaviors in Korean home environments into 50 scenarios: 15 in the kitchen, 13 in the living room, 12 in the bedroom, and 10 in other spaces. Across 50 locations, we captured 150 combinations of actions, totaling 7,500 recorded sequences.


**🔗 Related post:**[Superb AI Achieves Phase 1 Milestone in the State-Run Proprietary AI Foundation Model Project (1.08M Robot Data)](https://superb-ai.com/en/resources/blog/superb-ai-phase-1-robot-data-en)


The 5,000 behavior assets built in Phase 2 are the sequences that passed our quality criteria from the original 7,500 recordings. Each sequence is approximately 30 seconds to one minute long on average.


# **From Video to Behavior Asset**


1. **Multi-view Video:** Human movement captured simultaneously from multiple viewpoints
2. **3D Joint Estimation:** Three-dimensional estimation of joint positions
3. **SMPL Parameter Fitting:** Fitting the motion into body shape and pose parameters
4. **Motion Retargeting:** Transferring the motion to digital humans with different body types


The core of the process is SMPL fitting. SMPL, or Skinned Multi-Person Linear Model, represents the human body by separating body shape and pose. As a result, the output is not just a list of coordinates. It becomes 4D mesh data that includes both body shape and pose information.


**


# **Lessons from the Process: Occlusion and Mirrors**


This pipeline was not stable from the beginning. In the early stages, joints would sometimes jump to positions that were physically impossible for the human body, or motion would break between adjacent frames instead of continuing smoothly.


Errors occurred mainly in scenes where the body was occluded by furniture or where the camera viewpoints were not diverse enough. Narrow kitchens and bathrooms were the most difficult environments.


We applied three methods together to address these issues.


First, we used a state-of-the-art model trained with a human pose prior to estimate positions that are anatomically plausible. Second, we applied temporal smoothing to limit unrealistically fast movement between frames. Third, when a segment still failed to meet our confidence threshold, we discarded it and filled the gap through interpolation using the frames before and after it.


As a result, the error rate decreased significantly compared with the initial approach, and **more than 98% of the 7,500 recorded sequences** were processed successfully.


# **How a Single Behavior Asset Multiplies**


When body shape and pose are separated, one behavior asset does not remain just one asset. Under the current operating standard, **one behavior can be expanded across five human body models and four to ten rendering viewpoints** . Combinations with more than 30 background locations can also be added.


What matters even more is that there is no fixed ceiling to these multipliers. Human body models can be added continuously, and viewpoints are simply rendering settings. This is structurally different from teleoperation, where data collection scales in proportion to robot operating time.


# **Adding Language to Behavior**


As a separate task, we added situation and behavior-context captions to Phase 1 frames using VLM-generated drafts followed by human-in-the-loop expert review. This language layer becomes the foundation for multimodal integration of language and action. In the next article, we will explain the 50 3D space assets that provide the environments behind these behaviors.


# **Frequently Asked Questions**


## **Q. Which is better: teleoperation data or human behavior data?**


They serve different roles. Human behavior data is strong for pretraining at scale, diversity, and prior knowledge about intent. Teleoperation is strong for precise fine-tuning based on the robot’s coordinate system. In practice, the two approaches are used together.


## **Q. If the data was captured with only a small number of actors, how do you ensure body-type diversity?**


At the capture stage, we secure diversity in behavior and style. Body shape is separated through SMPL parameters, which means it can be diversified without the same constraints during post-processing. Retargeting transfers the same motion to digital humans with different body types.


If you are considering building behavior data, leave your information below. A Superb AI specialist will contact you shortly.


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Tech 50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/physical-ai-data-factory-dokpamo-phase-2-results-en)[Tech How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)[Tech Building Superb AI’s Synthetic Data Pipeline with NVIDIA Isaac Sim Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/synthetic-data-pipeline-using-nvidia-isaac-sim-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
