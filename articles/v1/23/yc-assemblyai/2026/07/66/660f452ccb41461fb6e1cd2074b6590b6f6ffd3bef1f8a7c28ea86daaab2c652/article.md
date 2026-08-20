---
schema_version: "1.0.0"
document_id: "660f452ccb41461fb6e1cd2074b6590b6f6ffd3bef1f8a7c28ea86daaab2c652"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/decoding-strategies-how-llms-choose-the-next-word"
published_at: null
first_seen_at: "2026-07-21T08:04:01.744114+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:f3904df12deeb7a49e3119ed263208e5abd68b8fd1ab1f2a8de567ccf9dd0bd2"
---

# Decoding Strategies: How LLMs Choose The Next Word

With ChatGPT’s widespread popularity, there’s been growing interest in practical techniques that can enhance the reliability and performance of Large Language Models (LLMs). For the thousands of developers bringing new ideas to life with popular open-source pre-trained models, such as Meta’s[Llama series](https://llama.meta.com/?ref=assemblyai.com) , understanding how to use LLMs more effectively is essential for improving their use-case-specific performance.


While much attention has been given to[prompt engineering](https://youtu.be/aOm75o2Z5-o?ref=assemblyai.com) —techniques for tweaking input prompts to improve model outputs—these methods are developed on top of a bedrock of anecdotal findings. Although useful, such techniques may not always generalize across different models and might become obsolete with future model iterations, especially due to changes in fine-tuning stages like[Reinforcement Learning from Human Feedback](https://www.assemblyai.com/blog/how-rlhf-preference-model-tuning-works-and-how-things-may-go-wrong/) .


Besides prompt engineering, a systematic way for improving LLM outputs relies on experimenting with the fundamental operations occurring in the text generation (decoding) phase. At their core, LLMs generate probability distributions over word sequences. To generate text with them, you still have to decide how to use these distributions in order to make word choices. These sets of rules, called **decoding strategies** , are what fundamentally determines *how language models are used* to generate text.


In this blog post, you’ll get an overview of LLM decoding strategies and how they work, starting with basic concepts and progressing to some recent research on this topic.


## Next-word Predictors vs. Text Generators


Before diving into the technical details, let’s recall the important distinction between two phases of language models design:


- **Modeling phase** : How language models learn during training.
- **Decoding phase** : How they generate text–the focus of this article.


During modeling, LLMs are trained to minimize an *objective function* (a measure of prediction error) to estimate the probability of the next word in a text sequence. This process results in a statistical model of language.


The[history of LLM development](https://www.assemblyai.com/blog/the-full-story-of-large-language-models-and-rlhf/) has proven that optimizing this objective function through massive model scaling—both in computational power and training data— *effectively* models human language. That is, LLMs have been regarded as empirical evidence for a somewhat surprising fact: that a relatively simple objective such as next-word prediction is in fact a good enough proxy for capturing the extremely rich variety of linguistic patterns that can appear in human language.


As a consequence, LLMs are often described as *“next-word predictors”* in non-scientific literature. This description, however, can lead to misconceptions about how LLMs generate text in practice: in the decoding phase, LLMs don’t always output the most probable next word iteratively. They actually employ various strategies that utilize their internal statistical models of language for text generation.


**Decoding strategies** (or *sampling* / *token-selection* *strategies* ) are the decision rules used to extract coherent text strings from a model’s probability estimates.


The modeling phase involves training an LLM to map sequences of words (circles) to probability estimates. In the decoding phase, various sets of rules can be applied to extract text sequences from these probabilities.


In the diagram above, the yellow box represents a decoding strategy, with an LLM as one of its components. In this framework, the LLM provides a mathematical function that acts as a probability estimator within any specific text-generation algorithm (we’ll provide more intuition behind this diagram in the next section).


The choice of a particular decoding strategy can impact various aspects:


- **Task-specific** **performance** – specific strategies may be more suitable for creative tasks versus more predictable or structured outputs.
- **Inference speed** – a decoding strategy determines the computational cost per generated word for a model of a given size.


**A note on terminology: Words vs. Tokens**


Throughout this article, we’ll primarily use the term *token* instead of “word”. LLMs typically operate on a vocabulary of tokens, which can represent whole words, parts of words, punctuation marks, etc. This approach allows for a more efficient numerical representation of language because it reduces the size of the vocabulary the model needs to handle. However, for most purposes, you can think of tokens as roughly equivalent to words in everything that follows.


## What is language model decoding?


Language Model Decoding is the process by which an LLM generates text.


For open-ended text generation, the goal of this process can be abstractly described as follows: given an input sequence of tokens s=x1,…,xm *s* = *x* 1​,…, *xm* ​, the goal is to choose a continuation of n additional tokens y1,…,yn *y* 1​,…, *yn* ​ that form the completed sequence s’=x1,…,xm,y1,…,yn *s* ’= *x* 1​,…, *xm* ​, *y* 1​,…, *yn* ​. This continuation should be coherent and contextually appropriate as an extension of the given input.


How do we quantify “coherence” and “contextual appropriateness” for this continuation? We can use the probability function from the language model. Trained on human-generated text sequences, the function estimates, for any token x in the entire vocabulary of tokens, the likelihood that x would follow the sequence s. This quantity is denoted as P(x∣s) *P* ( *x* ∣ *s* ) and regarded as the *probability of x conditioned on the sequence s* .


A language model maps a sequence of tokens s to a probability distribution, represented by a vector with one entry for each token in the model’s vocabulary. The value corresponding to a token x in this vector represents the probability of x continuing the sequence s.


You can then picture a language model as a function that maps a token *sequence* s to a *vector* of probabilities, with entries of the form P(x∣s) *P* ( *x* ∣ *s* ), one for each token x in the vocabulary. This is depicted in the diagram above. This vector encodes the probability distribution over the vocabulary of tokens given the sequence s.


Finally, this function allows us to compute the probability of any completed token sequence based on the *probability chain rule* , as illustrated in the next figure.


The probability chain rule allows us to compute the probability P(sequence) of any text sequence by factoring it as the product of conditional probabilities of the form P(token | sequence), which an LLM has been trained to estimate.


This approach to computing text probabilities defines **Causal** **Language Modeling** . Although other approaches exist, casual language modeling has established itself as the dominant framework for open-ended text generation tasks.


With this foundation, we can describe the general schema of any decoding strategy in the context of open-ended text generation:


**Steps of a decoding strategy**


1. Given the current context sequence s, sample a token x from the distribution conditioned on s.
2. Update the context sequence to s' = s, x.
3. Repeat steps 1 and 2 until a termination condition is met.


**The method by which we sample x** from the distribution (step 1) is precisely what **distinguishes different decoding strategies.**


With this general framework in mind, let’s proceed to explore specific decoding strategies in the sections below.


We divide these strategies into two categories: **deterministic methods** , where the same input always produces the same output, and **stochastic methods** , where the output can vary even with the same input. In the final part, we’ll discuss novel methods based on **information theory** , and finally, we’ll examine some cutting-edge techniques to improve **inference speed** .


## Deterministic methods


### Greedy Search


The simplest decoding strategy for language models is known as **greedy search** . It is the most straightforward approach: at each step, choose x as the most likely token given the context sequence s and repeat this for a specified number of steps or until a termination condition is met.


Greedy Search selects the highest probability token at each step.


Note: this strategy **doesn't necessarily produce the most likely sequence** . Similar to the[rod cutting problem](https://sites.radford.edu/~nokie/classes/360/dp-rod-cutting.html?ref=assemblyai.com) , choosing the most optimal choice at each individual step does not imply the most optimal overall sequence of choices.


To visualize this, imagine the process as exploring a probability tree. Greedy search only explores a single branch of this tree - the one that seems most promising at each step. To find the most likely sequence, one must explore the full tree of all possible token combinations, which is computationally infeasible for any practical text length, due to the size of the vocabulary.


Greedy search explores the probability tree by following a single path, possibly missing higher probability branches.


Despite its efficiency, greedy search doesn’t generally produce high-quality text and is empirically found to output generic and dull, or repetitive text sequences in open-ended generation.


### Beam Search


Beam Search is the natural generalization of greedy search, offering a way to explore multiple branches in the probability tree.


Instead of the next token with the highest probability, it maintains a beam of the K most probable sequences at each time step, where the K is referred to as the beam width. Depending on the beam size, this method produces higher-quality text than greedy search, but it can be slower due to more computations.


The algorithm works as follows:


1. Consider the top K most probable next tokens for *each sequence* in the current beam (the initial beam consisting of only the input sequence).
2. Expand each sequence with these K tokens, creating K new candidate sequences per existing sequence.
3. Update the current beam by selecting the top K sequences among these candidates.
4. Repeat these steps until a termination condition is met.


By maintaining multiple candidate sequences, beam search can effectively "look ahead" in the probability tree, potentially finding higher probability sequences that greedy search might miss. The beam width K determines the trade-off between the *depth of exploration* (larger K) and *computational efficiency* (smaller K).


Beam Search explores the probability tree by evaluating multiple branches at each step, determined by the beam width K (with K=2 in the picture).


However, Beam Search often underperforms in open-ended text generation tasks. Qualitative studies based on human preferences, along with quantitative analyses comparing the variability in human-generated text with beam search outputs, have highlighted its limitations. These studies[reveal](https://arxiv.org/pdf/1904.09751?ref=assemblyai.com) that Beam Search tends to produce repetitive and unnatural text, showing clear patterns of degeneration.


Repetitions may occur because of positive feedback loops, where the probability of a repeated phrase increases with each repetition ([source](https://arxiv.org/pdf/1904.09751?ref=assemblyai.com) ).


While Beam Search text sequences generally have high likelihood scores, they lack the natural *variance* and diversity characteristic of human-written text. This can manifest with frequent repetitions of phrases (see figure above), as well as a tendency to use a less varied vocabulary––two different examples of *“neural text degeneration”* phenomena.


In the next chart we see how the human-generated text exhibits greater variance in token probabilities, reflecting diverse and sometimes unexpected word choices. In contrast, the beam search output shows very little variance, indicating a more predictable and potentially repetitive text. In fact, Beam Search text tends to converge on a subset of the model's vocabulary.


This chart shows the probability assigned to tokens generated by beam search and humans, given the same context ([source](https://arxiv.org/pdf/1904.09751?ref=assemblyai.com) ).


How can we introduce more variance and unpredictability into the generated text while still maintaining overall coherence? The common solution is to introduce some degree of **randomness** into the decoding process, leading to stochastic methods for text generation.


## Stochastic methods


The most common stochastic methods are top-k, top-p, and temperature sampling. Let’s illustrate them one by one below.


### Top-k Sampling


Top-k sampling is a stochastic decoding strategy that introduces randomness by sampling the next token from the top k most probable choices, where k is a fixed value. More precisely, it works as follows:


1. Consider only the top k most probable next tokens for the current sequence.
2. Normalize the probabilities of these k tokens to sum to 1, creating a new, truncated distribution.
3. Randomly sample a token from this new distribution and append it to the current sequence.
4. Repeat these steps until a termination condition is met.


While top-k can often be effective and is much more efficient than Beam Search, it can face limitations in certain scenarios, such as the following two edge cases:


1. **Fat-tailed distribution** : Imagine the next-token distribution is very spread out, approaching a uniform distribution. Top-k sampling would arbitrarily cut off many potentially interesting tokens, possibly limiting the diversity of the generated text.


2. **Peaky distribution** : Conversely, if the distribution is highly concentrated, top-k sampling might include unnecessary tokens when k is too large or exclude equally probable ones when k is relatively small.


With the value for k being fixed, top-k may fail to adapt to edge situations, such as fat-tailed or peaky distributions for the next token.


These examples highlight the main challenge with top-k sampling: choosing an optimal k value. The ideal k may in fact vary depending on the context and the shape of the probability distribution at each step. A fixed k value might be too restrictive in some cases and too permissive in others.


### Top-p Sampling (Nucleus Sampling)


Top-p sampling represents an improvement over top-k sampling by offering a more dynamic approach to token selection.


With p being a fixed *probability threshold* (for example p = 0.7), here's how top-p sampling works:


1. Select the minimum number of tokens (ordered by highest probability) whose cumulative probability meets or exceeds the threshold p.
2. Normalize their probabilities to sum to 1, creating a new, truncated distribution.
4. Repeat these steps until a termination condition is met.


While top-k always considers a fixed number of tokens, top-p adapts based on the probability distribution at each particular step. At each step, the selected tokens form the "nucleus" from which the next token is sampled––which explains why top-p sampling is also referred to as *nucleus sampling* in the literature.


Note how top-p handles the two scenarios discussed above more dynamically than top-k:


- For flat distributions (where many tokens have similar, low probabilities), top-p would sample from a larger pool of tokens, preserving the diversity of possible choices.
- For peaky distributions (where a few tokens have high probabilities), top-p would sample from fewer tokens, focusing on the most likely options.


### Temperature Sampling


The idea behind temperature sampling is to adjust the "sharpness" of the probability distribution. This is achieved by introducing a parameter t (temperature) in the[softmax](https://en.wikipedia.org/wiki/Softmax_function?ref=assemblyai.com) function, which is used after the final layer of a transformer to compute token probabilities.


Figure: The softmax function is used to re-normalize the vector output of the last layer of an LLM into a vector of probabilities.


The temperature parameter t is proportional to the amount of randomness in the sampling process. To understand how temperature affects the distribution, let’s consider three different cases:


**Low Temperature (0 < t < 1)**


When the temperature is set below 1, the probability distribution becomes more concentrated or "peaky" around the most likely tokens. As t approaches 0, the distribution becomes increasingly skewed. The probabilities of the most likely tokens increase, while those of less likely tokens decrease. In the extreme case (t very close to 0), the sampling process approximates greedy search and becomes deterministic.


**Temperature t = 1 (Pure Sampling)**


Setting t = 1 leaves the original probability distribution unchanged. This is often referred to as *pure* or *ancestral sampling* . In this setting, the model samples from the full vocabulary according to the original probabilities. Unlike top-k or top-p sampling, no tokens are excluded from consideration.


**High Temperature (t > 1)**


Temperatures above 1 flatten the probability distribution, increasing randomness: this makes it more likely to choose less probable tokens. As t increases, the distribution approaches a uniform distribution, with each token having an equal probability of being selected.


Lower temperatures tend to produce more deterministic, but potentially repetitive text. Higher temperatures generate more diverse and unpredictable text, potentially at the cost of coherence and relevance.


Temperature sampling is one of the most common decoding strategies. Depending on the intended application, finding the optimal temperature value requires ad-hoc experimentation.


While pure sampling (t=1) might seem ideal in theory––as it matches the distribution the model was trained on––it often leads to less coherent text in practice. The accepted explanation is that the "long tail" of low-probability tokens is responsible for this. What happens is that the aggregated probability of these unlikely tokens can lead to frequent sampling from this unreliable tail, thus producing low-quality sequences.


Finally, even though stochastic decoding strategies like top-k, top-p, and temperature sampling introduce varying degrees of unpredictability compared to deterministic methods, they still fundamentally aim to **maximize the likelihood** of text sequences.


## Optimizing Information-Content via Typical Sampling


Is maximizing the likelihood (with some added randomness) the optimal way to generate high-quality text?


Intuitively, in human communication there exists a balance between predictability and surprise. And this, not so much at the grammatical level –with choices of less common vocabulary, but rather at the level of the **information content** of language.


This idea is the starting point for[typical sampling](https://arxiv.org/pdf/2202.00666?ref=assemblyai.com) , an alternative approach to language model decoding that aims to apply principles from information theory to text generation.


If natural language can be cast as a process of *information transmission* , it may be reasonable to assume that, for “effective” communication, this **information should be encoded in text sequences at an optimal rate** .


By viewing text generation through this lens, typical sampling builds upon two principles that guide what makes for effective communication:


1. Keep sentences short and information-dense. We want to maximize the amount of information conveyed in a given length of text.
2. Avoid moments of high information density. We want to avoid excessively complex or surprising sequences that can be too difficult for the listener to process.


The “expected information hypothesis” is the basic intuition behind typical sampling ([source](https://rycolab.io/media/ACL2022-final.pdf?ref=assemblyai.com) ).


Given that these principles obviously trade off against each other, how can we guide a language model to find the “sweet spot” between them?


Typical sampling proposes generating sentences with the **expected information content** given prior context, in order to produce text that is informative enough to be engaging, yet not so complex to overwhelm the reader.


In information theory,[entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)?ref=assemblyai.com) provides a mathematical measure of the *average information content in a probability distribution* . By targeting text with average entropy, typical sampling seeks to balance predictability and surprise in generated content.


Compared to top-k (green) and top-p (red) sampling, typical sampling (blue) offers competitive performance on story generation and summarization tasks, while consistently reducing repetitions indicated by lower REP values in the chart ([source](https://arxiv.org/pdf/2202.00666?ref=assemblyai.com) ).


On a short speculative note, although the current implementation of typical sampling focuses on token-level sampling, it would be interesting to see whether the underlying principles could also be applied at different levels of text generation, such as sentence level, or even at the overall text planning. Conceivably, these principles may also be applied to better inform techniques such as[chain-of-thought prompting](https://arxiv.org/abs/2201.11903?ref=assemblyai.com) and[Reflexion](https://arxiv.org/abs/2303.11366?ref=assemblyai.com) .


A code implementation of typical sampling can be found[here](https://github.com/cimeister/typical-sampling?ref=assemblyai.com) .


## Boosting inference speed via Speculative Sampling


So far we’ve discussed some of the most common LLM decoding strategies from the point of view of output quality. But there can be other factors that go into this choice. One that has become a pressing challenge in today’s LLM deployment is inference speed.


Beyond the immediate benefits of lowering costs and energy footprint, enhancing LLM inference efficiency is largely driven by a compelling idea: scaling up the compute used for LLM inference while keeping costs fixed could unlock new applications and capabilities which are currently unattainable with even the largest models. Concrete examples are[LLM-based workflows automation and AI agents](https://youtu.be/Q7KhrSbEnSQ?ref=assemblyai.com) .


All current LLMs rely on the[Transformer architecture](https://www.assemblyai.com/blog/transformers-for-beginners/) . By design, the time to generate a single token with one model call (forward pass) is *proportional to the transformers size* , in terms of both model parameters and memory.


**Speculative Sampling** is a decoding strategy––recently discovered independently by teams at Google[Research](https://arxiv.org/pdf/2211.17192?ref=assemblyai.com) and[DeepMind](https://arxiv.org/pdf/2302.01318?ref=assemblyai.com) ––that yields 2-3x speedups by generating multiple tokens per model pass and *without changes to the final output* .


The idea is to redistribute the model calls needed to generate a sequence between **two models working in tandem** instead of just one. Normally, we need 1 call per generated token. That is, the *exact same amount of compute is distributed equally* between all tokens.


However, this counters the intuition that some tokens should be easier to predict than others, as illustrated in the figure above. The “easy tokens” should also be successfully predicted by a much smaller model. But how to preserve the quality of the original model while not failing at predicting the “hard tokens”? Speculative Sampling redesigns the generation loop as follows:


**The Speculative Sampling Algorithm**


1. **Drafting** : A *draft model* (smaller, faster model) generates a fixed number K of tokens starting from the given context sequence. For sake of illustration, let’s stick to K=5 (which turns out to be optimal). This requires 5 passes from this model. On top of the 5 selected tokens, also the corresponding 5 next-token distributions are cached.
2. **Verification** : A *target model* (the original, larger model) is used as a verifier. With a single forward pass, it is evaluated on the whole sequence (prefix + 5 draft tokens). This yields 6 next-token distributions: one for the prefix sequence and one for each token position. These distributions are cached.
3. **Correction** : Following a technique called[rejection sampling](https://en.wikipedia.org/wiki/Rejection_sampling?ref=assemblyai.com) , the draft tokens are sequentially approved or rejected based on the *ratio of probabilities from the target and draft models* . If a token is rejected, the process stops, and the next token is sampled from an adjusted distribution derived from the target model's prediction.


Why may using two models instead of one improve speed? The key reason is in the verification step, and relies on a crucial feature of transformer models: the[attention mechanism](https://en.wikipedia.org/wiki/Attention_(machine_learning)?ref=assemblyai.com) computes (and caches) the attention weights for all tokens simultaneously by using matrix operations on the entire sequence, enabling the model to **process each token position concurrently within a single pass** .


Now, to understand why this method is more efficient than sampling each token directly from the target model, let’s analyze the best and worst case scenarios at each iteration:


- **Best Case** : All 5 draft tokens are accepted, generating 5 tokens per large model pass. The cost of the draft model is considered negligible compared to the target model. This gives roughly a 5x speedup (or slightly less) over the baseline.
- **Worst Case** : Even if the first token is rejected, 1 token is still generated using the target model's distribution. The cost is essentially the same as the baseline: one target model pass per 1 generated tokens.


Note that the target model will approve tokens only if its own predictions match with the draft model, and will reject the generated tokens otherwise. Intuitively, this ensures that *the final generated sequence aligns with the distribution of the target model* , even though we are not sampling from this distribution directly. Moreover, this technique works in *combination* with any other decoding strategy, as there is freedom to choose the one used by the draft model.


## Final Words


In this blog post we took a deep dive into decoding strategies, exploring how both deterministic and stochastic methods are used to shape the word choices of an LLM for open-ended text generation.


We took a closer look at less conventional approaches like typical sampling, which introduces information-theoretic principles to find an optimal balance between predictability and surprise of AI-generated text.


We’ve learned about the latest techniques such as speculative sampling to advance LLM inference speed and potentially unlock new possibilities in AI-driven automation.


As research in this area continues, we hope to see even more sophisticated decoding techniques emerge, and we’ll cover them in future blog posts. If you want to learn more about LLMs, we recommend the following articles:


- [How RLHF Preference Model Tuning Works (And How Things May Go Wrong)](https://www.assemblyai.com/blog/how-rlhf-preference-model-tuning-works-and-how-things-may-go-wrong/)
- [How Reinforcement Learning from AI Feedback works](https://www.assemblyai.com/blog/how-reinforcement-learning-from-ai-feedback-works/)
- [RLHF vs RLAIF for language model alignment](https://www.assemblyai.com/blog/rlhf-vs-rlaif-for-language-model-alignment/)


You can also follow our[YouTube channel](https://www.youtube.com/@AssemblyAI?ref=assemblyai.com) for more educational resources on AI.
