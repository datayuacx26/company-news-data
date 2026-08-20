---
schema_version: "1.0.0"
document_id: "54a434220ce49d55748bfa8baa99c27409836f655bb26c9513fccc5eceb9e285"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/how-to-maximize-value-and-minimize-risks-of-ai-assistants-in-commerce-software/"
published_at: "2026-07-29T15:39:35+00:00"
first_seen_at: "2026-07-29T18:38:53.736540+00:00"
fetched_at: "2026-07-29T18:38:57.027223+00:00"
content_hash: "sha256:394b8f2535cf66d98758b966e0b47d16899e6819692354b27087cb7d25a97461"
---

# How To Maximize Value — And Minimize Risks — Of AI Assistants In Commerce Software

Commerce software, from order management systems to site search, requires users to configure rules, create objects, tune experiences, and manage expectations. Now vendors are adding their own genAI assistants, embedded in their solution’s UI, to help users do that work using natural language (and perhaps to keep users within the platform rather than exploring a[vibe-coded](https://www.forrester.com/blogs/no-you-cant-just-vibe-code-commerce-yet/) replacement).


Embedded genAI assistants can accelerate work efficiency. They can also quietly create setting, strategies, and workflows that no one can find later, and that might not abide by the company’s AI policy. This is an AI use governance and risk problem that can compound over time.


#### Risk 1: The Invisible Audit Trail


Business users (“users”) may create things through chat that are not obvious in the traditional UI. For instance, if the UI doesn’t support creating a very complex type of promotion but the chat does, that promotion might not show in the promo list where users would see, enable/disable, and stack the promotions for compatibility. The risk is higher if software vendors recommend using their AI companions to skirt gaps in the UI, potentially leading the vendor to avoid closing that gap in their solution in the future.


If an assistant creates a promotion, adjusts a merchandising rule, configures a workflow, or changes a product attribute, your team needs to be able to find, renew, compare, edit, approve, and reverse that output the same way it would if a human created it manually. If the assistant’s work is buried in a chat history, visible only to the user who prompted it, or simply not visible in the UI, users have created a shadow configuration layer.


### Watch for these failure points:


- **Visibility.** If users can create or change something through chat, they must be able to see it in the standard admin UI.
- **Auditability.** The system must clearly document who prompted the action, what the assistant changed, what data it used, and whether a human approved it.
- **Authority.** Users often have permissions that allow them to access and configure only the areas of the UI that they should, based on their role. AI assistants may not respect those permissions, effectively authorizing users to control areas the organization doesn’t intend them to control. AI chat lacks a clear owner, so as users create work via the chat, that work also escapes proper ownership.
- **Comparability.** Human users must be able to compare AI-created entities with UI-generated ones. For example, these entities are not comparable if some very complex promotions are AI-created and invisible in the UI alongside human-created ones (or are visible but incomplete because they employ settings that are unavailable in the UI).
- **Explainability.** Users need to be able to explain (to their managers and potentially to auditors or other authorities) why the AI agent made the decisions, or took the actions, that it did.


#### Risk 2: GenAI Becomes UX Spackle


Some vendors use AI assistants to mask weak practitioner tools instead of improving them.


Natural language can make some tasks easier. It can help users get started, summarize options, draft content, or navigate a complex workflow. But if the answer to every usability gap is “ask the assistant,” you create a new dependency.


The risk is in operational fragility. Users rely on prompts instead of processes, leading to inconsistent outcomes, harder onboarding, and increasing complexity over time.


Digital leaders should determine whether the assistant is making strong functionality easier to use or whether it is compensating for functionality that remains incomplete.


### Evaluate whether the AI assistant is adding value that’s worth the risk:


- **Value: visible augmentation.** The assistant accelerates a task with a result that users can still complete, inspect, and mange in the core UI.
- **Risk: functional substitution.** The assistant becomes the only practical way to complete a task because the product workflow is incomplete or too hard to use or because the result of the AI-augmented work is invisible to the user in the UI.


#### Risk 3: AI Recommendations Risk Strategic Sameness Across Competitors


If every business customer asks the same assistant how to improve conversion, optimize search, reduce returns, personalize offers, or prepare for agentic commerce, many will receive similar answers. Some will be generic, overly confident, or reliant on incomplete context.


This sameness is especially risky in commerce because differentiation lives in the details. Each organization must make different strategic decisions due to variation across customer behavior when shopping with that brand, margin structure and seasonality, fulfillment logistics, and internal goals. Generic recommendations can push teams toward the same playbook as competitors.


#### Manage AI Assistant Adoption On Both Internal And External Fronts


### Externally, assess vendors based on how well they provide:


- **Object visibility.** AI-created objects (or rules, elements, etc.) must be visible to users in the same UI where manually created objects are managed.
- **Source traceability.** Users should be able to see the data, prompt, and assumptions that shaped the output.
- **Version control.** Teams need to be able to compare versions, roll back changes, and measure the impact of changes.
- **Permissions alignment.** The assistant should respect role-based permissions and approval workflows.
- **Reporting inclusion.** AI-driven changes should appear in standard reporting and analytics.


### Internally, create, communicate, and enforce governance policies for AI assistants that:


- **Involve security and risk teams early.** Commerce technology includes sensitive data across customers and finance. Although new functionality can be exciting — and sometimes at no extra cost — internal teams need to approve the new systems before they’re put into practice. Every software company is now a genAI company.[Organizations are responsible](https://www.forrester.com/report/introducing-forresters-aegis-framework-agentic-ai-enterprise-guardrails-for-information-security/RES185394) for protecting their data per applicable laws, even when tech partners house and manage the data.


- **Establish governance *before* adoption scales.** Define allowed and restricted use cases, validation requirements, data policies, and ownership across business, digital, IT, legal, and operations teams. Internal governance requirements must cover[regulations](https://www.forrester.com/report/navigate-the-patchwork-of-us-ai-regulations/RES180500) and extend to organizational standards.
- **Require visibility before adoption.** Ensure that anything a user creates or modifies through chat is visible, auditable, permissioned, versioned, and comparable to manually created work.
- **Test outputs before they affect customers or operations.** Use sandbox environments, approval checkpoints, and[careful testing](https://www.forrester.com/report/forresters-agentic-software-development-model/RES175620) for changes that impact revenue, experience, inventory, pricing, fulfillment, or compliance.
- **Train users to be critical.** Require users to question assumptions, request alternatives, and evaluate risks rather than defaulting to acceptance. “The bot suggested it” is not an acceptable explanation for a decision. Seek[explainability](https://www.forrester.com/report/in-a-world-obscured-by-cloudy-models-explainable-ai-shines-bright/RES185726) and justification as users decide how to use AI. Train users to anchor prompts in business goals and to identify weak assumptions, alternative approaches, and conditions where recommendations would fail.
- **Measure quality alongside adoption.** Track usage patterns (by asking employees and vendors, if reporting is unavailable), acceptance rates, and performance outcomes of AI-generated work. Watch for the “cool factor” to wear off if AI tools don’t add value beyond a natural language interface.
- **Protect strategic differentiation.** Keep final decisions grounded in your end customers, brand, products, margins, and operations. Discard recommendations that lack business-specific context that aligns with goals. Carefully review AI-suggested recommendations that could apply equally across industries or business models. Ensure that users scrutinize and know how to question recommendations, rather than accepting them easily.


In a new vendor selection process, evaluate whether AI assistants enhance existing workflows and efficiencies or whether they compensate for functional gaps in the UI. Prioritize solutions that continue to deliver functionality designed for humans while providing work-enhancing AI tools. For existing vendors, watch for gap-patching and push them to create a visible audit trail. Put your own governance in place, including business user training, to ensure that the value of these tools outweighs the risks.


To discuss how AI tools impact — or can impact — your users’ commerce tech work,[book a guidance session](https://www.forrester.com/inquiry) with me.


Thank you to my colleagues for their support for this blog post:[Lily Varon](https://www.forrester.com/analyst-bio/lily-varon/BIO2656) ,[Alla Valente](https://www.forrester.com/analyst-bio/alla-valente/BIO14124) ,[Rowan Curran](https://www.forrester.com/analyst-bio/rowan-curran/BIO4966) ,[Chuck Gahun](https://www.forrester.com/analyst-bio/chuck-gahun/BIO20107) , and[Joseph Schiavone](https://www.forrester.com/analyst-bio/joseph-schiavone/BIO193890) .


Share


-
-
