---
schema_version: "1.0.0"
document_id: "9f0e130fd78cd41d02b92b6ccc5628e7d1d70fab4562e984df549b4cc170e565"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-26"
published_at: "2024-05-22T19:45:13+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:bd051a06bc0e9325efb6c95584932322652166462971044d514c7078160d3e1c"
---

# The Deep Dive #26

# The Deep Dive #26


### Pipecat, steering LLMs, and copilots


[Nassim](https://substack.com/@nas07)


May 22, 2024


In a recent


[piece](https://www.anthropic.com/research/mapping-mind-language-model) , folks at Anthropic discussed how LLMs learn to develop an understanding of broad concepts by firing up certain groups of neurons together. Interestingly, an LLM can be pushed to behave in certain ways by tweaking the importance of the node group for a given concept, without spending the evening trying to find the best prompt to make it write legit comedy for e.g. Although, it remains to be seen if having a model think only about jokes is enough to make it fun at parties (If life experience is any indication of how that would work, then probably not).


Speaking of understanding how LLMs behave, we just released our


**new interface for comparing models on custom datasets** ! If you've always wondered which LLM would do better at writing dad jokes that sound exactly like yours, we got you covered. You can give it a try through the link below. (Oh, and we're also live on


[Product Hunt](https://www.producthunt.com/posts/unify-6) . Any feedback or upvotes there would be much appreciated!)


[Benchmark your datasets](https://unify.ai/chat)


## The missing link


Multimodal models are starting to emerge as the next frontier of AI models, but modality-specific models still handle their respective tasks better. Unfortunately, these models are often available through separate ecosystems, making it difficult to build comprehensive AI apps without juggling around services. Pipecat is a framework for building interactive, real-time AI applications with a simple API.


**Why would you care? -** If you need to quickly build real-time, multi-modal AI apps, this one's for you!


*High level representation of the Pipecat workflow. Source: [Pipecat Documentation](https://pipecat.ai/docs/category/understanding-bots)*


**How does it work? -** The core of Pipecat lies in its data processing pipeline, which facilitates the seamless flow of information between different AI components.


Pipecat uses


*frames* as fundamental data units, carrying various information like text, images, and audio. These frames are processed by


*services* - specialized AI modules that handle specific tasks like text-to-speech conversion, interaction, or logging, using. The


*pipeline* connects these services, enabling complex workflows for generating and processing information in real-time. Finally, the entire system is connected to the real world through a


*transport* , which handles the input and output of data, ensuring the bot can effectively interact with the user.


For e.g, a simple bot can be constructed as follows


```text
async def main(room_url: str, token):
async with aiohttp.ClientSession() as session:
messages = [
{
"role": "system",
"content": "You are a helpful LLM in a WebRTC call. Your goal is to demonstrate your capabilities in a succinct way. Your output will be converted to audio. Respond to what the user said in a creative and helpful way.",
},
]


transport = DailyTransport(...)


tts = ElevenLabsTTSService(...)
llm = OpenAILLMService(...)


tma_in = LLMUserContextAggregator(messages, ...)
tma_out = LLMAssistantContextAggregator(messages, ...)


pipeline = Pipeline(
processors=[tma_in, llm, tts, tma_out],
)


@transport.event_handler("on_first_other_participant_joined")
async def on_first_other_participant_joined(transport):
# Kick off the conversation.
messages.append(
{"role": "system", "content": "Please introduce yourself to the user."})
await pipeline.queue_frames([LLMMessagesFrame(messages)])
(...)
await transport.run(pipeline)
```


In the code above, the pipeline module aggregates the different services (OpenAI and ElevenLabs) with input and output parsers. The transport then handles the input/output flow between the user and the pipeline components asynchronously.


Check out the


[repository](https://github.com/pipecat-ai/pipecat) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-26?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### Plan Bs


LLMs can get stuck in dead ends when reasoning over multiple steps. To overcome this limitation,


[Fleet of Agents (FoA)](https://arxiv.org/pdf/2405.06691) spawns a fleet of N agents, each exploring the problem space independently for k steps.


After these steps, a heuristic value function evaluates each agent’s progress, and a resampling mechanism selects the most promising agents, keeping a balance between exploring new paths and exploiting promising ones


This approach leverages the strengths of individual agents by creating a collaborative search process that can navigate complex, dynamic search spaces more effectively than traditional tree search algorithms.


FoA outperforms Tree-of-Thoughts methods, significantly decreasing computational costs while preserving comparable or superior accuracy.


#### A game of statistics


Randomness aside, bad LLM output quality can sometimes stem from incorrect interpretation of instructions.


To address this issue, the


[Uncertainty-aware Reward Model (URM)](https://arxiv.org/pdf/2405.06424) integrates output preference uncertainty into the reward function when training models. URM is trained on datasets of human preferences for paired question responses and uses Bayesian approximation to estimate uncertainty in its reward scores.


This uncertainty factor can be applied to optimize model training by (a) curating mixed-quality datasets that favor more robust training, and (b) integrating uncertainty into existing training objectives for direct preference optimization (DPO) and reinforcement learning from human feedback. These modifications allow the model to focus more on data with high confidence, minimizing the impact of ambiguous or uncertain data during training.


By incorporating uncertainty into both data curation and training objectives, the instruction-following capabilities of language models can be significantly improved.


#### Branching pathways


Traditional LLM alignment methods focus on tweaking the weights of a model in certain ways to achieve desired patterns. Conversely,


[Spectral Editing of Activations (SEA)](https://arxiv.org/pdf/2405.09719) edits the internal representations of an LLM during inference.


SEA first uses spectral decomposition on the covariance matrices derived from positive (desired behavior), negative (undesired behavior), and neutral (typical behavior) LLM outputs, to calculate "editing projections". Editing projections are essentially representations of the most important directions in an LLM's internal representation, that will mostly determine the behavior of that model.


SEA then incorporates these calculated projections during inference to steer the model output. For each new prompt, the model's activations are projected into directions with maximal covariance with positive outputs, and minimizing covariance with negative outputs.


SEA demonstrates improvements in truthfulness and bias across several open-source LLMs, including LLaMA-2, Gemma, and Mistral.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**Just listening in -** Microsoft announced significant


[updates](https://www.microsoft.com/en-us/microsoft-365/blog/2024/05/21/new-agent-capabilities-in-microsoft-copilot-unlock-business-value/) to its Copilot AI assistant, focusing on expanding capabilities for businesses. The new features include Team Copilot, which transforms Copilot into a collaborative team member, and Agents, which are custom copilots designed to automate business pipelines.


**The sky's the limit -** GitHub introduced


[Copilot Extensions](https://github.blog/2024-05-21-introducing-github-copilot-extensions/) , a new feature that integrates popular dev tools and services into the Copilot platform. With Copilot Extensions, devs can interact with tools like Sentry, Octopus Deploy, and Azure directly in natural language, streamlining development processes.


**Another day another data partnership -** OpenAI is


[partnering with Reddit](https://openai.com/index/openai-and-reddit-partnership/) , bringing AI-powered tools for content summarization and generation into the platform. Reddit data will also be incorporated into GPT models to expand its context base.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
