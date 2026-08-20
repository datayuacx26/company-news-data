---
schema_version: "1.0.0"
document_id: "ce931f379e09cede678045a3dfe1fe3703beafbc590078ce713198d9b580b29d"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-05-28-introducing-app-verification-on-flower-hub"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:133a4e8f2bf256b7a4197786df0da112bf3e27a04fea0a71693200db9ad94949"
---

# Introducing App Verification on Flower Hub

In our previous post on the[launch of Flower Hub](https://flower.ai/blog/2026-03-10-announcing-flower-hub) , we introduced a platform for discovering and sharing federated AI applications. But enabling collaboration across organizations requires more than just access. It requires trust.


Today, we’re introducing **App Verification on Flower Hub** : a decentralized trust model designed to make federated AI safer, more transparent, and easier to operate at scale.


## The Trust Problem


Federated AI brings together multiple organizations to collaboratively train models without sharing raw data. But that also raises a critical operational question:


**How do you know what is running on your SuperNode?**


Imagine a healthcare federation where 100 hospitals collaborate on a medical imaging model. When a new hospital joins, it does not just start running code on sensitive patient data without scrutiny. It needs a clear way to understand what is being executed and whether that app has been reviewed by someone it trusts.


There are two common ways to approach this problem, and both have serious drawbacks.


### Approach 1: Every Organization Reviews the App Itself


One approach is self-review: every organization downloads the Flower App Bundle (FAB), inspects it, and makes its own trust decision.


At first glance, that sounds safe. In practice, it does not scale.


In a federation with 100 hospitals, the same app may need to be reviewed 100 times. The same work gets repeated across all participants. Onboarding slows down. Review processes differ from one organization to another.


And there is a less obvious issue: this model can be insecure in practice. If trust depends on every organization reviewing correctly, then overall security is limited by the weakest review process in the federation.


### Approach 2: One Central Authority Reviews Everything


Another approach is centralized review, where one trusted authority reviews apps for everyone else.


This is the model used by traditional app stores such as Apple’s App Store. It is operationally simpler and familiar, but it introduces a different tradeoff: every participant must trust the same central gatekeeper.


That creates a single trust bottleneck and concentrates control in one place.


## Decentralized App Verification on Flower Hub


Flower Hub takes a different approach: decentralized verification.


Here’s how it works:


1. Developers publish apps on Flower Hub.
2. Reviewers register their public keys.
3. Reviewers inspect the FAB and sign it using the Flower CLI.
4. Signatures are published as verifiable metadata.
5. Users decide which reviewers they trust and act accordingly.


This creates a powerful separation:


- Publication makes apps available
- Verification makes trust visible


Flower Hub distributes both the app and the verification metadata, but it does not force a single global trust policy. Each user or organization decides which reviewers they trust.


And because signatures are tied to specific app versions, users can be confident that what was reviewed is exactly what they run.


Examples:


```text
# Sign the latest version
flwr app review @flwrlabs/demo


# Sign a specific version
flwr app review @flwrlabs/demo==1.2.0
```


For more details, please refer to the[How to Sign Apps](https://flower.ai/docs/hub/how-to-sign-hub-apps.html) documentation page.


## Make Trust Visible


Verification is not hidden. It is built into the experience.


Every app on Flower Hub includes a Verifications section, where users can instantly see:


- Who reviewed the app
- Which version was signed
- Whether those reviewers are trusted in their ecosystem


This transforms trust from something implicit into something observable and actionable.


Instead of asking “Can I trust this app?”, users can now answer: “Do I trust the people who verified it?”


## Enforce Trust by Default


For organizations running federated workloads, trust is not optional. It is a requirement.


With Flower Hub, SuperNode operators can enforce trust automatically by defining a trusted-entities


list. Only apps signed by those trusted reviewers are allowed to run. Everything else is blocked.


This means:


- No unverified code touching sensitive data
- Consistent trust policies across teams
- Confidence for all federation participants


It is a simple mechanism with powerful guarantees, especially for enterprise and regulated environments.


## Built for Real-World Federations


App Verification is designed to scale with your federation:


- Independent reviewers (internal or external)
- Flexible, user-defined trust policies
- Version-specific guarantees
- Smooth integration with deployment (SuperNodes)


Whether you are collaborating across hospitals, enterprises, or research institutions, Flower Hub provides the infrastructure to establish trust without forcing everyone into self-review or a single central authority.


## Available Now (Preview)


App signing and verification metadata are currently available as preview features, and we will continue evolving the experience based on feedback.
