---
schema_version: "1.0.0"
document_id: "c9dcb29de7289062df522066cb8b3ff4fc509706d842a26e5187cafdfa2167b6"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/how-we-replaced-our-legacy-name-matching-algorithm-with-ai-and-boosted-accuracy-to-over-99-7e4bc02055ac"
published_at: "2025-09-02T07:31:39+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:06c78ea3627def83d12aa2c84e8607a88408f2bafa1d8c805d469c9fbd188d4f"
---

# How We Replaced Our Legacy Name Matching Algorithm with AI — and Boosted Accuracy to Over 99%

# How we replaced our legacy name matching algorithm with AI and boosted accuracy to over 99%


[Almog Zemach](https://medium.com/@almogze7?source=post_page---byline--7e4bc02055ac---------------------------------------)


8 min read


·


Sep 2, 2025


--


Press enter or click to view image in full size


## The real-world problem


At **Payoneer** , we take complianc **e** very seriously. Before any payment is approved, it goes through a series of validation steps designed to **** protect our customers and meet regulatory standards. One of these steps is verifying that the recipient’s name matches what we have in **** our system.


In practice, things can get a bit tricky. Imagine your name appears as “ **Rami Saleem** ” in one place and “ **Ramy Salim** ” in another, or maybe it’s written in a different language altogether. These small variations can trigger manual reviews, requiring document uploads and identity checks, and resulting in frustrating delays.


Now scale that across millions of global transactions, dozens of languages, and countless naming patterns. Our legacy fuzzy-matching algorithm, based on simple string similarity, couldn’t keep up. **We needed something smarter, so we built one** .


Same person, different outcome. All because of a name.


## Why the old algorithm wasn’t enough


The old algorithm had several difficulties, mainly because of its rule-based nature. I’ll demonstrate three different aspects:


### 1. Multilingual name variants


The algorithm was designed to compare character sequences, which works reasonably well for names written in Latin or Cyrillic scripts. However, it completely breaks down with languages like Arabic, Hindi, or Chinese, where the same name may appear entirely different across different scripts. For example, it couldn’t match **“Albert Einstein”** with **“ألبرت أينشتاين”** , or **“Mahatma Gandhi”** with **“महात्मा गांधी.”** These names may **sound identical, but share no visual similarity** , making them invisible to string-distance methods.


### 2. Personal and business name relationships


The algorithm couldn’t recognize when a personal name appeared inside a company name, a common pattern in our use case. For example, it failed to see that **“John Mish”** is likely affiliated with **“JMish Logistics Ltd.”** , or that **“J. Mish”** might be the owner of **“MISH GLOBAL TRADING.”** It treated these as unrelated names simply because the formats were different, **missing the deeper semantic and structural link** between individuals and businesses.


### 3. Recognizing patterns


Different markets follow unique patterns, and the old algorithm had no way of learning or adapting to them. One common example comes from Chinese business names, where it’s standard to append a variation of “有限公司” (which translates to “Limited Company”), which shows up in many different transliterations: **“youxian gongsi,” “you xian gong si,” “youxia”** , **** etc. These are regional norms. But to a basic string-matching algorithm, each variation looks unrelated. **Without understanding these naming structures, the system failed.**


Once we understood the limits of the old algorithm, the path forward became clear: **build a model that understands names** **the way humans do** .


## Preparing the data


We decided to build a machine learning model, but before anything could be created, we needed to overcome one of the biggest hurdles: data. We had to make a high-quality, labeled dataset **** that our model could learn from. We started with millions of name pairs sourced from Payoneer’s historical system records. The next step? Tagging each pair as either a match or a mismatch. Sounds simple, right? Not quite.


Manually labeling millions of name pairs wasn’t feasible, so we turned to a closed Azure-hosted instance of GPT-4o-mini to do the heavy lifting.


### My two cents on prompt engineering


At first glance, name similarity seems trivial. You might think: “Just ask an AI: *Are ‘John’ and ‘Jonathan’ similar?* and you’re done.” That’s what I thought too. But once we started digging, we hit all kinds of edge cases.


For example, names like **“Muhammad Ali”** and **“Hassan Ali”** share a surname but refer to completely different people. Or take **“Neil Armstrong”** and **“Armstrong Aeronautics LTD.”** a person and his company. Then there are abbreviations, reversed name orders, transliterations, and culturally specific patterns. Suddenly, what seemed like a simple yes-or-no question became a nuanced decision that could affect whether a payment goes through.


Same last name. Different person.


To handle these complexities, we had to invest time into prompt engineering, crafting smarter prompts, and reprocessing unclear cases with more powerful models.


To get reliable results, we first built a seed dataset of 1,000 manually tagged name pairs from production data: 40% mismatches, 60% matches. These weren’t random examples, they were carefully selected to reflect the most challenging edge cases we encountered, including tricky linguistic variants, cultural nuances, and ambiguous affiliations. Accuracy on this dataset was lower than on simpler samples, but that was intentional. We deliberately chose it that way to uncover where it struggled.


To increase confidence in the tagging, we ran a second iteration over all name pairs where GPT-4o-mini’s output differed from the legacy system, this time using a more powerful language model (GPT-4o) to review the edge cases with greater nuance. Finally, we sampled all false positives and false negatives cases where the tagging had conflict, and manually reviewed them to refine the dataset further.


### **What to avoid**


**Binary answers**


Asking for rigid “Yes/No” outputs often reduces the model’s flexibility. Names aren’t always easy to compare, and the AI needed room to reason. That aligns with the table below, which shows that the sole binary response receives a bad score for both GPT-4o-mini and GPT-4o.


**Overly specific system prompts**


We tried to improve the binary answer by giving the language model a detailed system prompt, like:


> “Before you compare the names, check the following: phonetic similarity, translation alignment. If the name reflects a company, the second name must be either a company name or an individual affiliated with the company. Do not accept partial matches unless both names share at least 70% of characters, etc…”


But ironically, **too many constraints caused the model to get stuck** or misinterpret instructions. Like a human overwhelmed by a checklist, it performed worse when over-directed.


### What works


**Few-shot prompting**


## Get Almog Zemach’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Showing the model 3–5 example name pairs (with labels) helped the model “understand the rules” we were following, like how to treat nicknames or order differences.


**Chain-of-thought**


This involved asking the model to explain its reasoning before answering. For example, adding prompts like *“Explain your reasoning before answering”* improved results across the board. While both the Mini and GPT-4o models benefited, the effect was **significantly stronger with GPT-4o** , which was better at understanding nuanced logic and applying consistent decision patterns.


The table below illustrates how each method performed. For reference, **FPR** (false positive rate) measures how often the model incorrectly labels mismatched names as matches, which reflects risk. **FNR** (false negative rate) shows how often true matches were missed, which leads to payment friction or poor user experience. Balancing these two was key to our evaluation.


Press enter or click to view image in full size


Evaluation results of different prompting strategies for name similarity classification, on a 1,000-name-pairs dataset. Few-shot reasoning with GPT-4o achieved the best performance, while over-specific zero-shot prompts performed the worst, with accuracy close to random chance.


## Training a Machine Learning (ML) model


With a labeled dataset in hand, we were ready to train a model. Rather than starting from scratch, we leveraged a pre-trained multilingual **SBERT** (Sentence-BERT) model, a powerful encoder that transforms text into dense vector embeddings, from[Hugging Face](https://huggingface.co/) **** – **** a popular open-source hub where the AI community can share and collaborate on models.


This gave us a strong foundation: SBERT understands linguistic patterns across many languages. What it didn’t know (yet) was how to compute name similarity in the way we wanted.


Using the *‘sentence_transformer’* library, we fine-tuned the model on millions of name pairs, teaching it to bring matching names closer together in embedding space and push mismatches farther apart.


Press enter or click to view image in full size


Fine-tuned SBERT Flow. The ML model takes two names, converts them into vector embeddings, and computes their similarity using cosine similarity. If the score is above a certain threshold, we classify them as a match.


At its core, this training process is known as **gradient descent** : the model looks at a batch of examples at a time, measures how wrong it is (the “loss”), and then adjusts its internal weights step by step to reduce the error. After thousands of times of doing this, the model starts getting very good at the task.


**A few key training parameters:**


- **Learning rate (LR):** how big each step is during gradient descent.
- **Batch size:** how many examples the model sees before each update.
- **Optimizer:** the method that controls how the steps are applied.
- **Epochs:** how many times we cycle through the dataset.


Press enter or click to view image in full size


Gradient descent process. The model adjusts its weights step by step to gradually minimize error.


Training is mostly about configuring these parameters, but choosing the right values matters a lot. Too high a learning rate can make the training process unstable, causing the model to bounce around instead of improving. Too many epochs can cause overfitting, and the wrong optimizer can slow progress. Getting these settings right has a significant impact on model performance.


For readers who’d like to explore more, the[Sentence-Transformers training guide](https://www.sbert.net/docs/sentence_transformer/training_overview.html) and the[Hugging Face course](https://huggingface.co/learn/llm-course/chapter1/1) offer excellent hands-on introductions.


## Why not just use GPT for inference?


We already used AI to classify the data. Why not use it instead of SBERT? Large language models (LLMs) like GPT are powerful, but they come with trade-offs that make them impractical for real-time, production-grade name matching:


- **They’re slow** . Even the fastest GPT variants operate at a latency that’s significantly higher (a few seconds in the worst-case) than a compact model optimized for direct inference. For high-volume systems like ours, this is impractical.
- **They cost money** . LLMs typically require API calls or heavy infrastructure, which adds recurring costs that scale with usage, especially when dealing with millions of name comparisons.
- They’re general-purpose tools, **not tailored for specific business logic** . A fine-tuned, domain-specific model like ours performs better on tasks like detecting name variations, transliterations, or abbreviations, because it’s trained explicitly for that.
- **LLMs can misfire** . They sometimes associate names with unrelated sensitive content, like jailbreaks, hate speech, or adult topics, due to patterns learned during pretraining. For example, ‘ *Alberto Nino’* was flagged incorrectly by GPT due to an unrelated internet reference.


For these reasons, we chose to use GPT as a tool during development to help generate labels and assist in exploration, but **we** **opted for a compact, accurate, and production-ready model** for inference.


## Not without challenges


While the new model delivers a significant leap in accuracy, it also introduces new challenges we had to carefully weigh:


- **Latency increased by a factor of 30** , from microseconds to around 5 milliseconds per request. While still fast, the additional computational load matters at scale.
- The model may degrade over time as naming conventions may change due to cultural shifts or emerging regions. **The model’s performance might gradually decline** unless retrained.
- There’s also the **issue of explainability** . Unlike rule-based systems, our model uses embedding-based similarity scores, which are very effective but harder to interpret. Explaining why two names are considered a match isn’t always straightforward for end-users or auditors.


## Wrapping up


This initiative aimed to enhance Payoneer’s compliance processes and minimize friction for our customers, using AI to address a core business challenge. From building a high-quality dataset to deploying a semantic ML model under real-world constraints, every step forced us to adapt, experiment, and embrace new concepts. The results speak for themselves: **more than** **15** % **fewer mismatches a year** , reduced payment friction, and a smarter approach to name matching. And this is just the beginning. We’re proud of this milestone and even more excited for what comes next.
