---
schema_version: "1.0.0"
document_id: "a41e40a8c8b3ce086899793aa39be6403c199647a19ed380c7af2b83e59cb3b6"
company_key: "yc-bitrise"
company: "Bitrise"
source_id: "yc-bitrise-news-import-b747fb40f1b3"
canonical_url: "https://bitrise.io/blog/post/open-source-registries-are-changing-how-bitrise-keeps-your-builds-running"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-24T03:43:06.304736+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:e69c3fe850016cbb347fa98ce3588885550beb290dd4e025904df42a7e5aaa1c"
---

# Open source registries are changing: Here's how Bitrise keeps your builds running

## What's up with Maven Central?


There is a shift happening in a previously quiet corner of the open source community. You may have experienced this in your own Android builds with an HTTP 429 ("Too Many Requests") error during dependency resolution from Maven Central. Over a period of a few days in late April to early May 2026, a subset of Bitrise users experienced these errors. Here's what happened, what we did about it, and what it means for you.


## The backstory


For years, public package registries like Maven Central provided unrestricted access to open-source artifacts. That model is now changing. In September 2025, the[OpenSSF published a joint statement](https://openssf.org/blog/2025/09/23/open-infrastructure-is-not-free-a-joint-statement-on-sustainable-stewardship/) co-signed by stewards of Maven Central, PyPI, crates.io, and others, making the case that open infrastructure cannot be free.


[Sonatype](https://www.sonatype.com/blog/open-is-not-costless-reclaiming-sustainable-infrastructure) , the sponsor of Maven Central, has begun acting on that message. As they migrated to new CDN infrastructure, they introduced rate limits for high-volume consumers such as CI/CD platforms, security scanners, and other services that generate outsized traffic.


In late April, an operational change on Sonatype's side caused rate limiting to be unexpectedly enforced on Bitrise's infrastructure. Some Android builds that resolved dependencies from` repo1.maven.org` ,` repo.maven.apache.org` , and related hosts began getting rate limited.


## What we've done about it


### Deployed a repository manager (proxy cache)


Our primary mitigation was deploying an in-datacenter repository manager that caches build dependencies. Instead of every build fetching artifacts directly from Maven Central, builds now pull from our proxy. If the proxy doesn't have an artifact, it fetches it from Maven Central a single time and caches it for all subsequent builds.


We had begun preparing the repository manager prior to April because we understood that this may be useful in the near future. We did not anticipate just how unexpectedly urgent the need would be! We immediately deployed the repository manager, and we have observed a **98.9% reduction** in upstream requests and a **2,000x** decrease in data transfer.


The proxy is automatically activated at the VM level, so every Bitrise-hosted build uses the cache without requiring any workflow changes.


#### How is this different from key-value cache and build cache?


The proxy cache (Repository Manager) stores **third-party dependencies** from Maven Central so they don't need to be re-downloaded from the internet on every build.


Build Cache stores **your project's own build outputs** (compiled classes, task results) so Gradle doesn't need to re-execute work that hasn't changed.


The Key-Value Cache stores whole files and directories you designate (like ~/.gradle/caches ) so they persist between builds on ephemeral CI runners.


All three are complementary and can be used together.


### Working directly with Sonatype


We've also been in direct contact with Sonatype's technical leadership, and we are working with them to ensure uninterrupted access for Bitrise users.


## What this means for Bitrise users


**Rate limit errors have been eliminated.** Our repository manager absorbs the vast majority of Maven Central traffic, and we are continuing to harden it against edge cases.


Beyond fixing the problem, the proxy cache is actually a performance improvement for our users. Because the repository manager is co-located with the CI runner in the same datacenter, artifact downloads are significantly faster than pulling from a remote CDN. The improvement varies by project, but as a reference point: the median build time for the DuckDuckGo Android app — a sample app we continuously profile — improved by 38 seconds after we introduced the proxy cache.


Sample project drop in median runtime after introduction of co-located repository manager.


## Our recommendations


During the initial incident, we issued several evolving recommendations as we addressed the situation and its edge cases.


The short version: Bitrise builds on supported stacks are automatically configured to use the proxy cache. You can remove any of the previously recommended mitigations from your Bitrise workflows. No action is required for most users.


## The bigger picture


The package registries' stance toward rate-limited access is an industry-wide shift, and it is affecting all of us. Check that status page of other CI systems, and you will see that everyone is experience bumps in the road.


We are committed to providing a stable experience to our users. We’ve invested in infrastructure to insulate our users from these changes — and in doing so, delivered faster builds as a side effect.


If you have questions, reach out to[Bitrise support](https://support.bitrise.io/) .


## Further reading


- [Maven Central 429 FAQ](https://central.sonatype.org/faq/429-error/) — Sonatype's official guidance on rate limiting
- [Open Is Not Costless](https://www.sonatype.com/blog/open-is-not-costless-reclaiming-sustainable-infrastructure) — Sonatype's blog on sustainable infrastructure
- [Open Infrastructure Is Not Free](https://openssf.org/blog/2025/09/23/open-infrastructure-is-not-free-a-joint-statement-on-sustainable-stewardship/) — Joint statement from OpenSSF and package registry stewards
