---
schema_version: "1.0.0"
document_id: "67196c202f1ba4977c310affbb169eac3c0d2bade78906b657babab7cf3535eb"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-36"
published_at: "2024-08-01T16:49:14+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:0372b9b82e849a8572c632639b8e0f2689e02f57b2083e7f3f064de4f9efd85e"
---

# The Deep Dive Issue #36

# The Deep Dive Issue #36


### Burr, deploying LLMs, and AI for all


[Nassim](https://substack.com/@nas07)


Aug 01, 2024


Slow weeks do happen in LLM world once in a bluemoon. After all, rising temperatures increase a model’s creativity but decrease our own productivity. Not much on the plate aside from this melted pun unfortunately, so let’s jump right in.


## State matters


LLM infrastructure tools allow you to quickly build and deploy robust LLM-based applications quickly and efficiently. However, these tools typically don’t provide simple solutions to deal with the unpredictability of LLM behavior, which then needs to be steered with experimental prompting. Burr provides a state machine framework that simplifies building consistent LLM workflows.


**Why would you care?**


**-** Spotting where an LLM pipeline fails can be tricky. With Burr, you can easily track and manage evolving states to improve predictability in your applications.


```text
from burr.core import action, State, ApplicationBuilder


@action(reads=[], writes=["prompt", "chat_history"])
def human_input(state: State, prompt: str) -> State:
# your code -- write what you want here!
return state.update(prompt=prompt).append(chat_history=chat_item)


@action(reads=["chat_history"], writes=["response", "chat_history"])
def ai_response(state: State) -> State:
response = _query_llm(state["chat_history"]) # Burr doesn't care how you use LLMs!
return state.update(response=content).append(chat_history=chat_item)


app = (
ApplicationBuilder()
.with_actions(human_input, ai_response)
.with_transitions(
("human_input", "ai_response"),
("ai_response", "human_input")
).with_state(chat_history=[])
.with_entrypoint("human_input")
.build()
)
*_, state = app.run(halt_after=["ai_response"], inputs={"prompt": "Who was Aaron Burr, sir?"})
print("answer:", app.state["response"])
```


*Simple example of using Burr to manage query states. Source: Repository [README](https://github.com/DAGWorks-Inc/burr) .*


**How does it work? -** Burr is a Python framework designed for creating state-driven applications. It leverages a state machine approach, providing a structured way to manage workflows, data persistence, and integration with various tools.


Burr is made of the following building-blocks:


-


**Applications** manage control flow, persistence to DBs, etc. and delegates tasks to external integrations. Applications are built using an Application Builder that specifies functions that modify state (Actions), the first action to execute (Entry Point), and connections between actions, triggered by state conditions (Transitions)


-


Immutagble


**States** that represent the application’s data. The State API provides methods for modifying state by adding or changing key-value pairs (Update), adding an element to a list in state (Append


**),** increase the value of a key in state (Increment


**),** and remove keys from state (Wipe)


-


**Hooks** allow you to customize and integrate various tools and features into Burr's lifecycle. Examples include logging, time delays, state synchronization with external databases, and result queuing.


Check out the


[repository](https://github.com/DAGWorks-Inc/burr) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-33?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxNTY2ODI5ODUsInBvc3RfaWQiOjE0NjUwMjc4MSwiaWF0IjoxNzIxMjkzNTI5LCJleHAiOjE3MjM4ODU1MjksImlzcyI6InB1Yi0xOTk5MjYzIiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.ttt1H-j4VUY_QCi79BIcdr9eM4n8RiJPaKkVmP9aOy4)


## The Lab


#### Path of least effort


Generating the first token during the inference of transformer-based large language models can be slow for long prompts, as it requires computing the full attention cache for every token in the input.


[LazyLLM](https://arxiv.org/pdf/2407.14057) addresses the issue of low Time-To-First-Token (TTFT) issue. Instead of computing the full attention cache for all tokens at once, LazyLLM selectively computes the cache only for the tokens that are important for predicting the next token. This selection happens progressively through the layers of the transformer. LazyLLM uses the attention scores from the previous transformer layer to determine which tokens are important for predicting the next token. Tokens with low attention scores are pruned and excluded from further computations in the later transformer layers. To avoid recomputing the attention cache for pruned tokens when they become relevant in subsequent decoding steps, LazyLLM employs an auxiliary cache to store the intermediate representations of the pruned tokens.


LazyLLM significantly accelerates the TTFT of large language models on various language tasks with negligible performance loss, achieving up to 2.34x speedup on the Llama 2 7B model.


#### Let’s grow step by step


A significant restriction with LLMs is that they struggle to improve their responses on complex tasks, even with access to necessary context. This inability to self-correct limits their use in scenarios requiring sequential reasoning and problem-solving.


[Recursive IntroSpEction (RISE)](https://arxiv.org/pdf/2407.18219) is a training method that teaches LLMs how to analyze and correct their mistakes over multiple attempts. It works by first converting a single-turn problem, into a multi-turn process. The LLM is then guided to produce multiple responses, attempting to improve its answer on each turn.


To facilitate learning, RISE uses two strategies. The first, called "distillation," involves using a more powerful LLM to provide the correct answers at each turn. The second, called "self-distillation," involves having the LLM generate multiple possible responses and selecting the best one as the "correct" answer for that turn. RISE then uses a reward system to train the LLM, where successful improvements are rewarded, and unsuccessful ones penalized. This iterative process of generating responses, receiving feedback, and refining the answers enables the LLM to learn from its mistakes and enhance its reasoning abilities over time.


RISE successfully enhances the reasoning abilities of LLMs, enabling them to produce increasingly accurate solutions over multiple attempts, outperforming existing single-turn methods.


#### Truly professional


Deploying LLMs for public use requires ensuring both safety and helpfulness. However, existing LLLM guardrails often struggle to balance these two goals, leading to a trade-off where safer models are less helpful and vice-versa.


[PrimeGuard](https://arxiv.org/pdf/2407.16318) dynamically routes user requests to different versions of the same language model. PrimeGuard works in two stages. First, a guard model analyzes the user's request and determines its potential safety risks based on predefined guidelines. The guard model then provides guidance on how to respond. If the request is deemed safe, it's passed to a main model designed for helpfulness. If the request is clearly unsafe, it's politely refused.


However, if the guard model is unsure about the risk, it doesn't simply guess. Instead, it triggers a re-evaluation stage where it takes a closer look at the request, considering the context and potential for harm. Based on this deeper analysis, the guard model refines its guidance, ensuring a response that prioritizes safety without unnecessarily sacrificing helpfulness. PrimeGuard is further trained on numerous synthetically generated examples encompassing various safety categories, levels of maliciousness, and potential for harm. This comprehensive training enhances PrimeGuard's ability to make nuanced decisions regarding safety.


PrimeGuard successfully minimizes the trade-off between safety and helpfulness, achieving a higher level of both compared to existing methods. Specifically, PrimeGuard increases safe responses to 97% and reduces the success rate of adversarial attacks to 8% while maintaining or even improving helpfulness.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**First words -** OpenAI has started


[rolling out](https://x.com/OpenAI/status/1818353580279316863) its new Advanced Voice Mode for ChatGPT, offering real-time conversation experiences. Currently in limited alpha, it is available for Plus users on iOS and Android. Advanced Voice Mode currently does not support memory or custom instructions, but extra features are expected to become widely available to all Plus users in the fall.


**V-grammers**


**-** Meta is launching


[AI Studio](https://about.fb.com/news/2024/07/create-your-own-custom-ai-with-ai-studio/) , a platform allowing users to create, share, and discover AI characters, accessible via Instagram or Meta's website. Users can personalize these AI characters with unique personalities, appearances, and functions. Creators can also use AI Studio to extend their presence on social media, enabling their AI to answer common questions and respond to messages.


**As tiny as it can get -** Google has


[released](https://developers.googleblog.com/en/smaller-safer-more-transparent-advancing-responsible-ai-with-gemma) Gemma 2, a new family of open AI models, prioritizing responsible AI with a focus on safety and transparency. Gemma 2 includes a smaller 2B parameter model offering high performance for on-device applications, ShieldGemma, a suite of safety classifiers to detect and mitigate harmful content, and Gemma Scope, a model interpretability tool that provides insights into model decision-making.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify dev team.
