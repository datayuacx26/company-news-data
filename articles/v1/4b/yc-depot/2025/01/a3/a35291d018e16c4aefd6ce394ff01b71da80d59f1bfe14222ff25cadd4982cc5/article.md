---
schema_version: "1.0.0"
document_id: "a35291d018e16c4aefd6ce394ff01b71da80d59f1bfe14222ff25cadd4982cc5"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/configuring-cache-retention-on-depot"
published_at: "2025-01-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:7a3544fbbd4b2ed8a51f54ea306bc213a76fa4a2f719c56d42ae5bc7dd190bf3"
---

# Now available: Cache retention policies for GitHub Actions & Depot Cache

Earlier this week we launched[Depot Cache](https://depot.dev/products/cache) , our new remote cache solution.


We know projects come in all shapes and sizes, which is why we offer up to 500GB of cache storage to accelerate your builds, no matter the scale. To help you manage these caches effectively, we’re introducing new cache retention policy settings.


In your organization settings, you'll now see management for two different types of cache: GitHub Actions cache and Depot Cache.


## Flexible Cache Retention Policies


To give you full control over your cache usage, we’ve introduced two retention policy options: **time-based retention** and **size-based retention** . These settings can be applied independently to GitHub Actions cache and Depot Cache.


### Time-based retention


Set a time limit for cache entries, so any unused entries are cleared automatically after the specified duration. Options include 7 days (GitHub’s default), 14 days, or 30 days.


Time-based options for cache retention


### Size-based retention


Additionally, manage storage by assigning a maximum size for your cache. When the cache exceeds your set limit, the oldest entries are removed to make room for new ones. You can configure limits from 25GB, all the way up to 500GB.


Size-based options for cache retention (Between 25GB and 500GB)


With these new settings, you can better manage your cache storage and usage for your organization's needs. Set your cache retention policies in your organization settings in the Depot app.


## What's next?


Today is the last day of our first informal Depot Drop Week of 2025. If you haven't already seen the latest, check out our[blog](https://depot.dev/blog) to catch up on this week's announcements - including the launch of Depot Cache, our new partnership with[Dagger](https://depot.dev/blog/depot-with-dagger) , and our new[larger Docker image builders](https://depot.dev/blog/larger-builders-for-faster-builds) .


Have any questions or feedback? Join us in our[Discord Community](https://depot.dev/discord) to chat with us.


## Related posts


- [Introducing Depot Cache](https://depot.dev/blog/introducing-depot-cache)
- [Now available: GitHub Actions Runners settings](https://depot.dev/blog/github-actions-runner-settings)
- [Introducing Ultra Runners — Up to 3x faster GitHub Actions jobs](https://depot.dev/blog/introducing-github-actions-ultra-runners)
- [Build Docker images faster using build cache](https://depot.dev/blog/faster-builds-with-docker-caching)


Kyle Tryon
