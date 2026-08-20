---
schema_version: "1.0.0"
document_id: "ea5a6a6c23e805ebd416eff6b31caf82c1f9c17859163f08f41250f09166c923"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Understanding-Foundation-Models"
published_at: "2025-11-12T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:3ffde749436e63448edbd12a84512b8594de7d21f1906c4111a4c7150d653da1"
---

# Maps, Misconceptions, and the Making of Modern Foundation Models

## Introduction


Foundation models can achieve astonishing performance on a variety of tasks, from image recognition to natural language processing. But how is it that large language models can describe the backpropagation algorithm in detail, but, for a long time (and still occasionally today),[fail to count the number of 'r's in 'strawberry' correctly](https://arxiv.org/abs/2412.18626?utm_source=chatgpt.com) \[1\]?


The answer is that foundation model behaviours are a symptom of the assumptions that shape their architecture and training regime. Humans come with strong priors built by evolution: for example, spatial reasoning, and continuity. Models, by contrast, inherit their bias from their architecture, tokenization strategy (when discussing certain model types), training data, and training objectives.


In this post we aim to help the reader better understand current foundation models. We discuss the key concepts behind foundation models, and discuss the principles of inductive bias, out-of-distribution generalization, data scaling, reasoning, and world modelling, before concluding with a discussion on the future of foundation models.


## Seriously, what are foundation models?


A foundation model is a model (usually deep learning–based) that serves as a **base or “foundation”** for many specific applications. It’s trained on broad data and **can later be specialized with smaller, task-specific data** .


Whilst the term is not rigorously defined, there are some key characteristics that are important to underscore right now:


- First, foundation models are *meant* to be **adaptable** . They can be fine-tuned or adapted to a variety of downstream tasks with (relatively) minimal additional training.
- Second, and relatedly, they are typically **large-scale models** trained on **massive datasets** , which is what allows them to be adapted to various tasks with minimal additional training. This is because they **learn general patterns and representations from the data that can be transferred to new tasks** .


It is a wrong framing to think of foundation models as 'giant lookup tables' that simply memorize their training data. Instead, they learn **representations** of the data that capture its underlying structure. This representation learning allows them to perform tasks they're not explicitly (re-)trained for. For instance, using in-context learning, we can show a foundation model the following pattern in its prompt:


```text
Translate English to French:
English: cat → French: chat
English: dog → French: chien
English: house → French:


```


The model will produce the correct answer 'maison', despite never being explicitly trained on this specific translation task.


However, **this task would literally be impossible if the model had no French knowledge at all** , irrespective of how many examples we provide in the prompt (unless we show it what the word 'maison' means). In other words, if the model had *no* prior knowledge of French, it would be unable to generalize from the examples provided in the prompt, because it would have never seen the word 'maison' ever before. This point cannot be stressed enough: **foundation models rely on their prior knowledge of the world to generalize to new tasks** .


The further away a task is from a foundation model's training distribution (prior knowledge), the less likely it is to perform well on that task. These systems are, in essence, massive statistical engines: **they learn to compress regularities in their training data into high-dimensional representations** (a manifold). When they perform a new task, they’re navigating the internal map of patterns that have proven useful before.


When a model writes Python code or describes a protein structure, it’s drawing from similar fragments in its training data. But if the problem’s statistical structure lies even slightly off manifold, the model’s predictions can degrade rapidly.


As an example in radiology, suppose you train a foundation model on chest x-rays of adults, and then ask it to interpret pediatric chest x-rays. The model may struggle because the anatomical and pathological features in pediatric x-rays can differ significantly from those in adults, leading to a distribution shift that the model wasn't trained to handle. If there are specific pediatric conditions or anatomical variations that are never present in the adult training data, **it will be impossible for the model to describe these pathologies** , because it has literally no prior knowledge of them. This means, in the finite data regime, no foundation model can simply generalize out of distribution in whatever way we like. We will discuss this slightly more in the 'Out-of-distribution generalization' section.


### A note on 'emergent capabilities'


Foundation models are often described as exhibiting 'emergent capabilities'; abilities that were not explicitly programmed into them but arise from their training on diverse data. For example, a language model trained on a vast corpus of text might suddenly be able to perform arithmetic or translate languages, even if it was never explicitly trained for those tasks.


In the seminal work[Are Emergent Abilities of Large Language Models a Mirage?'](https://proceedings.neurips.cc/paper_files/paper/2023/hash/adc98a266f45005c403b8311ca7e8bd7-Abstract-Conference.html) \[3\] published as an oral at NeurIPS 2023, a team from Stanford found that many tasks where people report a jump (e.g., a model below some scale is “useless”, then above it jumps to “useful”) often use metrics that are non-linear or discontinuous (e.g., exact string match or all-correct requirement) which exaggerate changes in performance.


It turns out that if you use smoother/continuous metrics (e.g., token-edit distance, proper scoring rules), performance tends to improve *gradually* and *predictably* with model scale, removing the “phase-transition” framing. They showed that most claims of emergence align with particular metrics (and not other metrics), suggesting **the effect is tied to measurement rather than underlying model behaviour** .


### Where does this leave us with foundation models?


Foundation models are powerful next-generation models that can *learn general patterns from their training data* . The training *datasets are usually large and diverse* . Because of this, the models can learn *broad and powerful representations* that allow them to be *effectively adapted* to new tasks and adjacent data distributions (e.g. adult CXRs -> paediatric CXRs). Furthermore, these *powerful representations allow them to generalize* **insofar as the new tasks can be predicated on their prior knowledge of the world** .


Given the above, it is clear to us that training a foundation model once, on one dataset (even if it is extremely large but not necessarily diverse), and expecting it to generalize to *all possible tasks and distributions* is a failing task. Instead, foundation models should be viewed as **powerful starting points** that can (and should) be **adapted and specialized** to attain optimal performance. Our internal results at Mecha Health strongly support this view. This view is further supported by the fact that Thinking Machines recently launched[Tinker](https://thinkingmachines.ai/tinker/) , a training API that fine-tunes models by training a small add-on (LoRA) instead of changing all the original weights. Similarly, OpenAI provides a[fine-tuning platform](https://platform.openai.com/docs/guides/model-optimization) with a number of case studies which demonstrate the utility (and indeed need) for fine-tuning foundation models to attain maximum value, even when talking about frontier models like GPT-4o.


## Inductive bias


An inductive bias is any built-in assumption, constraint, or preference that helps a learning algorithm generalize beyond the data it has seen. It’s what a machine learning model **believes about the world before seeing any data** , or what it assumes will generally be true when faced with new, unseen examples.


Learning from finite data is impossible without *some* bias. Otherwise, infinitely many explanations could fit the training examples. The inductive bias is what guides the model toward one explanation over others.


For example, when performing a linear regression, we assume a linear bias. What does that mean? It means that we assume that the target can be explained as a linear combination of inputs. Convolutional neural networks (CNNs) assume a type of structural bias. Namely, they assume spatial locality (nearby pixels relate), translation invariance (patterns can appear anywhere), and compositionality (complex patterns are built from simpler ones). These biases make CNNs particularly effective for image data.


Earlier, we mentioned that foundation models struggle to perform seemingly simple tasks like counting the number of 'r's in 'strawberry'. This happens due to an inductive bias. Large language models are trained on 'tokens' of text. That is, text is broken into tokens, small units like words, subwords, or (rarely) characters (depending on the *tokenizer* ). For instance, the word 'strawberry' might be broken down like this:


```text
"strawberry" → ["straw", "berry"]


```


Here is an illustration of this process:


Input Text


Text


The radiologist reviewed the CT scan for early signs of pneumonia.


Standardization


Standardized text


the radiologist reviewed the ct scan for early signs of pneumonia.


Subword Tokenization


Tokens


"the", "radio", "logist", "revi", "ewed", "the", "ct", "scan", "for", "early", "signs", "of", "pneu", "monia", "."


Indexing


Token indices


121, 127, 58, 38, 21, 121, 15, 21, 127, 141, 148, 13, 40, 132, 46


One-hot encoding or embedding


Vector encoding of indices


That means the model’s input space is made of tokens, *not characters* , and that’s already an inductive bias: **It assumes meaning and structure live mostly at the token (word/subword) level** , not at the individual character level.


Counting letters, like counting the number of rs in “strawberry,” is a **character-level task** . But the model’s inductive bias, inherited from its tokenizer, tells it that such fine-grained symbol-level reasoning is not typical or necessary for language understanding.


So when you ask “How many rs are in strawberry?”, the model doesn’t literally “see” rs repeated; it just sees token embeddings for "straw" and "berry". **The association between "berry" and how many rs it contains has to be memorized** , not computed.


The model is not reasoning over letters, it's operating in a continuous semantic space built on token-level biases. One solution is to try to split the word into individual characters, or, for a tool-capable model, to ask it explicitly to use a tool that counts characters (e.g. write a small piece of code that counts the rs).


Inductive biases are hugely important to get right for foundation models in radiology. There are instances out there where, for instance, a model is trained with a causal transformer architecture but is then expected to perform a task like dense pixel-level segmentation of medical images.


The inductive bias of a causal transformer is not well-aligned to the task of pixel-level segmentation, which requires spatial reasoning and local context. This misalignment can lead to suboptimal performance.


At Mecha Health, we pay close attention to the inductive biases of our models, ensuring that the architectures and training regimes we choose are well-suited to our tasks, and find that bad inductive biases are often a root cause of poor performance. In fact, bad inductive biases can overwhelm even large amounts of data, hugely limiting a model's ability to learn effectively.


## Out-of-Distribution generalization


Let's revisit the text translation example we considered earlier. You give the following prompt to the model:


```text
Translate English to French:
English: cat → French: chat
English: dog → French: chien
English: house → French:


```


Many foundation models will produce the correct answer 'maison' after being pretrained to simply produce the next token from a sequence, and with no explicit training on translation tasks. How is this possible?


Let's frame this through the lens of out-of-distribution (OOD) generalization. Clearly, the model is being asked to generalize to a new task (translation) that is different from its training data distribution (which may have included a variety of text, but not necessarily focused on translation). However, because the model has learned a rich representation of language during its training, it can leverage this knowledge to perform the new task effectively.


However, if the model had not been pretrained on any French text at all, it would be impossible for it to produce the correct translation. This is because the model would lack the necessary prior knowledge of the French language to generalize from the examples provided in the prompt. Clearly this isn't a binary in-distribution vs out-of-distribution scenario; rather, the model's ability to generalize depends on how closely the new task aligns with its prior knowledge.


The relative proximity of the new task (or indeed the new data distribution for the same task) to the model's training distribution is crucial in explaining the myriad papers reporting essentially the same phenomenon again and again. Alice C.Y. et al. \[4\] found that the vast majority of external-validation studies demonstrated diminished radiological algorithm performance on an external dataset, Domalpally A. et al. \[5\] emphasized that the transition from development of radiological algorithms to real-world scenarios is fraught with performance drops and generalisability issues, whilst Deng et al. \[6\] warn how a high AUC (or other metric) in development doesn’t guarantee clinical readiness, and so on.


### Types of distribution shift


There are several types of distribution shifts that can affect model performance:


- **Covariate Shift** : The input distribution changes, but the relationship between inputs and outputs remains the same. For example, a model trained on images from one hospital may encounter images with different lighting conditions or equipment in another hospital.
- **Label Shift** : The distribution of the output variable changes, but the relationship between inputs and outputs remains the same. For instance, a disease that was rare in the training data becomes more common in the real world.
- **Concept Shift** : The relationship between inputs and outputs changes. For example, a new variant of a disease may present different symptoms than those seen in the training data.


Click on a shift type below to see how the test distribution (orange) changes relative to the training distribution (charcoal).


### Adapting to distribution shifts: why fine-tuning and online learning matter


When a model encounters a distribution shift, its performance usually drops because the world it was trained on no longer matches the world it sees in deployment. Fine-tuning and online learning are two practical ways to bridge this gap.


**Fine-tuning** means updating a pretrained model with new examples from the changed environment. **Online learning** continuously updates the model as new data arrives, allowing it to adapt across time. Both approaches exploit the fact that while the world changes, not everything changes at once.


- For **covariate shift** , where the inputs look different but the underlying rules stay the same, continual updates let the model re-calibrate to new input patterns. For instance, to allow the foundation model to adapt to new imaging devices or lighting conditions without having to relearn the fundamental task itself.
- For **label shift** , where the frequency of certain outcomes changes, online updates help the model adjust to new class proportions (for example, a disease becoming more common), but once more, we can still leverage the already learnt appropriate mapping from input to output here.
- For **concept shift** , where the very relationship between inputs and outputs evolves, continual fine-tuning can at least soften the blow. The model gradually replaces outdated mappings with ones that reflect the new reality. This is reasonable in the healthcare space because while diseases may evolve, many underlying biological principles remain constant.


In short, fine-tuning and online learning turn static models into dynamic systems that can keep pace with changing data instead of being trapped in the moment they were trained.


At Mecha Health, we believe that continual adaptation is essential for maintaining optimal performance in the real world. Our foundation models are designed to remain effective even as the underlying data distribution shifts.


## Reasoning and world modelling


As humans, we appear to have agreed that we have a so-called 'world model', an internal representation of how the world works. This allows us to simulate scenarios, predict outcomes, and make decisions based on our understanding of cause and effect. In other words, we can 'reason' about the world. We can generalize from past experiences to new situations by applying our world model. We can account for unseen variables and hypothetical scenarios, adjusting for distribution shifts in real time. We have a 'general' intelligence. From a philosophical standpoint, this is all remarkably controversial and blurry in the first place, but for our purposes here we can accept this as approximately reasonable.


### Reasoning


Much has been discussed about whether foundation models can 'reason'. One of the main arguments *against* this is that these models are simply pattern matchers, and do not possess any true understanding of the world. This is often accompanied by a general critique of the transformer architecture.


However, we now have substantial evidence that the human (and mammalian) cortex uses a **repeating laminar-columnar architecture** (six layers, vertical columns) and that this architecture is thought to implement a canonical microcircuit (or **repeating computation** ) that can be **adapted to many modalities** (vision, audition, motor, and cognition).


In the seminal MIT paper published in Nature in 2000,[Visual behaviour mediated by retinal projections directed to the auditory pathway](https://www.nature.com/articles/35009102) \[7\], neonatal ferrets were surgically rewired such that retinal axons (normally destined for the visual thalamus) instead projected into the auditory thalamus (medial geniculate nucleus) and then into the auditory cortex.


The auditory thalamus and auditory cortex of these “rewired” animals **developed visually-responsive neurons** , retinotopic maps, and other features more typical of the visual cortex. Behaviorally, the rewired animals responded to light stimuli in the portion of visual space represented by the rewired input “as though they perceive the stimuli to be visual rather than auditory.”


The authors concluded that functional specification (i.e., what modality a cortical area processes) is to a significant extent instructed by its extrinsic inputs rather than being rigidly predetermined.


### Laminar-Columnar Architecture


The mammalian cortex uses a repeating six-layer architecture organized in vertical columns. This canonical microcircuit can be adapted to many modalities: vision, audition, motor, and cognition.


Six Cortical Layers


I


Molecular


II


External Granular


III


External Pyramidal


IV


Internal Granular


V


Internal Pyramidal


VI


Multiform (Polymorphic)


Input from thalamus (Layer IV)


Output to subcortical (Layer V)


Layer Details


Select or hover over a layer to see details


Canonical Microcircuit


This repeating six-layer structure implements a computational motif that can be adapted across sensory modalities. The architecture is remarkably consistent whether processing visual, auditory, or motor information, suggesting a universal cortical algorithm.


This is very strong evidence that the cortex implements a **general-purpose architecture that can be adapted to different modalities and tasks based on the input it receives** , which is precisely the premise of modern, multi-modal transformer architectures. In other words, both biological and artificial systems appear to share a common principle of using a general architecture that can be specialized for different tasks and modalities. There is no strong reason to believe that general functions (and possibly transformers) are incapable of implementing 'reasoning', or that this reasoning ability cannot be adapted to different tasks and modalities.


### World modelling


World modelling refers to the ability of a system to create an internal representation of the external world, allowing it to simulate scenarios, predict outcomes, and make decisions based on its understanding of cause and effect. This is closely related to reasoning, as a robust world model enables more effective reasoning about complex situations.


In ICLR 2024, a team from DeepMind presented the paper["Robust Agents Learn Causal World Models"](https://openreview.net/forum?id=1YkX3b2k0n) \[8\]. They showed that any agent capable of satisfying a regret bound for a large set of distributional shifts must have learned an approximate causal model of the data generating process, **which converges to the true causal model for optimal agents.**


In other words, agents that can adapt to distribution shifts must learn causal relationships in the world. This is a strong theoretical result linking the ability to handle distribution shifts with the need for causal world modelling. For instance, in the supervised learning setting, they showed that identifying regret-bounded policies under covariate and label shifts (as discussed above) *requires* learning the causal relations between features and labels.


In Appendix B, the authors run simple simulated experiments showing that if you watch how well an agent performs under certain controlled conditions, you can actually work backwards and figure out the hidden cause-and-effect structure of the environment. In other words, by studying the behaviour of agents that keep their mistakes low, you can reconstruct the underlying causal relationships. Their results suggest that agents could be powerful resources for causal discovery.


#### Implications for foundation models


This is all to say that general functions trained on vast (multimodal) datasets that include interventional data and sensible assumptions can implement both reasoning algorithms and causal world models. We believe that overly skeptical views on the reasoning and world-modelling capabilities of ML systems are unlikely to align with the evidence from both neuroscience and machine learning theory.


Are there foundation models that can robustly reason and build causal world models at present? No. For one, we don't know the computation that the brain implements. This computation appears to be very data efficient, and we don't yet know how to build models that learn as efficiently as biological systems. However, there is no strong reason to believe that foundation models cannot eventually implement reasoning and world modelling capabilities, especially as we continue to improve our architectures, training regimes, and data quality.


## On the obvious utility of foundation models


Current foundation models are not perfect. They have limitations, and there are many open research questions about how to improve them. However, they are already incredibly valuable in healthcare in general and radiology in particular.


### Foundation models for general diagnosis


AI systems have demonstrated strong performance in detecting rare pathologies when trained for that purpose. Several studies have reported such results. For example, work by Mao et al. \[9\] describes how a phenotype-based AI pipeline matched or exceeded human experts in diagnosing rare diseases from electronic health records. Similarly, Google’s AMIE study \[10\] found that AI could enhance diagnostic accuracy, especially for rare and complex cases.


### Foundation models for radiology


In a prospective trial at Northwestern Medicine, a foundation model for interpreting radiographs improved radiologist efficiency by 15.5% without impacting diagnostic accuracy \[11\]. The model provided preliminary reads that radiologists could then review and finalize, streamlining their workflow. Elsewhere foundation models have been shown to improve efficiency by up to 36% in controlled settings \[12\]. Despite foundation models only having recently been introduced to radiology, these early results are a clear indication of their potential to enhance clinical workflows.


## Conclusion: The future is adaptive and grounded


Foundation models are not static vaults of knowledge; every single frontier lab expands their data, improves their training strategy, and releases new models regularly. Foundation models are shifting maps of the world. Their power emerges not from knowing everything, but from being able to learn anything. Amid the noise and shifting narratives around foundation models, it has become increasingly important to frame them accurately and understand the principles that truly govern their behavior.


To be effective, they require the right guidance. Architectures that encode the right assumptions, methods that let them adapt beyond their training distribution, and training that pushes them toward richer internal models of the world. At Mecha Health, we’re building for that future: adaptive, grounded, and calibrated to the world as it actually is.


```text
@misc{mecha2025foundationmodels,
author = {Ahmed Abdulaal and Hugo Fry and Ayodeji Ijishakin and Nina Montaña Brown},
title = {Maps, Misconceptions, and the Making of Modern Foundation Models},
year = {2025},
month = {November 12},
url = {https://mecha-health.ai/blog/Understanding-Foundation-Models},
note = {A deep dive into foundation models, their training, and blindsides.}
}


```


## References


1. Fu, Tairan, et al. "Why Do Large Language Models (LLMs) Struggle to Count Letters?." arXiv preprint arXiv:2412.18626 (2024).
2. Balazevic, Ivana, et al. "Towards in-context scene understanding." Advances in Neural Information Processing Systems 36 (2023): 63758-63778.
3. Schaeffer, Rylan, Brando Miranda, and Sanmi Koyejo. "Are emergent abilities of large language models a mirage?." Advances in neural information processing systems 36 (2023): 55565-55581.
4. Yu, Alice C., Bahram Mohajer, and John Eng. "External validation of deep learning algorithms for radiologic diagnosis: a systematic review." Radiology: Artificial Intelligence 4.3 (2022): e210064.
5. Domalpally, Amitha, and Roomasa Channa. "Real-world validation of artificial intelligence algorithms for ophthalmic imaging." The Lancet Digital Health 3.8 (2021): e463-e464.
6. Deng, Jiawen, et al. "So You’ve Got a High AUC, Now What? An Overview of Important Considerations when Bringing Machine-Learning Models from Computer to Bedside." Medical Decision Making (2025): 0272989X251343082.
7. Von Melchner, Laurie, Sarah L. Pallas, and Mriganka Sur. "Visual behaviour mediated by retinal projections directed to the auditory pathway." Nature 404.6780 (2000): 871-876.
8. Richens, Jonathan, and Tom Everitt. "Robust agents learn causal world models." arXiv preprint arXiv:2402.10877 (2024).
9. Mao, Xiaohao, et al. "A phenotype-based AI pipeline outperforms human experts in differentially diagnosing rare diseases using EHRs." npj Digital Medicine 8.1 (2025): 68.
10. McDuff, Daniel, et al. "Towards accurate differential diagnosis with large language models." Nature (2025): 1-7.
11. Huang, Jonathan, et al. "Efficiency and Quality of Generative AI–Assisted Radiograph Reporting." JAMA Network Open 8.6 (2025): e2513921-e2513921.
12. Chen, Zhihong, et al. "A Vision-Language foundation model to enhance efficiency of chest x-ray interpretation." arXiv e-prints (2024): arXiv-2401.


Talk to us


## Interested in next-generation AI for radiology?


[Talk to a founder](https://cal.com/team/mecha-health/30mins)


From the journal


## Continue reading


[Research Foundation Models Research · November 12, 2025 · 20 min read Qualitative Case Studies for Assessing Foundation Models Most metrics used to evaluate machine learning models are quantitative, but qualitative assessment is crucial. Read article](https://www.mecha-health.ai/blog/Assessing-models-qualitatively)[Research Foundation Models Evaluations Research · June 24, 2025 · 10 min read Mecha Net v0.2 — Reaching Human Performance Performance update for our Chest X-ray report generation model. Read article](https://www.mecha-health.ai/blog/Mecha-net-v0.2)
