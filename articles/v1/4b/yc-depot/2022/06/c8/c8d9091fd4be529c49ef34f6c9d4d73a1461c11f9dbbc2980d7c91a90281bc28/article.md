---
schema_version: "1.0.0"
document_id: "c8d9091fd4be529c49ef34f6c9d4d73a1461c11f9dbbc2980d7c91a90281bc28"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/project-tokens"
published_at: "2022-06-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:814689e2474496697766a137d3a0977f6d99a09840bf3f3c3271977ba9a06085"
---

# New feature: project tokens

We recently released a new authentication method for builds in Depot, project tokens! With this new feature, you now have three different ways to authenticate and run builds with Depot:


1. User access tokens


- Created by individual users, user access tokens allow build access for any project in any organization that the user has access to.


2. OIDC tokens (GitHub only)


- Configured by organization administrators for specific projects via[trust relationships](https://depot.dev/docs/cli/authentication#oidc-trust-relationships) , OIDC tokens issued by GitHub Actions can be programatically exchanged for temporary build tokens, allowing access to specific projects inside the workflow run.


3. Project tokens


- Created by organization administrators for a single project, project tokens allow build access for only that one project.


## When to use project tokens?


A project token belongs to a specific project within a single organization. They are scoped to only allow building images for the project they are associated with. This a finer grained approach than user access tokens, and is the recommended way to authenticate builds to Depot from your CI provider.


*Note: If you are using GitHub Actions as your CI provider, you can use the[OIDC token approach](https://depot.dev/docs/cli/authentication#oidc-trust-relationships) , which doesn't require any static access tokens and instead issues temporary access tokens for each workflow run. This is a more secure option if available to you.*


## How to configure a project token


Project tokens can be configured from the Project Details page.


1. Open your Project Details page by clicking into a project from your projects list
2. Click on Settings
3. Enter a description under` Add project token` and click create token


You can then use your new token in your existing CI provider to route builds to that specific project in Depot. See our[integration guides](https://depot.dev/docs/container-builds/integrations/github-actions) for details on how to set these tokens in a variety of CI providers.


## Feedback


We are continually looking for ways to make Docker builds easier, faster, and more secure. If you have any ideas or questions about project tokens, or anything Depot related,send us an email .


If you are tired of waiting for your slow Docker builds to finish in CI,[sign up for an account](https://depot.dev/sign-up) and let us help speed things up! Depot can build Docker images 2-3x faster in CI with our fast compute and persistent SSD cache. Depot can even build native Arm containers or multi-platform images without any additional configuration.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
