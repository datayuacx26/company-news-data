---
schema_version: "1.0.0"
document_id: "ead5991b3d93108153dbc375bd2b80a3ea20c9a0fbca12352452cd06da8f36aa"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/fosdem-2024"
published_at: "2024-02-23T16:47:58.949+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:fadd1d5f59024f6bcdfbbb5120aeb35e2bc0f30c21a3ed230e8a6be9332ead9a"
---

# SpiceDB at FOSDEM 2024: Recap

Watch AuthZed's CPO and co-founder Jimmy Zelinskie's talk at FOSDEM 2024, as he delves into the world of authorization systems, specifically focusing on the evolution of access control models and the genesis of SpiceDB.


### Introduction


- The talk aims to discuss the broader context of authorization issues rather than just promoting SpiceDB.


### Authorization and Its Challenges


- Discussion on the evolution of web security threats, highlighting Broken Access Control's rise to the top of OWASP's threat list from 2017 to 2021.
- Overview of the historical context and development of authorization concepts by academia and industry.
- Introduction to various access control models: Discretionary Access Control (DAC), Mandatory Access Control (MAC), Role-based Access Control (RBAC), Attribute-based Access Control (ABAC), and Relationship-based Access Control (ReBAC).


### Evolution of Access Control Models


- Detailed explanation of DAC and MAC, their origins, and examples.
- RBAC's emergence in 1992, its core idea of mapping users to roles, and the challenges of defining roles consistently across different systems.
- ABAC was introduced in 2015, offering more dynamic and context-aware access control mechanisms.
- ReBAC's concept from around 2007, focusing on access control through relationships, popularized by systems like Facebook's social graph and Google's Zanzibar.


### The Impact of Zanzibar and SpiceDB's Origins


- Zanzibar's introduction by Google as a global, consistent authorization system, inspiring the creation of SpiceDB.
- SpiceDB's development story, from initial prototypes in Python to a mature system written in Go, inspired by Google's project and the novel Dune.


### SpiceDB Features and Capabilities


- SpiceDB as a parallel graph database optimized for authorization checks.
- Explanation of how developers use SpiceDB, including schema application, data storage, and querying.
- Description of SpiceDB's architecture, including its gRPC and HTTP APIs, Kubernetes-native design, and its ability to scale and maintain consistency globally.
- Introduction to developer tools like Zed and a web IDE for SpiceDB, enhancing developer experience and enabling easy integration and testing.


### Challenges and Extensions to Zanzibar


- While SpiceDB builds on Zanzibar's concepts, it extends them to be more flexible and applicable outside of Google's infrastructure.
- Additions include support for dynamic, context-based relationships and improvements in developer experience to encourage open-source community adoption.


On this page


- Introduction
- Authorization and Its Challenges
- Evolution of Access Control Models
- The Impact of Zanzibar and SpiceDB's Origins
- SpiceDB Features and Capabilities
- Challenges and Extensions to Zanzibar


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Jenessa Petersen · Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)


[Community AuthZed 2025 Year in Review Five years in, our mission remains the same, fixing access control. 2025 was about making our authorization infrastructure available to more teams in more ways. Dec 19, 2025 · 5 min](https://authzed.com/blog/authzed-2025-year-in-review)[Community AuthZed 2025 Year in Review Five years in, our mission remains the same, fixing access control. 2025 was about making our authorization infrastructure available to more teams in more ways. Sam Kim · Dec 19, 2025 · 5 min](https://authzed.com/blog/authzed-2025-year-in-review)
