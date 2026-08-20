---
schema_version: "1.0.0"
document_id: "bf9034717595d04a7dbe2ddf0557c74db0c03b503413004c0cae87967dd342b4"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/cvpr-win-interview-en"
published_at: null
first_seen_at: "2026-07-22T15:16:12.909358+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:59fc43b55f185366232796e72b6e1c2827ab3f10f32a279bea0215eb25255cfa"
---

# “Lightweight Means Fast—and Fast Wins”: An Interview with CVPR-Winning ML Engineer Kyeongryeol Go

Team


“Lightweight Means Fast—and Fast Wins”: An Interview with CVPR-Winning ML Engineer Kyeongryeol Go


Hyun Kim


Co-Founder & CEO | 2026/06/24 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


# **Key Takeaways**


- Superb AI took first place in the Overall Track of the CVPR 2026 Foundational Few-Shot Object Detection Challenge. We spoke with Kyeongryeol Go, a Machine Learning Engineer on the winning team, about the journey behind the result.
- Fourth place last year, first place this year. The pivotal change was a **shift in focus** —from validating pretraining performance to **determining how to utilize the model more efficiently in industrial environments** .
- **ZERO’s lightweight design** made it possible to achieve the result within a limited period of focused development. The lighter the model, the faster the experimental cycle—and the more hypotheses the team could test within the same amount of time.
- An **R&D culture of “building AI with AI,”** with the team actively using AI assistants throughout the research process, also played an important role.


On June 4, local time, Superb AI was announced as the winner of the Overall Track of the CVPR 2026 Foundational Few-Shot Object Detection Challenge at the Open-World Vision (VPLOW) Workshop in Denver, Colorado. The challenge drew more than 200 submissions from 17 teams. For Superb AI, the result marked a rise from fourth place to first in just one year.


To understand what changed over the past 1 year, we spoke with Kyeongryeol Go, a Machine Learning Engineer from the winning Superb AI team. He worked on the challenge alongside engineers Hyundong Jin, Taewoong Jang, and Wooseong Choi.


(The Superb AI team behind the CVPR 2026 challenge win)


# **"Last Year, the Fundamentals; This Year, the System"**


### **Q. Superb AI went from fourth place last year to first this year. What changed over those 12 months?**


“Both the model and the methodology improved, but the decisive difference was a shift in focus.


Last year, ZERO had only just been introduced, so our primary interest was its pretraining performance. The goal was to see how well ZERO’s fundamental capabilities would hold up. Fine-tuning was, in practice, close to a default setting.


This year, we changed the question: ‘How can we put this model to practical use in industry?’ We examined the data from all 20 domains individually and focused on building a system that could adapt ZERO to each domain quickly and efficiently. We wanted to create scalable modules that would remain useful in customer projects even after the challenge ended. I believe that difference in perspective has ultimately translated into the difference in scores.”


# **“When the Model Is Lightweight, Experiments Run Faster—and Fast Wins.”**


### **Q. What was the most difficult part of preparing for the challenge?**


“Time.


We had to prepare for the challenge alongside other projects, so the time available to us was extremely limited. Under those conditions, the only way to produce a strong result was to run experimental cycles as quickly as possible.


The first reason we could do that was that ZERO is relatively lightweight compared with other foundation models.


Finding the right configuration requires repeating the same cycle countless times: form a hypothesis, run an experiment, review the result, and form the next hypothesis. If the model had been significantly heavier, the number of experiments we could run within the same period would have fallen to a fraction.


Being lightweight makes the model faster, and being faster allows us to test more hypotheses. That gave us a structural advantage.”


### **Q. It sounds like ZERO’s lightweight design played a major role. Was it designed that way from the beginning?**


“ZERO was not designed to be lightweight for the challenge. It was a design decision made from the beginning with deployment in customer environments in mind.


Most industrial sites do not have the infrastructure required to run extremely large models. Even when they do, cost often becomes a major barrier to adoption. We therefore made a footprint suitable for real-world deployment as one of the model’s core requirements.


But a design decision originally made for industrial environments became a direct advantage under the time constraints of the challenge. For us, that was as meaningful a validation as the win itself. A design built for the field ultimately translated into research competitiveness.


The winning solution follows the same philosophy. Some teams relied on extremely large models to refine their results, while we developed our own lightweight, scalable reclassification module.


The key was not to deploy the largest possible model. It was to design a method that could adapt to real-world environments quickly and efficiently. ZERO’s efficiency allowed us to test multiple hypotheses within a short period and identify the optimal combination.”


# **“We Build AI with AI.”**


### **Q. Was there anything else that helped the team overcome the limited timeline?**


“We actively used AI assistants.


They were particularly helpful in writing experiment code and setting up systems that could run experiments automatically overnight. When validation continues while the team is sleeping, the number of experiments that can be completed in a single day changes significantly. Building AI together with AI has become a natural part of how our team works.


The other factor was the team itself. Four of us worked on the challenge, and it would not have been possible on this schedule without the support of my colleagues.”


# **“The Next Question Was Already Clear.”**


### **Q. What did it feel like when the win was confirmed?**


“I was genuinely happy, but at the same time, another question immediately came to mind: How do we bring the adaptation system validated through the challenge into the product and into customer environments?


For us, the challenge wasn’t the destination. It was an opportunity to put our work to the test in an external setting. The real test happens on the customer’s production line.”


### **Q. Finally, what would you say to engineers hoping to follow a similar path?**


“There is a great deal of research in which benchmark performance and real-world value are treated as separate goals. However, we work in an environment where those two goals can be addressed as part of the same problem. Research becomes a product, and the expertise developed through the product flows back into research.


We welcome anyone who wants to help build that cycle with us.”


Learn more about Superb AI’s CVPR achievement and the technical details of the winning solution in the content below.


### **\[Related Content\]**


- [Part 1: Superb AI’s ZERO Takes 1st Place in the CVPR 2026 Foundational Few-Shot Object Detection Challenge](https://superb-ai.com/en/resources/blog/1st-place-cvpr-2026-few-shot-challenge-en)
- [Part 2: How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)
- [Technical report & code of the winning solution (GitHub)](https://github.com/gokyeongryeol/zero-fsod)
- [Superb AI Careers](https://careers.superb-ai.com/?lang=kr)


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Team Looking Back at Superb AI’s 2025 Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/2025-retrospective-en)[Team Superb AI in 2022: A Look Back at a Big Year Hyun Kim Co-Founder & CEO | 6 min read](https://superb-ai.com/en/resources/blog/superb-ai-in-2022-a-look-back-at-a-big-year)[Team Employee Spotlight - Sean Lynch 2 min read](https://superb-ai.com/en/resources/blog/employee-spotlight-sean-lynch)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
