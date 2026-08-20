---
schema_version: "1.0.0"
document_id: "dcd02f79b8994c2e19aa5a5fd366d5c42ba9d068ac1c5793558270c6f7eff708"
company_key: "yc-buildjet"
company: "BuildJet"
source_id: "yc-buildjet-news-import-6b2f4e2d5236"
canonical_url: "https://buildjet.com/for-github-actions/blog/launch-buildjet-cache"
published_at: null
first_seen_at: "2026-07-24T23:34:21.347694+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:97a0c9e53c33f05b327ccc74b6994c1698bf2027dd2eda0d90168c48e4105b1f"
---

# Launching BuildJet Cache for GitHub Actions

A reliable, faster and free alternative to GitHub Actions cache.


We're thrilled to announce` buildjet/cache` , a **reliable, fast, and free alternative to GitHub Actions cache** . It is compatible with any runner - official, self-hosted, or BuildJet, and no matter the runner used, **BuildJet doubles the free storage, providing 20 GB/repo/week storage space for free** . Just like with GitHub Actions cache, when you consumed your free storage quota, we'll simply remove the oldest entry to make room for the new one.


To get started, simply replace` actions/cache` with` buildjet/cache` .


```text
yaml  1   ...
-     -     uses  :   actions/cache@v3
+     -     uses  :   buildjet/cache@v3
4       with  :
5         path  :   ~/.npm
6         key  :   buildjet  -  node  -  $  {  {   hashFiles('  **/package-lock.json')     }  }
7         restore-keys  :     |
8         buildjet-node-
9    ...


```


BuildJet also provides a **drop-in replacement for most popular setup actions** . Today, BuildJet has compatible setup actions for[setup-node](https://github.com/actions/setup-node) ,[setup-python](https://github.com/actions/setup-python) ,[setup-java](https://github.com/actions/setup-java) ,[setup-go](https://github.com/actions/setup-go) and[setup-dotnet](https://github.com/actions/setup-dotnet) . Simply replace` actions/setup-*` with` buildjet/setup-*` in your workflow, and you're good to go.


To fully take advantage of the new BuildJet cache in your setup action, **make sure you enable caching in the setup action** by setting the cache property to` npm` (or` pip` ,` gradle` , etc.). For example (using` setup-node` ):


```text
yaml  1   ...
2    -     name  :   Setup node
-        uses  :   actions/setup  -  node@v3
+        uses  :   buildjet/setup  -  node@v3
5       with  :
6         node-version  :     14
+          cache  :   npm
8    ...


```


# Why BuildJet Cache?


Our customers frequently report issues with` actions/cache` , ranging from stalling downloads to slow performance. The issues don't only slow down workflow runs significantly, but can even negate the speed gains given by the faster BuildJet hardware when caches get large.


Despite our best efforts to address these issues, they remain unresolved. Coupled with the recent staff changes at GitHub's India team - responsible for maintaining actions/cache - the commit activity has halted and numerous performance and stability issues remain unresolved.


Although cache issues are more pronounced on BuildJet and self-hosted runners, they are not exclusive to them. They occur frequently enough on official runners to warrant urgent resolution. In combination with the growth of BuildJet for GitHub Actions, we felt the need to address these issues.


Originally, we recommended` whywaita/actions-cache-s3` , for users who had issues with` actions/cache` , as it was much more reliable. However, it required users to set up their own object storage, a process not everyone is familiar with and one that goes against the values of BuildJet. It didn't feel right for us, and we wanted to provide a better solution. We wanted a solution that was as reliable and fast as` whywaita/actions-cache-s3` , but as easy to set up as` actions/cache` .


# The BuildJet cache action


The BuildJet cache action is designed with user-friendliness at its core, no need to set up your own object storage or learn a new syntax. It's a drop-in replacement for` actions/cache` that **works with any runner - official, self-hosted, or BuildJet.**


Let's take a look at the performance of the` actions/cache` action compared to` buildjet/cache` .


GitHub Cache uses Azure Blob StorageBuildJet Cache uses Cloudflare R2


As we can see from the graphs above, the BuildJet cache has a far more reliable and consistent performance, regardless of the runner used.


# BuildJet Cache Benefits


- **Reliability** : We've fixed the persistent issues of cache download stalling and slow cache download that plague` actions/cache` .
- **Speed** :` buildjet/cache` will be fast all over the world as it's powered by Cloudflare R2, which has 250+ data centers worldwide.
- **20 GB/repository/week free Storage** : Regardless of which runner it's being used in, BuildJet doubles GitHub's free storage, providing 20 GB/repo/week storage space for free


For more information about the BuildJet Cache, please visit our[documentation](https://buildjet.com/for-github-actions/docs/guides/migrating-to-buildjet-cache) .
