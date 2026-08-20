---
schema_version: "1.0.0"
document_id: "2a0f18222dfadd7f3af0496cf0187bdd39839fa83ef3c2fd1534a7c979388b8a"
company_key: "weave-communications-inc-common-stock"
company: "Weave Communications Inc."
source_id: "weave-communications-inc-common-stock-rss-cb397ff18858"
canonical_url: "https://engineering.getweave.com/talk/five-flavors-of-mtls-in-k8s/"
published_at: "2023-01-22T21:01:11+00:00"
first_seen_at: "2026-07-20T23:19:36.973215+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:76924666f74bfebf796d074fe4aafe83171c08109c4964887ba5c0fa6fd2dac0"
---

# Five Flavors of mTLS in Kubernetes

# Summary


This talk was presented at the[Utah Kubernetes Meetup](https://www.meetup.com/utah-kubernetes-meetup/events/290781295/) . It covers 5 different ways to get mututal TLS inside a Kubernetes cluster. This is an important feature that is often only discussed within the context of a service mesh. This talk proves that mTLS can actually be accomplished a number of ways with various trade offs.


# Key Takeaways


- mTLS can be done many different ways
- Every option has trade-offs
- The cloud native community provides some great tools to automate PKI


- [cfssl](https://github.com/cloudflare/cfssl)
- [cert-manager](https://cert-manager.io/)


- A service mesh may be the easiest way but you lose a lot of control


# Details


The full source code for the talk can be found at:


- [https://github.com/carsonoid/talk-5-flavors-of-mtls-in-kubernetes](https://github.com/carsonoid/talk-5-flavors-of-mtls-in-kubernetes)
