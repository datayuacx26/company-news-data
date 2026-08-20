---
schema_version: "1.0.0"
document_id: "9974ede582241d794cfe7648e8e5bc1ebf79733a9d22624cd8ac29df9be2d486"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding"
published_at: "2024-06-28T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:355244d9bb6f9d20e3e0166664b9503b45ca716ce407d9e06a40334d86c90b8c"
---

# CVPR 2024: Pushing the Boundaries of AI — Embodied AI, GenAI, Foundation Models, and Video Understanding

In this post we share with you the top highlights of CVPR 2024. Let’s get started! 🚀


‍


## Table of Contents


1. Embodied AI
2. Generative AI
3. Foundation Models
4. Video Understanding
5. What’s next?


‍


## 1. Embodied AI


Figure 1.[Chris Paxton](https://cpaxton.github.io/about/) argued that LLMs beat both classical and fancy (e.g., Imitation Learning) approaches to do long-horizon manipulation of any object.


‍


‍


### What is this about


Embodied AI is an approach to artificial intelligence that focuses on creating agents (e.g., robots, smart home systems) capable of learning and solving complex tasks through direct interaction with their environment.


‍


As[Joshua Bongard](https://www.youtube.com/watch?v=mxGkR8APX7U) , keynote speaker, mentioned: “Embodied AI can mean different things to different people. It has gone through a series of transformations over the years”, but a common feature is that Embodied AI is about systems able to perceive their surroundings (through vision and other senses), communicate using natural language, understand audio inputs, navigate and manipulate their environment to achieve goals, and engage in long-term planning and reasoning.


‍


### Key ideas


> 1. Current AI systems are vulnerable to adversarial attacks due to lack of true embodiment.


During Bongard’s keynote, he argued that simply putting deep learning systems into robots is not sufficient for true Embodied AI. He believes that embodiment is fundamentally about change, both internal and external. To create safe AI, we need technologies that undergo significant internal physical changes. “Morphological pretraining” \[1\] through internal change can help AI systems better handle new tasks and adversarial attacks.


‍


> 2. The path to truly “generalizable robots” is to scale simulation.


[Aniruddha Kembhavi](https://anikem.github.io/) , senior director of computer vision at the Allen Institute for Artificial Intelligence (AI2) in Seattle, thinks that without requiring any adaptation or fine-tuning, expanding simulation data enables agents to masterfully navigate and manipulate in the real world. In his work Robothor \[2\], he examined the critical issue of how effectively models trained in *simulation* generalize to real-world scenarios, a question that has largely remained unresolved.


‍


> *3.* LLMs are better suited for long-horizon manipulation of any object compared to previous fancy approaches like Imitation Learning or more traditional methods such as classical task and motion planning.


[Chris Paxton](https://cpaxton.github.io/about/) , AI researcher and head of Embodied AI at[Hello Robot](https://hello-robot.com/) , argues that (1) classical task and motion planning lack knowledge of objects in the world, and struggle with partial observability, (2) modern behavioural cloning techniques, like Imitation Learning \[3\], are incapable of generalize well to unseen environments. On the contrary, LLMs \[4\] can be used to do **long-term horizon manipulation of any object in any environment** :


- Train transformers \[5\] to predict how objects should move
- Use LLMs like GPT-4 for common sense reasoning and interpreting users
- Combine LLM outputs with planners to make sure constraints are met
- Train low-level motor skills using spatially-abstracted representations


‍


### Leaders & builders in the space


- [Joshua Bongard](https://www.youtube.com/watch?v=mxGkR8APX7U) : Director of the[Morphology, Evolution & Cognition Laboratory](https://www.meclab.org/) at the University of Vermont.
- [Aniruddha Kembhavi](https://anikem.github.io/) : Senior Director of CV at[AI2](https://allenai.org/) , and Professor of CS at the University of Washington.
- [Chris Paxton](https://cpaxton.github.io/about/) , AI researcher and head of Embodied AI at[Hello Robot](https://hello-robot.com/) .
- [Eric Jang](https://evjang.com/about/) , VP of AI at[1X Technologies](https://www.1x.tech/) .
- [Brian Ichter](https://brianichter.com/) , founder of[Physical Intelligence (π)](https://physicalintelligence.company/) .


‍


## 2. Generative AI


Figure 2. OpenAI researcher Tim Brooks during his GenAI keynote on Sora \[11\]


‍


### What is this about


Unless you’ve been living under a rock for the past 24 months, you probably use GenAI on a daily basis by now. Generative AI \[6\] refers to artificial intelligence systems, for instance Google’s Imagen \[7\], that can create new content, such as text, images, audio, or video, that resembles human-created work.


‍


GenAI was a *really* hot topic 🔥 during CVPR 2024. The conference hosted the following GenAI-related workshops:


- SyntaGen: Generative Models for Synthetic Visual Datasets[🔗](https://syntagen.github.io/)
- The Future of Generative Visual Art[🔗](https://cveu.github.io/)
- Responsible Generative AI Workshop[🔗](https://sites.google.com/view/cvpr-responsible-genai)
- Generative Models for Computer Vision[🔗](https://generative-vision.github.io/workshop-CVPR-24/)
- Evaluation of Generative Foundation Models[🔗](https://evgenfm.github.io/)


‍


### Key ideas


> 1. The creation of multimodal datasets (containing paired image-text examples) can be demistified by executing a rigurous dataset development process.


In his keynote at the Evaluation of Generative Foundation Models workshop,[Ludwig Schmidt](https://people.csail.mit.edu/ludwigs/) , a researcher at[AI2](https://allenai.org/) , argued that multimodal learning can be accelerated by adopting a data-centric approach. He described a benchmark called DATACOMP \[8\], which aids in engineering multimodal datasets. The key idea of this benchmark, composed of 38 classification and retrieval tasks, is to keep both the training code and the GPU budget constant while proposing different training sets.


‍


> 2. Training text-to-image models on richly detailed, generated image captions significantly enhances their prompt-following abilities.


[Tim Brooks](https://www.timothybrooks.com/about/) from OpenAI claimed that often GenAI models struggle with interpreting detailed descriptions, frequently overlooking words or misunderstanding prompts. This problem comes from the noisy and inaccurate captions usually found in training datasets. By training a specialized image captioner to *recaption the data* , a more reliable and detailed dataset was created. Building on these insights, DALL-E 3 \[9\] was developed.


‍


> 3. Learning vision without visual data is possible.


In a fantastic talk titled “Learning Vision with Zero Visual Data” by[Phillip Isola](https://web.mit.edu/phillipi/) from MIT, he argued that non-visual data such as noise \[10\], language, and/or code can be used to train a vision model. In particular, language models such as GPT-4 can correctly classify human drawings but struggle to identify concept categories that they are otherwise capable of rendering accurately.


‍


### Leaders & builders in the space


- [Ludwig Schmidt](https://people.csail.mit.edu/ludwigs/) : Professor of CS at the University of Washington and at[AI2](https://allenai.org/) .
- [Phillip Isola](https://web.mit.edu/phillipi/) : Professor of CV at MIT.
- [Tali Dekel](https://www.weizmann.ac.il/math/dekel/home) : Staff research scientist at Google and professor of CS at the Weizmann Institute of Science.
- [Gianluca Corrado](https://scholar.google.is/citations?user=POZPkQgAAAAJ&hl=en) : Research scientist at Wayve.


‍


## 3. Foundation Models


Fig 3. Alex Kendall from Wayve introducing a Foundation Model for autonomous vehicles \[13\]


‍


### What is this about


Foundation models are large-scale artificial intelligence systems trained on vast and diverse datasets, serving as a base for a wide range of AI applications. These models are characterized by their size, breadth of training data, and ability to be adapted to various tasks with minimal additional training.


‍


🔎 For a more detailed guide on the definitive Foundation Models reshaping the field computer vision, read our[post on the subject](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) ⭐️.


‍


### Key ideas


> 1. Foundation models can work as real-world simulators.


Google researcher[Sherry Yang](https://sherryy.github.io/) argued that one of the use cases for foundation models is to serve as real-world simulators. In his keynote at the Foundation Models for Autonomous Systems[workshop](https://opendrivelab.com/cvpr2024/workshop/) , he claimed that two requirements for foundation models to function as real-world simulators have already been covered:


- 1) The Internet’s data (in text and video form) provides a unified representation and task interface for a “world model”
- 2) Reinforcement learning is sufficiently advanced (for decision-making) to allow for planning in this “world model” \[12\]


‍


So, **what’s missing?** Two aspects: 1) hallucinations are still common in these models, and 2) better evaluation and feedback mechanisms.


‍


> 2. The true benefit of foundation models in robotics lies in their ability to serve as general models that excel at decision-making.


In his[talk](https://www.youtube.com/watch?v=Bf30cs5MU1I) titled “A General-Purpose Robotic Navigation Model”,[Sergey Levin](https://people.eecs.berkeley.edu/~svlevine/) , an AI researcher and professor of computer science at Berkeley, argued that foundation models in domains like computer vision aren’t pretrained to make decisions per se. Currently, pretraining is only loosely related to decision tasks. However, if foundation models were pretrained to directly make important and useful decisions, it could be valuable for both robotics and other fields, since downstream machine learning tasks ultimately involve decision-making.


‍


> 3. We won’t achieve a robotics-first foundation model until we address three key components: data scaling, steerability and promptability, and scalable evaluations


‍[Ted Xiao](https://tedxiao.me/) , a research scientist at Google working on robotics, argued that three crucial ingredients are missing to build a true robotics-first foundation model. In his[presentation](https://opendrivelab.github.io/CVPR%202024/[CVPR%202024]%20What's%20Missing%20for%20Robotics-First%20Foundation%20Models.pdf) , he explained these three components:


- 1) Data scaling has worked incredibly well for LLMs and VLMs, but there’s no equivalent for robot data yet. However, there is hope if data interoperability is increased by treating robot actions as just another data modality
- 2) There is no promptable generalist robot like in LLMs, partly due to large context bandwidths, and the lack of robot data makes this even harder to achieve
- 3) Generalist models that can do anything need to be evaluated on everything 🤔: LLMs are evaluated directly by humans, as they target a human data distribution. In contrast, robots target a physical data distribution, which might require real-world evaluations that we are not yet capable of conducting.


‍


### Leaders & builders in the space


- [Sergey Levin](https://people.eecs.berkeley.edu/~svlevine/) : Professor of computer science at Berkeley.
- [Alex Kendall](https://alexgkendall.com/) : Co-founder of Wayve.
- [Sanja Fidler](https://www.cs.utoronto.ca/~fidler/) : AI researcher at NVIDIA and professor of computer science at the University of Toronto.
- [Ted Xiao](https://tedxiao.me/) : Senior researcher scietist at Google.


‍


## 4. Video Understanding


Figure 4. A multimodal model used to transform long-form video content into audio descriptions


‍


### What is this about


Video Understanding refers to the field of artificial intelligence that focuses on developing systems capable of comprehending and analyzing the content, context, and events within video sequences. It goes beyond simple object recognition or scene classification to interpret complex temporal and spatial relationships, actions, and narratives depicted in video data.


‍


### Key ideas


> 1. Multimodal in-context learning is posed to transform the task of audio description (AD).


[Zicheng Liu](https://zicliu.wixsite.com/mysite) , an AI researcher at AMD, described how visual content in long-form videos can be transformed in audio descriptions using multimodal models, in particular GPT-4, using in-context learning (MM-ICL) with few-shot examples \[14\]. He claims that this strategy beats beats both fine-tuning-based and LLM/LMM-based approaches to generate audio descriptions for videos of extensive length.


‍


> 2. LLMs are one of the key stones to solve long-range video captioning.


According to[Lorenzo Torresani](https://ltorresa.github.io/home.html) from FAIR, the reasoning abilities from LLMs make these models the perfect companion for hierarchical video captioning tasks \[15\]. In his keynote at a workshop focused on[procedural videos and language](https://mango-workshop.github.io/2024.html) , Torresani explained why LLMs can be so powerful for these tasks:


- 1) Given short-term clip captions, LLMs can successfully generate descriptions and long-range video summaries
- 2) LLMs can be used to augment training data, effectively complementing manually annotated data to improve performance on caption creation


‍


### Leaders & builders in the space


- [Chunyuan Li](https://chunyuan.li/) : Principal researcher at Microsoft Research.
- [Dima Damen](https://dimadamen.github.io/) : Professor of computer vision at the University of Bristol and research scientist at Google.
- [Fei Xia](https://fxia22.github.io/) : Senior researcher at Google.
- [Long Chen](https://long.ooo/) : AI researcher at Wayve.
- [Zicheng Liu](https://zicliu.wixsite.com/mysite) : Senior director of GenAI at AMD.
- [Lorenzo Torresani](https://ltorresa.github.io/home.html) : AI researcher at FAIR.


‍


## 5. What’s next?


It’s been less than 1 week and we’re already missing the energy and the enthusiasm of the crowd at CVPR.


Tenyks at CVPR 2024: We help ML teams to manage large amounts of visual data to extract actionable insights! Try our[sandbox](https://sandbox.tenyks.ai/) for free! 🚀


‍


👉 Stay tuned for more CVPR 2024 posts!


‍


## References


\[[1](https://www.youtube.com/watch?v=lUl8dUSSczE) \] Josh Bongard talk, DAY 2 EI’23 Conference


‍


\[[2](https://openaccess.thecvf.com/content_CVPR_2020/html/Deitke_RoboTHOR_An_Open_Simulation-to-Real_Embodied_AI_Platform_CVPR_2020_paper.html) \] RoboTHOR: An Open Simulation-to-Real Embodied AI Platform


‍


\[[3](https://arxiv.org/pdf/2309.02473) \] A Survey of Imitation Learning: Algorithms, Recent Developments, and Challenges


‍


\[[4](https://arxiv.org/pdf/2005.14165) \] Language Models are Few-Shot Learners


‍


\[[5](https://arxiv.org/pdf/1706.03762) \] Attention is all you need


‍


\[[6](https://arxiv.org/pdf/2402.16369) \] Generative AI in Vision: A Survey on Models, Metrics and Applications


‍


\[[7](https://imagen.research.google/) \] Imagen


‍


\[[8](https://arxiv.org/pdf/2304.14108) \] DATACOMP: In search of the next generation of multimodal datasets


‍


\[[9](https://cdn.openai.com/papers/dall-e-3.pdf) \] Improving Image Generation with Better Captions


‍


\[[10](https://arxiv.org/pdf/2106.05963) \] Learning to See by Looking at Noise


‍


\[[11](https://x.com/hamadakoichi/status/1802754579920277603) \] Sora at CVPR 2024


‍


\[[12](https://universal-simulator.github.io/unisim/) \] UniSim: Learning Interactive Real-World Simulators


‍


\[[13](https://x.com/amirreza_yz/status/1802842721000050995) \] PRISM-1 by Wayve


‍


\[[14](https://mm-narrator.github.io/preprint_MMNarrator.pdf) \] MM-Narrator: Narrating Long-form Videos with Multimodal In-Context Learning


‍


\[[15](https://arxiv.org/pdf/2402.13250) \] Video ReCap: Recursive Captioning of Hour-Long Videos


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


‍


*\\If you’d like to know more about* ***Tenyks*** *, explore*[sandbox](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*


‍
