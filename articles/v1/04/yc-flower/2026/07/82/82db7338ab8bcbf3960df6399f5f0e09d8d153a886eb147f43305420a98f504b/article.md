---
schema_version: "1.0.0"
document_id: "82db7338ab8bcbf3960df6399f5f0e09d8d153a886eb147f43305420a98f504b"
company_key: "yc-flower"
company: "Flower"
source_id: "yc-flower-news-import-15689314081b"
canonical_url: "https://flower.ai/blog/2026-07-01-announcing-flower-1.32.1-release"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-21T20:31:29.302072+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:57ce6c4f33635461579bcfa667b7e504606cd72c7f99e5f996656b8aa29a8b2c"
---

# Announcing Flower 1.32.1

The Flower team is excited to announce the release of Flower 1.32.1 stable!


**Flower is a friendly framework for collaborative AI and data science.** It makes novel approaches such as federated learning, federated evaluation, federated analytics, and fleet learning accessible to a wide audience of researchers and engineers.


### Thanks to our contributors


We would like to give our special thanks to all the contributors who made the new version of Flower possible (in git shortlog


order):


Chong Shen Ng


, Daniel Nata Nugraha


, Heng Pan


, Javier


, Micah Sheller


### What's new?


-


**Fix flwr-simulation failing to exit** ([#7504](https://github.com/flwrlabs/flower/pull/7504) )


Closes the gRPC channel on exit, preventing flwr-simulation


from hanging in some cases.


-


**Improve agent reliability and security** ([#7505](https://github.com/flwrlabs/flower/pull/7505) ,[#7507](https://github.com/flwrlabs/flower/pull/7507) ,[#7509](https://github.com/flwrlabs/flower/pull/7509) )


Improves agent reliability and security with several small enhancements and fixes.
