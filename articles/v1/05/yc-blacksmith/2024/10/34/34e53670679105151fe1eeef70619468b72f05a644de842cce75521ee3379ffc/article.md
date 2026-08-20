---
schema_version: "1.0.0"
document_id: "34e53670679105151fe1eeef70619468b72f05a644de842cce75521ee3379ffc"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/deployments-still-suck-but-maybe-they-dont-have-to"
published_at: "2024-10-29T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:0b25e62186839bfa1b1e8a490b31a2da3529d04da99ea40c51db494c4f43ffc4"
---

# Deployments still suck, but maybe they don’t have to

Deploying apps has certainly gotten a lot simpler over the years, but it still somehow feels like a half-solved problem. This is especially true on the hyperscalers of today (AWS, GCP, Azure). You spend a bunch of time building this awesome new web app, and then when it's time to show it to the world, you hit a wall of DevOps nonsense that has nothing to do with your actual code.


## The AWS deployment nightmare


Take deploying a basic Rails app to AWS ECS. Sounds simple enough, but here’s the mess you have to deal with:


1. Dockerize your app (easier now with nixpacks, or even LLMs)
2. Set up ECR because you need somewhere to store those Docker images
3. Create an ECS cluster (whatever that means)
4. Figure out task definitions and services (more AWS jargon, yay!)
5. Set up a load balancer because apparently that's your job now
6. Configure security groups and IAM roles (hope you like security headaches)
7. Set up a database (probably RDS, make sure you set it up with the right VPC / security group so that your web app can access it)
8. Deal with secrets management (AWS Secrets Manager or Parameter Store, take your pick)
9. Set up CloudWatch because you need to know when stuff breaks
10. Configure Auto Scaling because your app might get popular (optimism!)
11. Set up a CI/CD pipeline (CodePipeline? GitHub Actions? Have fun choosing!)


And that's just the start. We haven't even talked about CDNs, SSL certs, or how to roll back when you inevitably break something at 2 AM on a Saturday.


## Heroku: The hero we needed


Back in 2007, Heroku showed up and was like, "Hey, what if deploying wasn't absolute torture?" Their pitch was simple: "git push" and you're done. No more infrastructure headaches, no more cursing at AWS documentation at 3 AM. Just code and deploy.


It was a hit. The value prop instantly resonated, and suddenly a whole bunch of companies popped up trying to do the same thing. PaaS was the new hotness.


Heroku's success wasn't just about making deployments easy. They also provided a whole ecosystem of add-ons for databases, caching, monitoring, and more. It was a one-stop shop for getting your app up and running without having to become a part-time systems administrator.


Over the years, many alternatives came to market, each with their own attempts at differentiation. Some of these alternatives (like[FlightControl](https://www.flightcontrol.dev/) , or[Porter](https://www.porter.run/) ) are built on top of AWS, which decouples *their* reliability with the reliability of their customers’ apps. Others, like[Fly.io](http://fly.io/) or[Railway.app](http://railway.app/) , built out their own hardware fleets and tried to leverage their control over the hardware into better DX for customers (sometimes, perhaps with questionable reliability).


## When PaaS becomes a pain in the ass


But here's the thing about all these PaaS options - they're great until they aren't. These platforms optimize for the average use case, which is fine if you're building yet another CRUD app. But step outside those boundaries, and you're in for a world of hurt.


### Off the beaten path


For instance, most PaaS platforms do not support deploying a gRPC service. Or maybe you need to do something specific for compliance reasons. Or you're working with some bleeding-edge tech that doesn't play nice with their predefined stacks.


Suddenly, you're faced with a choice: contort your app to fit their mold, or say "forget it" and go back to managing your infra on AWS. Neither option is great. Some platforms, like` Porter` , make it easy to “eject” out of their platform simply by exporting a Kubernetes deployment manifest that you can control going forward.


### Deployment management


Another gap these platforms exhibit is *more granular deployment management.* Frequent deploys are generally a good idea for most services. For instance, if a bug is found in production, your devs are more likely to have all the context “paged in” and are likely to be able to fix the bug faster if they worked on it just days before. Frequent deployments also allow engineering teams to stay nimble and win customer trust by rolling out fixes to bugs, or tiny feature requests quickly. However, actually maintaining a high deployment frequency often requires painstaking care to avoid trading off reliability for. As a result, as teams grow, they have to either lower their deployment frequency or embark on a long (and often one-way!) journey towards microservices.


Branch deployments refer to the idea of deploying non-default branches directly to staging or production after review and CI but before merging to main. The core idea behind this is that it introduces a deeper link between the developers building a feature and the actual deployment of said feature. The deployment and its follow-up responsibility ultimately remain with the person who owns the code.


[Cased](http://cased.com/) is an interesting new platform that enables this workflow and encourages some best practices that have emerged from its founders’ experience[growing GitHub](https://cased.com/about) . Cased doesn’t stop here though — by connecting Cased with your existing monitoring tools (like Datadog, or Sentry), Cased will automatically detect and track anomalies associated with your deployments and even summarize them with an LLM.


Cased also comes with a rich notification subsystem that lets stakeholders know when a feature they care about is ready to be deployed to a staging or production instance.


## The future


Right now, we're all playing mix-and-match with deployment tools. GitHub Actions for CI, Datadog for metrics, Cased for branch deployments, and your PaaS of choice for infrastructure. Each new tool means another set of credentials, another dashboard, and another bill to pay.


But maybe there's a better way. The success of Vercel in frontend deployments hints at a future where we don't need to cobble together five different services just to get a decent deployment pipeline. A single, opinionated platform that handles everything from CI/CD to monitoring, while maintaining the flexibility we need for modern applications.


Sure, it means trading some choice for convenience. But after a decade of stitching together deployment pipelines, that's starting to sound pretty good.
