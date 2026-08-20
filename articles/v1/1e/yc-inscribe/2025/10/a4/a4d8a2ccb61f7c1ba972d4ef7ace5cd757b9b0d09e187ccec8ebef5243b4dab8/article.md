---
schema_version: "1.0.0"
document_id: "a4d8a2ccb61f7c1ba972d4ef7ace5cd757b9b0d09e187ccec8ebef5243b4dab8"
company_key: "yc-inscribe"
company: "Inscribe"
source_id: "yc-inscribe-news-import-71d84a865bd8"
canonical_url: "https://www.inscribe.ai/blog/from-forgeries-to-deepfakes-document-fraud-in-the-age-of-generative-ai"
published_at: "2025-10-06T00:00:00+00:00"
first_seen_at: "2026-07-22T00:02:47.549015+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:655b0738730576c7cb11c0a88b871ef08e03f548e10f80f639064e8210fd4113"
---

# From forgeries to deepfakes: Document fraud in the age of generative AI

Document fraud has quietly become one of the most complex and manually intensive forms of financial crime.


We see it every day: forged paystubs, fake bank statements, synthetic utility bills, and more all designed to mislead underwriting and compliance systems.


In the last few months, especially, generative AI has redefined what's possible. But it's also redefined what's risky. Among the many sectors disrupted by the rise of deepfake media, document-based financial workflows are among the most vulnerable. We’re entering an era where it’s no longer a question of whether a document looks real — but whether it can prove it’s real.


Sam Altman, CEO of OpenAI, recently[warned the Federal Reserve of an impending "fraud crisis."](https://www.cnn.com/2025/07/22/tech/openai-sam-altman-fraud-crisis) His words were blunt: "AI has fully defeated most of the ways that people authenticate currently, other than passwords."


With modern AI tools, replicating a voice (or soon, a face) is trivial. The same applies to documents.


### Document deepfakes are here


Receipts, paystubs, utility bills, and bank statements can now be spun up in seconds using open-source image generation models, or created by a single prompt using multimodal LLMs.


At first glance, these fakes look legitimate. They contain realistic lighting, crinkled paper textures, shadows, and logos. But a closer inspection reveals subtle inconsistencies — mismatched number formats, phantom store locations, odd typos. Errors that are easy to overlook and increasingly rare as these models improve.


What makes this threat especially dangerous is the low cost and high scalability. Fraud-as-a-service marketplaces are proliferating, offering thousands of high-quality forgeries for under $10 each. Meanwhile, financial institutions are still verifying documents with systems built for a different era, one where fraud was rare, not embedded in the volume.


At Inscribe, we’ve studied these models closely. We’ve reverse-engineered their quirks and trained our systems to catch the digital fingerprints they leave behind. But the pace of generative AI means that traditional detection methods are already outdated. This isn’t a future problem—it’s live, growing, and impacting businesses today.


### Founding insight: why we started Inscribe


This challenge isn’t new to us. My brother Conor and I founded Inscribe in 2017 after experiencing the broken state of document verification firsthand. Conor, working at a major bank in Ireland, saw how digitization efforts constantly failed at one critical point: authenticating documents without overwhelming fraud and risk teams. He and his colleagues were drowning in manual reviews, with no scalable way to separate genuine applications from fraudulent ones.


At the same time, I felt the customer-side pain: delays and frustration when applying for financial products that should have been instant. We realized that the core problem wasn’t just inefficiency, but rather the growing sophistication of fraud itself. To fix it, we would need to build systems that could reason like the best fraud analysts, but with the speed and scale only AI could provide.


From day one, we approached document fraud by going back to first principles. Instead of relying on static rules, we built specialized detectors to uncover tampering, template reuse, and synthetic identity patterns. We invested in foundational infrastructure to parse complex financial documents, extract subtle fraud signals, and flag inconsistencies with confidence. That early work enabled our customers to catch millions in fraud that would have otherwise slipped through undetected.


But we also saw the limits. Even with advanced detectors, investigations were still painfully manual. Analysts spent hours toggling across systems, validating employers, matching addresses, and stitching together context. Detection alone wasn’t enough. The system needed to explain itself, to reason, and to adapt as fraud evolved.


### Our journey from linear systems to agentic systems


Most fraud tools today are linear. They classify, run a detector, and spit out a score. That works for common fraud patterns, but breaks down when tactics shift or when fraud shows up in unexpected forms.


Agentic systems are different. Instead of running a fixed playbook, they create a plan, choose the right tools, and adapt in real time as new evidence emerges. An agent can combine document forensics, database queries, web search, and external APIs into a single investigation — validating results along the way and explaining how it reached its conclusion.


This shift also breaks the 1-to-1 model of fraud detection. Traditional systems rely on one detector for one fraud type. But with agentic systems, a single AI Agent can orchestrate many tools at once, tackling multiple fraud types in parallel. That means catching fraud previously too rare or too complex to justify building a dedicated detector for — and even catching the first instances of entirely new fraud patterns.


The impact is profound: agentic systems don’t just automate workflows; they expand the boundaries of what’s possible. They allow fraud teams to defend against the long tail of fraud, reduce false negatives, and move from reactive detection to proactive defense.


That realization led us into large language models. In 2022, we began experimenting with LLMs to enhance document parsing and context understanding. By 2023, we had integrated them into workflows to generate clearer, audit-ready explanations of risk signals. But the real breakthrough came in 2024, when we moved beyond using LLMs as helpers and started building **agentic systems** .


Here’s how the evolution unfolded:


- **Detectors (2017–2021):** One detector for one fraud type — effective for common fraud, but narrow in scope.
- **LLMs (2022–2023):** Better understanding of documents, more contextual explanations, but still limited by linear workflows.
- **Agentic Systems (2024+):** LLMs orchestrating multiple tools, creating dynamic investigation plans, validating results, and adapting mid-flight.


With this shift, we broke the 1-to-1 model of fraud detection. Instead of one detector for one type of fraud, our agents can run many tools in parallel, catching fraud that was previously uneconomical or technically impossible to stop — including the very first instances of new fraud types.


### **Why it matters right now**


Deepfake fraud is no longer rare. It’s routine.


In today’s lending and onboarding pipelines, fraudulent documents show up regularly, often buried among thousands of legitimate ones. As attackers scale their tactics using generative tools, defenders can’t rely on filters alone. They need agents that investigate.


Financial institutions face a dual challenge: increase efficiency while preserving trust. But the systems many teams still use were built for a different era — one where fraud was infrequent and somewhat predictable. That’s no longer the case.


The opportunity in front of us is this:


- **Catch fraud that wasn’t previously worth catching** — because it was rare or too manual to justify
- **Catch fraud we’ve never seen before** — because it doesn’t resemble anything in the training data


And do it all without increasing operational burden.


The key to closing the fraud reaction window — that critical gap between attack and response — is systems that can reason, adapt, and act.


### Our progress and what’s next


Since launching our AI Agents in late 2024, we’ve seen:


- **$3M+ in fraud prevented** by Logix Federal Credit Union in just eight months
- **Manual research time cut from 30 minutes to under 90 seconds**
- **Detection of AI-generated forgeries** , including GenAI paystubs and synthetic bank statements


But this is just the beginning. Over the next few years, our vision is to transform fraud detection from static, rule-based systems into adaptive, agentic workflows that can reason across documents, tools, and data sources to uncover fraud previously undetectable.


### A call to action for financial services


The fraudsters are already using AI. To stay ahead, financial institutions need systems that don’t just detect fraud, but adapt to it.


That’s why at Inscribe, our mission is to help risk teams catch fraud with confidence, and our vision is AI for the modern fraud fighter. We believe the future isn’t rules-based — it’s agentic. And that future is already here.


If you’re defending against document fraud, it’s time to move beyond static tools. AI Agents are not just the next step. They’re the foundation for modern, adaptive fraud defense — and they’re ready to work alongside your team today.


If you’re a bank, lender, or fintech interested in exploring how you can too, I’d love to chat.[Grab time on my calendar](https://calendar.google.com/calendar/u/0/appointments/schedules/AcZssZ0ocF7r_QR5zpNv1Tc6EYdOaDcw8ZFOZdhA2fv30xu_aFzOG7C6QGv6a_micjWHfStYRyg_T0k-) or[reach out to me on LinkedIn](https://www.linkedin.com/in/rnnbrk/) .


‍
