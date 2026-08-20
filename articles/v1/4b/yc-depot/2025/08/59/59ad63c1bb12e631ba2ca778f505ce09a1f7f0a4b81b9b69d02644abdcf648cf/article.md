---
schema_version: "1.0.0"
document_id: "59ad63c1bb12e631ba2ca778f505ce09a1f7f0a4b81b9b69d02644abdcf648cf"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/hidden-cost-of-self-hosting-ci-runners"
published_at: "2025-08-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:1832ece27ac4896e899910a6252aeb13834aa3ed1144862e7e912c04561aa40d"
---

# The hidden cost of self hosting CI runners

Managing your own CI infrastructure sounds appealing in theory. You get complete control, can optimize for your specific workloads, and avoid vendor lock-in. But after months of wrestling with[self-hosted GitHub runners](https://depot.dev/blog/self-hosting-github-actions) , many DevOps teams have to learn the hard way that the operational overhead can seriously outweigh the benefits.


Here are a few things that typically happen when teams go it alone, and where a managed solution might be a better fit.


## Hidden time drains


1. AMI maintenance becomes a not unsubstantial timesuck. Security compliance typically requires updating AMIs at least monthly, and that time patching, testing, and rolling out updates adds up. What started as "just keeping things current" can turn into a part-time job for your platform team.
2. Rollouts are more than a little scary. With non-ephemeral runners, every deployment carried risk. One bad rollout can take down an entire CI pipeline, and rollbacks aren't always clean.
3. GitHub's Runner APIs are... quirky. It's easy to hit bugs like[this one](https://github.com/github-aws-runners/terraform-aws-github-runner/issues/3589) regularly. Runners might randomly disappear from GitHub's registry, leaving teams scrambling to figure out why builds were hanging. Other times, the APIs might sometimes just forget about perfectly healthy runners, forcing a manual cleanup.


## The cost optimization quagmire


1. Spot instances might save money, but they can cause bad DX. Sure, you might have lowered your bill, but try explaining to your engineering team why their builds are randomly failing because AWS reclaimed the instance mid-job. The developer productivity cost has to be judged against the infrastructure savings.
2. Do you really want your eng team spending their time going deep on scaling discussions? Getting scaling curves right is complex. Too conservative and you're wasting money on idle capacity. Too aggressive, and developers wait for runners during peak times. It’s surprisingly easy to spend countless hours in meetings debating scaling parameters instead of shipping features.
3. Commitments like AWS Savings Plans can secure significant discounts, but they also create financial lock-in, which means you might end up paying for weekend capacity you don’t actually use. Beyond that, a big financial commitment to one vendor can make it harder to experiment with other solutions, even when your current setup isn't working well.
4. The egress cost blindside. Without careful network architecture, data transfer costs can easily hit six figures annually. Every DevOps team has a story of learning this the hard (aka expensive) way when a gobsmackingly large AWS bill shows up.


## The perks of a managed CI


Contemplating a switch to a managed CI provider shouldn’t only be about avoiding things like operational overhead. There are plenty of perks that are really hard to achieve on your own:


1. Frequent and ongoing micro-optimizations across the entire stack. While your eng team is focused on your application code, your managed CI partner should be going deep on optimizing kernel parameters, disk I/O, network configurations, and container runtime performance, and shipping performance gains there regularly.
2. Deep domain expertise. Your CI provider might know about GitHub outages before GitHub's status page is even updated, and to flag any issues with proactive and detailed communication. Having true subject matter experts managing this infrastructure can free your team to focus on what they do best.
3. Operational support. Rather than scrambling to come up with the answer to the pressing question of ‘what caused this OOM error to pop up?’ by yourself, when working with a trusted managed CI provider, you’ve now got a team you can rely on (likely over Slack) to debug issues, share best practices, and get feature requests prioritized. It should feel like you’ve suddenly got an entire team of CI experts at your fingertips.
4. Cheaper, better, faster. It might be surprising, but despite all your "on-disk cheats" and optimizations, a managed solution can often exceed your self-hosted performance on both cost and speed.


## The Real Question


This isn’t just about self hosting versus using a managed CI provider. It’s about where you want your engineering team to spend its time.


If you enjoy tuning CI performance, have a platform team ready to support it, and want full control, self hosting might be the right call. But if your team would rather focus on building product and moving fast, the hidden costs of running your own CI stack can slow you down.


Sometimes the best technical decision is choosing a partner who cares as much about performance as you do. That's exactly what we built[Depot](https://depot.dev/blog/depot-github-actions-runners) for.


## FAQ


Is self hosting really cheaper than managed CI?


It depends. Raw compute might be cheaper, but hidden costs like AWS egress fees, idle runner time, and lost engineering hours often tip the scale in the other direction.


What takes the most time when managing your own CI infrastructure?


Maintaining AMIs, handling rollouts, debugging GitHub runner issues, managing spot instance interruptions, and debating scaling logic. All of this pulls engineers away from shipping code.


What do you get with a good managed CI provider like Depot?


You get speed, stability, performance tuning across the stack, and a team that helps you troubleshoot and improve. It frees your engineers to focus on what actually drives your business forward.


## Related posts


- [Self-hosted GitHub Actions runners aren't free](https://depot.dev/blog/self-hosting-github-actions)
- [Why platform teams are focused on CI/CD optimization](https://depot.dev/blog/why-platform-teams-are-focused-on-ci-cd-optimization)
- [How we cut GitHub Actions queue times by 4x](https://depot.dev/blog/reducing-queue-time-with-cached-schemas)
- [Introducing Depot managed GitHub Actions Runners](https://depot.dev/blog/depot-github-actions-runners)
- [Introducing Ultra Runners — Up to 3x faster GitHub Actions jobs](https://depot.dev/blog/introducing-github-actions-ultra-runners)


John Stocks


Head of Solution Engineering at Depot
