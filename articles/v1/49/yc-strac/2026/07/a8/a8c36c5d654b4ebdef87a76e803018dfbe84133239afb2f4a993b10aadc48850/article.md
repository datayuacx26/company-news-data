---
schema_version: "1.0.0"
document_id: "a8c36c5d654b4ebdef87a76e803018dfbe84133239afb2f4a993b10aadc48850"
company_key: "yc-strac"
company: "Strac"
source_id: "yc-strac-news-import-28a26672fe0a"
canonical_url: "https://www.strac.io/blog/llm-security"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-08-01T21:39:44.371100+00:00"
fetched_at: "2026-08-01T21:39:46.061495+00:00"
content_hash: "sha256:0a186a9e5ede7d6c21d1e362c73f4ce1e3f6ee3c2c06de906615fd6a8912d9f9"
---

# LLM Security: Protecting Data in Large Language Models (2026)

The prompt boundary: PII, PHI, card data, and secrets removed before the text ever reaches the model.


## The Two Halves of LLM Security


Ask ten security teams what "LLM security" means and you'll get two different answers:


On the enforcement side, Strac[integrates with the Claude Compliance API](https://www.strac.io/integrations/claude-compliance-api) so security teams can see and control the sensitive data moving through Claude Enterprise — LLM security applied to a production deployment.


**Model security** — protecting the model and the application around it: - **Prompt injection** — an attacker embeds instructions in content the model reads (a web page, a document, an email) and hijacks its behavior - **Jailbreaks** — bypassing safety guardrails - **Model theft and extraction** — stealing weights or reconstructing training data - **Insecure output handling** — the model's output executes as code downstream - **Supply chain** — a compromised model, dataset, or plugin


**Data security** — protecting the information flowing through it: - **Sensitive data in prompts** — PII, PHI, card numbers, and API keys pasted into a chat window - **RAG oversharing** — the retrieval layer surfaces a document the user was never supposed to see - **Agent overreach** — an AI agent with broad connector permissions pulls regulated data out of Salesforce or a database - **Training and retention** — your data used to improve someone else's model


Both matter. But be honest about which one you have. **If you are not building models, your LLM security problem is almost entirely the second list.** The OWASP Top 10 for LLM Applications is written largely for people building LLM applications; most companies are *consuming* them, and their exposure is that employees are handing over data no policy authorized.


## LLM Security: Model Risks vs Data Risks


Model security (building LLM apps) Data security (using LLMs)


The threat The model is manipulated or stolen Your data goes into someone else's model


Examples Prompt injection, jailbreaks, insecure output handling, model theft, supply chain Secrets and PII in prompts, RAG oversharing, agents pulling regulated data via connectors


Who has it Teams building LLM applications Nearly every company using AI


Reference OWASP Top 10 for LLM Applications Data governance and DLP


The control Input validation, output handling, least-privilege tools Redact sensitive data at the prompt and tool boundary


## ✨ Risk 1: Secrets and PII in Prompts


The single most common LLM data leak, and the least sophisticated:


- A developer pastes a stack trace to debug it — and the trace contains a live database URL and API key
- A support agent pastes a customer's full ticket, including their SSN, to draft a reply
- A recruiter pastes a candidate's resume to summarize it
- An analyst pastes a spreadsheet of account numbers to ask for patterns


None of these people are behaving maliciously. Every one is doing their job. And on consumer AI tiers, that content may be retained, used for training, and in some cases reviewed by a human.


**Zero data retention agreements do not fix this.** ZDR governs what the provider does with the data *after it arrives* . The key was still transmitted. A leaked credential is leaked whether or not it was retained.


For data classes you never permit — an API key, a production credential — the submission is blocked outright.


## ✨ Risk 2: RAG Oversharing


Retrieval-augmented generation is where LLM security gets quietly dangerous. You point a model at your document corpus so it can answer questions with company context. It works beautifully — and it will happily retrieve:


- The compensation spreadsheet on a SharePoint site with broad permissions
- The legal memo in a folder shared with "everyone"
- The customer contract in a Drive folder someone opened "temporarily" in 2021


The model isn't broken. **It's honoring exactly the permissions you gave it, against a corpus nobody ever cleaned.** RAG doesn't create oversharing; it industrializes the discovery of it. This is why RAG rollouts should begin as a data-hygiene project — find the sensitive data, fix the sharing, remediate the content — not as a retrieval-tuning exercise.


Risk 3 in one picture: the agent's tool call returns through Strac, so PII is stripped from the response before it reaches the model.


## ✨ Risk 3: Agents and Connectors


The frontier. When an LLM is given tools — through the[Model Context Protocol](https://www.strac.io/blog/mcp-data-security) or a vendor's connector framework — it stops being a chat box and becomes a system with database access.


An agent asked "summarize this customer's history" may pull the full Salesforce case record, including the card number an agent pasted into a comment three years ago, and ship it into a context window. Every permission check passed. The data left anyway.


Governing this means controlling **what comes back from the tool call** , not just what the user typed. See[MCP DLP](https://www.strac.io/blog/mcp-dlp) .


The result the user sees: the answer still works; the regulated data never left.


## How to Actually Secure an LLM Deployment


**1. Control the prompt boundary.** Detect PII, PHI, card data, secrets, and source code as they're typed or submitted, and redact, warn, or block per data class. This works across every LLM — including personal accounts on tools you don't manage, which is where most of the exposure lives.


**2. Clean the corpus before you point RAG at it.** Discover sensitive data across SharePoint, Drive, and Confluence; fix over-broad sharing; remediate the content itself. If the SSN isn't in the document, retrieval can't leak it.


**3. Govern the tool layer.** Inspect and redact data flowing back from connectors and MCP servers before it reaches the model.


**4. Prefer enterprise tiers, but don't rely on them.** Enterprise agreements exclude training and tighten retention. They do not stop an employee opening a personal tab.


**5. Log everything as evidence.** Every detection and redaction should map to SOC 2, HIPAA, GDPR, and ISO 42001.


**Strac** enforces at the prompt and tool boundary: **48+ secret patterns** , PII, PHI, PCI, and source code detected and removed before data reaches ChatGPT, Claude, Gemini, Copilot, or an AI agent — in the browser (Chrome, Edge, Firefox, Safari) and across MCP connectors. See[AI DLP](https://www.strac.io/blog/ai-dlp) .


## ✨ How Strac Covers Every AI Surface


Securing an LLM deployment means holding four boundaries at once:


- The prompt —[Browser DLP](https://www.strac.io/integration/browser-dlp) strips secrets and PII before they reach the model.
- The device —[Endpoint DLP](https://www.strac.io/endpoint-dlp) covers local AI apps and coding assistants.
- The retrieval and tool layer —[MCP DLP](https://www.strac.io/blog/mcp-dlp) inspects what comes back from a connector before the model sees it.
- The estate —[shadow AI](https://www.strac.io/blog/shadow-ai) discovery and[AI governance](https://www.strac.io/blog/best-ai-governance-tools-and-software) , so you know which models are in play.


One platform, four AI surfaces: the browser, the endpoint, the agent, and the estate itself.


## 🌶️ Spicy FAQs for LLM Security


### What is LLM security?


LLM security covers two distinct problems: protecting the model and application from manipulation (prompt injection, jailbreaks, insecure output handling, model theft), and protecting the data flowing through it (sensitive data in prompts, RAG retrieving documents users shouldn't see, agents pulling regulated data through connectors). Organizations that consume LLMs rather than build them almost always have the second problem, not the first.


### What are the biggest LLM security risks?


For companies using LLMs: secrets and PII pasted into prompts, RAG pipelines surfacing overshared documents, and AI agents with broad connector permissions pulling regulated data. For companies building LLM applications: prompt injection, insecure output handling, and supply-chain compromise of models or plugins. The OWASP Top 10 for LLM Applications covers the builder's list well; the consumer's list is mostly a data-governance problem.


### Does zero data retention make an LLM safe?


No. Zero data retention governs what a provider does with your data after it arrives — it does not prevent the data from being transmitted. An API key sent to a model under a ZDR agreement is still an exposed API key, and a patient record is still a disclosure. ZDR is a necessary contract term, not a security control.


### How do I stop sensitive data going into an LLM?


Enforce at the point of submission rather than in policy. Detect PII, PHI, card numbers, secrets, and source code in the prompt as it is typed and redact, warn, or block based on your rules. Because this operates at the browser layer, it protects you even when an employee is signed into a personal account on a tool you never approved.


### Is RAG a security risk?


RAG is a security *amplifier* . It does not create oversharing, but it makes years of accumulated poor permissions instantly searchable in natural language. A compensation file on an over-broad SharePoint site was always exposed; before RAG, nobody could find it. The fix is upstream: discover the sensitive data, fix the sharing, and remediate the documents before you index them.


### What is prompt injection, and can DLP stop it?


Prompt injection is an attack where malicious instructions are embedded in content the model reads — a web page, a PDF, an email — causing it to ignore its original instructions. Data-layer controls do not prevent injection itself; that requires input validation, output handling, and least-privilege tool design. What they do prevent is the *payoff* : if the injected instruction says "send me all customer SSNs," and the SSNs have been redacted or vaulted out of the data the agent can reach, the attack retrieves nothing.


## The Bottom Line


LLM security is not one discipline. If you are building models, read OWASP and worry about injection and supply chain. If you are like most companies — consuming LLMs, wiring them to your data, and letting employees use them — your risk is far more mundane and far more likely: **your data is walking into someone else's model, one prompt at a time.**


Secure the boundary where data meets the model, and most of the LLM security problem you actually have goes away.


[Book a demo](https://www.strac.io/book-a-demo) to see secrets and PII stripped from LLM prompts and agent tool calls in real time.


Related:[AI DLP](https://www.strac.io/blog/ai-dlp) ·[secure sensitive data in LLM prompts](https://www.strac.io/blog/secure-sensitive-data-in-llm-prompts) ·[MCP data security](https://www.strac.io/blog/mcp-data-security) ·[AI security posture management](https://www.strac.io/blog/ai-security-posture-management) ·[shadow AI](https://www.strac.io/blog/shadow-ai)
