---
schema_version: "1.0.0"
document_id: "3f53af82a38f0c8356d1e75c12259b7211c5e2ddc2e7d5039b9e5ceda31eb954"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/when-feature-flags-werent-enough-91a441e5cabd"
published_at: "2026-06-09T13:33:43+00:00"
first_seen_at: "2026-07-22T17:27:13.648832+00:00"
fetched_at: "2026-08-20T02:26:39.473331+00:00"
content_hash: "sha256:367c028b5cd336f1bd2f6173ec377057f02458efdd52a68f2d0c6eea7b8695c3"
---

# When Feature Flags Weren’t Enough

We rebuilt remote configuration because *tooling must match the problem* and working like code with review, validation, and live delivery without redeploys.


*By: Mikhail Baranov, Software Engineer III, & Erick Lopez, Software Engineer III*


**Quick Bytes:**


- Feature flags worked for tracking changes, but they fell short when we needed richer configuration across thousands of restaurants
- We rebuilt remote configuration to work like code — version‑controlled, validated in CI, and delivered live to running services without redeploys
- Teams now manage configuration by exception, ship changes safely in real time, and operate with far more confidence and control


Runtime configuration carries real weight in a restaurant technology environment. Thresholds get tuned. Features roll out region by region. One location needs a special rule; another needs a noisy signal dialed down. These aren’t hypotheticals — they’re everyday decisions that shape how our backend fleet behaves across thousands of restaurants.


For a long time, we managed these decisions through a third-party feature-flag platform. It gave us auditability and a clear record of who changed what. But over time, an uncomfortable truth emerged: the tool wasn’t built for the shape of our problem.


We didn’t need toggles and targeting rules. We needed rich, structured configuration — hierarchical defaults, per-restaurant overrides, clear environment promotion, and real-time delivery to distributed backend services. Forcing that model into a flags-first product created friction: awkward workarounds, unnecessary cost, and extra operational steps for changes that should have felt routine.


We wanted configuration changes to feel as safe and controlled as code changes, while still reaching running services immediately. So, we set out to build exactly that.


**Treating configuration like a managed product** Our solution treats configuration with the same rigor as production code.


Structured configuration lives in version control. Changes move through pull requests and CI-enforced validation. Once approved, data is published to a real-time document store, and running services subscribe to updates — no restarts, no redeployments.


Every change is peer-reviewed, automatically validated, and permanently auditable.


*End-to-end architecture of the remote config pipeline: repository-sourced config, validated in CI, stored on the cloud, and applied live to services via listeners.*


**Designed for inheritance, not duplication** At the heart of the system is a data model built around **inheritance** . We define a baseline configuration for a group of restaurants, then layer on location-specific overrides only when needed. This lets us manage configuration by exception rather than brute force.


The model is intentionally straightforward:


- Every restaurant belongs to a group with a default configuration
- A restaurant can have an optional override document
- The system merges the two, with overrides taking precedence


One important detail: the merge happens at the top-level. If a configuration has a section for Alerts, an override replaces the *entire* Alerts block — not a single nested field. This deliberate choice eliminates ambiguity about what value wins.


*This diagram shows how each store maps to a group, inherits the group’s default configuration, and can optionally override specific top-level sections with its own document.*


**Shipping changes live, without redeploys** When a service starts, it loads its initial configuration and opens a persistent listener to the document store. When a change occurs, the system pushes the update, merges inherited data, diffs the new snapshot against the current one, and applies only what changed.


Services only poll for document existence flips (e.g., store mapping doc created/deleted, store override doc created/deleted). Operators never redeploy just to adjust a threshold or a location-specific rule.


**Versioning for safe evolution** Configuration versioning happens at three levels:


1. **Repository versioning:** Commit and pull-request history provide immutable change lineage per environment
2. **Contract versioning:** Additive-first changes and explicit key migration patterns keep consumers compatible during transitions
3. **Rollout versioning:** Promotion follows dev → QA → prod with verification checkpoints, making rollout versioned operationally, not just structurally


**Why the right fit matters** The biggest win wasn’t moving to a new data store. It was treating runtime configuration as a first‑class engineering system.


Configuration deserves code-level rigor. If a value can change production behavior, it should go through review, validation, promotion, and rollback — just like code. By rebuilding remote configuration around pull requests, CI validation, and controlled promotion, we made that rigor the default instead of the exception.


Rebuilding also forced us to confront a simple truth: better fit beats more features. A smaller, purpose-built platform can increase reliability when it matches the data model and operating model of the system. Rather than forcing structured configuration into a flags-first tool, we built a control plane that reflects how our services run and evolve.


That foundation sets us up for what comes next. The architecture is designed to support gradual rollouts and A/B testing, including rollout percentages per configuration key and deterministic variation using consistent hashing — all without redeploying services or introducing centralized state.


Most importantly, the system gives teams confidence. Operators can change behavior without shipping new builds. Engineers can evolve configuration safely over time. And when runtime behavior changes, everyone knows exactly how, why, and when it happened.


When the tool didn’t fit, rebuilding for the right fit made all the difference.


---


[When Feature Flags Weren’t Enough](https://medium.com/mcdonalds-technical-blog/when-feature-flags-werent-enough-91a441e5cabd) was originally published in[McDonald’s Technical Blog](https://medium.com/mcdonalds-technical-blog) on Medium, where people are continuing the conversation by highlighting and responding to this story.
