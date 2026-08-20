---
schema_version: "1.0.0"
document_id: "ba2c38a6ada2fcb316c6a11b69969af7a34c220df5348bbfb40f9ea256ba2035"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/behavioral-privacy-agentic-negotiation"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:938652e5f74e1d076c4acb838ce3fdc48e0ad81e5e57ae236546970788ef54f0"
---

# Behavioral Privacy Leakage in Agentic Negotiation: Formalizing and Mitigating Inference Attacks via Randomized Policies

[View publication](https://arxiv.org/abs/2607.06815)


This paper was accepted at the AI4TCI (Workshop on AI for Secure and Trustworthy Critical Infrastructure Systems) Workshop at the International Conference on Availability, Reliability and Security (ARES) 2026.


Autonomous negotiation agents are increasingly deployed in high-stakes settings such as insurance and procurement. While cryptographic techniques protect explicitly disclosed constraint values, they fail to address a subtler threat: behavioral privacy leakage, where an adversary infers private constraints from observable negotiation dynamics such as concession trajectories, timing, and convergence patterns. This paper investigates behavioral differential privacy in multi-round negotiation protocols. We design an adaptive stochastic negotiation policy that jointly guarantees (ε,δ)-differential privacy, almost-sure convergence of the offer sequence (reaching agreement when the counterparty’s reservation value permits), and high negotiation utility. Evaluated on 3,000 synthetic bilateral negotiations, our mechanism reduces adversarial inference accuracy by 43–50% while maintaining a negotiation success rate and utility above 90%, demonstrating that strong privacy guarantees can be achieved without significant loss of performance.
