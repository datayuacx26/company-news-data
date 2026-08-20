---
schema_version: "1.0.0"
document_id: "cfd8152a4764a94a249bf9e370ab789cee033288513f122c50c8e0736792006f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/end-of-support-for-router-v1-x-long-term-support-lts-policy-update"
published_at: "2025-12-19T15:42:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:2176c5a259b4d8fa97cb532c85092ade67761470728177c48c5e41d76bfd6366"
---

# End of Support for Router v1.x; Long-Term Support (LTS) Policy Update

Apollo introduced the Long-Term Support (LTS) policy for the Router to give users a predictable, stable release cycle and the confidence to operate the Router in long-running or regulated production environments. The policy ensures teams can take advantage of new capabilities at their own pace, while always having a supported and secure baseline to rely on.


To better support upgrade planning and provide greater clarity around version lifecycles, we are refining our Router LTS policy and introducing updates to the current maintenance schedule for both Router and Federation.


## New LTS Release: Router v2.10 and Federation v2.12


To support users migrating from v1.x and to provide a dependable maintenance baseline on the Router v2.x platform, Apollo has released **Router v2.10 with Federation v2.12** as the next LTS version as of December 12, 2025.


**Router v2.10 and Federation v2.12**


- Will receive **critical security patches and essential reliability updates** throughout its maintenance window until September 2026
- Offers a stable upgrade target for users[transitioning from older Router versions.](https://www.apollographql.com/docs/graphos/routing/upgrade/from-router-v1)
- Router v2.x will continue active development with future feature releases and new minor versions, while the v2.10 line will receive fixes via the new LTS policy (see below)


## End of Support for Router v1.x and Federation v2.9


Router v1.x, including v1.61.x LTS, and Federation v2.9 will reach **End of Support (EOS) on March 31st, 2026** .


After EOS, these versions will no longer receive updates, including security or critical reliability fixes. Users operating on Router v1.x should begin planning their upgrade to the Router v2.x and Federation v2.12 line to remain on a supported version. Upgrading your Federation version can be done by changing the graph variant settings in Studio, or changing the federation composition version in your build configuration locally.


## New Support Policy


With the End of Support for the Router v1.x line, we have[updated our LTS policy](https://www.apollographql.com/docs/graphos/resources/router-release-lifecycle) to provide consistent and scheduled releases as we look forward to 2026.


- New major Router versions yearly with **12 months of Active development**
- Recurring LTS releases **every 6 months** to provide continued support
- LTS releases **supported for 9 months** , with an additional **3 months** of support for our **Premier Support** customers
- New policy will start in 2026


## Required Actions


- If you are currently running **Router v1.x** , please[begin planning your upgrade path](https://www.apollographql.com/docs/graphos/routing/upgrade/from-router-v1) before March 2026.
- Router v2.10 (released December 2025) will serve as a strong, stable LTS target for teams seeking minimal operational risk.
- After updating to Router v2.x, review and update your Federation composition settings in Apollo Studio to ensure you’re using a supported version (LTS or latest). Note that **no changes to your subgraphs are required** if you are already using Federation v2 **.**


## Our Commitment


Apollo values your partnership and trust as we continue to innovate. We are here to support you through this transition to keep your API infrastructure powerful and future-proof.


For additional support, please visit our[Apollo Community](https://community.apollographql.com/t/migrate-apollo-federation-1-0-supergraphs-to-apollo-federation-2-0/8125) or, for GraphOS users, contact our Technical Support team at[support.apollographql.com](http://support.apollographql.com/)


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
