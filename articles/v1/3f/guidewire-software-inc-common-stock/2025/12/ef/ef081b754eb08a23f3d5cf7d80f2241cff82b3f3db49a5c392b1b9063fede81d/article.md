---
schema_version: "1.0.0"
document_id: "ef081b754eb08a23f3d5cf7d80f2241cff82b3f3db49a5c392b1b9063fede81d"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/introducing-guidewire-rules-service-in-olos"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:0a80b2e563a48695812384f842774272f6eeb02fadb2e19facd65914e4e6766a"
---

# Introducing Guidewire Rules Service in Olos

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
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- Introducing Guidewire Rules Service in Olos


Every P&C developer or business analyst knows the drill: A simple rule change that should take hours turns into weeks of release cycles. From an eligibility tweak to a referral-threshold adjustment, hard‑coded rules are the bottleneck of modern insurance. As a developer, you are required to keep the workflows agile to meet changing business needs.


## Guidewire Rules Service


Introducing **Guidewire Rules Service** , a purpose-built, fully-managed cloud native service on Guidewire Cloud Platform that provides centralized, low-code authoring and execution of business rules for Autopilot workflows in ClaimCenter and PolicyCenter. It enables you to decouple decision logic from core applications, ensuring faster changes and consistent outcomes.


Rules Service integrates natively with Autopilot Workflow Service, serving as its core decision support mechanism. This tight integration enables real-time rule execution within workflows while providing the flexibility to update decision logic independently. It serves as the single source of truth for all the Autopilot workflow decisions, providing enterprise-wide governance and ensuring consistent rule application across all insurance processes.


By leveraging the industry-standard Decision Model and Notation (DMN) framework, Rules Service provides a common, visual language for both business and IT teams. The externalization of decision logic accelerates speed-to-market and ensures consistency, allowing you to rapidly implement decision logic for workflows automated by Autopilot Workflow Service in ClaimCenter and PolicyCenter.


*Fig. Design rules using the industry-standard Decision Model and Notation (DMN)*


### Key Features


These are the key features of Rules Service that help replace hard-coded rules with API-first flexibility to improve workflows:


1. **Low-code, high-control designer** : The rules designer provides a low-code framework for authoring and reusing decisions and data nodes.
2. **Standards-based modeling (DMN/FEEL)** : The service uses Decision Model and Notation (DMN) to give business analysts and developers a shared language to author rules. You can model decisions using Decision Requirements Diagrams (DRDs), decision tables, and Friendly Enough Expression Language (FEEL) expressions.
3. **API-first approach** : Decision services are exposed as an API and are discoverable by Autopilot Workflow Service immediately upon creation.
4. **Native Autopilot integration** : Rules Service is natively integrated with Autopilot Workflow Service. Autopilot workflows for ClaimCenter and PolicyCenter can invoke decisions synchronously to guide straight-through processing.


Want to see these in action? Watch this demo video for a comprehensive walkthrough of creating and invoking your first decision service.


### Usage Patterns


These are common usage patterns for Rules Service:


- **Decision support for Autopilot** : Centralize routing, eligibility, and threshold decisions that drive straight‑through or assisted flows in ClaimCenter and PolicyCenter.
- **Custom rules in InsuranceSuite applications** : Externalize frequently changing custom rules to provide business users visibility and control. For example, claim rejection rules, policy integration rules and so on. Please note that you will be required to write the Gosu code to call Rules Service for custom rules in InsuranceSuite applications, with clear caveats regarding latency and the overhead of writing this integration code.


### Developer Experience Highlights


- **Low‑code authoring** : The rules designer offers a visual, low-code interface for creating, modifying, and reusing decision models and their associated data definitions without extensive programming.
- **Traceability and auditability** : All decision artifacts are version-controlled, and comprehensive execution logging enables full traceability of which rules were applied, when they were executed, and what outcomes they produced.
- **Separation of concerns** : Business logic is decoupled from process workflows. Workflows handle orchestration and sequencing while complex decision-making is delegated to Rules Service, reducing complexity and improving maintainability.
- **Business-IT collaboration** : Business analysts can directly define and modify rules, while you focus on system integration, testing, and deployment pipelines.


Stay tuned for more exciting features coming up, including AI-assisted decision modeling and Decision auditing & analytics.


**Ready to get started?** Partner with your business teams to pinpoint a high‑impact decision inside your current workflows, then model it in the rules designer. Publish your first decision in Rules Service and connect its endpoint to the Autopilot workflow in PolicyCenter or ClaimCenter.


Visit the official[Developer Documentation](https://docs.guidewire.com/rules-service/release/topics/c_about-guidewire-rules-service.html) for more details, or reach out to me atsmadishetty@guidewire.com with any questions.


Subscribe to our **Developer Newsletter** for future release highlights delivered directly to your inbox.


[Subscribe Here](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
