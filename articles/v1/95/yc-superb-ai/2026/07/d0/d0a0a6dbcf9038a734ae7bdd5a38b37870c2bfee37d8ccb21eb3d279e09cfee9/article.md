---
schema_version: "1.0.0"
document_id: "d0a0a6dbcf9038a734ae7bdd5a38b37870c2bfee37d8ccb21eb3d279e09cfee9"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en"
published_at: null
first_seen_at: "2026-07-22T15:16:12.909358+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:9f65a51ca55ae2daa7a5b8d1cdded9ca1873af24175ac68e8ebb75cc9483ff16"
---

# Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge

Announcements


Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge


Hyun Kim


Co-Founder & CEO | 2026/06/18 | 10 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


# **Key Takeaways**


- Superb AI took **first place in the Overall Track of the CVPR 2026 Foundational Few-Shot Object Detection Challenge** . With an average mAP of **53.9** , the company rose from fourth place with a score of 47.2 last year to the top of the leaderboard in just one year.
- The winning system was built on **ZERO** , Superb AI’s proprietary Vision Foundation Model designed for industrial applications. It achieved the **highest average score across 20 specialized domains** spanning X-ray, thermal, aerial, and other forms of imagery, ranking first in five of the seven industry categories.
- Superb AI became the **first Korean company to win the challenge** , outperforming **17 participating teams and more than 200 submissions** , including a joint academic-industry team from Fudan University and Lenovo in China.
- The latest version of ZERO, which served as the foundation of the winning solution, is **available now on AWS Marketplace** .


Superb AI has reached the top of an international AI challenge held at CVPR 2026, one of the world’s most prestigious conferences in computer vision. Built around ZERO, the company’s proprietary Vision Foundation Model designed for industrial applications, the system took first place in the Overall Track of the Foundational Few-Shot Object Detection Challenge. The result marks a major leap from fourth place in the same challenge last year. It was officially announced on June 4, 2026, local time, at the CVPR Open-World Vision (VPLOW) Workshop in Denver, Colorado.


“This win is more than an award. It demonstrates that an industry-focused strategy and efficient methodology can achieve world-class results without relying on costly, large-scale infrastructure,” said Hyun Kim, CEO of Superb AI. “We will continue focusing not only on benchmark performance, but on building AI that works in real-world industrial environments.”


Organized by Carnegie Mellon University (CMU) and Roboflow, the challenge has been held annually since 2024 and marked its third edition this year. After placing fourth and receiving an honorable mention last year, Superb AI reached the top of the leaderboard this year. This is the first time a Korean company has won the challenge.


# **What Is Few-Shot Object Detection?**


### **Recognizing New Objects from Just a Few Examples**


Few-shot object detection enables an AI model to identify new objects using **only around 10 examples per class** , rather than tens of thousands of training images. The challenge evaluates whether AI can be deployed immediately across industrial imagery in healthcare, manufacturing, agriculture, logistics, aviation, and other fields without extensive data labeling and training.


The Roboflow20-VL dataset used this year consists of 20 specialized domains, including X-ray, thermal, and aerial imagery, that differ significantly from general internet data. It therefore serves as a demanding test of the limitations of general-purpose models. The official baselines prepared by the organizers using widely adopted models such as GroundingDINO and Qwen2.5-VL recorded scores below 1% on multiple datasets, illustrating the difficulty of the evaluation.


# **Why Few-Shot Object Detection Is Critical for Industrial AI**


For companies looking to deploy Vision AI in industrial environments, the first major barrier is **data** . Teaching a model to recognize a new product defect, a specific type of medical image, or an object unique to a particular site typically requires collecting and labeling thousands or tens of thousands of images individually. The process is costly and time-consuming, and must often be repeated whenever the model is transferred to a new environment.


Few-shot object detection directly addresses this bottleneck. If a model can quickly adapt to new objects using only a few examples, organizations can **substantially reduce data collection and labeling costs while deploying AI to new sites much faster** . The challenge provided independent validation that Superb AI’s technology can perform across 20 distinct industrial domains.


# **Highest Average Score Across 20 Industrial Domains**


Seventeen teams submitted more than 200 entries to this year’s Overall Track. The 2026 challenge also introduced a stricter qualification requirement: only teams that surpassed the previous year’s highest score were eligible for an award.


Superb AI achieved an average mAP of 53.9 across the 20 domains, with a precise leaderboard score of 53.866. This placed the company 2.3 points ahead of the second-place joint team from Fudan University and Lenovo, which scored 51.6. Superb AI’s result exceeded the previous year’s best score of 50.1 by 3.8 points and outperformed the organizers’ official GroundingDINO baseline of 33.3 by more than 20 points.


The defining strength of the result was consistency. Superb AI ranked first in five of the seven categories defined by the organizers: Aerial, Docs, Industry, Medical, and Other. Its score of **64.4 in the Industry category** was the **highest by a substantial margin** and was highlighted in the organizers’ presentation. In the Medical category, Superb AI scored **51.4, more than nine points higher than the second-place team.** Rather than excelling in only one specialized field, the system maintained top-tier performance across a wide range of environments. The results quantitatively demonstrated its industrial applicability: a single model can quickly cover diverse industrial settings using only a limited amount of data.


(Slide from the presentation of the challenge organizer at the Open-World Vision Workshop)


In accordance with the challenge rules, the technical report and source code for the winning solution have been made publicly available. They can be accessed through the[CVPR official results page](https://www.neeharperi.com/VPLOW26-Foundational-FSOD-Challenge) and the[EvalAI leaderboard](https://eval.ai/web/challenges/challenge-page/2672/leaderboard/6922) .


# **From Fourth to First in One Year: What Changed?**


Last year, Superb AI placed fourth in the same challenge with a score of 47.2. Behind the improvement of more than six points and the rise to first place was a fundamental shift in perspective.


“Last year, we focused on validating ZERO’s core capabilities, particularly its pretraining performance,” said Kyeongryeol Go, Machine Learning Engineer on the research team. “This year, we concentrated on building a system that could adapt those capabilities to each industrial domain quickly and efficiently.”


In other words, refining the methodology that connects the model’s underlying potential to practical industrial applications led directly to the improved score.


“The key to this result was considering research performance and industrial application together,” said Moonsu Cha, CTO of Superb AI, who led the research team. “ZERO is not intended to be a model that only performs well on benchmarks. It is designed to be a practical model that can be deployed quickly and efficiently in customer environments.”


The winning solution uses a multistage pipeline that combines multimodal prompts based on text, visual examples, and surrounding context with a lightweight reclassification module that validates detection results. While some competing teams relied on extremely large models, Superb AI chose a lightweight, scalable architecture designed not only for challenge performance, but also for practical deployment in real-world environments.


A detailed, step-by-step explanation of the technology behind the solution will be provided in **Part 2: A Technical Walkthrough of the Winning Solution** .


# **ZERO: A Vision Foundation Model Built for Real-World Industry**


Superb AI plans to translate this achievement into more advanced solutions across key industries, including manufacturing, mobility, security, and logistics. The latest version of ZERO, which formed the foundation of the winning solution, is already available on[AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk) . Module capabilities that enable the model to adapt to individual domains within minutes will also be introduced progressively to Superb Platform.


ZERO is a core model designed to serve as the “eyes” of Physical AI systems that interact with the physical world, including robots, autonomous vehicles, and smart factories. Building on its validation on the global stage, Superb AI plans to further strengthen its leadership in the industrial Vision AI market.


# **Frequently Asked Questions**


### **Q. What is the CVPR 2026 Foundational Few-Shot Object Detection Challenge?**


It is an international competition that evaluates whether AI can recognize new objects using only around 10 examples per class across industrial imagery in healthcare, manufacturing, aviation, and other fields. Organized by Carnegie Mellon University and Roboflow, the challenge has been held annually since 2024 as part of the CVPR Open-World Vision (VPLOW) Workshop and marked its third edition this year.


### **Q. How did Superb AI perform in the challenge?**


Superb AI took first place in the 2026 Overall Track with an average mAP of 53.9. The 20 industrial domains were grouped into seven categories, and Superb AI ranked first in five of them. Its lead was particularly strong in the Industry category, with a score of 64.4, and the Medical category, with a score of 51.4. The company rose from fourth place with a score of 47.2 last year to first place in just one year, becoming the first Korean company to win the challenge.


### **Q. What is ZERO, Superb AI’s Vision Foundation Model?**


ZERO is a Vision Foundation Model developed by Superb AI specifically for industrial applications. It detects objects using a multimodal prompting approach that combines text, visual examples, and contextual information. The model has demonstrated strong performance across datasets in healthcare, autonomous driving, retail, and other industries.


### **Q. Why is few-shot object detection important for industry?**


The cost and time required for data collection and labeling are among the greatest barriers to deploying Vision AI in industrial environments. A model that can adapt to new objects using only a small amount of data can significantly reduce the labeling burden and enable faster deployment across new sites.


### **Q. Is ZERO available to use now?**


Yes. The latest version of ZERO is available now on AWS Marketplace. Module capabilities that enable rapid adaptation to individual domains will be introduced progressively to Superb Platform.


### **Learn More About the CVPR Challenges:**


- [CVPR 2025 Foundation Few-Shot Object Detection Challenge: Transforming Future Industries with AI](https://superb-ai.com/en/resources/blog/cvpr-few-shot-challenge)
- [CVPR 2025 Object Instance Detection Challenge: Advancing Practical AI for Industrial Applications](https://superb-ai.com/en/resources/blog/cvpr)
- [Introducing ZERO: Korea’s First Vision Foundation Model Tailored for Industry](https://superb-ai.com/en/resources/blog/introducing-vfm-zero)
- [Try ZERO now in AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-xm2s45njvoiqk)


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Announcements Celebrating Superb AI’s 8th Anniversary — 130 Million Images, Korea’s First Vision Foundation Model, and the Next Chapter of Physical AI Hyun Kim Co-Founder & CEO | 5 min read](https://superb-ai.com/en/resources/blog/8th-anniversary-en)[Announcements We're heading to NVIDIA GTC — and we have a lot to show you! Hyun Kim Co-Founder & CEO | 2 min read](https://superb-ai.com/en/resources/blog/superb-ai-nvidia-gtc-en)[Announcements Superb AI Joins NVIDIA’s Physical AI Ecosystem as a Key Vision AI Partner Hyun Kim Co-Founder & CEO | 20 min read](https://superb-ai.com/en/resources/blog/superb-ai-nvidia-vision-ai-partner-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
