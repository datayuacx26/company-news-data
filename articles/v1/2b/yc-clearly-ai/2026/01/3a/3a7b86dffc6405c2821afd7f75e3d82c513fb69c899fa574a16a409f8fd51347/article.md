---
schema_version: "1.0.0"
document_id: "3a7b86dffc6405c2821afd7f75e3d82c513fb69c899fa574a16a409f8fd51347"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/applying-ai-to-threat-modeling"
published_at: "2026-01-20T00:00:00+00:00"
first_seen_at: "2026-07-23T05:49:00.156409+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:b32f1ce9bc71bf8e9033529d1e5dc160ed4f14295b167b0b1f102c3aa5c36aa5"
---

# Applying AI to threat modeling - where AI thrives, survives, and dies

*This post is a summary of my talk at*[ThreatModCon 2025](https://www.youtube.com/watch?v=Wf58UkYPRAw)


AI isn’t uniformly good or bad at threat modeling. It depends on how you structure AI in your workflows. Using these powerful tools without clear goals is where teams get into trouble.


The viability of AI for threat modeling use cases falls into three categories:


-


**Thrives** - provides value without a lot of fuss


-


**Survives** - needs some supervision but works well


-


**Dies** - doesn't work or leads to dangerous mistakes


I’ve spent my career at the intersection of AI and security, from building NLU systems at Amazon Alexa to securing an enterprise AI platform at Moveworks, so when ChatGPT 3.5 landed, I immediately saw both the opportunity and the risks.


At Clearly AI, we've been building an AI-powered security assessment platform since before it was trendy, working with companies like Webflow and HID Global to scale their threat modeling programs. These principles aren't theoretical; they're what we've learned building AI workflows that actually work.


For this discussion, threat modeling refers to design-focused analysis with security goals: understanding the architecture (threat actors, trust boundaries, and data flows), determining attack vectors and threats, and identifying compensating controls while surfacing residual risk.


AI can help achieve these goals faster and at scale, but only if applied correctly. LLMs can crunch docs, code, and diagrams, and apply security frameworks like STRIDE to scale analysis without the[bottlenecks](https://www.clearly-ai.com/blog/solving-the-threat-modeling-bottleneck-with-ai-workflows) human teams face. But knowing AI *can* help doesn't tell you *how* to use it effectively. Here are the do's and don'ts that separate useful automation from unreliable robo-text.


## Thrives: Where AI works out of the box


Whenever we use AI, we should be asking ourselves if this is an appropriate situation, or if we're seeing nails while holding a shiny new hammer. Fortunately, AI excels at certain threat modeling tasks without much hand-holding.


#### Rapid system comprehension


A big challenge in threat modeling is getting security engineers with broad scope and scant time to understand what's being built and how the system works. The multi-modal aspect of modern LLMs (great at analyzing code, documentation, images) means you can rapidly tie together ad hoc information to create meaningful system descriptions. All of this preserves security research and developer interaction time.


#### Scaling rote analysis


Prompts in AI systems can be reused to apply the same analysis to different contexts. This lends itself to applying well-defined threat modeling frameworks like STRIDE consistently, and systematically checking for common security controls: Does this application have encryption at rest? Are credentials stored in a secrets manager? Is authentication required for all endpoints? Are there rate limits on API calls? Unlike our treasured security colleagues, AI systems don't get tired, they don't get hungry, and they have immeasurable patience for running through these checklists across hundreds of systems.


## Survives: AI needs help to help you


In some circumstances, AI needs optimizations to prevent vague, incomplete, or just plain wrong answers. If you just ask ChatGPT “threat model this PRD”, you'll get a generic analysis that lacks the broad, deep, and unique organizational perspective that attuned security analysts use to build trust with their engineering counterparts.


#### Applying organizational context


For most organizations, the knowledge about the underlying business that drives the risks implicit in any security analysis lives in cloud knowledge management systems like wikis and file shares. To incorporate this context, LLMs need to augment their generation by retrieving it first (hence the catchy "Retrieval Augmented Generation", or RAG, terminology).


While earlier RAG systems involved dumping all your documentation into a vector database upfront, modern agentic systems let you call tools to search your organizational context at runtime (e.g. "go find this doc in Confluence"). This means connecting it to your internal ticketing systems, compliance frameworks, enterprise policies, and requirements docs. The AI pulls what it needs when it needs it, grounding threat models in your organization-specific best practices.


This is exactly what we built at Clearly AI. Our platform intelligently connects to your internal systems (e.g. Confluence, Jira, GitHub) and pulls threat modeling context at analysis time rather than requiring document uploads beforehand.


#### Ensuring trustworthy outputs


When I think about reliable AI systems, the first thing I consider is trustworthiness: stopping hallucinations and catching errors with humans-in-the-loop when necessary. This is a deep topic, but here are four techniques that reduce the risk of AI making things up and fooling humans with faulty information:


-


**Chain of Thought (CoT) reasoning** forces the AI to show its work, improving accuracy through explicit reasoning steps.[Amazon Science](https://www.amazon.science/blog/automating-hallucination-detection-with-chain-of-thought-reasoning) demonstrates using CoT to check claims against sources, as does the "[Chain of Verification](https://arxiv.org/pdf/2309.11495) " paper.


-


**Source citation** requires the AI to link to reference-able documentation in the output. Google’s[Grounding guide](https://cloud.google.com/vertex-ai/generative-ai/docs/grounding/overview) describes using Retrieval-Augmented Generation (RAG) to anchor responses to verifiable data.


-


**Prompt engineering** for uncertainty explicitly rewards the model for saying "I don't know" rather than guessing, overriding the default "helpful assistant" behavior. This is Anthropic's #1[basic hallucination minimization strategy](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations#basic-hallucination-minimization-strategies) .


-


**LLM-as-judge** uses a second AI instance to review the first one's output against source context, performing peer review with distinct context windows.[Datadog](https://www.datadoghq.com/blog/ai/llm-hallucination-detection/) explains how this Judge LLM approach identifies inaccuracies.


At Clearly AI, we leverage all four: every threat or finding includes source citations linking back to the specific section of design or code, the reasoning process is reinforced for uncertainty and visible for security teams to verify, and we use LLM-as-judge validation before surfacing results.


#### Producing consistent outputs


The value of a threat model is when analysis turns into action - the answer to "so what?". Using a structured output format for threat modeling means the AI's analysis can automatically populate databases, integrate with ticketing systems, and maintain consistent risk ratings across different models or prompts. This rigor helps organizations scale their secure software development lifecycle to big distributed teams.


Check out this[BAML example](https://www.promptfiddle.com/BAML-Examples-WI3ZW) (BAML is a programming language for LLMs that enforces structured outputs) which ensures consistently formatted data with controlled vocabularies. Threat categories must be one of the six STRIDE types, severities must be High/Medium/Low, and findings follow a defined schema. The results can be used to populate a semantically consistent organizational memory.


This is how Clearly AI generates threat models that automatically create tickets with the right severity labels, populates a risk register with standardized categories, and tracks remediation status across an entire application portfolio.


## Dies: Where AI fails


Being a better security professional also means knowing when *not* to use AI. These caveats describe missteps and losing strategies to avoid.


#### Undefined threat modeling objectives


AI thrives in goal-directed systems. If you can't articulate why you're threat modeling - compliance checklist, creative attack simulation, building developer buy-in - adding AI will only muddy the waters. Define your objectives first.


#### Complex organizational tradeoffs


High-stakes security decisions involving multiple stakeholders, politics, or cross-functional context require human judgment. Don't outsource recommendations that need organizational awareness and tactical framing.


#### Requirements for deterministic results


LLMs are probabilistic. They can miss things, misinterpret context, or produce inconsistent outputs. If your threat modeling can't tolerate any margin of error, LLMs aren't the right tool.


## In summary


AI is not a panacea for threat modeling, but it shines with foundational skillsets like systems understanding and goal-based reasoning. It’s a force multiplier, enabling lean security teams supporting fast-moving product orgs, where threat modeling either doesn’t happen or happens too late to matter.


AI handles the mechanical work, like rapid comprehension, consistent framework application, and baseline control verification. Security teams focus on critical thinking: validating results, thinking outside the box with creative threat discovery, and ensuring multi-stakeholder recommendations actually make sense for the organization building the system.


At Clearly AI, we've operationalized this framework into a platform that scales threat modeling for lean security teams. Our system handles rapid comprehension, applies flexible threat modeling processes consistently across hundreds of systems, grounds analysis in your organization's context, and produces structured outputs that integrate with your existing workflows, all while keeping humans in charge of high-stakes decisions. Check out our[AI Engineer Cheat Sheet](https://www.clearly-ai.com/blog/ai-engineer-cheat-sheet) which offers tools, techniques, and tips for using AI in your engineering workflows orreach out to me to chat further!
