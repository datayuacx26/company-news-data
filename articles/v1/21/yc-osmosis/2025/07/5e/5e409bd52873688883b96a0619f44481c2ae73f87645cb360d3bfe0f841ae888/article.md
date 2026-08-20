---
schema_version: "1.0.0"
document_id: "5e409bd52873688883b96a0619f44481c2ae73f87645cb360d3bfe0f841ae888"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/applying-rl-improving-code-merging"
published_at: "2025-07-03T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:ab33fbc2d6280a2efe057703235e1330ffe763023490084c53d02022dd43977c"
---

# Applying RL: Improving Code Merging

While foundation models have continued to improve at coding capabilities, using foundation models for high specificity, low complexity tasks like code merging can be overkill.


With that in mind, we saw an opportunity to use reinforcement learning to fine-tune a model (Qwen3-1.7B) for code merge - the result is a small model that’s better and faster than foundation models, while also able to run locally. **You can use the model via MCP server[here](https://github.com/Osmosis-AI/Osmosis-Apply-1.7B-MCP) !**


**Download Osmosis-Apply-1.7B here:[Ollama](https://ollama.com/Osmosis/Osmosis-Apply-1.7B) |[Hugging Face](https://huggingface.co/osmosis-ai/Osmosis-Apply-1.7B)**


After training, we tested the model (1xH100 running on SGLang) against OpenAI o3, Claude 4 Sonnet, and Gemini 2.5 Flash on 10,000 validation examples to measure performance. We also defined a reward criteria for these models, it breaks down into 3 types. If the model merges the code perfectly, we reward with a score of 1. If the model merges the code correctly but has extra new lines, we reward with a score of 0.2. All other cases receive a score of 0. **Osmosis-Merge-1.7B outperformed all three with a 0.98 reward score (1.00 being perfect):**


Osmosis-Apply-1.7B is also significantly[cheaper](https://artificialanalysis.ai/models/qwen3-1.7b-instruct) than the foundation model options, being 3X-5X (input-output token cost) cheaper than the next cheapest model, Gemini 2.5 Flash.


Model Latency (ms) Reward Score Cost ($/M tokens in) Cost ($/M tokens out)


Osmosis-Apply-1.7B 151 0.9893 $0.11 $0.42


Claude Sonnet 4 1,180 0.9328 $3.00 $15.00


OpenAI o3 1,230 0.8639 $2.00 $8.00


Gemini 2.5 Flash 1,050 0.7745 $0.30 $2.50


We trained Osmosis-Apply-1.7B on[CommitPackFT](https://huggingface.co/datasets/bigcode/commitpackft) , a 2GB dataset of code commits, using GRPO. Given the size of the dataset, we only used a portion for training (100K examples, or roughly 1/7 of the dataset, uniformly sampled). The reward function was really, really simple - we rewarded the model when it merged code successfully, gave a minor reward when formatting was slightly off, and didn’t reward it when it failed. Like so:


```text
def    extract_solution  (solution_str)  :
matches = list(re.finditer( r'<code>(.*?)</code>'  , solution_str, re.DOTALL))


# If nonempty matches and exactly one <code> block exists
if  (matches  and   len(matches) ==  1  ):
return   matches[ 0  ].group( 1  ).strip()
return    None


def    filter_empty_lines  (lines)  :
return   list(filter( lambda   line : line.strip() !=  ""  , lines))


def    calc_score  (answer, ground_truth)  :
answer = answer.strip()
ground_truth = ground_truth.strip()


if  (answer == ground_truth):
return    1.0
else  :
answer_lines = filter_empty_lines(answer.splitlines( True  ))
ground_truth_lines = filter_empty_lines(ground_truth.splitlines( True  ))
# Give small positive reward if lines are almost correct
if  (answer_lines == ground_truth_lines):
return    0.2
return    0


def    compute_score  (data_source, solution_str, ground_truth, extra_info=None, format_score= 0.0  , score= 1.0  )  :
answer = extract_solution(solution_str=solution_str)
if   answer  is    None  :
return    0
else  :
return   calc_score(answer, ground_truth)


```


We trained the model using GRPO with a learning rate of 1e-5 and batch size of 64. The training was optimized for efficiency with FSDP (Fully Sharded Data Parallel) strategy across 8 GPUs, using parameter offloading to manage memory. We set maximum prompt length to 3,072 tokens and maximum response length to 6,144 tokens to handle the typical size of code merge scenarios.


Notably, we disabled KL divergence regularization and entropy bonuses, allowing the model to focus purely on the reward signal from successful merges. The model was trained for just one epoch with 16 rollout samples per iteration, demonstrating that even minimal training can achieve strong performance when the reward function is well-designed.


Let us know what you think - and[reach out](https://osmosis.ai/cdn-cgi/l/email-protection#b4dfd5c7d1cdf4dbc7d9dbc7ddc79ad5dd) if you’re interested in learning more about reinforcement learning!
