---
schema_version: "1.0.0"
document_id: "d900202d9cd91161e5c599e2746591318f9dfa03ab0b66af5a35d534aa2138ab"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/how-llms-work/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:721bcea9360740ccf37029862b7e835cb07cda25b0d011d4981b7ad8b34cbf1a"
---

# How LLMs Actually Work

A large language model is a neural network trained to predict the next token in a sequence; it converts text into numeric vectors, uses attention to let each token gather context from the others, and generates output one token at a time by sampling from a probability distribution over its vocabulary. That single mechanism — predict a token, append it, predict the next — is the whole engine behind ChatGPT, GitHub Copilot, Claude, and Gemini. Everything else is scale, training procedure, and engineering on top of it.


If you use these tools daily but have never opened the hood, the behaviors you’ve noticed — confident wrong answers, a chat that “forgets” how it started, wildly different output from a slightly reworded prompt, an inability to count the letters in “strawberry” — are not random quirks. Each one falls directly out of the mechanics. This article gives you an accurate mental model of those mechanics: tokens and embeddings, the transformer’s attention and feed-forward layers, the three training phases, how generation actually samples text, and a named pass through the misconceptions the word “actually” in the title is here to correct.


## Key Takeaways


- An LLM does not retrieve answers from a database — it computes a fresh probability distribution over its entire vocabulary at every step, picks one token, then repeats, so the “reasoning” you read is produced left to right rather than looked up whole.
- LLMs read sub-word tokens, not individual letters, which is why spelling and letter-counting tasks fail more often than the model’s general fluency would predict.
- Training runs in three stages: self-supervised pre-training (the next word is its own label), instruction fine-tuning on human-written prompt–response pairs, and reinforcement learning from human feedback (RLHF).
- By default an LLM is probabilistic, not deterministic: at any temperature above zero, the same prompt can return different answers because the model samples from a distribution instead of always taking the most likely token.
- Hallucination is the default behavior of a system optimized to produce statistically plausible text — it is rewarded for sounding right, not for being right.


## What an LLM actually is: next-token prediction


At its core, a large language model solves one narrow problem over and over: given a sequence of tokens, what token comes next? It frames this as a classification problem across its entire vocabulary — tens of thousands of possible tokens — and outputs a probability for each. The model picks one, appends it to the sequence, and runs the whole thing again. Text generation is that loop running until a stop condition.


This reframing is the key insight. “Write a function that debounces a callback” is not a command the model looks up an answer to. It is a prompt the model continues, token by token, because sequences that look like that prompt were, in its training data, followed by sequences that look like working code. The model is a continuation engine pointed at your input.


The architecture that made this work at scale is the **transformer** , introduced in the 2017 paper[Attention Is All You Need](https://arxiv.org/abs/1706.03762) by Vaswani et al. The “GPT” in ChatGPT stands for Generative Pre-trained Transformer. Before transformers, models processed text more or less left to right and struggled to connect words far apart in a sentence; attention let a model weigh every token against every other token in parallel, which is what unlocked both quality and training speed.


## Tokens and embeddings: how text becomes numbers


Before a model can do anything with your prompt, it breaks the text into **tokens** — sub-word chunks, not words and not letters — and maps each token to a number. A token can be a whole word (` " the"` ), a word fragment (` "ing"` ), a single character, or punctuation. This tokenization step is invisible in the chat UI but it explains an entire class of failures.


Because the model reads sub-word tokens rather than individual letters, asking it to count the letters in a word works against the grain of how it represents text — which is why letter-counting and spelling tasks fail more often than the model’s general fluency would suggest. The model never “sees” the three r’s in “strawberry”; it sees a couple of opaque integer IDs standing in for token chunks, with the letters baked away.


You can watch this happen with OpenAI’s open-source[tiktoken](https://github.com/openai/tiktoken) tokenizer:


```text
# pip install tiktoken
import   tiktoken


enc   =   tiktoken.get_encoding(  "  cl100k_base  "  )
ids   =   enc.encode(  "  strawberry  "  )
print  (ids)                         # a short list of integer token IDs
print  ([enc.decode([i])   for   i   in   ids])    # the sub-word chunks, not letters
```


Run it and you get a handful of integer IDs, each decoding to a multi-character chunk — not ten separate letters. The model operates on those IDs. Counting characters would require information the tokenization threw away, so the model approximates and frequently gets it wrong.


Each token ID is then mapped to an **embedding** : a long list of numbers (a vector) that positions the token in a high-dimensional “word space.” In that space, similar words cluster —` cat` sits near` dog` ,` kitten` , and` pet` — and the geometry encodes meaning. The classic demonstration comes from[word2vec](https://arxiv.org/abs/1301.3781) (Mikolov et al., 2013), which showed that vector arithmetic captures relationships: roughly,` biggest − big + small ≈ smallest` , and` Paris − France + Italy ≈ Rome` . Those same vectors inherit human biases present in the training text, which is why` doctor − man + woman` can drift toward` nurse` . Embeddings are learned during training, not hand-built.


## Inside the transformer: attention and feed-forward layers


A transformer is a stack of identical layers, each doing two jobs in sequence: an **attention** step, where tokens pull in context from other tokens, and a **feed-forward** step, where the model applies knowledge stored in its weights to each token individually. A frontier model stacks dozens of these layers, refining the representation of every token a little more at each one. Early layers handle local structure like grammar and ambiguity; later layers track higher-level meaning — who did what to whom, and what the passage is about.


**Attention works like a matchmaking service.** For each token, the model produces a query vector (“what am I looking for?”) and a key vector (“what do I offer?”). It compares every query against every key, and where they match, information flows between those tokens. In the sentence “When John gave the book to Mary, she thanked him,” attention is the mechanism that links “she” to “Mary” and “him” to “John” so later layers can use that. A single layer runs many such attention operations — called heads — in parallel, each learning to track a different kind of relationship.


The **feed-forward** step is where learned facts live. After attention has gathered the relevant context, the feed-forward network processes each token on its own, transforming it based on patterns absorbed during training. The clean division of labor: attention retrieves information from the prompt in front of it, while feed-forward layers supply information learned from the training corpus. When the model continues “The capital of Poland is” with “Warsaw,” that fact comes from the feed-forward weights, not from anything in your prompt.


For the linear algebra and the exact equations behind attention, the original transformer paper is the primary source. The mental model — match, gather context, then apply stored knowledge, repeated across many layers — is enough to reason about behavior.


## How LLMs are trained: pre-training, fine-tuning, and RLHF


Training happens in three stages: self-supervised pre-training, where the next word in ordinary text is its own label; instruction fine-tuning on human-written prompt–response pairs; and reinforcement learning from human feedback (RLHF), which aligns the model’s outputs with human preferences. Each stage produces a different kind of model.


Stage Input Where the “label” comes from What it produces


**Pre-training** Massive unlabeled text (web, books, code) The next word in the text itself — no human labeling needed A fluent *text completer* that continues any prompt but doesn’t follow instructions


**Instruction fine-tuning** Curated prompt–response pairs Humans write the desired responses A model that answers questions and follows instructions like an assistant


**RLHF** Model outputs ranked by humans Human preference judgments train a reward model Outputs aligned to be helpful, harmless, and on-task


Pre-training is **self-supervised** : because the correct next word is already sitting in the text, the training signal is free and effectively unlimited — every sentence on the internet is a labeled example. The model starts with random weights, predicts terribly, and improves over hundreds of billions of examples by adjusting its weights to reduce prediction error (the math of that adjustment is backpropagation, well documented elsewhere). The 2020[scaling-laws paper](https://arxiv.org/abs/2001.08361) (Kaplan et al.) found that model accuracy improves as a power law with model size, training data, and compute — the empirical result that justified building ever-larger models.


A purely pre-trained model is just a completer. Ask it “What is your first name?” and it might continue with “What is your last name?” because that’s a plausible continuation, not because it’s being unhelpful. **Instruction fine-tuning** and **RLHF** fix that. The pipeline for turning a raw model into an assistant was laid out in the[InstructGPT paper](https://arxiv.org/abs/2203.02155) (Ouyang et al., 2022), the work directly behind ChatGPT, which launched as a public research preview on November 30, 2022. Most current chat models are trained with some variant of this pipeline, though the exact recipes are proprietary.


Two notes on numbers. The frequently quoted GPT-3 figures — 175 billion parameters, 96 layers, 12,288-dimensional embeddings, a 50,257-token vocabulary — come from the[2020 GPT-3 paper](https://arxiv.org/abs/2005.14165) and are useful as a concrete illustration, not a current spec. Frontier labs no longer publish parameter counts for their latest models: there is no official parameter figure for the current GPT-5 series, Claude Opus/Sonnet/Haiku, or Gemini 3.x lines. Treat any “X trillion parameters” claim about a current model as rumor.


## How generation works: sampling, temperature, and why output is probabilistic


When an LLM generates text, it runs the prediction loop one token at a time and *samples* the next token from the probability distribution it computes — it does not simply emit “the answer.” Here is the loop in pseudocode:


```text
tokens   =   tokenize(prompt)
while   not   done:
logits   =   model.forward(tokens)          # one score per vocabulary token
probs    =   softmax(logits   /   temperature)   # convert scores to probabilities
next_token   =   sample(probs)               # pick one, weighted by probability
tokens.append(next_token)
if   next_token   ==   END_OF_TEXT:
break
output   =   detokenize(tokens)
```


The` temperature` parameter controls how the scores become probabilities. By default an LLM is probabilistic, not deterministic: a temperature above zero means the same prompt can yield different answers, because the model samples from a distribution rather than always taking the single most likely token. At temperature 0 the model becomes greedy — it always takes the top token — which is roughly deterministic and good for code or extraction. Higher temperatures flatten the distribution, making lower-probability tokens more likely and the output more varied. The` temperature` knob is documented in the[OpenAI API reference](https://platform.openai.com/docs/api-reference/chat/create) and the[Anthropic Messages API reference](https://docs.anthropic.com/en/api/messages) ; both expose it per request.


This is why re-running the same prompt in ChatGPT can give you a different answer each time, and why “set temperature to 0” is standard advice when you need reproducible output from an API.


## Common misconceptions about LLMs


Several intuitive beliefs about how LLMs work are wrong, and each wrong belief leads developers to misuse the tools or misread their failures. The word “actually” in this article’s title is here to correct them.


Misconception What’s actually true


”It looks up the answer in a database.” There is no stored table of facts to retrieve. The model computes a fresh probability distribution over its whole vocabulary at every step.


”It knows the answer before it writes it.” Each token is chosen from the tokens before it. The answer is built left to right, not retrieved whole.


”Parameters store facts as readable text.” Parameters are numeric weights. “Facts” are diffuse statistical patterns across billions of weights, not retrievable strings.


”It’s deterministic — same input, same output.” At temperature > 0 it samples, so identical prompts can produce different completions.


**It is not a database lookup.** An LLM does not look up answers in a database — it has no stored table of facts to retrieve; it computes a fresh probability distribution over its entire vocabulary at every single step, then picks one token and repeats. Framing parameters as “a knowledge base” is misleading: weights encode statistical regularities, not indexed records.


**It does not know its answer in advance.** The model does not know its answer before it writes it: each token is chosen based on every token that came before, so the “reasoning” you see is produced left to right, not retrieved whole. This is why a model can start a confident sentence and paint itself into a corner — there’s no plan being read out, just a continuation being computed.


## Why LLMs behave the way they do


Once you see the mechanism, the everyday behaviors of these tools stop being mysterious. Each one is a direct consequence of next-token prediction over learned probabilities.


**Hallucination.** Hallucination is not a bug bolted onto the system — it’s the default behavior of a model optimized to produce statistically plausible text, which is rewarded for sounding right rather than for being right. When the model has no strong signal for a fact, it still produces the most plausible-looking continuation, complete with a confident tone, because confident text dominates its training data. A fabricated function name or a non-existent citation is the model doing exactly what it was trained to do.


**Context windows.** A context window is the fixed number of tokens the model can attend to at once; anything pushed beyond that limit is simply not visible to the model, which is why a long chat eventually “forgets” how it began. As of June 2026, current frontier models converge around a roughly one-million-token window —[Gemini 3.5 Flash](https://ai.google.dev/gemini-api/docs/models/gemini-3.5-flash) handles 1,048,576 input tokens,[Claude Opus 4.8](https://www.anthropic.com/news/claude-opus-4-8) ships a 1M-token window by default, and the GPT-5 series offers a 1M-token window via the API. Large, but still fixed: once your conversation or codebase exceeds it, the oldest tokens fall outside what the model can see.


**Prompt sensitivity, chain-of-thought, and few-shot.** The exact wording of your prompt shifts the probability distribution, and therefore the answer, because output is computed token by token from the preceding tokens. That same property is why prompting tricks work. Few-shot prompting (showing worked examples) seeds the context with the pattern you want continued. Chain-of-thought prompting (“think step by step”) works because the model’s own generated text becomes part of the input it reads next — the intermediate steps act as working memory the model can build on, instead of forcing the final answer out in a single jump.


**Grounding and RAG.** Retrieval-augmented generation puts relevant documents directly into the prompt so the model can pull facts from attention (the prompt in front of it) instead of from its lossy feed-forward weights. This is why a model with web search or your docs in context is more accurate on specifics — you’ve moved the burden from “recall it from training” to “read it from the prompt,” which the architecture does far more reliably.


## The “stochastic parrots” question


Whether next-token prediction amounts to genuine understanding or sophisticated mimicry is an open and contested question. The skeptical view was crystallized in the 2021 paper[On the Dangers of Stochastic Parrots](https://dl.acm.org/doi/10.1145/3442188.3445922) (Bender, Gebru, et al.), which argued that a model stitching together training text by statistical likelihood, with no grounding in meaning, is more parrot than mind. The opposing view holds that predicting text well enough demands a compressed, usable model of the world it describes. The mechanics in this article are settled; this interpretation is not, and you don’t need to resolve it to use the tools well.


What you do need is the mechanical model: tokens in, attention and feed-forward layers refining them across many transformer layers, a probability distribution out, one token at a time. Hold that picture and the next time Copilot invents a method or ChatGPT contradicts itself three turns into a chat, you’ll know exactly which part of the machine produced it — and, just as usefully, which prompt change or retrieval step will fix it.


## FAQs


What is the difference between a token and a word in an LLM?


A token is a sub-word chunk, not a word. The tokenizer breaks text into pieces that can be a whole word, a word fragment like 'ing', a single character, or punctuation, then maps each to an integer ID. A common word may be one token while a rarer word splits into several. The model only ever operates on these token IDs, never on raw words or letters, which is why character-level tasks like counting letters work against how it represents text.


Does increasing the context window improve an LLM's accuracy?


Not directly. A larger context window only lets the model attend to more tokens at once; it does not make the model better at reasoning or recall. Quality can still degrade when relevant information sits far from where it is needed, and putting too much irrelevant text in the prompt can dilute the signal. The benefit is that fewer earlier tokens fall outside the window, so the model 'forgets' less of a long conversation or document.


If I set temperature to 0, will an LLM always give identical output?


Temperature 0 makes generation greedy, always selecting the highest-probability token, which is roughly deterministic for a given model and prompt. In practice, exact reproducibility is not always guaranteed across API calls because of factors like floating-point non-determinism on parallel hardware, model version changes, and provider-side routing. Temperature 0 is the right setting when you need consistent output for code or extraction, but treat it as near-deterministic rather than a hard guarantee.


Why does retrieval-augmented generation reduce hallucination?


RAG places relevant documents directly into the prompt, so the model can pull facts from attention over the text in front of it rather than from its lossy feed-forward weights. The architecture retrieves information from the prompt far more reliably than it recalls diffuse patterns learned during training. This shifts the task from 'remember this fact' to 'read this fact', which is why a model with your docs or web results in context answers specifics more accurately, though it can still misread or misuse what it is given.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
