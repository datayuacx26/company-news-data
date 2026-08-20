---
schema_version: "1.0.0"
document_id: "e98bcd77e1ad55e25c2e3a71d1fd30fd4f581fb3617dcd42fbb7c3667e2b358f"
company_key: "yc-osmosis"
company: "Osmosis"
source_id: "yc-osmosis-news-import-de01a8838bba"
canonical_url: "https://osmosis.ai/blogs/applying-rl-fixing-structured-outputs"
published_at: "2025-05-29T00:00:00+00:00"
first_seen_at: "2026-07-25T18:22:44.798253+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:724fe1b5e5a2f32695abdb4d7011f2b97b0570f177df8b2f2ef52c5cb91c6c58"
---

# Applying RL: Fixing Structured Outputs

A significant portion of AI use cases revolve around structured outputs - i.e. using the model to ingest unstructured textual data to generate a structured output, typically in JSON format. However, this leads to a performance decrease in tasks that are not strictly just formatting changes since structured output mode enforces a schema and stops the model from thinking ‘freely’.


So instead of structured output mode, we used reinforcement learning to train an ultra small model (Qwen3-0.6B) to do it instead! All you have to do is feed in the unstructured data with the desired output schema.


**Download Osmosis-Structure-0.6B here:[Ollama](https://ollama.com/Osmosis/Osmosis-Structure-0.6B) |[Hugging Face](https://huggingface.co/osmosis-ai/Osmosis-Structure-0.6B)**


We tested the most recent Anthropic and OpenAI models on math questions ([1983-2024 AIME](https://huggingface.co/datasets/di-zhang-fdu/AIME_1983_2024) and[DAPO-Math-17k-Processed](https://huggingface.co/datasets/open-r1/DAPO-Math-17k-Processed) ), comparing accuracy between structured output mode and unstructured responses (with Osmosis to output the same structure after):


(For the Anthropic models, we used Assistant prefill as a proxy for structured output mode)


Using Osmosis-Structure-0.6B to structure outputs *significantly* improved performance from Sonnet 4, Opus 4, and GPT-4.1. Interestingly, o3 performed well even with structured output mode. We speculate that this may be due to a double pass - i.e. o3 generates an output, and then 4o-mini (or another small model) is used to validate / structure the output, similar to Osmosis-JSON-0.6B. We came to this hypothesis since GPT-4.1's structured output time to completion is significantly faster compared to its unstructured completions (>5% time required). In comparison, o3's time to completion for structured and unstructured calls was similar - sometimes even longer for structured outputs.


In production environments, we've also observed users opting to feed unstructured outputs from more expensive models into cheaper models (e.g. GPT-4o-mini) to structure the response. Osmosis-Structure-0.6B acts as an open source, smaller replacement of the second model.


We trained Osmosis-Structure-0.6B using GRPO on a synthetic dataset of 500K examples of inputs/outputs where the prompt relies on a structured output (e.g. reasoning traces of math solutions with the response as structured outputs, data extraction & multi-nested JSON formatting from complex unstructured output, etc.) - the model was rewarded based on the amount of correct value fields that was recalled from the input text.


If you're interested in learning more about reinforcement learning,[reach out](https://osmosis.ai/cdn-cgi/l/email-protection#a3c8c2d0c6dae3ccd0ceccd0cad08dc2ca) !
