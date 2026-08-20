---
schema_version: "1.0.0"
document_id: "f716f373e05c35c6a03ea4c4f0ca8154a374a402c4c096169bc92eef47ee5933"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/openauth-beta/"
published_at: "2024-12-09T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:75fdcdb08afa8308047435a9e1bdbb375f6934beb38048f98fc352230d579a59"
---

# OpenAuth Beta

Today we are launching OpenAuth —[openauth.js.org](https://openauth.js.org/)


A universal, standards-based auth provider. It’s in beta along with the new[Auth](https://sst.dev/docs/component/aws/auth) component for SST v3 that uses this.


[Play](https://youtube.com/watch?v=mKKx8uXw5ak)


---


## What is OpenAuth


[OpenAuth](https://openauth.js.org/) is a new auth provider that’s built on top of OAuth 2.0. It’s a single-page app that you can deploy anywhere.


- **Universal** : You can deploy it as a standalone service or embed it into an existing application. It works with any framework or platform.
- **Self-hosted** : It runs entirely on your infrastructure and can be deployed on Node.js, Bun, AWS Lambda, or Cloudflare Workers.
- **Standards-based** : It implements the OAuth 2.0 spec and is based on web standards. So any OAuth client can use it.
- **Customizable** : It comes with prebuilt themeable UI that you can customize or opt out of.


[Play](https://youtube.com/watch?v=SSjNUuQ06tk)


You can learn more about it over on the[OpenAuth GitHub](https://github.com/toolbeam/openauth) .


---


## Why build OpenAuth


Auth has been one of the most requested features in SST. With v1 and v2 of our` Auth` components, we found that most folks wanted a standalone, easy to self-host auth provider.


Most of the existing solutions were either libraries, or SaaS services that held your user data. And the standalone self-hosted ones were just too complicated to set up.


We also think a universal, standards-based auth provider is something that should exist outside of SST, which is why we built OpenAuth as a separate project.


---


## Using it in SST


OpenAuth needs some minimal infrastructure to run. The new[Auth](https://sst.dev/docs/component/aws/auth) component in SST v3 can do this for you.


sst.config.ts


```text
new   sst  .  aws  .  Auth  (  "  MyAuth  "  , {         domain:   "  auth.myapp.com  "  ,         authorizer:   "  src/auth.handler  "    });
```


---


## Next steps


OpenAuth and the new` Auth` component are in beta. Over the next few weeks we’ll be adding more docs, examples, and tutorials, as we finalize the API.


If you are eager to try it out and play around, head over to the[OpenAuth README](https://github.com/toolbeam/openauth) and check out the examples.


You can[star the repo](https://github.com/toolbeam/openauth) and[follow us on X](https://x.com/SST_dev) as we get closer to a 1.0 release.
