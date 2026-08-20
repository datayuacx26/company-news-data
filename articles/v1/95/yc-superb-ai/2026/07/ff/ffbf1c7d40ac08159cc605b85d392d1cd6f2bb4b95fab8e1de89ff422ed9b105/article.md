---
schema_version: "1.0.0"
document_id: "ffbf1c7d40ac08159cc605b85d392d1cd6f2bb4b95fab8e1de89ff422ed9b105"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/cvpr-zero-data-flywheel-en"
published_at: null
first_seen_at: "2026-07-22T15:16:12.909358+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:f1ab102e59a6fb6c9335b99b8207d9640857019755c68b936f8d7646c415e125"
---

# How to Restart an AI Project That Stalled for Lack of Data—with Just 10 Images

Insight


How to Restart an AI Project That Stalled for Lack of Data—with Just 10 Images


Hyun Kim


Co-Founder & CEO | 2026/07/01 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


## *Why General-Purpose Models Fail in Industrial Environments—and What Makes ZERO Different*


# **Key Takeaways**


- The most common reason industrial AI projects stall is not the model, but the **data** . When collecting and labeling thousands or tens of thousands of images is treated as a prerequisite for deployment, nothing can begin until data preparation is complete.
- General-purpose models do not solve this problem. In the CVPR 2026 Foundational Few-Shot Object Detection Challenge, leading general-purpose model baselines recorded **accuracy below 1%** across multiple industrial domains. Under the same conditions, a system built on **ZERO** , Superb AI’s industry-focused Vision Foundation Model, ranked first on average across 20 domains with an mAP of 53.9.
- What makes the difference is not few-shot learning as a technique alone, but the **model being adapted** . Adaptation using around 10 examples per class works only when the underlying model is already strong in industrial domains.
- **ZERO changes the starting point for AI adoption.** Instead of waiting until data preparation is complete, teams can first validate feasibility using around 10 images they already have, then reach production-level accuracy through a data flywheel built on real-world operational data.


# **The Real Reason AI Projects Stall**


Companies looking to deploy Vision AI in industrial environments tend to encounter the same point of failure. It is not the stage where they compare model performance. It comes earlier, **while preparing the training data.**


In the traditional deployment model, data is a prerequisite. Teams must define the target objects, collect thousands or tens of thousands of images from the field, and have specialists complete the labeling before model training can even begin.


This preparation can take weeks or months. The same process must then be repeated whenever a new product is introduced, a production line changes, or the system is expanded to another factory.


That is why many projects stall at the proof-of-concept stage—not because the technology does not work, **but because too much of the cost and effort required to get started is front-loaded.**


There have been attempts to change this structure. The most common expectation is straightforward: general-purpose AI models have improved significantly, so why not simply use one of them?


# **The Failure of General-Purpose Models Is No Accident**


Objective data from this year’s CVPR Few-Shot Object Detection Challenge shows what happens when that expectation meets real industrial environments.


The organizers created baselines using representative general-purpose models and evaluated them across 20 industrial domains, including X-ray, thermal, and aerial imagery. On many of the datasets, **accuracy fell below 1%.** For models trained on general internet imagery, industrial data was effectively a world they had never seen.


Under the same conditions—with only 10 examples per class— **the ZERO-based system ranked first with an average mAP of 53.9 across the 20 domains** . It placed first in five of the seven industry categories, and its score of 64.4 in the Industry category was highlighted in the organizers’ presentation.


The conclusion is clear:


**The ability to adapt from limited data does not depend on the adaptation technique alone. It depends on the foundation model beneath it.** Only a foundation model designed with industrial domains in mind can turn 10 examples into a meaningful starting point. Few-shot adaptation on top of a general-purpose model breaks down. Few-shot adaptation on top of an industry-focused model works.


# **A New Starting Point: Validate Before Data Preparation, Not After**


ZERO does not change the entire deployment process. It changes the **order** .


Traditionally, teams had to complete data preparation before they could begin validation. Now, they can first test feasibility using around 10 images they already have and decide whether to invest further only after the potential has been confirmed.


This reversal has significant implications for AI adoption. Companies can spend months of data preparation costs after gaining confidence in the technology, rather than before.


Here is what that shift looks like across different industries.


### **Manufacturing: Responding to New Defects**


Consider a new defect discovered during the visual inspection of electronic components. Under the traditional approach, the response would remain on hold until thousands of images of that defect had been collected. With a ZERO-based approach, teams can evaluate whether the defect is detectable on the same day using around 10 available samples. Detection can begin while additional field data continues to accumulate.


### **Logistics: Adapting to Changing Product Assortments**


The products handled in a logistics center change continuously depending on the season and the customer. Instead of launching a new retraining project whenever a new packaging format arrives, teams can immediately expand the system’s recognition coverage using a few examples captured when the items are received.


### **Healthcare and Precision Imaging: Overcoming Specialized Domains**


Medical imaging is one of the areas where general-purpose models are least effective. Even category names are often specialized abbreviations, making it difficult for the model to understand what it is being asked to find. ZERO addresses this ambiguity through multimodal prompting that combines text, visual examples, and contextual information. In the CVPR 2026 challenge, its largest lead over the second-place team—more than nine points—came in the particularly challenging Medical category.


# **From Initial Validation to Production: Start with 10 Images, Improve Through a Data Flywheel**


One point is important to clarify: **10 images are the starting point, not the finish line.**


The challenge demonstrated world-class adaptation under the same limited-data conditions, but the level of accuracy required in production differs by use case. For tasks such as inspection lines, where high recall is essential, there is still a necessary path from proof of concept to production readiness.


What makes a ZERO-based deployment different is that **this path becomes a flywheel in which data accumulates while the model is already operating** .


Once a model that began with 10 images starts running inference in the field, real operational data accumulates naturally. Curation technology then identifies the samples most valuable for model improvement, automated labeling rapidly produces ground-truth annotations, and the model is updated with the resulting data.


Superb AI’s expertise in data curation and labeling automation, developed through its data infrastructure business, supports this entire process.


The same family of techniques also contributed to the winning CVPR solution. Selecting the most effective examples through curation and expanding labels through AI-powered pseudo-labeling helped create the performance advantage.


The structure can be summarized simply: **Start quickly with 10 images. Reach production-level performance with real-world data.** Data preparation is no longer a wall that blocks deployment. It becomes an asset that grows naturally through operations. Compared with conventional approaches, this method can potentially reduce data collection costs by more than 90%.


# **Start with a Foundation That Has Already Been Validated**


The most objective answer to the question, “Will this work at our site?” is independent validation.


The ZERO-based system ranked first on average across 20 distinct industrial domains in the CVPR 2026 Foundational Few-Shot Object Detection Challenge. For enterprises that need to expand AI across multiple production lines and factories, the most important capability is not excellence in one narrow field. It is consistent performance and resilience across widely different environments.


More details about the result and the technology behind it are available in[Part 1](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en) , which covers the win, and[Part 2](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en) , which provides a technical walkthrough of the winning solution.


The latest version of ZERO is available now on[AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk) . The domain adaptation capabilities validated through the challenge will also be introduced progressively to Superb Platform.


Before spending months preparing data, consider first testing feasibility with around 10 images from your own environment. That is the new starting point.


# **Frequently Asked Questions**


### **Q. Can 10 images really deliver production-level accuracy?**


Ten images are the starting point for validating feasibility. The CVPR 2026 challenge evaluated exactly this condition—10 examples per class—across 20 industrial domains, and the ZERO-based system ranked first on average.


However, production accuracy requirements vary by task. A ZERO-based deployment begins quickly with 10 images, then feeds data generated in the field back into the model through curation and automated labeling until it reaches the required production level.


### **Q. Why not simply use a general-purpose AI model?**


General-purpose models have clear limitations in industrial domains. In the CVPR 2026 challenge, representative general-purpose model baselines recorded accuracy below 1% on multiple industrial datasets. The gap becomes especially wide in domains far removed from general internet data, such as X-ray, thermal, and precision inspection imagery. These environments require a specialized model developed with industrial data in mind.


### **Q. What images should I prepare?**


Prepare around 10 field images per class in which the target object is clearly visible. The quality and diversity of the examples are more important than the number of images alone. Superb AI also provides curation technology that helps identify the most effective examples.


### **Q. What happens when a new defect or product is introduced?**


The recognition scope can be expanded using only a few examples of the newly added object. This is one of the biggest differences from the traditional approach, which requires a new retraining project involving thousands of images whenever conditions change.


### **Q. How can I get started?**


The latest version of ZERO is available now on AWS Marketplace. To explore a proof of concept using images from your own environment, contact Superb AI through **\[[Contact Us](https://superb-ai.com/en/company/contact) \]** .


**\[Related Content\]**


- [Part 1: Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en)
- [Part 2: How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)
- [Introducing ZERO: Korea’s First Vision Foundation Model Tailored for Industry](https://superb-ai.com/en/resources/blog/introducing-vfm-zero)
- [Try ZERO now in AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk)


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Insight Three Years of the Few-Shot Object Detection Challenge: Mapping the Global Vision AI Landscape Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/cvpr-challenge-global-vision-ai-landscape-en)[Insight ⑪ Germany's Physical AI Moment: Siemens, BMW, and the Robot Unicorn Counteroffensive Hyun Kim Co-Founder & CEO | 15 min read](https://superb-ai.com/en/resources/blog/physical-ai-series-11-germany-s-physical-ai-en)[Insight ⑩ Big Tech Physical AI Trends (2): Tesla vs. Amazon Strategy Breakdown Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/physical-ai-series-10-tesla-vs-amazon-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
