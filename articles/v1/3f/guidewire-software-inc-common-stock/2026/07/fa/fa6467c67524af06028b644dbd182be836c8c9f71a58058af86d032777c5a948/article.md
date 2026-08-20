---
schema_version: "1.0.0"
document_id: "fa6467c67524af06028b644dbd182be836c8c9f71a58058af86d032777c5a948"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/intelligent-experience/from-search-to-flying-car-the-story-behind-the-guidewire-documentation-assistant"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T23:54:12.743685+00:00"
fetched_at: "2026-07-31T23:54:22.338368+00:00"
content_hash: "sha256:f5e0329a1891c9f0f17bdbd74da75cbb9dd8767724971d9a7860deb941669f22"
---

# From Search to Flying Car: The Story Behind the Guidewire Documentation Assistant

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Intelligent Experience](https://www.guidewire.com/resources/blog/intelligent-experience)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)


- From Search to Flying Car: The Story Behind the Guidewire Documentation Assistant


**Our journey began with a bold challenge I gave our Information Experience team** :
*“Build a flying car; don’t spend effort trying to make horses run faster.”*


For us, the "horse" was the traditional search functionality to find product documentation along a human-created navigation structure. The "flying car" was something far more ambitious: a truly conversational experience where users could simply ask, "Does product X do Y?" and get a direct answer.


We knew our users were spending quite a bit of time hunting for answers to their product questions. Complex questions could take up to five minutes to find pertinent information. That was five minutes too many. We needed a new way for people to find the product information they needed without digging through pages of documentation.


We focused on three core problems:


1. **Speed** : How fast can a user get a valuable answer to their product question?
2. **Trust** : The assistant must never invent unsupported guidance.
3. **Security** : It had to respect our existing permissions and system architecture.


The early results gave us confidence: after introducing the Documentation Assistant internally, the time for those complex questions dropped to **under a minute** . An overwhelming **90%** of surveyed users said finding information they needed felt “very easy.”


So how did we build this “Flying car”?


Under the hood, the Guidewire Documentation Assistant is a **Retrieval-Augmented Generation (RAG)** system. It's a specialist, not a generalist. It’s an LLM that *only* knows Guidewire product documentation.


- **Grounded in Docs** : It queries a semantic database of our documentation, finds the relevant chunks, and uses *only* those to construct an answer.
- **A "Responsible No"** : If a query is outside of Guidewire product information, it will explicitly say “I don’t know,” redirecting the user back to what answers it can help the user find. This was vital for building trust.
- **Access-Controlled** : The Documentation Assistant relies on our existing security backend, ensuring users only see answers based on documentation they are already authorized to access.


### The Journey to Conversation


We treated the “information finding” experience as our top priority throughout the project, which was critical to our content strategy and architecture. Key to teaching the assistant were the **“golden Q&A sets.”** We extracted common user intents from our search logs, created ideal answers, and used them as our automated accuracy test set.


We deliberately started small, running stress tests and user research with a **beta tester group** to observe how people framed their questions and when they fell back to traditional search. The feedback loop was constant, helping us refine everything from the retrieval parameters to the Documentation Assistant experience.


This work also forced a massive content cleanup. We standardized URLs, reduced redundant content, and integrated better with our taxonomy to handle Guidewire-specific terminology. This less-visible content and infrastructure work is the foundation that can correctly interpret and find the right information every time.


Today, the Documentation Assistant is a specialized RAG solution with strong early metrics. We’re backed by a systematic testing framework that lets us measure and improve accuracy over time. We look at questions without answers, we evaluate what was found and what was not found, we improve answers.


The “flying car” is now reliably off the ground. In the first 60 days since external launch, we helped over 5,000 customers, partners, and employees who rely on Guidewire documentation for their projects to find the 45,000 answers they needed, effortlessly.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
