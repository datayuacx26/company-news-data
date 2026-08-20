---
schema_version: "1.0.0"
document_id: "90818639575ca570bc258041445fc280355d9c1f1b2b98b87a6cd4ed6ea29b63"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/why-platform-teams-are-focused-on-ci-cd-optimization"
published_at: "2025-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:415fc9d538b5d4e2661ca6ec844d5020f134a71b96b71a5acbfcc5edc8e62dfe"
---

# Why platform teams are focused on CI/CD optimization

71% of platform engineering teams say streamlining CI/CD pipelines is their top priority. That number climbs to 85% for mid-sized organizations, with large companies not far behind.


It tracks. The more mature the platform effort, the more painful slow builds become.


Meanwhile, platform engineering itself is gaining traction. 67% of organizations have either adopted it or are actively exploring it. As more teams shift away from scattered DevOps tooling, a clear pattern is emerging: CI/CD performance is where platform work starts.


[Build acceleration isn't magic. It's just good infrastructure. See how Depot fits into your CI/CD setup. Read the docs →](https://depot.dev/docs)


## The platform engineering revolution


Platform engineering has emerged as the discipline that bridges the gap between DevOps and developer productivity. While DevOps often pushes infrastructure responsibility onto developers, platform teams are building internal platforms that handle complexity behind the scenes so developers can stay focused on shipping.


Instead of every team owning their own Terraform scripts, CI pipelines, and environment configs, platform teams provide shared infrastructure that works out of the box. They standardize tooling, abstract away low-level decisions, and give teams a paved path to production that is fast, reliable, and secure.


The goal isn't to slow developers down with more processes. It's to give them fewer things to worry about and to fix the bottlenecks that tooling alone hasn't solved.


## Why CI/CD became priority #1


### Developers Are Spending Hours Waiting for Builds


The average Docker build now takes 5-15 minutes. Multiply that by several deploys a day, and developers can easily lose 30 to 45 minutes waiting, *every single day* . Across a team, that adds up fast.


The impact isn't just time lost. Slow builds impact everything:


- [GitHub Actions bills](https://depot.dev/blog/how-to-calculate-github-actions-usage) scale with team size
- Productivity drops
- Build wait times are the top complaint in developer experience surveys


Platform engineering teams recognize this as an infrastructure problem, not a process problem.


### Most teams already did the easy stuff


Basic automation is table stakes now. The real challenge is infrastructure-level optimization. Moving beyond "it works" to "it works fast." And that requires treating builds differently.


## The platform engineering approach: Builds are infrastructure


Platform engineers understand that build performance is an infrastructure concern. Just as you wouldn't accept slow database queries, slow builds represent an infrastructure bottleneck.


- **Traditional thinking:** "Builds are just part of development"
- **Platform engineering thinking:** "Builds are critical infrastructure"


This shift changes everything. Instead of asking each dev team to optimize their own builds, platform teams provide centralized build acceleration as a service.


### What this actually looks like


Smart platform teams treat build infrastructure with the same rigor as production infrastructure:


- Standardized build environments across all projects
- Performance monitoring and alerting for build pipelines
- Cost tracking and optimization at the infrastructure level


The key insight: platform teams track build performance as a core developer experience metric, not an afterthought.


[Builds are infrastructure. Optimize them like it. Try Depot free and cut your build times in half. Get started →](https://depot.dev/sign-up)


## Enter Depot: Build infrastructure that actually works


Here's where most platform engineering initiatives hit a wall. You can standardize configs and add monitoring, but you still need infrastructure that's actually fast.


```text
# Before
runs-on  :   ubuntu-latest


# After
runs-on  :   depot-ubuntu-latest    # 4x faster, better caching
```


Depot provides the build acceleration layer that makes platform engineering CI/CD strategies work:


### Infrastructure that performs


- 4x faster builds through optimized compute and caching
- Instant runner startup vs 2-5 minute[GitHub runner](https://depot.dev/products/github-actions) boot times
- 99.9% uptime SLA because builds are production infrastructure


### Integration that makes sense


- One-line config change:` runs-on: depot-ubuntu-latest`
- API-driven management for programmatic configuration
- Per-second billing with detailed cost analytics


### Developer experience that doesn't suck


- Zero workflow changes for dev teams
- Automatic optimization without developer intervention
- Clear performance metrics and cost visibility


## What success looks like


Teams that treat build performance as infrastructure see dramatic improvements. Not just in build times, but in deployment frequency, developer satisfaction, and infrastructure costs.


The question isn't whether to optimize CI/CD pipelines. It's how quickly you can implement infrastructure-grade build acceleration.


Because 71% of platform engineering teams are already working on this. The ones who get it right first will have a significant competitive advantage.


If you're ready to make that shift, Depot is built for it.[Give it a try.](https://depot.dev/sign-up)


## Related posts


- [How slow builds are costing you money](https://depot.dev/blog/slow-builds-are-costing-you-money)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Tori Elstrom


Head of Developer Marketing at Depot
