---
schema_version: "1.0.0"
document_id: "9097823e673b5005a1586303dad0445538dde22bff6097df649dfca57404489d"
company_key: "yc-rdash"
company: "RDash"
source_id: "yc-rdash-rss-1daaac79f0ad"
canonical_url: "https://techblog.rdash.io/building-infrastructure-as-code-from-day-one-e67d30dcacbd"
published_at: "2023-09-03T11:07:36+00:00"
first_seen_at: "2026-07-25T20:33:24.755356+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:e50566a30cb3dadec2c4c2e1a4c1a89b8797992674d13a31f687217e9b6c8228"
---

# Building Infrastructure as Code from Day One

# Building Infrastructure as Code from Day One


[Amit Kumar](https://medium.com/@rdash_io?source=post_page---byline--e67d30dcacbd---------------------------------------)


2 min read


·


Sep 3, 2023


--


Press enter or click to view image in full size


Photo by[Growtika](https://unsplash.com/@growtika?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


One of the decision, that we took at R’Dash, was to build all of our tech infrastructure as Code.


This approach allowed us to automate the provisioning and management of resources across different environments. The infrastructure code is versioned and stored in a repository, which makes it easy to review, test, and deploy changes.


This become quite easy for us to adopt as we took cloud native approach to build all of our infrastructure. Kubernetes became our first choice for container deployment and orchestrator tool, because of it being open source and large community backed.By using Kubernetes, we were able to create and manage containerized applications and services with ease.


## Get Amit Kumar’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


We have made a practice of committing all of our kubernetes deployment configuration files within the application repository itself. It gave us tons of advantage, starting with following git-ops approach to infrastructure changes.


- All of our infrastructure changes goes to deployment post review which drastically reduces accidental mistakes which are primary reasons for major outages. We use raw yaml files to configure all the kubernetes resources, to override the dynamic parameters on the yaml files, we use` kustomize` . For building resources for multiple environment we use` Skaffold`
- Other advantage that we get by following a git-ops approach is that we lower the entry barrier to make infrastructure changes to developers. We don’t have a separate DevOps team to manage our infra, instead developers maintains it.
- Having a separate repository for deployment makes it hard to keep sync between different environment variables whenever we deploy some thing new which requires configuration to keep as environment variable.


> In my previous organization one time system got down because developer forgot to make the environment variable change in the separate repo and merge it before triggering the deployment.


This approach has also allowed us to quickly and easily spin up new environments for development, testing, and staging. With just a few commands, we can create fully functional environments that mirror our production environment. This has saved us a lot of time and effort in setting up and managing these environments manually. Additionally, it has made it easier for new team members to get up and running quickly, as all they need to do is run a few scripts to start the environment.
