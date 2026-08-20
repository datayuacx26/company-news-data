---
schema_version: "1.0.0"
document_id: "51382e58ef7236dc4a569cb58d000ea0b5d3edf3e1e132f3d5dfb67d419f801c"
company_key: "yc-theorem-2"
company: "Theorem"
source_id: "yc-theorem-2-rss-ef0b9cf422a4"
canonical_url: "https://theorem.dev/blog/catching-bugs-with-fractional-proofs/"
published_at: "2025-10-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:37.219761+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:331b9c157fd37f4efe4da37b06773fb70f840462f45474c645464ca6c77cf087"
---

# Catching bugs with fractional proofs

Fractional proof decomposition fuses partial evaluation and property-based testing to scale testing compute logarithmically with bug rarity, instead of linearly.


## Introduction


Most large projects have a limited compute budget for testing, lest it slows down the CI pipeline. This means missing rare edge cases until customers find them in production. I love formal verification! And I want to bring the power of reasoning about program structure to testing. In this blog post, I will briefly introduce fractional proof decomposition, our new technique that makes testing compute scale logarithmically with bug rarity instead of linearly.


As a demo, I’ll walk through how this approach would have caught Anthropic’s recent[approximate top-K bug](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) . Note that we generate these unit tests without relying on the bug reproducer code, using just the high-level specification of top-K sampling.


Figure 1:


Unit-level PBTs are fast but miss edge cases. Proofs offer exhaustive coverage but require extensive reasoning and code refactoring. End-to-end PBTs have coverage but are not compute efficient. Fractional proofs sit at the intersection, using proof decomposition to generate targeted unit tests that balance compute efficiency, developer accuracy, and speed.


## Top-K sampling


For background, Anthropic shared that a bug in the TPU implementation of approximate top-K was causing the most likely token to *sometimes* be excluded. This kind of bug usually slips through to production because exhaustively testing every behavior is just not feasible. After discovering the bug, Anthropic provided a simple reproducer, but it is the sort of test you only manage to write after a laborious bug minimization process.


We used fractional proof decomposition to automatically generate the unit test without the benefit of hindsight. You can run the unit test on[colab](https://colab.research.google.com/drive/1u3gUEb-_POObIU5wtR-UX7OG8hlOkoRu?usp=sharing) . This technique is generally applicable to any codebase: rare bugs can be systematically found if testing pipelines use fractional proof decomposition.


```text
@given  (k =  st .  integers(min_value =  0  , max_value =  TOP_K_RANGE), arr =  arr_strategy)
def    test_approx_max_k  (k, arr):
N  =   len(arr)
k  =   int(k  %   min(N  -   MIN_TOP_K, TOP_K_RANGE))  +   MIN_TOP_K


approx_values, _  =   lax .  approx_max_k(arr, k =  k)
assert   jnp .  max(approx_values)  ==   jnp .  max(arr), \


```


Figure 2:


Top-K sampling should always have some chance of picking the most likely token. We encode this property with a PBT (property-based test) for` max(approximate_top_k(arr, k=k)) == max(arr)` . If the implementation of lax.approx_max_k is correct, we should expect the test to pass because the approximate top-K algorithm is[implemented](https://arxiv.org/abs/2206.14286) by dividing data points into L bins and computing the true max in each bin. L is chosen based on the desired average recall r as $L \\approx \\frac{k-1}{1-r}$.


## Fractional proof decomposition


There are three steps to fractional proof decomposition.


Figure 3:


We encode theorems as property-based tests (PBTs), then recursively decompose them into smaller sub-theorems using reasoning, and fuzz the theorems, aka run PBTs, once the decomposition creates compute efficient unit-level PBTs.


### Step 1: Identify the theorem and encode it as a PBT


A theorem is just the property that your implementation must satisfy, and can be encoded as a PBT. If you code in Python, I highly recommend the[Hypothesis](https://hypothesis.readthedocs.io/en/latest/) framework. We call the top-level theorem an end-to-end PBT because it corresponds to the end-to-end behavior of the function.


For the top-K bug, the theorem is:


$$\\forall\\ \\text{prompt}, k,\\ LLM_{\\text{top-1}}(\\text{prompt}) \\in LLM_{\\text{top-}k}(\\text{prompt})$$


In words: for any prompt, the most likely next token should always be *somewhere* in the top-K candidates. Now, we encode this as an end-to-end PBT. Since the end-to-end PBT does not need to run on TPU, we set up a different[colab](https://colab.research.google.com/drive/1EHTacAGfL-od6j8gy2AUdoIX4ivu1g0J?usp=sharing) .


```text
@given  (prompt =  st .  text(min_size =  1  , max_size =  MAX_SIZE), k =  st .  integers( 1  , VOCAB_SIZE))
@settings  (max_examples =  50  , deadline =  None  )
def    test_top_token_present  (prompt, k):
greedy_params  =   SamplingParams(temperature =  0.0  , max_tokens =  1  , logprobs =  1  )
topk_params    =   SamplingParams(temperature =  1.0  , max_tokens =  1  , logprobs =  MAX_LOGPROBS, top_k =  k)
most_likely_token  =   llm .  generate([prompt], greedy_params, use_tqdm =  False  )[ 0  ] .  outputs[ 0  ] .  token_ids[ 0  ]
logprobs           =   llm .  generate([prompt],   topk_params, use_tqdm =  False  )[ 0  ] .  outputs[ 0  ] .  logprobs[ 0  ]


assert   most_likely_token  in   logprobs


```


Figure 4:


The theorem stating that the most likely token should always be included in the top-K tokens, encoded as a PBT.


Although the end-to-end PBT has comprehensive coverage, catching rare bugs requires generating a massive number of test cases. The rarer the bug, the more compute you need. This scales linearly with bug rarity.


$$\\text{pbt compute} \\;\\sim\\; \\frac{\\text{total cases}}{\\text{failing cases}}$$


### Step 2: Recursively decompose into smaller theorems


This is where we break the linearly scaling test compute requirement. We decompose the end-to-end property into a collection of smaller sub-properties, each also encoded as a PBT. These sub-properties are intermediate results that compose to establish the original end-to-end behavior.


For the top-K bug, decomposition gives us three theorems:


1. ` max(approximate_top_k(arr, k=k)) == max(arr)` (true max always included)1
2. On any input tokens, the logits are finite (not ∞ and not NaN)
3. In vLLM, the token ids are the same as the logprobs dict keys


You can think of PBTs as fractional components of the brute-force proof. Just as you would optimize the brute-force proof by decomposing properties into logical sub-properties via reasoning (the technique better known as[partial evaluation](https://en.wikipedia.org/wiki/Partial_evaluation) ), we’re applying reasoning to decompose the fractional brute-force proofs.


The reasoning bootstraps trust in PBT coverage—so even though you’re not exhaustively checking every single input like a formal proof, you get systematic understanding of your programs, and control over how you spend your testing compute. We’re calling this sampling technique fractional proofs.


### Step 3: Keep decomposing until the input space is small enough to be compute efficient


The criteria for stopping decomposition are:


1. Each sub-test runs sufficiently quickly
2. Each sub-test covers enough of its input distribution to catch bugs at the complexity level of the code being tested
3. The sub-properties provably compose to cover the full end-to-end property


We found the top-K bug in about 10 minutes of sampling. But we found the XLA:TPU bug (also discussed in Anthropic’s post), involving an issue with excess-precision, in just a few seconds.2


## Efficiency in testing compute


Systematic decomposition catches rare bugs without sacrificing developer speed or compute efficiency. Instead of scaling compute in proportion to the rarity of the bug, fractional proofs scale compute as the logarithm of rarity.


$$\\text{pbt compute} \\;\\sim\\; \\log\\!\\left(\\frac{\\text{total cases}}{\\text{failing cases}}\\right)$$


This isn’t a toy example, and we can straightforwardly extend the approximate top-K example in this post to real-world codebases. For example, top-K can be decomposed into a sequence of PBTs testing how libtpu implements the algorithm described in its reference[paper](https://arxiv.org/abs/2206.14286) . Or, we can use this reasoning to establish how single-TPU behavior composes into cluster behavior.


At Theorem, we’re training models that can automatically reason about program correctness. If you want to catch bugs earlier and make your devs happy,[send me an email](https://theorem.dev/cdn-cgi/l/email-protection#670d0614080927130f020815020a49030211) .


---


1.


More generally, for any` k' <= k` , at most` floor(k'(L-1)/L)` of the true top` k'` values are excluded.↩︎


2.


Because the older version of Anthropic’s code includes more computation around approximate top-K, we decompose the theorem` max(top_k_computation(arr, k=k)) == max(arr)` into:` max(arr) >= min(arr) and max(softmax(arr)) >= min(top_k(softmax(arr), k=k))` . You can find the work at the bottom of the same[colab notebook](https://colab.research.google.com/drive/1u3gUEb-_POObIU5wtR-UX7OG8hlOkoRu?usp=sharing) .↩︎
