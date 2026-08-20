---
schema_version: "1.0.0"
document_id: "c38cdf9204ed578000fd0d0d7549a2b1d858d83887308536c2678823b82182db"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/circleci-oidc"
published_at: "2023-09-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:f2feb91e405a5ffef204d0c5e19d996f28cfbc04e49299a434a7ad8331087088"
---

# Now available: CircleCI OIDC for Depot builds

We're excited to announce that our integration with CircleCI just got even better! We have rolled out the ability to directly integrate with[CircleCI OIDC](https://circleci.com/docs/openid-connect-tokens/) via trust relationships.


You can now configure a trust relationship in Depot that allows your CircleCI jobs to access your project via a token exchange. So now, rather than having to embed a project token in your CircleCI environment, you can dynamically exchange tokens when your build runs to authenticate to your project from CircleCI.


## How to use CircleCI OIDC


You need to configure a trust relationship for your Depot project to leverage this new functionality. To add a trust relationship for CircleCI, you can go through the following steps:


1. Open your Project Details page by clicking on a project from your projects list
2. Click the Settings button next to your project ID
3. Click the Add trust relationship button
4. Select CircleCI as the provider
5. Enter your CircleCI organization UUID (this is found in your CircleCI organization settings)
6. Enter your CircleCI project UUID (this is found in your CircleCI project settings)
7. Click Add trust relationship


**Note:** CircleCI requires entering your organization and project UUID, not your organization's or project's friendly name.


Once the trust relationship is configured, you can use the` depot` CLI inside of your CircleCI jobs without any additional configuration in your job. See our[CircleCI integration guide](https://depot.dev/docs/container-builds/integrations/circleci) for common examples of[depot build](https://depot.dev/docs/cli/reference/container-builds#depot-build) in your CircleCI jobs.


## Conclusion


We're excited to continue making it easier and more secure to integrate Depot into your existing CI/CD workflows. The OIDC exchange eliminates the need to store static access tokens inside your CI provider. Instead, Depot can identify the CI job or workflow via a trust relationship tied to the project and issue short lived one-time build tokens.


Today, you can connect to your Depot projects via OIDC token exchanges from[GitHub Actions](https://depot.dev/docs/container-builds/integrations/github-actions) ,[Buildkite](https://depot.dev/docs/container-builds/integrations/buildkite) , and now[CircleCI](https://depot.dev/docs/integrations/circleci) .


Pop into our[Community Discord](https://discord.gg/MMPqYSgDCg) and let us know what you think!


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
