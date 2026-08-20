---
schema_version: "1.0.0"
document_id: "cb5b0dcd828595e29075e5fee92fd8ce7eb9c9b8287b58c6f2ac6c8249b61356"
company_key: "aurora-innovation-inc-class-a-common-stock"
company: "Aurora Innovation Inc."
source_id: "aurora-innovation-inc-class-a-common-stock-rss-b929390c0bbe"
canonical_url: "https://aurora.tech/newsroom/auroras-approach-to-cybersecurity-for-autonomous-trucking"
published_at: "2025-08-19T21:22:21+00:00"
first_seen_at: "2026-07-25T01:09:45.755868+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:6859d1d054c31536f420cb022879c25b373896763cd417b450451341508a4318"
---

# Aurora's Approach to Cybersecurity for Autonomous Trucking

Effective cybersecurity is paramount for building trust in autonomous technology. As Aurora's self-driving trucks haul freight across Texas, we're not just moving goods — we're committed to the security of the American supply chain. We recently sat down with Aurora's Chief Information Security Officer, Brett Wahlin, to discuss how we protect our technology and customers from malicious actors.


***Can you share your perspective on the cybersecurity threat landscape and how Aurora protects its vehicles, technology, and customers?***


To protect our autonomous vehicles from the wide range of security threats, we apply diligence throughout their design, development and operation. The safety and security of these vehicles are inextricably linked. Both on-board components like the sensors and compute, as well as off-board components like cloud, data, and monitoring all present potential targets for bad actors — which is why Aurora approaches security from both a product and a process perspective, providing defense in depth through layered controls.


As part of this holistic approach to protecting our technology, we rigorously implement[Zero Trust Architecture](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf) (ZTA), a security model that ensures every user and component is continuously verified before gaining access.


We also use cryptographic attestation to verify the authenticity and integrity of all hardware and software components to prevent unauthorized code or tampering. Additionally, we have deployed custom intrusion detection capabilities to monitor and alert in the event of cyber anomalies.


Our security approach draws from guidance from the National Institute of Standards and Technology (NIST), National Highway Traffic Safety Administration (NHTSA), and industry groups. Learnings from other safety-critical industries have prompted us to deeply integrate security into our engineering itself – helping prevent compromises to vehicle operation, data integrity, and privacy before a single vehicle hits the road.


> There’s the technical side of security and then there’s the people and processes. Both are essential to protecting your product and customers.


Brett Wahlin


Aurora's Chief Information Security Officer


***How does in-house expertise contribute to this approach?***


There’s the technical side of security and then there’s the people and processes. Both are essential to protecting your product and customers. Aurora has an incredibly experienced team that maintains the security health of our product ecosystem, both in terms of cybersecurity experts, but also operations personnel trained to detect and handle threats.


We have developed processes around managing privileged access, including having strict control over which individuals can launch our autonomous trucks in driverless mode. We also empower our fleet managers, remote assistance specialists, and vehicle operators to serve as an important line of defense in mitigating security threats. They’re trained to detect, annotate, and diagnose any irregularities they see in the system or in vehicle performance. When necessary, they can escalate potential threats and initiate an incident response process.


***What is to prevent someone from taking over the truck or using remote assistance for criminal purposes?***


The first step is minimizing attack opportunities for bad actors. So, for example, the Aurora Driver’s designed to *not* allow remote control of steering, braking, or accelerator functionality. That means that neither Aurora’s team members nor external actors can remote control the vehicle when it’s on the road. Instead, the Aurora Driver is designed to accept *suggestions* from remote assistance specialists.


Even if a bad actor was in our command center, the Aurora Driver will not take remote assistance recommendations if it is not safe to do so. Minimizing attack opportunities makes it easier for Aurora’s security team to track and manage potential weaknesses in the system.


***The rise of sophisticated cyberattacks, data breaches, and AI-driven threats has made it so many traditional security models aren’t aware when they’re being attacked. How is Aurora to know when a cyberattack is underway or when it is under threat?***


Detection and response to incidents are critical functions for an autonomous vehicle. We have integrated cybersecurity detection systems directly into the fault handling processes of the Aurora Driver. If a threat detection warrants a response, the Aurora Driver’s Fault Management System will be triggered, in which case the vehicle will safely pull over to the side of the road.


So we’re confident in our approach to identifying potential threats, gaps, and risks posed to the Aurora Driver — so we can continue to innovate and serve customers, while safeguarding our technology and stakeholders.


***As Aurora expands its fleet, how are you thinking about maintaining this high level of cybersecurity?***


Over the coming years, Aurora’s fleet will grow by orders of magnitude — and our security program must be ready against all manner of threats. Our Zero Trust Architecture requires us to continuously authenticate every user, every component, and every system, which maintains an ecosystem of safety and security as we work with external customers and partners. As part of this work, our release process for software updates is built around continuous validation of threat models and mitigating controls.


It won't be long before we transition from just a few trucks on the road to tens, hundreds, and thousands. Throughout this process, we are committed to uphold the highest standard of cybersecurity principles, constantly surveying for potential threats, and collaborating with government agencies and security experts to keep our technology safe and secure.


As we continue hauling loads for customers on public roads, we invite you to learn more about our[approach to safety](https://ir.aurora.tech/news-events/press-releases/detail/117/aurora-releases-safety-report-in-preparation-for-driverless-trucking) , cybersecurity and how we’re shaping the future of autonomous freight.
