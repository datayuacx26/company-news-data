---
schema_version: "1.0.0"
document_id: "ac8fce48ac8b7c174f171fb78ca93e8e0123df106dd1375f9347edb67c252d38"
company_key: "yc-solve-intelligence"
company: "Solve Intelligence"
source_id: "yc-solve-intelligence-news-import-a6d4afe4e85a"
canonical_url: "https://www.solveintelligence.com/blog/post/the-honest-risks-of-using-ai-in-patent-prosecution"
published_at: "2026-08-05T16:51:28.568+00:00"
first_seen_at: "2026-08-06T02:52:35.662917+00:00"
fetched_at: "2026-08-06T02:52:37.764839+00:00"
content_hash: "sha256:b10eaf669d743297b1f90fe694886a809c550a0303209ed099f36913d8afc454"
---

# 5 Risks of Using AI in Patent Prosecution

Most writing about AI in patent prosecution sells the upside: faster office-action responses, claim charts in minutes, fewer hours lost to mechanical work. That upside is real, and here at Solve Intelligence we build toward it every day. The downside deserves equal honesty, because prosecution is an ex parte, on-the-record process. Statements made to the examiner become part of the public file wrapper and can be used against the patent for its entire life. Consequently, AI output here deserves more care than it would in lower-stakes work.


Solve Intelligence builds patent-specific AI used by[700+ IP teams](https://www.solveintelligence.com/#customers) , so we see these risks up close. None are imaginary, but all become manageable when the tool is built and used correctly. Below are the five recurring risks, together with concrete steps on how to reduce these risks.


## **Risk 1: Hallucinations and the duty to verify**


Large language models can produce content that looks authoritative but isn’t. This can include citations that don't exist, quotations not found in the case cited, or summaries that subtly misstate what a reference teaches. Courts have sanctioned attorneys who filed AI-generated citations without adequately checking them. The same verification risk applies to office-action responses and PTAB briefs.


### **USPTO guidance on AI-generated content**


The[USPTO’s April 2024 guidance](https://www.federalregister.gov/documents/2024/04/11/2024-07629/guidance-on-use-of-artificial-intelligence-based-tools-in-practice-before-the-united-states-patent) gives AI-generated content no exemption and requires that “if an AI system is used to draft a portion of a response to an examiner Office action, the party should review the response, including checking the accuracy of the citations and ensuring the arguments are legally warranted”.


### **European guidance: EPO and epi**


Europe takes the same line: the[EPO Guidelines](https://www.epo.org/en/legal/guidelines-epc/2026/foreword_5.html) state that using an AI tool to prepare a document does not absolve a representative of responsibility for its content, and the[epi Guidelines on generative AI](https://information.patentepi.org/issue-4-2024/epi-guidelines-use-of-generative-ai.html) remind European Patent Attorneys that AI is no excuse for errors or omissions. Wherever you practise, the duty stays with the person who signs.


### **How tooling makes verification practical**


This is why a human has to stay in the loop, and why AI output should never be treated as final. Purpose-built tooling makes that practical: Solve Intelligence ties every assertion back to a verifiable source, so the attorney can confirm each point in seconds rather than rechecking from scratch.


## **Risk 2: Confidentiality and client data**


To be useful in prosecution, an AI tool has to handle confidential material, including unpublished applications, draft amendments, and confidential client data. Many consumer chatbots retain inputs, train on them, or store them on infrastructure no one has vetted. The USPTO tells practitioners to be especially vigilant about client confidentiality and to understand a tool's terms of use, privacy policy, and security before feeding it anything. European practice sets the same expectation: the epi Guidelines require members to maintain adequate confidentiality in any AI tool they use.


### **The risks don’t stop at filing**


An application stays confidential until it publishes, usually around 18 months from its priority date, so entering an unpublished specification, a draft amendment, or sensitive client data about product or patent strategy into the wrong tool could breach confidentiality obligations.


**The safeguards are concrete** :


- Zero-data-retention agreements
- Per-customer isolation
- Encryption in transit and at rest
- Certifications such as SOC 2 Type II and ISO 27001


See our[full list of vendor-vetting questions](https://www.solveintelligence.com/blog/post/how-should-patent-practitioners-evaluate-ai-tools-for-data-security-and-confidentiality) if you want to check these claims against your own AI tool


## **Risk 3: Export control and foreign filing licenses**


Many AI tools route data through servers outside the United States, and some prosecution material is subject to US export controls. Under the[Export Administration Regulations](https://www.ecfr.gov/current/title-15/subtitle-B/chapter-VII/subchapter-C) , transmitting controlled technical data to a foreign server can count as an export that requires authorization, depending on the technology and destination. Much ordinary subject matter needs no license, but some does, and separate rules restrict exporting inventions that are unfiled or under a secrecy order.


The real difficulty is visibility: an ordinary chatbot gives no indication of where your data is processed, so you cannot tell whether a transfer is routine or a violation. In Europe the analogous concern is data protection, since the[GDPR limits](https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng) transferring confidential or personal data outside the EEA. Managing this means choosing a tool that lets you keep data in a specified jurisdiction and is open about its infrastructure.


## **Risk 4: Prosecution-history estoppel and claim-scope errors**


Arguments and amendments made to overcome a rejection become part of the file and can permanently surrender claim scope under prosecution-history estoppel. A quick, plausible AI argument distinguishing over a prior art document may characterize the invention more narrowly than necessary, or read later as a disclaimer the applicant never intended. Because it sounds reasonable, this error can survive review and only surface years later when the claim is asserted. No software removes the need for attorney judgment here.


However, good tooling should be designed to make it easier to surface these issues. In particular, purpose-built tools like Solve Intelligence are programmed to be aware of these patent-specific issues, providing sufficient reasoning when proposing arguments or amendments, and keeping the attorney in charge of what gets submitted to the Office.


## **Risk 5: Automation bias**


When a tool is fast and usually right, the temptation is to rubber-stamp it. The risk is less the tool than the habit it can encourage. It can lead even experienced attorneys to approve output with less scrutiny than the work deserves, especially when a tool hides its reasoning and makes it difficult to verify the sources.


The counterweight is tooling built to invite scrutiny, with exposed reasoning and paragraph-level citations to sources. This makes it easy for the attorney to use the AI as an extension and enhancement of their work rather than a replacement for it.


## **What separates risky AI use from defensible AI use**


Across all five risks, the danger is rarely AI in the abstract. It comes from the gap between a general-purpose tool used casually and a purpose-built tool used with care. A consumer chatbot will answer a patent question with no sourcing, no data guarantees, no visibility into where its servers sit, and no awareness of estoppel, leaving the practitioner to carry all of the resulting risk.


That is why patent prosecution work calls for[purpose-built patent AI](https://www.solveintelligence.com/blog/post/why-patent-attorneys-need-purpose-built-ai) .


## **How Solve Intelligence verifies citations and secures client data**


Solve Intelligence is built around exactly the above distinction. Our Patent Copilot™ and[prosecution](https://www.solveintelligence.com/#patent-prosecution) workflows tie every conclusion to a citation the attorney can verify, keep the underlying reasoning visible, and allow the practitioner to review and own every statement made to the Office.


On data, Solve Intelligence maintains zero-data-retention agreements across its LLM providers and isolates each customer's data. Everything is encrypted in transit (TLS 1.3) and at rest (AES-256), with data-residency control available. Solve Intelligence is also SOC 2 Type II and ISO 27001 certified, and GDPR and CCPA compliant, all documented in our[Trust Center](https://trust.solveintelligence.com/) .


None of this removes the practitioner's duty. What it does is lower the cost of doing the work carefully, so the checks these risks demand become routine rather than a burden. AI is not risk-free in patent prosecution. But the risks are specific, well understood, and largely addressable by choosing tools built specifically for the duties the work carries.


## **Frequently asked questions**


#### **Do I have to tell the USPTO that I used AI?**


There is no general, per se duty to disclose that an AI tool was used, unless the USPTO specifically requests it. You still must disclose information material to patentability, though, and you remain fully responsible under the duty of candor for the accuracy of everything you sign. See our summary of the[USPTO’s AI guidance](https://www.solveintelligence.com/blog/post/how-to-use-ai-in-patent-practice-uspto-guidance-and-compliance-tips) for compliance detail.


#### **Is it safe to put confidential or unpublished application material into an AI tool?**


Only with the right tool. Consumer chatbots may retain inputs, use them for training, or process them on servers abroad, which raises confidentiality and export-control concerns for material that is still non-public. A tool with zero data retention, per-customer isolation, and controllable data residency is built to handle sensitive prosecution material. A general consumer tool usually is not.


#### **Does using AI to draft arguments create estoppel risk?**


The risk comes from what is said to the examiner, not the tool that helped draft it. An AI-drafted argument or amendment can surrender more claim scope than necessary, or read later as a disclaimer. Review every draft for its effect on scope, and keep the decision about what to argue with the attorney.
