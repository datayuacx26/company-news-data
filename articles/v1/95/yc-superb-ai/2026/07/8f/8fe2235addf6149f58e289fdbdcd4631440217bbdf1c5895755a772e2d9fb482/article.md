---
schema_version: "1.0.0"
document_id: "8fe2235addf6149f58e289fdbdcd4631440217bbdf1c5895755a772e2d9fb482"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/cvpr-challenge-global-vision-ai-landscape-en"
published_at: null
first_seen_at: "2026-07-22T15:16:12.909358+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:3ec0950484701181366d7c71ab3135fa09ba1d4ee7da2e9e22c63ac1f5d9cadf"
---

# Three Years of the Few-Shot Object Detection Challenge: Mapping the Global Vision AI Landscape

Insight


Three Years of the Few-Shot Object Detection Challenge: Mapping the Global Vision AI Landscape


Hyun Kim


Co-Founder & CEO | 2026/06/26 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


# **Key Takeaways**


- The few-shot object detection (FSOD) challenge ecosystem centered on CVPR has evolved along two parallel tracks: **the Foundational FSOD Challenge at the VPLOW Workshop and the CD-FSOD Challenge at the NTIRE Workshop** . The two address closely related problems, but follow different evaluation philosophies.
- The trajectory over the past three years is clear: a single autonomous driving domain in 2024, 20 industrial domains in 2025, and separate fine-tuning and training-free tracks in 2026. **Which foundation model a team starts with—and how quickly and efficiently it can adapt that model to real-world environments—** has become the essence of the competition.
- The clearest lesson from three years of leaderboards is that while general-purpose model baselines broke down in industrial domains, the top positions went to teams that **designed the foundation model and adaptation system as an integrated whole** .
- The competitive landscape has largely been shaped by the strength of Chinese academic-industry teams. In 2026, Superb AI changed that dynamic by becoming the **first Korean company to win the challenge.**
- Among the leading teams, **Superb AI was the only one competing with a commercially available model: ZERO** . Winning with a product already offered to customers—not a research-only model stack—adds another layer of significance to the result.


Few-shot object detection enables AI to recognize new objects using only around 10 examples per class. Because it directly addresses one of the biggest bottlenecks in industrial AI adoption—the cost of data—it has become one of the most competitive areas of global AI research over the past three years.


This article reviews three years of the CVPR-centered FSOD challenge ecosystem and examines how its competitive landscape and methodological trends have evolved.


# **Why This Challenge, Why Now: The Data Bottleneck in the Physical AI Era**


The rapid growth of these challenge series over the past three years reflects a broader transformation taking place across industries. As **AI moves beyond text on a screen and enters the physical world** through robots, autonomous vehicles, and smart factories, the requirements placed on Vision AI are changing fundamentally. This is the emerging era of Physical AI.


Objects in the physical world are endlessly diverse. They vary from one site to another and change continuously. It is fundamentally impossible to collect and label tens of thousands of images in advance for every component a robot may encounter or every defect that may appear on an inspection line.


**The “eyes” of Physical AI must therefore be able to adapt quickly to previously unseen objects using only a few examples** . Few-shot object detection is the yardstick for measuring precisely that capability. This is why the challenge leaderboard serves as a microcosm of the global race to build the eyes of Physical AI.


# **Two Challenge Series: The Same Question, Different Philosophies**


### **Foundational FSOD Challenge at the VPLOW Workshop**


Organized by Carnegie Mellon University and co-organized by Roboflow since 2025, the series began with a fundamental question: Can foundation models replace human annotators?


Traditional FSOD benchmarks based on datasets such as COCO have lost much of their ability to differentiate model performance because modern vision-language models can already detect many common categories effectively in a zero-shot setting.


To address this limitation, the challenge adopted a protocol that evaluates an arbitrary pretrained model after aligning it to a domain using only 10 multimodal examples—combining text and visual information—per class.


### **CD-FSOD Challenge at the NTIRE Workshop**


The CD-FSOD Challenge focuses on **cross-domain scenarios** in which models trained on general-purpose data break down when deployed in fundamentally different environments, such as remote sensing or underwater imagery.


It evaluates aggregate performance across 1-shot, 5-shot, and 10-shot settings under conditions where the source and target class sets are completely disjoint. The challenge also operates both a track that strictly restricts the use of source data and one that permits it more freely.


In short, the Foundational FSOD Challenge asks what realistic few-shot evaluation should look like in the foundation model era. The CD-FSOD Challenge asks how robustly a model can adapt under severe domain shift. Only a small number of teams have reached the top tier in both challenge series, demonstrating how difficult it is to solve both problems well.


# **Three Years of Evolution: From a Single Domain to a Two-Track System**


### **2024: Proof of Concept**


The inaugural challenge used a single autonomous driving dataset, nuImages.


The winning team, NJUST KMG, used a large multimodal language model to generate multiple descriptions for each category and select the most effective one. The approach offered an early glimpse of a trend that would become increasingly important: restating a category in language the model can understand.


### **2025: Expansion Across Domains**


The evaluation moved to Roboflow20-VL, significantly expanding the scope of the challenge.


The dataset introduced 20 specialized domains, spanning supermarket products, defect detection, X-rays, thermal imagery, and aerial imagery. The difficulty rose sharply, with leading general-purpose models recording zero-shot accuracy below 2% on some medical datasets.


This was the year that the ability to adapt to “a world not found on the internet” became a central test.


### **2026: Methodological Divergence**


In 2026, the challenge was divided into two tracks.


The **Overall Track** allowed all strategies, including fine-tuning. The **In-Context Prompting Track** prohibited gradient-based training and allowed adaptation only through in-context prompting at inference time.


The introduction of a dedicated track formally established adaptation without training as an independent research direction.


Participation also grew to 17 teams and more than 200 submissions in the Overall Track. Evaluation standards became more demanding as well: teams that failed to exceed the previous year’s best score were not eligible for an award.


# **The Competitive Landscape: Who Is Competing?**


Overlaying three years of leaderboards reveals several clear patterns.


### **The Strength of Chinese Academic-Industry Teams**


The NJUST group from Nanjing University of Science and Technology, which won the 2024 challenge, is the only academic team to place on the leaderboard for three consecutive years. Chinese research groups also took both first and second place in the newly introduced In-Context Prompting Track in 2026. Joint teams combining universities and companies have consistently occupied many of the leading positions across both challenge series.


### **The Strongest Rival: The Fudan University–Lenovo Team**


Among them, FDUROILab Lenovo, a joint team from Fudan University and Lenovo, stands out. The team placed second in the 2025 Foundational FSOD Challenge, won the 2026 NTIRE CD-FSOD Challenge, and returned to second place in the 2026 Foundational FSOD Challenge. It is effectively the only team to have maintained top-tier performance across both challenge series. In the 2026 Foundational FSOD Challenge, Superb AI finished 2.3 points ahead of this team.


### **New Participants Enter the Field**


A Saudi Arabian technology and security company entered the challenge for the first time in 2026. This may signal that demand for Vision AI in Middle Eastern security and CCTV applications is beginning to reach the global challenge stage. Meanwhile, aside from the organizers at CMU and Roboflow, Western Big Tech companies and academic institutions are largely absent from the leaderboard. That absence is itself noteworthy.


### **And Then There Is Superb AI**


Superb AI first emerged on the leaderboard in 2025, placing second in the Object Instance Detection Challenge and fourth in the Foundational FSOD Challenge at the same VPLOW Workshop.


In 2026, the company reached the top by winning the Foundational FSOD Overall Track with an average mAP of 53.9. This marked the first win by a Korean company in the history of the challenge.


# **Three Methodological Trends Revealed by the Leaderboard**


### **1. The Rise of Training-Free Adaptation**


The introduction of the In-Context Prompting Track reflects the growing importance of approaches that adapt models through prompting and retrieval without fine-tuning.


The ability to “communicate” with a model through multimodal prompts is becoming a competitive capability in its own right.


### **2. Synthetic Data Proves Its Value**


The winning team in the source-restricted track of NTIRE 2026 introduced a framework that generated synthetic images for the target domain and then applied pseudo-labels using a vision-language model.


The result demonstrated at the challenge level that synthetic data can help fill gaps in domains where real-world data is scarce.


### **3. The Shift to System-Level Design**


Top-performing solutions are no longer built around a single model. Teams are increasingly competing through system design, combining multiple Vision Foundation Models with augmentation and adaptation strategies.


The ecosystem has even produced a “solution provider” model in which a team releases an open-source system rather than competing directly, enabling other teams to achieve award-winning results.


The common thread across all three trends is clear: the center of competition is shifting from building a larger model to designing a smarter adaptation system.


# **Superb AI’s Position: The Only Team to Win with a Commercially Available Model**


The most important lesson from three years of leaderboards is that the success of few-shot adaptation depends on more than the adaptation technique itself. **It begins with the foundation model.**


While general-purpose model baselines fell to single-digit accuracy in industrial domains, the top positions went to teams that designed the foundation model and adaptation system together.


Adaptation is a methodology, but the model is the foundation that determines whether that methodology can work.


Within this landscape, Superb AI occupies a unique position.


Most leading teams compete with combinations of research models. **Superb AI is the only top-performing team to enter with ZERO itself—the same industry-focused Vision Foundation Model already available commercially on AWS Marketplace.**


The publicly released code for the winning solution also abstracts ZERO behind an API service. This architecture satisfies the challenge’s reproducibility requirements while protecting the product’s intellectual property.


The significance of the win therefore extends beyond its score: a product model, rather than a research-only model stack, took first place on the global stage.


Its alignment with broader methodological trends is also worth noting.


The rise of training-free adaptation aligns with ZERO’s multimodal prompting approach. The growing use of synthetic data aligns with Superb AI’s synthetic data pipeline for Physical AI. The shift toward smarter adaptation aligns with the lightweight adaptation system behind the winning solution.


In other words, the direction of the challenge ecosystem and Superb AI’s technology roadmap are converging.


# **Frequently Asked Questions**


### **Q. What is the difference between the Foundational FSOD and CD-FSOD Challenges?**


The Foundational FSOD Challenge at the VPLOW Workshop evaluates how effectively a foundation model can align with diverse industrial domains using only 10 multimodal examples per class. The CD-FSOD Challenge at the NTIRE Workshop evaluates a model’s ability to adapt to domains that differ fundamentally from its original training domain. The two challenges address related problems, but use different datasets and evaluation protocols.


### **Q. What are the latest trends in few-shot object detection?**


Three trends stand out: the rise of training-free approaches that adapt models through prompting without fine-tuning, the use of synthetic data to bridge domain gaps, and competition around system-level design that combines multiple Vision Foundation Models. Across all three trends, the center of competition is shifting from larger models to smarter adaptation. At the same time, three years of leaderboards have demonstrated that successful adaptation still requires a strong foundation model for industrial domains.


### **Q. How has Superb AI performed in these challenges?**


At the CVPR 2025 VPLOW Workshop, Superb AI placed second in the Object Instance Detection Challenge and fourth in the Foundational FSOD Challenge. In 2026, it became the first Korean company to win the Foundational FSOD Overall Track, achieving an average mAP of 53.9.


### **Q. Where can I find the challenge results?**


Complete results and technical reports from participating teams are available on the[conference’s official results pages](https://www.neeharperi.com/VPLOW26-Foundational-FSOD-Challenge) and the[EvalAI leaderboard](https://eval.ai/web/challenges/challenge-page/2672/leaderboard/6922) .


## **\[Related Content \]**


- [Part 1: Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en)
- [Part 2: How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)
- [CVPR 2025 Foundation Few-Shot Object Detection Challenge: Transforming Future Industries with AI](https://superb-ai.com/en/resources/blog/cvpr-few-shot-challenge)
- [CVPR 2025 Object Instance Detection Challenge: Advancing Practical AI for Industrial Applications](https://superb-ai.com/en/resources/blog/cvpr)
- [Introducing ZERO: Korea’s First Vision Foundation Model Tailored for Industry](https://superb-ai.com/en/resources/blog/introducing-vfm-zero)


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Insight How to Restart an AI Project That Stalled for Lack of Data—with Just 10 Images Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/cvpr-zero-data-flywheel-en)[Insight ⑪ Germany's Physical AI Moment: Siemens, BMW, and the Robot Unicorn Counteroffensive Hyun Kim Co-Founder & CEO | 15 min read](https://superb-ai.com/en/resources/blog/physical-ai-series-11-germany-s-physical-ai-en)[Insight ⑩ Big Tech Physical AI Trends (2): Tesla vs. Amazon Strategy Breakdown Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/physical-ai-series-10-tesla-vs-amazon-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
