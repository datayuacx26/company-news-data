---
schema_version: "1.0.0"
document_id: "24dfa9f98e93bdb313321eaad1bd0114243a0af0632c94db75a5e6ec2a49f79e"
company_key: "yc-giga"
company: "Giga"
source_id: "yc-giga-news-import-92619addbaaf"
canonical_url: "https://giga.ai/news/building-a-prefill-first-classifier-llm"
published_at: "2025-12-04T00:00:00+00:00"
first_seen_at: "2026-07-29T04:46:42.853576+00:00"
fetched_at: "2026-07-29T04:46:44.288934+00:00"
content_hash: "sha256:ca2f2b161f7bc906d10e9fc226d855a94b23e931b8e23ae601984cee1d232352"
---

# Building a Prefill-First Classifier LLM

## TL;DR


Most classification problems do not require an LLM. However, for problems with long, messy, or conversational inputs, a classifier LLM is the best approach for capturing semantic meaning, ensuring accurate decision-making. Because we are building a voice AI for customer support, we needed a classifier LLM for multi-turn interaction logs, system messages, and policy texts.


Classifier LLMs have a unique topography. Specifically, they have a very asymmetrical input-to-output token ratio. Inputs often extend up to 10,000 tokens (sometimes, even 60,000+ tokens); conversely, outputs rarely cross a dozen tokens. A fair share of LLM research is focused on the decode stage, when the LLM generates output tokens. However, for classifiers, the focus is instead on prefill, when the LLM reads tokens.


Last quarter, we decided to build a classifier LLM atop an open-weight instruction model that satisfied a few internal design requirements. We improved our system’s latency, precision, and recall compared to our build that used an off-the-shelf model. This blog discusses how we accomplished all of this with a sub-12 hour fine-tuning time.


## Design Goals


At[Giga](https://giga.ai/) , we build[voice AI agent](https://giga.ai/voice-experience) s for customer support. Because our agent directly interacts with our customers’ customers, we needed a product that could work with diverse clusters of end users. This translates to a few non-negotiable technical constraints:


-


**Low latency:** Poor latency can make verbal conversations feel unnatural; humans are used to quick responses and get frustrated if they perceive that they’re talking to a robot. Our classifier needed to accommodate tight time budgets. Additionally, because inbound customer support lines surge during an outage, latency cannot collapse during bursty loads.


-


**Modularity:** GigaML’s customers have different customer service needs, so we needed to fine-tune behavior without retraining the base model for each customer. Otherwise, we’d be faced with a computationally expensive runtime for every customer and whenever a customer’s needs evolve. Instead, we required a tenant-host system that would allow for hot-swapping different trained variants.


-


**Cost control:** We valued having a predictable per-request cost. Because classification is a prefill-heavy problem, our backend had to be optimized for prefill efficiency, not decode efficiency.


## Our Approach


For our base model, we chose[Qwen3-8B](https://huggingface.co/Qwen/Qwen3-8B) , a modern ~8B-parameter open-weight instruction model. There were two primary reasons why Qwen3-8B was the ideal choice:


-


**Capacity and latency balance:** 8B parameters are enough to handle long contexts and subtle label boundaries. It’s also small enough to hit aggressive real-time benchmarks by optimizing prefill.


-


**Ecosystem:** Qwen3-8B has a great tokenizer and instruction obedience out of the box. It also supports fine-tuning, quantization, and modern inference stacks.


We chose to keep Qwen3-8B as a **frozen base** ; we do *not* fine-tune the model directly, keeping all of the original weights. Instead, we use lightweight LoRA (low-rank adaptation) adapters to create an efficient low-dimensional representation of fine-tuning updates. If you are interested in the details surrounding this approach, we highly recommend this[Thinking Machines post](https://thinkingmachines.ai/blog/lora) .


There are many benefits of using LoRA adapters that closely align with our design criteria:


-


**Isolated by design:** Each adapter has its own set of weights, each tuned to a single customer success policy. By changing (or breaking) one adapter, we don’t affect the behavior of another. We could have hundreds of available adapters at scale.


-


**Faster:** Compared to full fine-tuning, LoRA tuning takes hours rather than days due to the significantly smaller vector space. LoRA is similar to tailoring a standard suit with a few precise alterations instead of sewing an entirely new one.


-


**Footprint:** Adapters are very small. The significantly smaller memory footprint, often dipping under 175MB per adapter, allows us to load multiple in parallel.


-


**Safer practices:** Adapters can easily be rolled back if metrics are moving in the wrong direction. Additionally, they could be versioned, cached, and hot-swapped. This allows us to easily A/B test, loading both in parallel.


### Our Specific Classification Problem


We trained our classifier LLM on a few anonymized data sources pulled from Giga:


-


**Long-form interaction logs:** These are logs detailing conversations between the AI voice agent and our customers’ customers. Our classifier LLM served as context management, ensuring agents had the right guidelines to answer questions without hallucinating.


-


**System messages:** System messages are the prompts for each LLM request. This is a constant prefix for *every* prompt.


-


**Policy/configuration snippets:** Policies are instructions specific to a customer or use case that the voice agent needs to follow. For example, an airliner might have a policy for issuing ticket changes.


-


**Templated edge cases for tricky boundaries:** For most customer service operations, there are repeatable edge cases that could be over-sampled during training to teach the adapter the fine-grained distinctions.


For the actual ingestion, we slotted our data into a fairly simple schema:


```text
{
context: string, //long-horizon background, including all metadata + configs
input_window: string, //a short snippet of text we're classifying
labels: [string] //an allowed subset of labels from a fixed catalog
}
```


Additionally, to protect the quality of our data, we:


-


**Performed inter-rater agreement checks** , where we ensured multiple human raters would assign the same labels


-


**Created label-consistency tests for near-duplicate contexts** , where we ensured that near-duplicate or duplicate data was marked with the same labels


-


**Scanned for leakage between train and validation data** , where we ensured that training and validation data were discrete sets to avoid overfitting.


### Training Strategy


Our training strategy was to use a teacher model, where a larger LLM with high-precision prompts generates labels in a structured JSON. Then, humans reviewed certain things, such as label frequency, often confused label pairs, and other edge cases. We performed adversarial training to target near-miss phrasing, code-switching, input channel quirks, and formatting noise. All fine-tuning ran on a single H200 GPU hosted on[Runpod](https://www.runpod.io/) .


There were a few other unsuccessful trials. We originally tried[Together AI](https://www.together.ai/) , but they lacked the flexibility to modify chat templates (thereby making it impossible to disable thinking). We also tried directly training FP8 weights on a Qwen3-8B model, but this proved to be significantly inefficient and just comparable on accuracy to LoRA adapters. Additionally, we tried building on a[Qwen3 30B A3B](https://huggingface.co/Qwen/Qwen3-30B-A3B) model, but we also achieved only comparable accuracy but with worse latency due to its Mixture of Experts (MoE) architecture.


## Results


When we compare our fine-tuned Qwen to our previous model, GPT-4.1 mini, we see a significant difference:


### Latency


Our fine-tuned Qwen had a P50 latency of 150ms, over 3x faster than GPT-4.1 mini’s latency of 450ms. At P90 and P99, the discrepancy continues, with fine-tuned Qwen’s latency at 220ms and 380ms, respectively, and GPT-4.1 mini’s latency at 580ms and 870ms, respectively.


### Accuracy


We measured the **precision** (measurement of true versus false positives), **recall** (percentage of true positives identified), and **F1** (a derivative metric of precision and recall). For all three metrics, our fine-tuned Qwen beat GPT-4.1 mini. Due to our LoRA adapter architecture, we were able to incrementally improve accuracy metrics without heavy overhead.


### Training Time


By choosing LoRA adapters, we trimmed our training time to just 5-10 hours per fine-tuning. Had we chosen to do a full fine-tuning, training runs would’ve taken over 3 days. This is a significant improvement in the context of our business; with just 24 hours’ notice, we’ll be able to produce LoRA adapters tailored to customer needs.


## Lessons learned


There were four specific learnings:


-


If your outputs are tiny, optimize prefill. Nothing else matters.


-


For diverging label spaces and policies, LoRA adapters on a solid base (like Qwen3-8B) beat one giant monolith—you get isolation, faster iteration, and safer rollbacks.


-


For multi-label problems with a small catalog, confusion-focused data curation (hard negatives and label pairs the model mixes up) is more valuable than just piling on raw volume.


-


You’ll move faster if metrics are per-adapter and per-label (micro-F1, macro-F1, and “no-label-active” quality) rather than one global number.
