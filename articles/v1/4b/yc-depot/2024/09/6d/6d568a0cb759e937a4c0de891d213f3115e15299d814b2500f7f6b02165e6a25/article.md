---
schema_version: "1.0.0"
document_id: "6d568a0cb759e937a4c0de891d213f3115e15299d814b2500f7f6b02165e6a25"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-actions-gpus-now-available"
published_at: "2024-09-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:75b80a9b5bb0f2e63d3ef17e69dbbd8bca08e67bf1d9e14aa7a2211a6ffa02fd"
---

# GPUs now available for Depot GitHub Actions runners

Since launching Depot GitHub Actions runners earlier this year, we've seen a lot of folks looking to leverage their own compute resources for their GitHub Actions jobs but want to avoid dealing with the headache of managing all the CI runners themselves. We immediately upgraded[Depot Managed](https://depot.dev/docs/managed/overview) to support GitHub Actions runners to get all the benefits of Depot on your own compute without the headache of managing your own CI runners.


Since rolling that out, one of the most requested features has been the ability to run GitHub Actions jobs with GPUs that folks have already reserved capacity for.


Today, we're excited to announce that GPUs are now available for Depot GitHub Actions runners when using Depot Managed!


## How it works


When using Depot Managed, we provision a dedicated connection inside of an AWS sub account that lives under your main AWS organization. You can see more details on how this gets configured in our[Depot Managed on AWS guide](https://depot.dev/docs/managed/on-aws) .


This dedicated connection allows us to configure your` runs-on` label to map down to your own AWS compute resources. So, if you're using` runs-on: depot-ubuntu-latest` in your GitHub Actions workflow, we'll spin up an EC2 instance in your subaccount that matches the` ubuntu-latest` label.


With the addition of GPUs, you can now specify a specific set of` runs-on` labels that we provision for you to map down to custom resource classes in your AWS account. Maybe you want to use an` nvidia-tesla-k80` for your machine learning workflows, or an` nvidia-tesla-v100` for your CUDA builds. We can configure these labels to map down to your own set of GPU resources.


## Getting started


If you're not currently using Depot, you can[sign up for a free trial](https://depot.dev/sign-up) to get up to 40x faster builds for Docker images and GitHub Actions.


For folks looking to test drive our new GPU capabilities, you can reach out to us directlyvia email or in our[Community Discord](https://discord.gg/MMPqYSgDCg) .


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
