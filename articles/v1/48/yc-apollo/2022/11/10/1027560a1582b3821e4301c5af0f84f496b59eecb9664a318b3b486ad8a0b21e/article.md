---
schema_version: "1.0.0"
document_id: "1027560a1582b3821e4301c5af0f84f496b59eecb9664a318b3b486ad8a0b21e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-the-end-of-life-schedule-for-apollo-gateway-v0-x"
published_at: "2022-11-15T08:31:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:b6ee956a9c87cf0e336247aae89889dae088b020a387dbff670ea1fbd9d3b3a7"
---

# Announcing the end-of-life schedule for Apollo Gateway v0.x

With[Apollo Router v1.x](https://www.apollographql.com/docs/router/) being rapidly adopted as our next-generation[supergraph](https://www.apollographql.com/blog/announcement/backend/the-supergraph-a-new-way-to-think-about-graphql/) runtime and Apollo Gateway v2.x and Federation 2[being GA for over 7 months](https://www.apollographql.com/blog/announcement/backend/apollo-federation-2-is-now-generally-available/) , Apollo Gateway v0.x is now officially[deprecated](https://www.apollographql.com/docs/resources/product-launch-stages/#deprecated) and will reach its[end-of-life](https://www.apollographql.com/docs/resources/product-launch-stages/#end-of-life) on **September 22, 2023** . As of today, **Apollo Gateway v0.x** is no longer under active development. We will, however, continue to release security patches and fixes for major regressions until September 2023, after which Apollo Gateway v0.x will **no longer be supported or maintained by Apollo** .


We recommend[migrating to Apollo Router](https://www.apollographql.com/docs/router/migrating-from-gateway/) today for its significant performance gains and capabilities.[Apollo Router 1.x](https://www.apollographql.com/blog/announcement/backend/announcing-apollo-router-v1-0/) is the re-platforming of the Gateway offering[10x better performance](https://www.apollographql.com/blog/announcement/backend/apollo-router-our-graphql-federation-runtime-in-rust/) , enhanced security, and many built-in capabilities like cors, logging, telemetry, and traffic shaping that can be[configured declaratively with YAML](https://www.apollographql.com/docs/router/configuration/overview#yaml-config-file) rather than with Javascript code. Optionally, the Router can be customized with Rhai scripts and native Rust plugins for advanced use cases. Our roadmap for the router includes new features like entity-based` @defer` support (now in preview),[live queries](https://www.apollographql.com/events/virtual-event/graphql-summit-october-2022/the-future-of-real-time-data-in-graphql/) , authorization, caching, demand control, and much more.


If you are unable to migrate to the Router, **Apollo Gateway 2.x** is a drop-in upgrade from Gateway 0.x. Plugins and middleware written for 0.x will work unmodified in Gateway 2.x. Most future feature development will be focused on Apollo Router, so we would love to understand any obstacles you encounter migrating to it. Please[open a discussion on GitHub](https://github.com/apollographql/router/discussions) if you need general help or[open an issue](https://github.com/apollographql/router/issues) if you have any troubles!


Written by


Phil Prasek


[Read more by Phil Prasek](https://www.apollographql.com/blog/author/philprasek)
