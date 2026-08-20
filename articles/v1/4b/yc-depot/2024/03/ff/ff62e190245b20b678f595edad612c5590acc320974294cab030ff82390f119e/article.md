---
schema_version: "1.0.0"
document_id: "ff62e190245b20b678f595edad612c5590acc320974294cab030ff82390f119e"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-github-actions-runners"
published_at: "2024-03-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:2c36a44d12ed86011c8461789798e595f6a61ac49b0a575b4c59c0db438cc45a"
---

# Introducing Depot managed GitHub Actions Runners

Depot launched[almost two years ago](https://depot.dev/blog/introducing-depot) , and today Depot builds container images up to 40x faster, makes them easier to debug, and integrates with all your existing tool chains.


However, Docker image builds are only one step of the build process. Waiting for slow builds is frustrating, and we've spent a lot of time waiting for builds of all types when trying to ship a feature, deploy a bug fix, or just not interrupt momentum.


We've been working to bring Depot's performance to **all builds** . Today, we're excited to announce the release of[Depot-managed GitHub Actions Runners](https://depot.dev/products/github-actions) 🚀


## Problems with GitHub Actions Runners


GitHub Actions is a powerful CI/CD platform, but it has flaws:


- GitHub-hosted runners are often slow; job execution often doesn't have the fastest CPUs and tend to have slow single-core and multi-core performance
- The time to pick up jobs can vary drastically, from a few seconds to almost a minute — just to start the GitHub Actions runner, let alone run the job
- Cache performance, both upload and download, is pretty poor (at ~145 MiB/s)
- Network performance can bottleneck a build's interactions with artifact hosts, container registries, or the internet at large, either because GitHub-hosted runners have a throughput limiter, or due to network throughput and peering from European datacenters
- Self-hosting runners requires overhead to deploy, maintain, and scale them, something that not all teams want to deal with


The tl;dr is that GitHub-hosted Actions runners are solid, but they're not fast, limited in resources, and not necessarily cheap once you get outside default runner sizes.


## Introducing Depot managed GitHub Actions Runners in AWS


Since launch, we've been building the key components to make Docker image builds fast, and we're now applying that same platform to the GitHub Actions service.


Our GitHub runners are fully managed and deploy in our cloud or in your AWS account, and they provide the same benefits as our Docker image builds for GitHub Actions jobs:


- **Faster compute:** We are launching with GitHub Actions Runners running on **4th Gen AMD EPYC Genoa** CPUs that are **30% faster** than GitHub's native runners.
- **Faster cache:** Depot runners automatically integrate with our existing distributed cache for cache upload and download speeds up to **1000 MiB/s** , resulting in **10x faster cache** with no additional configuration or actions needed in your workflows — no 10GB cache limit either.
- **Faster network:** Our GitHub Actions runners are running in AWS` us-east-1` region for close data locality to your repositories and the GitHub API with **12.5 Gbps** of throughput.
- **Faster runner start time:** We've optimized our runner start time to be **sub 5 seconds** for most runners, and **sub 10 seconds** for the rest.
- **Single tenant:** Our GitHub Actions runners are single tenant, meaning you don't have to worry about noisy neighbors or runners being reused across builds.
- **Fully managed:** No concurrency limits, no cache size limits, no network limits, and no maintenance overhead. Run your GitHub Actions jobs in our cloud or your AWS account, fully managed by Depot's control plane.
- **Half the cost:** Our runners are half the cost per minute of GitHub-hosted runners. Minutes included with your plan can be used with any size runner you choose, larger runners included
- **Billing by the second:** We bill by the second, so you only pay for what you use. No rounding up to the nearest minute or one minute minimums.


Depot-powered GitHub Actions Runners are faster, come without limitations on concurrency or cache, are more reliable, more cost-effective, and are fully managed in our cloud or your AWS account.


## How to get started


To get started with our GitHub Actions Runners, you can[sign up for Depot](https://depot.dev/) and connect your GitHub account to install our GitHub app. Once connected, you can add a Depot label in your` runs-on` to use Depot runners in your workflow file.


```text
name: Build
-runs-on: ubuntu-22.04
+runs-on: depot-ubuntu-22.04
```


Check out our[GitHub Actions documentation](https://depot.dev/docs/github-actions/overview) for a step-by-step guide to configuring Depot runners in your GitHub Actions workflows.


### How it works


Under the hood, installing our GitHub app in your organization allows us to launch a GitHub Actions runner in response to webhook events that are triggered by your jobs on either public repositories or private repositories. When a workflow run is initiated, the jobs within that workflow trigger webhook events that we listen for. When we receive an event for a new job, we launch a new runner and register the runner machine inside of your default runner group.


Once the runner machine is online and registered, GitHub sends the job down to the new runner where we are running the standard runner image GitHub provides. When the job is complete, we receive a webhook event from GitHub as such, mark the machine to be deleted, and the GitHub Actions runner is then killed and never reused.


We maintain a fleet of larger runners and default 2 CPU runners so that you get the fastest job execution possible without any additional configuration outside of the` run-on` label change.


## One more thing...


Our GitHub Actions Runners and our existing Docker image builds live in the same infrastructure! By combining our image builds with our GitHub Actions runner in the same private network, you can get even faster Docker image builds. This is particularly relevant when building an image and using --load to pull it back into your workflow for additional testing.


But it also extends to using a Docker container for your actual job execution as well. You can use out Depot accelerated container image builds to quickly build the Docker image that your GitHub Actions job will execute in, all without leaving Depot infrastructure or our fast network links. Check out our blog post on[running GitHub Actions jobs in a container built with Depot.](https://depot.dev/blog/github-actions-jobs-in-a-container)


## Conclusion


Accelerating both your Docker image builds and GitHub Actions pipelines is now possible with Depot! We're very excited to be bringing the performance of Depot into the GitHub Actions runner for both larger runners and default runners alike. We're planning to build a lot of exciting enhancements around this in the coming months, like GitHub Actions usage reports, default Docker build acceleration, Windows runners, MacOS runners, Arm runners, and much more.


If you're interested in trying out Depot, we have a 7-day free trial that gets you access to both our accelerated Docker image builds and our managed GitHub Actions Runners. No credit card required. If you have any questions, don't hesitate toreach out via email or pop into our[Discord Community](https://discord.gg/MMPqYSgDCg) to say hello.


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
