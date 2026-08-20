---
schema_version: "1.0.0"
document_id: "7ce656e3c377e321e2fa16dd7553f491d44e2234568bef9c3af79d5803ac7d74"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/self-hosting-github-actions"
published_at: "2025-04-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:d7eb2945723d0ef34f061f75116175964644c78001434e911e0d525d886130a0"
---

# Self-hosted GitHub Actions runners aren't free

We released[Depot GitHub Actions Runners](https://depot.dev/products/github-actions) a year ago. Our runners are anywhere between 3-10x faster with 10x faster caching as well. They come pre-configured with a lot of slick automatic add-ons, like RAM disks for faster disk access in jobs that need it, and automatic integration with our remote cache service for tools like Bazel, Gradle, Turborepo, and others.


Since launching, we've seen a lot of teams come to us from self-hosted GitHub Actions runners. Why? Because they're burned out from all of the operational overhead and complexity of it.


In this post, we highlight the problems and hidden costs with self-hosted GitHub Actions runners.


## Do you like operational overhead?


Self-hosting GitHub Actions runners isn't the "set it and forget it" option that folks may think it is.


The truth is that self-hosted runners increase your operational overhead. The entire system will have to be monitored and maintained. This includes things like:


-


**Maintaining AMIs** : You'll need to regularly update your AMIs with the latest security patches and updates. GitHub publishes all of the[runner image definitions](https://github.com/actions/runner-images) for Packer. But it's a time-consuming and error-prone process.


-


**Babysitting infrastructure** : You can choose between running ephemeral runners or persistent runners. Ephemeral runners are easier to maintain but can be slower to start up and may not be as reliable. Persistent runners are faster but require more maintenance and can be more prone to failure. Either way, you'll need to monitor the underlying infrastructure and rapidly fix any issues that arise.


-


**Debugging GitHub Runner API issues** : The GitHub Runner API has bugs and inconsistencies that can cause problems with your self-hosted runners. This can be frustrating to debug and a total time drain.


The overhead of self-hosting GitHub Actions is significantly more cumbersome and time-consuming than people expect when getting started.


## Free isn't actually free with GitHub Actions


The biggest misconception about self-hosting GitHub Actions runners is that it's free. This is simply not true. There are a lot of hidden costs associated with self-hosting GitHub Actions runners that can add up quickly.


While the cost per minute is $0, compared to the $0.008 per minute for GitHub-hosted runners, the reality is that self-hosting GitHub Actions runners can be significantly more expensive than you might think.


The most obvious cost is infrastructure. You'll need to pay for the instances, storage, network transfer, and any other resources you use to support the runners.


The infrastructure costs only become more problematic the larger your team and CI workloads grow over time.


Here are some common ways folks try to combat the rising costs:


### Leverage spot instances or savings plans to reduce instance costs


Spot instances are the first thing folks grab to reduce the cost footprint of self-hosted runners. These can certainly save you money, but you have to accept the risk of losing runners midway through a job, and live with the developer experience around that. The spot market can also be hit and miss, depending on the instance types you're requesting.


When Spot fails, folks often turn to AWS Savings Plans. This is a great way to save money, but now you're financially locked into self-hosted, and you end up paying for unused capacity on nights and weekends.


### Egress traffic


Egress traffic is another hidden cost that can add up quickly. If you're not careful with what you're sending out from the runner, you can spend truckloads of money on egress traffic.


### The human cost


This is hard to calculate out of the gate, but it's there from the beginning and throughout the entire time you choose to self-host GitHub Actions. Humans always have to maintain and babysit the infrastructure and glue components for anything self-hosted.


The reality is that self-hosting GitHub Actions runners can be a time sink. You'll need to maintain the infrastructure, monitor the runners, and troubleshoot any issues. You'll have to become an expert in GitHub Actions.


This takes time away from the critical stuff like building features and shipping products for your customers and users.


This also, ultimately, impacts the developer experience. Spending time dealing with CI issues can be frustrating and downright demoralizing for developers who want to get their work done. The constant context switching between working on the code that matters and dealing with CI issues or quirks like queue time can be a huge drain on morale. This is especially true if you're using self-hosted runners that are unreliable or slow.


## There are tools to help


There are solutions and workarounds. Before making the case for Depot, it's worth mentioning solutions that we have seen folks trying. While they have their own set of problems as well, they do provide advantages over going it entirely alone.


### [ARC](https://github.com/actions/actions-runner-controller)


Also known as` actions-runner-controller` , ARC is a project from GitHub that allows you to run GitHub Actions runners inside of your own Kubernetes cluster. It is designed to orchestrate and scale based on the number of workflows running across your GitHub repository, organization, or enterprise connections.


Effectively, the runners scale up and down as containers inside of your K8s cluster. This comes with tradeoffs like needing to manage docker-in-docker setups, choose autoscaling policies, and deal with the complexity of Kubernetes (which some teams may have no problem with and others find more complicated).


It *doesn't* solve the human cost of maintaining the infrastructure, the cost of the infrastructure itself, and it's still prone to the instability and quirks from GitHub's API.


### Self-hosted runners with the Terraform module:[github-aws-runners/terraform-aws-github-runner](https://github.com/github-aws-runners/terraform-aws-github-runner)


Formerly known as the` philips-labs` module, this strategy allows you to deploy self-hosted GitHub Actions runners on AWS using Terraform. Much like ARC, it provides the glue components in a self-contained module that you can deploy to your AWS account and get connected to your GitHub organization.


It runs on EC2 instances and defaults to creating ephemeral runners on spot instances. You can bring your own AMIs, and it has similar scaling policies to ARC that you can choose from. However, the infrastructure footprint is even larger than ARC in that there are a lot of glue pieces running in Lambda, EC2, and S3, and additional one-off things you have to configure, like a GitHub App to authenticate with GitHub. To date, it also only supports Linux and Windows runners.


It comes with all of the infrastructure and human costs of self-hosting GitHub Actions runners, but it provides a lot of flexibility and control over the infrastructure. The complexity and challenges of dealing with GitHub API outages, quirks, and optimizing for queue times still rests with you.


## What about Depot?


Depot is a build acceleration platform for engineering teams that want to build faster without the operational overhead, complexity, and time drain of doing it all themselves. We focus on accelerating GitHub Actions by providing our own runners optimized for performance, reliability, and interfacing with GitHub Actions.


If you use Depot GitHub Actions runners on our hosted platform, you get all of the following benefits:


- Up to 10x faster runners. We've optimized everything about the GitHub Actions runner to be faster. This includes things like fast I/O via ramdisks, faster caching, and automatic integration with our suite of remote cache supported tools like Bazel, Gradle, Turborepo, and others.
- Single tenant runners. We launch a runner within 3-5 seconds, and it is dedicated to your job. When your job is done, we nuke the runner from orbit so it will never be used again.
- No concurrency limits. Launch as many jobs as you'd like, we don't limit the number of jobs that you can run at a time.
- Per-second billing. We don't do the bizarre GitHub thing where they round up your 30 second build to 1 minute to charge you for it. We charge by the minute but track by the second. So you get two 30 second builds before we charge you 1 minute.
- No egress charges. We don't charge you for egress traffic. We don't charge you for ingress traffic either, but we don't have any limits on that.
- Infrastructure is our concern, not yours. You don't have to think about your infrastructure costs, wasted capacity on nights and weekends, network or storage costs, or any other hidden costs that come with self-hosting GitHub Actions runners. We take care of all of that for you.
- No human costs. You don't have to worry about maintaining the infrastructure, monitoring the runners, or troubleshooting any GitHub API issues. We take care of all of that for you.
- No operational overhead. You don't have to worry about maintaining AMIs, debugging GitHub Runner API issues, or dealing with the complexities of self-hosting GitHub Actions runners. We take care of all of that for you.


### You can self-host the data plane if you want


We also offer the ability to deploy the Depot data plane into your own environment. This means you get all the security & compliance benefits of self-hosted, leverage any compute commitments you have, and don't have any of the operations overhead of self-hosting GitHub Actions yourself. The pain and quirks of running GitHub Actions runners rest with us, and you get all of Depot's performance.


## Conclusion


Every team and organization is different. Some teams are happy to self-host GitHub Actions runners and deal with the overhead and complexity of it. Others are not.


Either way, having all of the information and thinking through the complexities and challenges, allows you to at least go into the process with eyes wide open and avoid any surprises. We personally believe that all builds, no matter whether they're GitHub Actions, Docker, Bazel, Gradle, or whatever, should be fast, reliable, and as easy to get started with as possible. You should get exponentially faster builds and never have to think about it again.


That's why we built Depot. We think Depot is the best way to accelerate your builds and get the most out of your GitHub Actions runners with whatever flexibility you prefer, whether it's self-hosted or in our cloud. If you're interested in learning more,[sign up for a 7-day free trial and give things a spin](https://depot.dev/sign-up) and if you have any questions or just want to bounce around ideas, feel free to hop in our[Community Discord](https://discord.gg/MMPqYSgDCg) .


## Related posts


- [How we launch GitHub Actions runners in 5 seconds](https://depot.dev/blog/github-actions-breaking-five-second-barrier)
- [macOS GitHub Actions Runners are now twice as fast](https://depot.dev/blog/ultra-runners-for-macos)
- [How we cut GitHub Actions queue times by 4x](https://depot.dev/blog/reducing-queue-time-with-cached-schemas)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
