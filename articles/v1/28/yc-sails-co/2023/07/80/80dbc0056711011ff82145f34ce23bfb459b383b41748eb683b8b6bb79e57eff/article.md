---
schema_version: "1.0.0"
document_id: "80dbc0056711011ff82145f34ce23bfb459b383b41748eb683b8b6bb79e57eff"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/wish-v0-0-7/"
published_at: "2023-07-04T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:2c98f389779f32d93747bfb214f97045bf2b3c07ba99a1688272f4e97ce64bd9"
---

# wish 0.0.7

I wrote the first version of[wish](https://docs.sailscasts.com/wish/) - the OAuth hook you wish exists for Sails - when I needed GitHub OAuth flow for[sailscasts.com](https://sailcasts.com/) .


With wish, initiating the authentication flow by redirecting to the OAuth provider requires just a single line of code in your Sails[action](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) 🤯.


```text
return   sails.wish.  provider  (  'github'  ).  redirect  ()
```


## v0.0.7


I’m excited to introduce the latest release of wish - **v0.0.7** 🎉


This release introduces Google as an OAuth provider.


I have been eager to work on incorporating Google as an OAuth provider since the first release of wish, but I lacked the motivation because I didn’t have any use case for it.


However, given the need to support Google OAuth flow in[The Boring JavaScript Stack](https://sailscasts.com/boring) ’s[mellow template](https://github.com/sailscastshq/boring-stack/issues/13) , it was the ideal opportunity to integrate Google as an OAuth provider into wish.


The redirect flow remains unchanged:


```text
return   sails.wish.  provider  (  'google'  ).  redirect  ()
```


For detailed instructions on configuring a Google OAuth flow in your Sails application, please refer to the[Google section](https://github.com/sailscastshq/sails-hook-wish#google) of the README.


## ✅ Upgrading


To upgrade simply run:


```text
npm   i   sails-hook-wish@latest
```
