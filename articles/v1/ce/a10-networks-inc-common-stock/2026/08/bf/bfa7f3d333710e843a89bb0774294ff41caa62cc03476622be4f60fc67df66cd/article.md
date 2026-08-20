---
schema_version: "1.0.0"
document_id: "bfa7f3d333710e843a89bb0774294ff41caa62cc03476622be4f60fc67df66cd"
company_key: "a10-networks-inc-common-stock"
company: "A10 Networks Inc."
source_id: "a10-networks-inc-common-stock-rss-284845abe605"
canonical_url: "https://www.a10networks.com/blog/ai-security-architecture-problem/"
published_at: "2026-08-05T23:18:15+00:00"
first_seen_at: "2026-08-06T23:45:24.349806+00:00"
fetched_at: "2026-08-06T23:45:25.346110+00:00"
content_hash: "sha256:19b5342c510e20f5e8fc30aeeabdb8b7209d42a56159d9f406ebb70225b70d3e"
---

# Why AI Security is an Architecture Problem

For the last year, I’ve been watching security teams argue about AI as if it fits neatly into one of two buckets. Either it’s “just another application” and we can handle it with standard AppSec controls, or it’s some radically new category of risk that demands entirely new security theory. Having worked as both a pentester and a software engineer, I think both positions miss what’s happening. It’s not that AI breaks security. AI breaks a foundational assumption that most of our security practices quietly depend on, and that’s determinism.


## Key Takeaways


- AI doesn't introduce a new category of security risk; it breaks the determinism that traditional security controls quietly depend on
- Every AI product has two systems at once: a deterministic application layer to harden and a probabilistic intelligence layer to contain
- Prompt injection exploits interpretation, not syntax, so it cannot be patched like code and must be bounded with architectural controls
- Effective AI security aggressively secures the deterministic infrastructure and designs containment, monitoring, and separation of authority around the model


Traditional software systems are deterministic. Given the same input and the same state, you get the same output. That property is what makes debugging possible; what makes unit testing meaningful; and what makes exploitation reliable. When I used to find a vulnerability during a pentest, the first thing I’d confirm was repeatability. If the exploit only worked once out of ten times, it wasn’t a clean finding. Determinism is what turns a bug into something actionable.


Security controls rely on that same stability. Access control checks are deterministic. Input validation is deterministic. A policy engine evaluates a rule set and produces the same decision every time. Even when systems are complex, they are still executing fixed logic.


[Large language models](https://www.a10networks.com/blog/llm-security/) don’t behave that way. They approximate probability distributions over tokens conditioned on context. In plain English: they infer likely continuations rather than executing explicit logic. Even if you lock down temperament and try to make them “behave,” you are still operating a probabilistic model that interprets context and intent through statistical patterns.


That difference sounds academic until you try to[secure a system built around it](https://www.a10networks.com/solutions/ai-security/) .


When you deploy an AI product, you’re deploying two systems at once. The first is the application layer: APIs, authentication, databases, retrieval pipelines, tool wrappers, logging, rate limiting, and all the usual infrastructure. That part is still deterministic. It still needs hardened interfaces, strict schema validation, capability-based access control, sandboxing, and proper separation of privilege. If your model can trigger arbitrary shell commands because you passed its output directly into a subprocess call, that’s not an[“AI vulnerability.”](https://www.a10networks.com/blog/generative-ai-security-risks/) That’s bad engineering.


As a former pentester, I can tell you that most real-world AI deployments are still fragile at this layer. Tool wrappers trust the model too much. Output validation is loose. Retrieval pipelines ingest untrusted data without clear boundaries. These are familiar problems. They’re solvable with disciplined engineering.


But that’s only half the story.


The second system is the intelligence layer: the model’s interpretation of intent: its choice of tools, its planning steps, and its evolving context. This layer is not deterministic in the way the surrounding application is. It doesn’t execute a fixed decision tree. It evaluates context and selects actions based on patterns learned during training. That is where the new attack surface lives.


Prompt injection is a good example. People describe it as if it were a new form of injection vulnerability, but it’s not exploiting a parser. It’s exploiting interpretation. You’re not breaking syntax. You’re influencing how the model weighs instructions. You’re trying to convince it that malicious instructions outrank system constraints.


From an offensive perspective, this feels very different from SQL injection. With SQL injection, I’m exploiting a predictable grammar and a predictable execution engine. With prompt injection, I’m probing how the model resolves conflicting authority and context. I’m looking for goal drift. I’m testing whether the model prioritizes “helpfulness” over policy. I’m seeing if I can cause it to misuse a tool by reframing the task. The vulnerability isn’t a missing escape character. It’s a shift in reasoning.


That’s what makes AI security uncomfortable. We’re used to proving that a control either works or it doesn’t. With AI, controls operate around a probabilistic core. You don’t eliminate non-determinism. You bound it.


This is where architectural discipline becomes critical. The application layer must assume the model is untrusted. Every tool call should be validated against a strict schema. Every action should be authorized independently of the model’s recommendation. The model should not have implicit authority just because it generated a structured response. If it wants to access a database, that request should go through the same enforcement layer any other service would.


At the same time, you need to recognize that you cannot “patch” probabilistic reasoning the way you patch code. You can shape it. You can constrain its action surface. You can separate planning from execution so that no single model instance has end-to-end authority. You can add oversight models or rule-based validators that check high-risk decisions. You can monitor behavior over time and look for anomalous tool sequences or unusual escalation patterns. But you cannot force it into perfect predictability without destroying the very capability you’re trying to leverage.


This is why I think the framing matters so much. If you treat AI as just another microservice, you will miss cognitive attack vectors like prompt injection and memory manipulation. If you treat the model as an uncontrollable black box, you will neglect the very real deterministic controls that can and should contain it.


The right mental model is simpler. Secure the deterministic system aggressively. Treat the intelligence as probabilistic and design containment around it. In practical terms, that means never letting raw model output directly trigger high-privilege operations. It means building capability-based tool interfaces instead of giving the model broad access. It means logging reasoning artifacts and tool calls so you can audit decisions after the fact. And it means assuming that user input is adversarial not only at the string level but at the intent level.


The biggest mistake I see right now is teams collapsing these two domains into one. They’ll say “we’ve secured the model” as if that were a coherent statement. You don’t secure a model in isolation. You secure the system that embeds it. The model is one component, and it happens to be the least deterministic component in the stack.


From a leadership perspective, this has implications for ownership. Application security teams understand deterministic enforcement. Machine learning teams understand model behavior and failure modes. AI security lives at the boundary. If those groups don’t collaborate intentionally, gaps will appear exactly where the deterministic system hands control to the probabilistic one.


After years of breaking systems and building them, I don’t think AI requires abandoning security fundamentals. It requires remembering why those fundamentals worked in the first place. They worked because behavior was predictable. Where behavior is no longer predictable, you compensate with containment, monitoring, and separation of authority.


We are not securing magic. We are securing a probabilistic engine wrapped in deterministic infrastructure. If we design the infrastructure correctly, the non-determinism becomes manageable. If we don’t, we end up giving statistical inference engines more authority than we would ever give a human engineer.


And that’s not an AI problem. That’s an architectural failure.


---


## FAQs


It means the risk lies in the system design, not the model itself. AI breaks the determinism that most security controls assume. Securing it requires hardening the deterministic application layer while building containment around the probabilistic model, treating the two as distinct domains that must work together.


SQL injection exploits syntax against a predictable grammar and execution engine. Prompt injection exploits interpretation, probing how a model resolves conflicting authority and context. The vulnerability isn’t a missing escape character; it’s a shift in the model’s reasoning, which is why it can’t be patched conventionally.


Large language models approximate probability distributions over tokens rather than executing fixed logic, so you cannot force them into perfect predictability without losing capability. Instead of patching, you shape behavior, constrain the action surface, separate planning from execution, and monitor for anomalous patterns over time.


Never let a model output trigger high-privilege operations directly. Validate every tool call against a strict schema, authorize actions independently of the model, build capability-based tool interfaces instead of broad access, log reasoning and tool calls for auditing, and treat user input as adversarial at both the string and intent levels.


### Share this post:


[Share on X (Twitter)](https://twitter.com/intent/tweet?text=Why%20AI%20Security%20is%20an%20Architecture%20Problem&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fai-security-architecture-problem%2F)[Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fai-security-architecture-problem%2F)[Share on LinkedIn](https://www.linkedin.com/shareArticle?mini=1&url=https%3A%2F%2Fwww.a10networks.com%2Fblog%2Fai-security-architecture-problem%2F&title=Why%20AI%20Security%20is%20an%20Architecture%20Problem&source=https%3A%2F%2Fwww.a10networks.com)Share on Email


Categories:


[AI](https://www.a10networks.com/blog/category/ai/)[Cyber Security](https://www.a10networks.com/blog/category/cyber-security/)


---


#### Related Resources


1. [Why is Leadership the Biggest AI Risk in Financial Services?](https://www.a10networks.com/blog/why-is-leadership-the-biggest-ai-risk-in-financial-services/)
2. [Federal Agencies – Driving Modernization Toward Zero Trust Architecture and Ease of Management](https://www.a10networks.com/resources/ebooks/federal-agencies-driving-modernization-toward-zero-trust-architecture-and-ease-of-management/)
3. [Zero Trust Architecture: 5 Reasons You Need It](https://www.a10networks.com/blog/zero-trust-architecture-5-reasons-you-need-it/)


Previous Post


[Has Your AI Gateway Matured into an Intelligent Control Plane?](https://www.a10networks.com/blog/ai-gateway-intelligent-control-plane/)


---
