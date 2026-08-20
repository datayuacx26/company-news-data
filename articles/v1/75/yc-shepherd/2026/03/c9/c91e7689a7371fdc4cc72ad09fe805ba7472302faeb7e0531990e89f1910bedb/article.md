---
schema_version: "1.0.0"
document_id: "c91e7689a7371fdc4cc72ad09fe805ba7472302faeb7e0531990e89f1910bedb"
company_key: "yc-shepherd"
company: "Shepherd"
source_id: "yc-shepherd-news-import-bb872756570a"
canonical_url: "https://shepherdinsurance.com/blog/nova-shepherds-ai-engine"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-24T01:19:36.558879+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:15f43d234cacb6ab76be8676397c7b89b4fdccd23c059dc9526d9ada2b2d8c4e"
---

# Nova, Shepherd's AI Engine

In insurance, critical data flows via outdated and time-sensitive formats like chat, email, PDFs, and Excel. Key information is buried in hundred-page guidelines, legal documents, minor revisions, and ephemeral clarifications. The resulting complexity fosters the kind of tribal knowledge and narrow specialization that drags on business growth and operations.


At Shepherd, we are committed to leveraging technology to make risk frictionless. Our[bespoke underwriting platform](https://www.shepherdinsurance.com/blog/building-the-bloomberg-terminal-for-commercial-insurance) has been an incredible multiplier in our ability to scale. However, traditional engineering alone was insufficient to meet our rapidly expanding informational needs as we set our sights on new markets. Building the level of insight and record-keeping we needed using traditional engineering practices was impractical. Until, that is, the introduction of AI.


##### The Hypothesis


Recent advancements in AI-powered assistants (ChatGPT-style chatbots with specialized domain knowledge) have shown to broaden a user’s contextual awareness and domain fluency. Our team hypothesized that such an AI engine powered by a frontier large language model (LLM) and tuned specifically to Shepherd’s business could multiply our underwriting operations. It would be integrated into our platform’s UI and granted access to platform data and process documents. Its users would expand their scope, deepen their expertise, and boost their confidence, effectively increasing underwriting consistency and reducing operational friction.


Q4 of 2025 presented the perfect opportunity to test this hypothesis: frontier LLMs were quickly improving, retrieval-augmented generation (RAG) for data retrieval was maturing, and a new AI tool use standard was emerging. A small engineering team could rapidly prototype a solution, then iterate on it both as the underlying technologies developed and through user feedback. The proverbial stars aligned, and we set out to build an AI-powered underwriting system we dubbed **Nova** .


##### Building the Prototype


With an ambitious one-month deadline for an alpha prototype, our three-person engineering team focused on speed and iteration, and a loosely-defined goal of adoption. We gathered a handful of past underwriting questions from our Slack channels and identified which data sources would help answer them. These would form our eval set and guide our agent's capabilities. We also agreed that the system would be most effective if it were embedded into the platform, as opposed to a third-party UI like Claude Desktop.


To build this, our team relied heavily on[an existing internal RAG pipeline](https://www.shepherdinsurance.com/blog/your-evals-dont-matter) , open-source libraries, and third-party services, innovating only when essential. We scoped the system around five core features to answer user queries:


- ‍ **A tailored system prompt:** An AI system prompt crafted specifically for underwriters, including business context, common terminology, and workflow instructions. **‍** ‍
- ‍ **Platform database access:** A "tool" enabling the system to query the structure and content of Shepherd’s database. **‍**
- **Guideline search via RAG** : Allowing the system to read underwriting guidelines and answer operational questions. **‍**
- **Submission document search via RAG** : Enabling the system to query submission documents, regardless of format (PDF, Doc, Excel). **‍**
- **Web access** : For answering questions that require external information.


Since Nova was integrated into the platform's UI, we wanted it to “see” what the underwriter was looking at when they prompted it. To give Nova vision, we the ID of the current web page’s policy, quote, or account along with the user’s query. This ID is then used to enhance the context of the prompt sent to the LLM with data from the platform’s database. Nova could then answer questions about "this" policy, “this” quote, or “this” account.


We also rolled out a custom evaluation framework based on the initially-collected underwriting questions. Each question has a pre-written answer, and an LLM call evaluates whether Nova’s response to each question matches its expected answer. This framework created guardrails to ensure that any change to Nova would not degrade the system's performance or quality.


Finally, we wired up observability and analytics to provide insight into every user interaction. These metrics continue to inform the team on where to invest to refine the current Nova implementation.


By the end of the first month, we felt ready to unveil our work.


##### The Initial Setback and Pivot


The initial demo to our alpha users, and their subsequent testing, failed to elicit enthusiasm. Constructive feedback coalesced around three main issues:


1. ‍ **Lackluster user experience:** Users felt the responses from Nova came in too slowly and were too technical. **‍**
2. **Limited domain knowledge:** Nova occasionally failed to understand questions because it lacked an understanding of underwriting terms or our internal processes. **‍**
3. **User uncertainty:** Some users were unsure of what to ask or asked for things outside of Nova’s capabilities.


In the weeks following the unveiling, our team invested in solving these issues:


1. Improved user experience: To improve *perceived* performance, we introduced UI to indicate when Nova was "thinking." We also improved actual performance by reducing tool calls (notoriously slow) through *context-stuffing* the system prompt (injecting into it frequently used database schemas and contextual information, when appropriate). Nova’s response language was also improved with an updated system prompt.
2. Domain knowledge: This was solved by expanding the system prompt to include common underwriting acronyms, process information, and instructions on where to find information to answer user queries.
3. User adoption: We initially attempted to solve the adoption issue with canned questions, but they were not particularly useful. Instead, we addressed adoption friction through education, engaging with our users, updates, and regular listening sessions.


##### Success


By the end of Q4, a growing number of alpha users had integrated Nova into their daily workflow. After further polish, Nova moved from alpha to beta, generating increasing internal excitement across the company. Today, Nova is used not just by underwriters and CX, but by engineers, actuaries, and leadership, bridging the gap between the platform's user-facing capabilities and the company's vast operational data.


In the last four months, approximately 80% of Shepherd employees used Nova to ask 2,400 questions across 1,000 individual sessions. Slack-channel underwriting questions have dropped to near zero and people now use those same channels to share helpful Nova prompts and responses.


What about the original hypothesis that an AI engine would reduce underwriting friction? That metric remains elusive. Only time will tell how well Shepherd is able to scale its operations. For now, we’ll continue to lean on adoption and feedback to grow and refine Nova’s utility.


##### Lessons Learned


Throughout the process of planning, prototyping, shipping, and iterating on Nova, our team learned many invaluable lessons:


- **Trust is paramount** : we learned that to drive Nova’s adoption, we needed to build trust, and to build trust, we needed to ship a polished system (even at the prototype stage). Nova had to be usable, fast, and relatively accurate for users to continue leaning on it in their day-to-day work.
- **Evals allowed us to move quickly and confidently** : early on, we collected over a dozen real-world questions the underwriters asked one another and used those as our eval set. These questions allowed our team to maintain a level of performance and and accuracy with each iteration of Nova
- **Long tail adoption** : Nova’s adoption follows a long-tail curve. We see a few power users actively engaging with the tool, while the majority of users use the tool occasionally or lean on prior experience
- **AI impact metrics are elusive** : in the early days, we tried everything to measure the impact of Nova. In the end, we settled on telemetrics (e.g. questions/person), calculating “disliked” responses over all responses, and reading individual conversations to glean insight into the types of questions being asked.


##### Road Ahead


As we look forward to the future, we believe that Nova can become both a platform to supplement our UX/UI and a proactive partner in our underwriting and CX work. Over the last few months, we’ve been exploring ways to give Nova a heartbeat: a way to have it take initiative and action, or highlight opportunities without being prompted. In doing so, we hope to supplement deterministic rules and controls, and to multiply the experience of our underwriters. Nova will unlock a future where Shepherd continues to deliver consistent decision-making, underwriting quality, and higher accuracy across every stage of the underwriting process as we scale our operations.


[Join us and shape the future of AI-powered insurance](https://www.shepherdinsurance.com/careers)


‍
