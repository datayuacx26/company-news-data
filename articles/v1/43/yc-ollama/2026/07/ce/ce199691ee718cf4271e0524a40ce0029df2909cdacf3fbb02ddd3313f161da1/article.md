---
schema_version: "1.0.0"
document_id: "ce199691ee718cf4271e0524a40ce0029df2909cdacf3fbb02ddd3313f161da1"
company_key: "yc-ollama"
company: "Ollama"
source_id: "yc-ollama-news-import-14866557b877"
canonical_url: "https://ollama.com/blog/gpt-oss-safeguard"
published_at: null
first_seen_at: "2026-07-24T07:14:45.991039+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:c245eef019f79cf827e95d9c9b57ddfdd26e2b75f3bbe426a74ed48b5bc8f9a8"
---

# OpenAI gpt-oss-safeguard

Ollama is partnering with OpenAI and[ROOST](https://roost.tools/) (Robust Open Online Safety Tools) to bring the latest gpt-oss-safeguard reasoning models to users for safety classification tasks.` gpt-oss-safeguard` models are available in two sizes: 20B and 120B, and are permissively licensed under the Apache 2.0 license.


### Get started


1. [Download](https://ollama.com/download) Ollama
2. Open a terminal and run the model:


**20B:**


```text
ollama run gpt-oss-safeguard:20b


```


**120B:**


```text
ollama run gpt-oss-safeguard:120b


```


### Highlights


- **Trained to reason about safety:** Trained and tuned for safety reasoning to accommodate use cases like LLM input-output filtering, online content labeling and offline labeling for Trust and Safety use cases.
- **Bring your own policy:** Interprets your written policy, so it generalizes across products and use cases with minimal engineering.
- **Reasoned decisions, not just scores:** Gain complete access to the model’s reasoning process, facilitating easier debugging and increased trust in policy decisions. Keep in mind Raw CoT is meant for developers and safety practitioners. It’s not intended for exposure to general users or use cases outside of safety contexts.
- **Configurable reasoning effort:** Easily adjust the reasoning effort (low, medium, high) based on your specific use case and latency needs.
- **Permissive Apache 2.0 license:** Build freely without copyleft restrictions or patent risk—ideal for experimentation, customization, and commercial deployment.


### Performance results


OpenAI evaluated the gpt-oss-safeguard models on both internal and external evaluation sets. In the internal evaluation, OpenAI provided multiple policies simultaneously to gpt-oss-safeguard at inference time. For each test input, OpenAI evaluated whether gpt-oss-safeguard correctly classifies the text under all of the included policies. This is a challenging task—the model is counted as accurate only if it exactly matches the golden set labels for all the included policies.


OpenAI further evaluated these models on[the moderation dataset](https://github.com/openai/moderation-api-release) they released with their[2022 research paper](https://ojs.aaai.org/index.php/AAAI/article/view/26752) and on[ToxicChat](https://arxiv.org/abs/2310.17389) , a public benchmark based on user queries to an open-source chatbot.


> *“gpt-oss-safeguard is the first open source reasoning model with a ‘bring your own policies and definitions of harm’ design. Organizations deserve to freely study, modify and use critical safety technologies and be able to innovate. In our testing, it was skillful at understanding different policies, explaining its reasoning, and showing nuance in applying the policies, which we believe will be beneficial to builders and safety teams.”* - **Vinay Rao, CTO of ROOST**


***About ROOST (Robust Open Online Safety Tools)***


*ROOST is a non-profit organization focused on providing accessible, high-quality, open source safety tools for digital organizations of all kinds in the age of AI. Established in 2025 by a diverse group of leading technology companies, philanthropic organizations and academic institutions, ROOST believes that solutions for online safety can best be achieved by providing organizations with innovative open source tools and technical support.*


### Reference


- OpenAI[blog](https://openai.com/index/introducing-gpt-oss-safeguard/)
- OpenAI[gpt-oss-safeguard developer cookbook](https://cookbook.openai.com/articles/gpt-oss-safeguard-guide)
- ROOST’s[model community repository](https://github.com/roostorg/open-models) on GitHub
