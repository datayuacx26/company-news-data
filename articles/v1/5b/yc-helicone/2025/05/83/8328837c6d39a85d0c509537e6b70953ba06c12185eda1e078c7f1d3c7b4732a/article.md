---
schema_version: "1.0.0"
document_id: "8328837c6d39a85d0c509537e6b70953ba06c12185eda1e078c7f1d3c7b4732a"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/self-hosting-journey"
published_at: "2025-05-07T00:00:00+00:00"
first_seen_at: "2026-07-21T22:44:44.297475+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:6ed9c7e22ddad00dad51c85df77bdc740b6b498edbe1899dce1f240a2d3d5fbc"
---

# How We Simplified Helicone's Self-Hosting in 30 Days

# How We Simplified Helicone's Self-Hosting in 30 Days


May 7, 2025


· 5 minute read


Juliette Chevalier


· May 7, 2025


After a month of engineering work, we're excited to share a big milestone in our journey: **a complete revamp of our self-hosting solution!**


Learn more about how to[self-host Helicone here](https://docs.helicone.ai/getting-started/self-host/docker) .


## The Problem: "Too Onerous"


Self-hosting has been part of our offering since we launched Helicone two years ago.


However, we recently received feedback that hit home - a potential customer told us our deployment was "too onerous" compared to competitors. **They passed on using our solution.**


And they were right.


Our original self-hosting architecture required managing twelve separate containers with complex configuration requirements.


For large enterprise clients with dedicated DevOps teams, this might be manageable, but it created an unnecessary barrier for many organizations who needed to keep their LLM data within their own infrastructure.


## Helicone's Cloud is HIPAA, SOC 2, and GDPR-Compliant


Keep your LLM data entirely within your infrastructure while gaining comprehensive observability with Helicone.


## The Solution: One Month of Focused Engineering


We decided to tackle this challenge head-on.


Over the course of one month, our engineering team completely rebuilt our self-hosting architecture, **reducing it from twelve containers to just four.**


The result? What once took days to set up now takes minutes.


```text
git  clone   https://github.com/Helicone/helicone.git
cd   docker
./helicone-compose.sh helicone up


```


That's it. Your Helicone dashboard is now available at` localhost:3000` .


-- To learn more about how to self-host Helicone, check out our[documentation](https://docs.helicone.ai/getting-started/self-host/docker) .


## The Technical Journey


### Moving Away from Supabase


The biggest architectural change was **moving away from Supabase for our self-hosting solution.**


When we first built Helicone, we chose Supabase to help us move quickly from zero to one. It was an excellent tool for getting started quickly.


> When building stuff, you're a startup and you just want to move really quickly. We always take the 90-10 solution.


- Justin Torre, CEO of Helicone


However, Supabase comes bundled with multiple services, many of which weren't necessary for our core functionality. This added complexity to our deployment process and increased the resource requirements for self-hosting.


We spent a significant portion of the month rewriting our backend to eliminate this dependency, replacing it with simpler, more focused components.


### Architecture Simplification


Our new architecture consists of just four essential services:


1. **Main Application Container** : Handles the core Helicone functionality
2. **ClickHouse Database** : For fast, efficient storage and querying of LLM request data
3. **Authorization Container** : Manages user authentication and access control
4. **Mailer Container** : Handles email notifications


This streamlined approach not only simplifies deployment but also reduces resource requirements.


**A T2 medium EC2 instance is sufficient to handle about 90% of typical workloads - easily scaling up to a million logs per day.**


Your browser does not support the video tag.


## Scaling Considerations


While the Docker Compose setup is perfect for most use cases, we've also developed Helm charts for organizations needing enterprise-level scalability.


These charts allow you to connect Helicone to an Aurora database or a dedicated ClickHouse cluster, enabling horizontal scaling for organizations processing billions of logs.


> We spent a lot of time working on performance and making sure that when you load up Helicone and it's instant. If you have millions or billions of logs, which some of our customers do, all of those metrics and all those filters get aggregated instantaneously, materialized really quickly.


- Justin Torre, CEO of Helicone


For context, you'd typically need this level of scaling **when you're spending around $100,000 per month on OpenAI or similar services** - a good problem to have!


## Lessons Learned


This project reinforced some valuable engineering principles:


1. **The 90-10 solution** : As a startup, we always try to optimize for speed to market. However, this experience reminded us that sometimes taking a bit more time upfront can save significant effort later.
2. **Custom interfaces** : One thing that would have helped us tremendously would have been wrapping our services within our own custom interfaces that we could re-implement later. This would have made the transition away from Supabase much smoother.
3. **Choose infrastructure wisely** : In retrospect, simply using AWS Aurora from the start wouldn't have been much more effort than integrating with Supabase, but would have saved us significant re-architecting work.


## Who Benefits from Self-Hosting?


Self-hosting is particularly valuable for:


- **Security-conscious organizations** : Companies that need complete control over their data environment
- **Privacy-focused teams** : Organizations working with sensitive information that cannot leave their infrastructure
- **Regulated industries** : Financial services, healthcare, and government agencies with strict data residency requirements


## Continuous Improvement


We're committed to keeping our self-hosted solution up to date. Our Docker images are updated multiple times per week and published to our[Docker Hub](https://hub.docker.com/u/helicone) .


For users who have deployed using our instructions, staying current is as simple as:


```text
git pull
cd   docker
./helicone-compose.sh helicone up


```


For Helm chart users, the latest containers are published to Docker Hub regularly.


## What Helicone Enables


Beyond just observability, self-hosted Helicone provides four key capabilities:


1. **Comprehensive Observability** : Track metrics, monitor costs, and analyze agentic sessions step-by-step
2. **Governance Layer** : Add caching, rate limiting, and other controls to your LLM usage
3. **Experimentation** : Test production data in a playground environment to compare models and optimize prompts
4. **Performance** : Instant aggregation and visualization of metrics, even with billions of logs


## Try It Today


## Deploy Helicone Self-Hosting Today


Keep your LLM data entirely within your infrastructure while gaining comprehensive observability.


Whether you're handling sensitive financial records, HIPAA-protected healthcare data, or just want complete control over your LLM analytics, our new self-hosting solution makes it easier than ever to bring Helicone's capabilities within your own security perimeter.


We're proud of how far we've come and excited to make Helicone more accessible to teams that prioritize data sovereignty without sacrificing powerful observability.


Visit our[documentation](https://docs.helicone.ai/getting-started/self-host/docker) to get started, or[reach out to our team](https://helicone.ai/contact) for guidance on optimal configuration for your specific needs.


### You might find these useful


- [5 Powerful Techniques to Slash Your LLM Costs](https://www.helicone.ai/blog/slash-llm-cost)
- [Complete Guide to Monitoring Local LLMs with Llama and Open WebUI](https://www.helicone.ai/blog/monitoring-local-llms)
- [How to Test Your LLM Prompts (with Helicone)](https://www.helicone.ai/blog/test-your-llm-prompts)


## Frequently Asked Questions


### Is Helicone Self-Hosting compatible with all LLM providers?


Yes, Helicone Self-Hosting works with all major LLM providers including OpenAI, Anthropic, Gemini, as well as open-source models you may be running locally.


### What are the infrastructure requirements for self-hosting Helicone?


Helicone Self-Hosting is designed to be lightweight and can run on modest hardware. For most implementations, a T2 medium-sized EC2 instance is sufficient.


### Does self-hosting impact any features compared to the cloud version?


No, Helicone Self-Hosting provides all the core observability features of the cloud version, including request tracking, cost optimization, and analytics. The only difference is that everything runs within your own infrastructure and you need to maintain it as it upgrades.


### How can I update my self-hosted Helicone when new updates are released?


Almost every week we release a new version of Helicone which you can find in our Docker Hub repository: https://hub.docker.com/u/helicone. You can easily update your self-hosted Helicone by pulling the latest changes from our GitHub repository and rebuilding the containers.


---


### Questions or feedback?


Are the information out of date? Please[raise an issue](https://github.com/Helicone/helicone/issues) or[contact us](https://www.helicone.ai/contact) , we'd love to hear from you!
