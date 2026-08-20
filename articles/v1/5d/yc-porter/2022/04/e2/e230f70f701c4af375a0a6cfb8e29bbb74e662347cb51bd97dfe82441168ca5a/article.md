---
schema_version: "1.0.0"
document_id: "e230f70f701c4af375a0a6cfb8e29bbb74e662347cb51bd97dfe82441168ca5a"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-7cfcee1b8c2a"
canonical_url: "https://www.porter.run/blog/why-companies-move-off-heroku-besides-the-cost"
published_at: "2022-04-27T00:00:00+00:00"
first_seen_at: "2026-07-22T09:55:21.208768+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:e77c5cecb56965ae1e13dc72971a3e94ef7ef972873e83a5a1e5d2f1ca746863"
---

# Why companies move off Heroku (besides the cost)

In light of multiple recent outages, much has been said about the love-hate relationship many engineering teams have with Heroku. Depending on who you ask, Heroku has been in steady decline - or at the very least, has stagnated at the technological level - since its acquisition by Salesforce in 2010 (see[this article](https://www.infoworld.com/article/3614210/the-decline-of-heroku.html?ref=blog.porter.run) for an excellent overview).


‍


At this point, Heroku's exorbitant pricing (especially with Private Spaces and Heroku Shield) is well-documented on threads across Hacker News and Twitter. For most teams that migrate off Heroku, however, cost is often just one part of the equation - and we should know. Many of our users switch to Porter directly from Heroku, and to date our team has migrated over 150 Heroku applications as part of our[migration program](https://blog.porter.run/heroku-migration-program/) .


‍


As such, we've decided to compile the most common technological reasons companies decide to move off of Heroku. Needless to say, this is just from what we've empirically observed, and the reasons have been listed in no particular order.


## No. 1 - Lack of resource controls


Resource limitations manifest themselves in two primary ways on Heroku: (1) fixed dyno types and (2) restricted autoscaling.


‍


First, Heroku only provides six basic dyno types with no ability to adjust available CPU:


**Source: https://devcenter.heroku.com/articles/dyno-types**


Even for Heroku Enterprise users, the pickings are still slim:


**Source: https://devcenter.heroku.com/articles/heroku-enterprise#available-dyno-types**


While chances are that there is some Heroku dyno that can accommodate the resource demands of a given service, little to no room is available for optimization. As engineering teams mature and scale, these inflexible resource constraints become a significant area of unnecessary overhead.


‍


In addition, autoscaling is only made available on[Performance-tier and Private Spaces dynos](https://devcenter.heroku.com/articles/scaling?ref=blog.porter.run) . Aside from arbitrarily restricting scaling to specific dyno types, Heroku also provides extremely limited controls over the scaling policy. Heroku's built-in autoscaler only allows dynos to scale based on p95 response time, and strict[global scaling limits](https://devcenter.heroku.com/articles/dyno-types?ref=blog.porter.run#default-scaling-limits) are enforced across all dynos.


## No. 2 - Networking limitations


Many teams need their applications to integrate with third-party services that require a static IP to whitelist (we frequently see this for fintech companies, data management tools, and other services that require direct access to their users' environments).


‍


Unfortunately, Heroku's Common Runtime provides no native support for static egress IPs. Here's an explanation from QuotaGuard:


> *"Heroku does not provide Static IP addresses in the Common Runtime Environment. In Heroku Private Spaces, which costs thousands of dollars a month, Static IP’s are available, but they aren’t load balanced, run on dedicated proxies, or highly customizable." --*[QuotaGuard](https://www.quotaguard.com/how-to-get-a-heroku-static-ip/?ref=blog.porter.run)


Yet while custom add-ons (like QuotaGuard) can offer a more robust solution for static IPs on Heroku, these can cost over a thousand dollars a month for unlimited traffic. By comparison, on a service like Porter, static egress IPs come out of the box and are cluster-wide.


‍


Also, for users not on Heroku Private Spaces (which itself incurs a significant fee just for an isolated VPC), many add-ons have to be publicly exposed, including services like databases and caches.


## No. 3 - Limited regions


Heroku only provides two shared and six private spaces regions for running user workloads. While this is usually fine for applications that are just starting out, as compliance and performance needs evolve, the lack of regional optionality can become a significant bottleneck.


‍


For teams with certain data handling requirements (especially in Europe), Heroku might simply not offer a viable region for hosting critical services. With regards to performance, the inability to deploy applications to a region closer to the majority of users might also introduce undesirable latency for user-facing workloads.


‍


While this bleeds into the next reason, lack of control over which regions an application can be deployed to also results in decreased tolerance to regional outages in AWS (which Heroku relies on under the hood).


## No. 4 - Frequent outages


While some Heroku outages are owing to AWS, many issues are solely due to Heroku’s own infrastructure. Just a month ago, all common runtime apps on Heroku were down for an hour due to an[invalid DNS config change](https://status.heroku.com/incidents/2402?ref=blog.porter.run) on Heroku's end.


‍


More recently, all GitHub-based deployments (which is to say, virtually all deployments) to Heroku were blocked and review apps were halted for all users as a result of a GitHub OAuth token leak. As of writing this, the Heroku team is still[actively investigating](https://status.heroku.com/incidents/2413?ref=blog.porter.run) the security incident to identify the root cause.


## Conclusion


While a few honorable mentions were omitted from this list (e.g. lack of support for crons/jobs, inflexible timeouts and connection limits, etc.), the above are the most common technological reasons we see teams migrate off Heroku.


‍


Often, however, the decision to leave Heroku is as much strategic as it is related to specific technical considerations. For most growing engineering teams, the question of moving from Heroku to a cloud provider like AWS or GCP isn't a matter of "if" but "when."


‍


While the platform is still an excellent way to deploy initial services with minimal overhead for small teams and individual developers, there inevitably comes a point as a company grows where Heroku holds back developers more than it helps them.


‍


*Looking to get the Heroku experience in your own cloud? Learn more about our migration service by visiting*[migratefromheroku.com](https://migratefromheroku.com/?ref=blog.porter.run) *.*[Subscribe](https://blog.porter.run/#/portal) *to be notified about our next article.*
