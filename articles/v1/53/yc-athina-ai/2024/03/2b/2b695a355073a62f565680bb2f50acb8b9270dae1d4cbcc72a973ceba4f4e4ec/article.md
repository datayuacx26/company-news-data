---
schema_version: "1.0.0"
document_id: "2b695a355073a62f565680bb2f50acb8b9270dae1d4cbcc72a973ceba4f4e4ec"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/badclip-trigger-aware-prompt-learning-for-backdoor-attacks-on-clip"
published_at: "2024-03-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:e64937f59667292bb3e37a7339fa6c2e5fc2cb88273505b71a5d0115ffecbd70"
---

# BadCLIP: Trigger-Aware Prompt Learning for Backdoor Attacks on CLIP

Do not index


Original Paper


[https://arxiv.org/abs/2311.16194](https://arxiv.org/abs/2311.16194)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2311.16194](https://arxiv.org/abs/2311.16194)


**By:**[Jiawang Bai](https://arxiv.org/search/cs?searchtype=author&query=Bai%2C%20J) ,[Kuofeng Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20K) ,[Shaobo Min](https://arxiv.org/search/cs?searchtype=author&query=Min%2C%20S) ,[Shu-Tao Xia](https://arxiv.org/search/cs?searchtype=author&query=Xia%2C%20S) ,[Zhifeng Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Z) ,[Wei Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20W)


**Abstract:**


> Contrastive Vision-Language Pre-training, known as CLIP, has shown promising effectiveness in addressing downstream image recognition tasks. However, recent works revealed that the CLIP model can be implanted with a downstream-oriented backdoor. On downstream tasks, one victim model performs well on clean samples but predicts a specific target class whenever a specific trigger is present. For injecting a backdoor, existing attacks depend on a large amount of additional data to maliciously fine-tune the entire pre-trained CLIP model, which makes them inapplicable to data-limited scenarios. In this work, motivated by the recent success of learnable prompts, we address this problem by injecting a backdoor into the CLIP model in the prompt learning stage. Our method named BadCLIP is built on a novel and effective mechanism in backdoor attacks on CLIP, i.e., influencing both the image and text encoders with the trigger. It consists of a learnable trigger applied to images and a trigger-aware context generator, such that the trigger can change text features via trigger-aware prompts, resulting in a powerful and generalizable attack. Extensive experiments conducted on 11 datasets verify that the clean accuracy of BadCLIP is similar to those of advanced prompt learning methods and the attack success rate is higher than 99% in most cases. BadCLIP is also generalizable to unseen classes, and shows a strong generalization capability under cross-dataset and cross-domain settings.


---


###


Summary Notes


####


Introduction to BadCLIP: A New Backdoor Threat to CLIP Models


With the rapid advancement of artificial intelligence (AI), the security of machine learning models has become a pressing concern. CLIP models, known for their innovative approach to correlating text and images, are not immune to these security risks, particularly backdoor attacks.


This post delves into BadCLIP, a new method for compromising CLIP models, highlighting a significant challenge in AI security.


####


Understanding CLIP Model Vulnerabilities


CLIP models have revolutionized AI's understanding of text and image relationships by training on vast datasets of image-text pairs.


This capability, while powerful, also introduces a vulnerability due to the model's reliance on text prompts, making it susceptible to manipulation.


Traditional backdoor attacks have been somewhat limited, requiring extensive additional data and access to pre-training datasets.


####


Introducing BadCLIP: A Novel Backdoor Strategy


BadCLIP represents a breakthrough in backdoor attack methodologies by exploiting prompt learning—adapting models to new tasks through text prompts—to inject backdoors into CLIP models.


This innovative method does not require additional data, making it a more feasible and concerning approach to compromising AI systems.


####


How BadCLIP Operates


- **Trigger-Aware Prompt Learning** : BadCLIP uses a learnable trigger in images that, when detected by the model, activates the backdoor, altering outcomes.


- **Optimization Strategy** : It optimizes these triggers and the corresponding text prompts to ensure the backdoor's effectiveness and concealment.


- **Integration with CLIP** : BadCLIP manipulates both image and text encoders in CLIP, causing misclassification of only the images with the trigger while preserving accuracy on clean images.


####


Evaluating BadCLIP's Impact


Through extensive testing across 11 diverse datasets, BadCLIP has demonstrated its effectiveness and alarming potential:


- **Experimental Results** : BadCLIP maintains high accuracy on clean images and achieves high attack success rates, outperforming existing methods.


- **Robustness and Stealth** : Its ability to evade detection by common backdoor detection methods points to the need for new defensive approaches.


####


The Bigger Picture: Implications and Future Work


BadCLIP's development highlights a critical vulnerability in CLIP models and AI systems that rely on vision-language pre-training.


This calls for continued research into more advanced defensive measures to counter such backdoor attacks.


####


Conclusion


BadCLIP marks a significant development in the realm of AI security, presenting a new and potent threat to the integrity of CLIP-based models.


This advancement not only necessitates a deeper understanding of such vulnerabilities among AI professionals but also underscores the urgent need for innovative defenses.


####


Acknowledgments


This research was supported by contributions from the National Natural Science Foundation of China, the Shenzhen Science and Technology Program, and the PCNL KEY project, showcasing the collaborative effort needed to enhance our understanding of AI security challenges.


As AI evolves, so do the strategies to exploit its weaknesses, emphasizing the importance of ongoing research and collaboration to develop effective countermeasures against threats like BadCLIP.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
