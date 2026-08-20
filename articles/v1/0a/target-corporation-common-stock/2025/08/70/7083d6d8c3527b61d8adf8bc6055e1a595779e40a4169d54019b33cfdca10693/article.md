---
schema_version: "1.0.0"
document_id: "7083d6d8c3527b61d8adf8bc6055e1a595779e40a4169d54019b33cfdca10693"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/fraud-taxonomy"
published_at: "2025-08-04T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:6d9cb2b52e783003b9a679961400ba5b6f189377c5fcc6a08d27b0b913b392d7"
---

# A Structured Approach to Tackling Theft, Fraud, and Abuse

Organized retail crime continues to evolve at a rapid pace. Today’s fraud operations are more sophisticated, tech-enabled, and damaging than ever before. To fight back, retailers need more than isolated controls. They need shared knowledge, outside partnerships and a common vocabulary.


That’s where the Fraud Taxonomy comes in. Built in collaboration with the National Retail Federation (NRF), RH-ISAC, Target, and a coalition of retailers and industry experts, the taxonomy represents a foundational tool for understanding and mitigating the growing threats of theft, fraud, and abuse.


Building a living knowledge base


Criminals can often quickly scale their operations to cause significant damage to your organization, consumers, and public trust if fraud goes unnoticed or unmitigated. It is critical to have a body of knowledge and framework to draw from and foresee these problems to build effective controls.


The NRF Fraud Taxonomy is a living knowledge base of theft, fraud, and abuse schemes taken from real-world examples. Practitioners with years of experience in fighting organized retail crime created this taxonomy. It enables retailers to:


- Develop policies and processes


- Justify business cases for investment


- Share information


- Evaluate vendors and solutions


How the taxonomy works


Many of the concepts in the Taxonomy were inspired by what has been successful in the cybersecurity domain. The Taxonomy refers to


schemes


as a corollary to the Cyber Kill Chain, a concept proposed in cybersecurity by researchers at


[Lockheed Martin](https://protect.checkpoint.com/v2/___https:/www.lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf___.YzJ1OnRoZWNoZXJ0b2ZmZ3JvdXA6YzpvOmQxYWJkZmFhNjk0MjhjMmYwYjY0NzQyY2ViZTA1NDcxOjY6ODllMjpmYmYzN2Y2MzQzMTQyY2ZjYzE3Mjg5Y2E1NDM0ZjZkOTcwMWE5MDFmY2M5YjA1ZjA2NmQxNDY1YWI2OGExMWJiOnA6RjpG) in 2011. It describes phases (links in “the chain”) of intrusions, mapping adversary techniques at each stage to the defender’s courses of action. There have been several variations created since the concept was proposed. The most notable and by far the widest adopted is


[MITRE ATT&CK](https://protect.checkpoint.com/v2/___https:/attack.mitre.org/___.YzJ1OnRoZWNoZXJ0b2ZmZ3JvdXA6YzpvOmQxYWJkZmFhNjk0MjhjMmYwYjY0NzQyY2ViZTA1NDcxOjY6OWRiNTozYzdhOTJmYWY5MDkxN2RlMzY4YjkyMjU1MzQ5ZmI0ZmZhMzI4MzczOTllMzZlM2NmNjNhN2VlYTQ3OTE4MmIwOnA6RjpG) , which introduced a growing encyclopedia of detailed techniques leveraged by cyber threat actors to accomplish their objectives in the context of a Cyber Kill Chain.


The NRF Fraud Taxonomy refines these threat-informed defense concepts for use in the retail domain. It breaks down complex retail crime into:


- Schemes: The high-level objective of the fraudster.


- Tactics: The stepwise goals the criminal is aiming to achieve in a scheme.


- Techniques: The methods an actor uses to accomplish each tactic.


Let’s run through an example of an account takeover scheme. Criminal operations can become extremely specialized to support a larger fraud scheme. One group might focus on initially logging into a compromised customer account. In this case, the tactic, or stepwise objective of the theft actor is gaining


Initial Access


.


The way the actor accomplishes Initial Access might be through


Credential Stuffing


, which would be the specific technique used by the actor. Before conducting


Credential Stuffing


, they will likely need to develop and test out automation with some initial


Reconnaissance


activity.


Specific to retail crime and the last step in the chain, the actors working to monetize fraud may use a technique to


Checkout


and a sub-technique to use in-store


Point of Sale


leveraging compromised digital accounts scanned at the register. Finally, a fencing operation might specialize in selling ill-acquired goods without asking questions through an


Unwitting Buyer


.


By providing this level of detail, defenders are better equipped to form their strategy of how they will prevent, detect, and mitigate these threats to their business and customers.


Exploring the web application


We’ve built this expert-driven knowledge base as a React web application so anyone in the retail industry can easily visualize and use the information. The project uses Vite as a build tool and the React web framework. The current version of the taxonomy is available


[here](https://protect.checkpoint.com/v2/___https:/target.github.io/retail-fraud-taxonomy-viewer___.YzJ1OnRoZWNoZXJ0b2ZmZ3JvdXA6YzpvOmQxYWJkZmFhNjk0MjhjMmYwYjY0NzQyY2ViZTA1NDcxOjY6MmE3NTplM2I0NTg1NDI3N2I3YmZkZmJhMjZkZGVlYjY4ZjgxNThlNzgzODM2NjU0NDY0Yjk2MjRiYWFhODg5Zjk3NTFkOnA6RjpG) . All source code has been released on Target’s GitHub repository


[target/retail-fraud-taxonomy-viewer](https://github.com/target/retail-fraud-taxonomy-viewer) , along with simple installation instructions if you would like to run an instance within your organization’s environment.


When first viewing the Fraud Taxonomy via web browser, the user is presented with all the techniques available. We expect these techniques to grow in number over time as more schemes and contributions are made by other retailers, NRF, and industry partners.


Clicking on the “Filter By” button opens a panel where one can filter by


Scheme


and


Mitigation


. Selecting a specific Scheme will narrow the focus to the subset of techniques that apply. Similarly, selecting a specific mitigation shows how impactful a given mitigation is across multiple techniques to help users understand the impact a given control implementation would have.


Clicking on a given technique, like


Proxy Abuse


, will show a definition from the attacker’s perspective and provide mitigations, detection opportunities, and references to other resources.


Putting the taxonomy to use


The Fraud Taxonomy is intended support several use cases, including:


Developing Business Processes and Policies:


The mitigation section of each technique provides actionable guidance and ideas that can be used to establish or validate internal controls. These can be used to shape customer-facing policies, influence how a services team responds to customers, or what fraud controls an engineering team might need to consider in their implementation of an application or service.


Business Case Justification:


because the NRF Fraud Taxonomy is written as best practices from many retailers and practitioners, we can map a given organization’s controls and mitigations to a scheme and find gaps. Those gaps might require resources to close them. This Taxonomy can help as justification for resource allocation in support of that business case.


Information Sharing:


This body of work establishes a common vocabulary for retail fraud and abuse problems, which is foundational work required to share information between organizations.


Testing and Vendor Evaluation:


The techniques described can be used to help guide the creation of a custom test or evaluation plan for a given fraud scheme. With the future addition of assigning scores and colors to techniques, it will allow retailers to assess and visualize how much benefit a specific vendor or in-house solution provides against the overall fraud scheme.


The taxonomy will grow and evolve with the retail industry’s needs. Future plans include the ability to customize techniques to your organization, more advanced assessment and scoring features, and expanding the types of schemes and techniques, such as returns and refund abuse.


Foundation for what’s next


While in its early stages, the Fraud Taxonomy represents a significant advancement in the fight against retail theft, fraud, and abuse. By providing a structured, detailed, and collaborative framework, it empowers retailers to better understand and combat sophisticated criminal activities.


The taxonomy's real-world applicability, inspired by successful cybersecurity models, ensures that businesses can proactively develop strategies to mitigate and detect threats. As the taxonomy evolves and expands, it will continue to serve as an invaluable resource for retailers, fostering a unified approach to safeguarding their operations, customers, and public trust.


Special thanks to the NRF, Chertoff Group, RH-ISAC, retail partners, and multiple team members at Target for helping make this project come to life for the benefit of the retail industry.


We plan to further update this taxonomy over the coming year and develop new tools to support its use and adoption. If you are interested in participating in these activities, please reach out to NRF at


cdri@nrf.com .


## RELATED POSTS


### Applying Cyber Principles to Combatting Organized Retail Crime


By Jodie Kautt, August 29, 2023


How Target's cyber team collaborates to combat fraud


### Implementing TLSH Based Detection to Identify Malware Variants


By Ryan Borre and Paul Hutelmyer, December 3, 2024


TLSH is a fuzzy hashing algorithm that focuses on identifying similarities between files.
