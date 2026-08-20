---
schema_version: "1.0.0"
document_id: "ba36b6681c3384e82dcb2818e2f6172eea899c1fc8facd1346846c4cbe935268"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/open-by-design-tech"
published_at: "2026-06-02T18:29:57.287+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:ae573e20bcf5f879e20faafbb968eae0958f60bf2dc879ea5d71d163d12e92af"
---

# Open by Design: How NVIDIA and DigitalOcean Are Building the Stack for the Always-On Agentic Era

The growth of generative AI isn’t driven solely by AI companies with proprietary models. Open-source AI is reshaping the developer ecosystem, fueled by a growing community of builders. But what does it take to go from open models to production-ready agentic AI, and what do developers need to know to get there?


This question was the focus of the DigitalOcean Deploy session, “Open by Design: How NVIDIA and DigitalOcean Are Building the Stack for the Always-On Agentic Era.” During this 30-minute chat, Kari Briski, VP Gen AI at NVIDIA, and Salman Paracha, SVP AI at DigitalOcean, discuss why AI-native teams are demanding openness, model flexibility, and infrastructure built for agents that never sleep—and what NVIDIA and DigitalOcean are doing to build support for this next generation of AI development.


**Watch the full recorded session from Deploy 2026** :


## Open-Source Models Need Commitment, Not Just a Launch


There are many open models in the ecosystem, but having great models doesn’t guarantee they will be consistently improved or regularly updated. NVIDIA noticed a potential gap in this space for its enterprise customers, who regularly wanted access to open-source models that are launched and then left untouched.


This spurred the development of open models such as NVIDIA[Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) . Released in March 2026, it serves as a family of multi-modal models designed for agentic AI. Having access to these open models enables developers to create agentic applications that require advanced reasoning, high compute efficiency, and open source standards. With Nemotron models and NVIDIA software libraries, developers can evolve their projects over time and receive regular updates and expanded support.


Running open-weight LLMs locally gives you more control over performance, privacy, and customization. This NVIDIA[Nemotron 3 tutorial](https://www.digitalocean.com/community/tutorials/nemotron-3-models-run-gpu-droplet) walks through deploying NVIDIA’s Nemotron 3 Nano on a DigitalOcean GPU Droplet, helping you experiment with efficient open models on dedicated GPU infrastructure without relying entirely on hosted AI APIs.


“We’ve been building these models for ourselves because we want to build great systems,” Briski says. “We’re treating \[these models\] like a library and are committed just like we are with our GPUs and \[CUDA\] libraries and our stack that we’ll improve upon.”


Beyond the models themselves, there’s also a proliferation of harnesses—the orchestration frameworks that wrap around models to manage agent lifecycle, memory, tool calling, and scaling—which are just as important for building agentic systems.


## Agents Are Only as Good as Their Evaluations


Paracha highlighted that most developers building AI-native applications are still facing a high hurdle and admission rate in determining whether it’s possible to build something as durable as[OpenClaw](https://www.digitalocean.com/resources/articles/what-is-openclaw) or Claude Code.


Figuring out true evaluations and observability becomes a challenge, and these developers are left wondering whether they can truly compete with AI companies that have funding for research and top-of-the-line hardware. So what does lowering that barrier to entry (and creating developer confidence) look like?


Evaluation is where it starts, according to Briski. While there are many test cases and verifications for specific use cases (such as coding), other applications lack readily available benchmarks, and academic options don’t necessarily effectively evaluate real-world models or optimize performance.


Without these standards, it becomes harder for developers to gauge the viability of their idea. For broader development, more test cases need to be created and data pulled from, which requires human knowledge and labeling. For industries like electronic automation, NVIDIA is currently working with Synopsys and Cadence to develop these test cases and benchmarks to encourage development and agent creation.


## Sub-Agents Only Scale When You Can Trace Them


Developers running AI-native applications have adopted sub-agent workflows that break a problem into subtasks and delegate them to a single agent. Paracha has seen developers do this, but is curious about how this subset of AI development might shape up over the next few years, and what engineering principles still apply.


If you’re curious about what a sub-agent (or multi-agent) system can do, read about the[TradingAgents LLM](https://www.digitalocean.com/resources/articles/tradingagents-llm-framework) , which is designed to function as a simulation for financial trading through specialized agents.


“There’s a thread in engineering right now where you still have to understand how the system was built, even though the agent is writing the code. So when sub-agents are going off, you are able to test them, you are able to verify, and break it down to where something might be going wrong, so that you, as the architect, can understand the system,” Briski explains.


This philosophy also pairs well with adding traceability throughout the system, so you can have references during troubleshooting instead of just the end product to look at, leaving you with a black box. While there is a newer approach of feeding a system a whole bunch of information and having it develop an answer, having the “divide and conquer” approach still seems to be the standard.


## Good Tokenomics Starts With Outcomes, Not Token Count


Scaling AI comes with a new problem: token usage. How can developers run AI systems that are consistently generating tokens and simultaneously build an effective business around them? What it really comes down to is the product’s value; the items delivered and the workflow efficiencies created.


“We’re in a stage right now where tokens are going to be counted differently as model architectures change. \[But\] we have to evolve our way of thinking because the way we count tokens generated with diffusion models and the latent spaces of tokens could all change. So I think instead of spinning out on how many tokens are being generated, it’s more about the value,” Briski explains.


But organizations do need to consider cost, especially with the larger models. NVIDIA is taking technical measures to improve the efficiency of token use. This includes using a hybrid-state-space transformer in the latest Nemotron Model, rather than combining a dense model with[mixture-of-experts](https://www.digitalocean.com/community/tutorials/expert-parallelism-in-deep-learning) (MoEs).


Model architectures are fundamental to token economics, and there’s been a general shift: from a very model-dense view of the world (megamodels with 8 billion or a trillion parameters) to a sparser proliferation of MoEs and the use of solid-state models (SSMs) backed by NVIDIA. These SSMs, Briski says, remove some of the attention layers for pre-processing and reduce the compute you need for the data prefill.


## Open Source Forces the Whole AI Ecosystem to Move Faster


Beyond using SSMs, NVIDIA’s applied research team is consistently reviewing academic papers, testing new models, exploring new architectures, and collaborating with the open-source community.


“We actually put out a paper about the hybrid Mamba architecture in \[early\] 2025. What was interesting was that the Qwen model adopted it before our Nemotron product did. The point of how important open source is to share these ideas and learn from each other. We’re not just putting \[our tech\] out there. We’re also picking up ideas from other open source projects,” Briksi says.


At DigitalOcean, one of Paracha’s focuses is expanding the ecosystem around open-source projects. There’s Plano (DigitalOcean’s data-plane technology), along with a push for research on small action models (SAMs). These models can complete tasks using context compression (instead of requiring reasoning tokens) to perform specific tasks more efficiently without requiring long context windows. Paracha’s team is also looking into AI system harnesses and how DigitalOcean can use open source to empower developers with the freedom to choose the harness they want to run their models.


“The open harness is a zero-instrumentation plug-in architecture where you can bring in open code or a LangChain or LangGraph type of agent, and we help you manage and scale it. There are a lot of lifecycle events of an agent that still have to be solved for. With the open harness story, the real mantra is, ‘how do we enable choice and freedom and support the ecosystem versus create our own?’” Paracha asks.


## Agent Workloads Are Always Changing Shape


Going forward with these multi-agent and multi-layered systems, Paracha says, there will be a lot of work to be done on context compression and expansion.


Briksi expands on this idea, saying inference workloads are dynamically changing and that there’s been a shift from long context input to long context output and initial reasoning and long context output; all the in-between steps are still evolving.


“Everything in the \[Deploy\] keynote is heading to ‘how can you optimize for these dynamically changing workloads with routing, keeping the cache and context right, even with compression for really long horizon tasks?’” Briski says.


Going forward, developers will need to become familiar with long-horizon and long-running tasks, as well as self-evolving systems, which are related but ultimately distinct. Knowing these tasks are beneficial to how developers manage their memory, compute power, and model architectures.


## Every Legacy Application Is an Agent Opportunity


As the market moves from generative AI to open source AI, organizations and individual developers alike are looking at what might change over time and what won’t when it comes to how we think about and build AI-native applications.


Briski says that the need for compute won’t go away. It’s been proven across many scaling laws in pre-training, post-training, interface training, and agents that more computing power means greater intelligence capabilities.


What she’s most excited to see is more domains beyond coding pick up and create verifiers and reinforcement learning environments to support a wider range of AI-native and agent-based applications across different industries.


“There are so many things in our lives where I can’t wait until agents are infused into these applications. And so that’s why when you think about when agentic AI will be integrated into all of these legacy software applications, I get really excited,” she says.


---


DigitalOcean and NVIDIA are building together. DigitalOcean’s serverless inference runs on NVIDIA accelerated computing including NVIDIA Blackwell GPUs, NVIDIA Nemotron models are available directly on DigitalOcean’s AI Platform, and builders can prototype on[build.nvidia.com](http://build.nvidia.com/) before deploying to DigitalOcean GPU Droplets without rebuilding their stack.


With NVIDIA Dynamo 1.0 integrated for production inference scaling and the joint NemoClaw project bringing secure, always-on agent deployment, the collaboration gives developers a direct path from experimentation to production.


→[Get started with DigitalOcean’s AI-Native Cloud](https://cloud.digitalocean.com/registrations/new)


### About the author


Jess Lulka


Author


Content Marketing Manager


[See author profile](https://www.digitalocean.com/community/users/jlulka)


Jess Lulka is a Content Marketing Manager at DigitalOcean. She has over 10 years of B2B technical content experience and has written about observability, data centers, IoT, server virtualization, and design engineering. Before DigitalOcean, she worked at Chronosphere, Informa TechTarget, and Digital Engineering. She is based in Seattle and enjoys pub trivia, travel, and reading.


[See author profile](https://www.digitalocean.com/community/users/jlulka)
