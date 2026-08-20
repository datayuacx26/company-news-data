---
schema_version: "1.0.0"
document_id: "8ae7a7526f13b2d8988a8708f66d77454b58477032e148417f1a5f3512b038f9"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/migrate-your-internet-resource"
published_at: "2024-02-16T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:396d4dda8fa197d9093c6c10127b85b41f0e4a31de367df22ea21f03d468e546"
---

# Migrate Your Internet Resource | Firezone Blog

---


## tl;dr


We're making a change to the way the[Internet Resource](https://www.firezone.dev/kb/concepts/resources#the-internet-resource) works in Firezone to improve security and performance. Beginning **March 15, 2024** , the Internet Resource will exist exclusively in a new Site dedicated to routing Internet traffic.


**Action Required:** If you're currently using the Internet Resource, deploy at least one Gateway in the new Internet Site and then click the orange` Migrate Internet Resource` button on the` Sites` page to migrate your existing Internet Resource to the Internet Site.


If you're not using the Internet Resource, **no action is required** .


## Why the change?


The Internet Resource provides full-route tunneling for traffic that doesn't match any other Resource in your account. This is useful when you want to ensure all traffic is routed through Firezone, but it can also be a security risk if not properly configured.


Consider the case where the Internet Resource lives in a Site with other Resources. Because the Internet Resource (by definition) matches all traffic, any actor in your account with access to the Internet Resource would also by default have access to all other Resources in that Site. If you had no other mechanism to limit traffic from the Gateway(s) in the Site to those Resources, you could inadvertently grant access to protected Resources you didn't intend to.


## What's changing?


The Internet Resource will now exist in a dedicated Internet Site that only contains the Internet Resource. This means that the Internet Resource will no longer be able to route traffic to other Resources in your account. This change will be enforced beginning **March 15, 2024** .


This also means you'll need to deploy dedicated Gateway(s) to the Internet Site to handle traffic for the Internet Resource. This ensures Internet-bound traffic is properly isolated from other Resources in your account.


## What do I need to do?


If you're using the Internet Resource, perform the following steps:


1. [Sign in](https://app.firezone.dev/getting_started) to the admin portal.
2. Click` Sites` in the sidebar.
3. Scroll down and click the` Manage Internet Site` button.
4. Deploy at least one Gateway in the Internet Site, and make sure it's connected.
5. Head back to` Sites` .
6. Click the orange` Migrate Internet Resource` button to your Internet Resource to the Internet Site.


That's it! Your Internet Resource is now migrated to the Internet Site.


## What if I don't migrate?


If you don't migrate your Internet Resource to the Internet Site by **March 15, 2024** , it will be migrated automatically after that date. Unless you have deployed Gateway(s) in the Internet Site, this will result in a loss of connectivity for any actors using the Internet Resource.


## Questions?


If you have any questions or need assistance with this change, please reach out to our[support team](https://www.firezone.dev/support) .
