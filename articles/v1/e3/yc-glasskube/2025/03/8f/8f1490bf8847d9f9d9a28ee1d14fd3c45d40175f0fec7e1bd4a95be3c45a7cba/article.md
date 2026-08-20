---
schema_version: "1.0.0"
document_id: "8f1490bf8847d9f9d9a28ee1d14fd3c45d40175f0fec7e1bd4a95be3c45a7cba"
company_key: "yc-glasskube"
company: "Glasskube"
source_id: "yc-glasskube-news-import-f40b83a58804"
canonical_url: "https://distr.sh/blog/distr-v1.4-release-post/"
published_at: "2025-03-25T00:00:00+00:00"
first_seen_at: "2026-07-21T21:44:16.826534+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:a22cf2893f9868e6a2f269cb76d47bee8e4b616676560444fb178c7dfd1437eb"
---

# Distr v1.4 is here!

# Distr v1.4 is Live!


Hey there! We listened to your feedback, and we’re back with a massive new feature.


This one was inspired by some interesting insights into the real-world use cases a considerable amount of vendors told us they were up against, and it’s why we’re shipping **v1.4** today. It’s a minor version bump, but it unlocks new possibilities for advanced self-hosted distribution.


Each vendor use case we come across is different, so with **v1.4** we aim to give software vendors even more control over **what** they ship and **how** they ship it.


Want a hint? It’s all about **artifacts** , and a whole new way to manage them.


---


## 🚀 What’s New: Distr’s Built-in OCI Artifact Registry (Beta)


We are rolling out the **beta version of the Distr OCI Registry** . Initially enabling the feature to users who specifically asked for it, but if you’re interested, just let us know, and we’ll unlock it for you.


### Why it matters


Self-hosted deployments often require strict control over what software gets deployed, where, and by whom. That’s where Distr’s new **OCI Artifact Registry** comes in.


With the Distr OCI registry, software vendors can now publish and distribute not just container images, but also **Helm charts, SBOMs, policy modules** , and other **OCI-compliant artifacts** , all managed natively within your Distr account.


### This means software vendors get


- **Granular access control** : Define exactly which customer can pull which artifacts and versions
- **Multi-artifact support** : Distribute Helm charts, images, config bundles, and any artifact that is OCI-compliant
- **Software supply chain security** : Store SBOMs and signatures for safer distribution
- **Visibility** : Track where, how, and by whom artifacts are consumed
- **Open standards** : Built on the OCI spec and compatible with existing tooling (Docker, ORAS, etc.)


Whether you’re distributing a single binary or an entire application stack, the new registry gives you the building blocks to do it **securely** , **reliably** , and in an **OCI-compliant** way, inside your end-customers’ networks.


## Watch the video to see how it works


**Want to access the OCI Registry feature? Let us know via email atsupport@distr.sh !**


## What’s Coming Next


This is just the start! Here’s a peek at what we’re working on for upcoming releases:


- Built-in vulnerability scanning and security checks
- Display audit trails
- UI improvements for better overall usability


Follow us on[LinkedIn](https://www.linkedin.com/company/glasskube) to stay up to date on the latest Distr news.


---


## Join the Conversation


We’d love to hear your thoughts on this release! Share your feedback, ideas, or questions:


- [Join the discussion](https://github.com/distr-sh/distr/discussions)
- [Book a demo call](https://cal.glasskube.com/team/gk/demo?duration=30)
- Reach out on[LinkedIn](https://www.linkedin.com/company/glasskube) or[Twitter](https://twitter.com/glasskube)


---


## Thank You!


Thank you for being part of the Distr community. We’re excited to continue building the best **Open Source Software Distribution Platform** together.
