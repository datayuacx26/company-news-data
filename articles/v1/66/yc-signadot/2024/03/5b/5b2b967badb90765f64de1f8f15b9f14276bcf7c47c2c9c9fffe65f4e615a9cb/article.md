---
schema_version: "1.0.0"
document_id: "5b2b967badb90765f64de1f8f15b9f14276bcf7c47c2c9c9fffe65f4e615a9cb"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/improve-developer-velocity-by-decentralizing-testing/"
published_at: "2024-03-23T23:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:4a2bf486eef6b78b5cce4997d3a2dcf364caae4538d7f5f77f95662dda0dca94"
---

# Improve Developer Velocity by Decentralizing Testing

Shifting testing left is the best way to eliminate the inefficiencies caused by today’s decentralized microservices testing environment.


As I discussed in a[recent article](https://www.signadot.com/blog/we-need-a-new-approach-to-testing-microservices/) , centralized testing interferes with developer velocity. In a[microservices-led model](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) , centralized testing has become something of a “pinch point” for release processes, since developing code and managing production operations have been effectively democratized and their functions isolated on smaller teams. The issue is not one of bad tools. In fact, modern testing tools give teams amazing abilities to detect problems that would have previously been uncovered by end users. The issue is that too many problems aren’t discovered before final end-to-end (E2E) and acceptance testing.


This is understandable, as modern microservice interdependencies and dependencies on outside APIs make it harder than ever to simulate how code will run on production. While preproduction testing on staging is supposed to be the phase where only rare, emergent failures are detected, it’s often now the stage where you get the first clear indication of whether code works at all. I remember when staging was the most reliable place to run code since only well-vetted releases were run there — and we didn’t have the scale issues of production. But from reading[concerns](https://www.reddit.com/r/devops/comments/16carej/how_many_environments_is_enough/) expressed in the Reddit[r/qualityassurance](https://www.reddit.com/r/QualityAssurance/) and[r/softwaretesting](https://www.reddit.com/r/softwaretesting/) forums, it seems like for many teams, staging has become extremely unreliable, as many releases are delayed by defects.


## 6 Reasons Centralized Testing Slows Developer Velocity


Centralized testing can significantly[hinder developer velocity](https://www.signadot.com/blog/7-reasons-developer-experience-is-a-strategic-priority/) . Let’s break down the issues associated with this approach.


- **Batched deployments on staging:** When code changes from several teams or microservices are batched together and deployed on a staging environment, it creates a bottleneck. This approach delays the integration of new code, making it difficult to identify which change caused a problem if an issue arises.


*This is why batching is quickly becoming the norm.*


- **Testing frequency and commit freezes:** If the batch is tested infrequently and new commits are disabled during this period, it leads to a significant delay in the feedback loop. Developers have to wait longer to see how their changes perform in a quasi-real environment, slowing down the overall development process.
- **End-to-end testing:** When E2E testing is conducted only by a QA team, it often turns into black-box testing. Not knowing how the underlying system works means the likely points of failure aren’t tested as thoroughly as they could be. While E2E testing is crucial, relying solely on a QA team for this can create a disconnect between what is developed and what is tested.
- **Bug reporting and resolution processes:** When bugs are found, they need to be filed formally, then developers must reproduce and fix them. This process is inherently slow. The time taken to file, assign, reproduce, fix and then retest bugs can be substantial, especially if the bug is elusive or intermittent. Further, with the black box problem mentioned above, engineers running the tests can only describe behavior without having knowledge of the underlying system. This adds friction to the bug-reporting process.
- **Late feature acceptance testing:** When feature acceptance testing happens late in the development cycle, it can lead to steep delays. If feedback or required changes come at this stage, it can mean significant rework for developers. This not only slows the release of the current feature but also impacts the development schedules of other features.
- **Cumulative delays and reduced morale:** These delays can accumulate, leading to longer release cycles. This not only affects the business by delaying the time to market but can also reduce the development team’s morale. Developers often prefer quick feedback loops and seeing their work in production as soon as possible.


Though I think it’s important to list these drawbacks, I don’t think anyone is explicitly in support of “highly centralized testing” or “testing that happens only on staging/test environments.” No one sets out to break the reliability of developers’ unit and E2E tests, but the complexity of emulating a production cluster for each developer has produced this outcome. (My previous article describes in detail how this system evolves.)


## How to Decentralize Testing Again


What we want is to shift testing left: letting realistic testing[start right at the pull request (PR) stage](https://www.signadot.com/blog/the-struggle-for-microservice-integration-testing/) rather than waiting to do it when a separate team is working with our code. At teams like[Uber, DoorDash](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) and[Lyft](https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/) , platform engineers have built tools to let developers test earlier in a more realistic environment. At these companies, the solution isn’t to fiddle with a so-called “developer environment” that never really represents reality, but rather to give all users access to a single shared cluster that is kept very very close to the Production state.


Specifically, these platform engineers are using[request isolation](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) to let a single test version of a service (or a set of services if need be) interact with a cluster without colliding with others’ experiments. At Uber, this system is called a SLATE, and it isolates test services, letting them interact with specially tagged requests while otherwise relying on production services. (Yes, this allows Uber Engineering to test in production, but it has a ton of safeguards.)


However it is implemented, this system lets developers test their code against the cluster’s other dependencies earlier in the replication process. In years past, this capacity was really only open to enterprise teams with large, dedicated platform engineering teams. With a service like[Signadot](https://www.signadot.com/docs/overview) , it’s possible for large teams to implement request isolation and shift testing left with a standard set of tools to isolate requests.


This can also lead to cultural shifts within an organization: Empowered to run E2E and acceptance testing earlier, development teams can integrate expertise that was previously concentrated in QA teams. We may even see QA engineers shifting left as they help product engineers test more things, more accurately and sooner.


## 5 Benefits of Decentralized Testing


A request isolation system for developer experimentation involves a significant technical lift and isn’t as simple as deploying a few open source packages. It also requires integration with CI/CD tooling. This isn’t the focus of this article, but I mention it since the benefits are worth listing before you embark on this journey.


- **Earlier tests against accurate dependencies:** Rather than attempting to replicate some version of the cluster, a shared cluster with request isolation lets each team test independently with the most up-to-date, stable version of other teams’ work.
- **White box testing:** With developers testing the code they wrote, they can understand more quickly what might be causing problems, and their knowledge of what’s changed makes it easier to know where to focus testing.
- **Testing can span types:** Functional and nonfunctional tests can run at the same time, such as acceptance testing running along with monitoring memory leakage, CPU usage and performance testing.
- **Devs can group PRs as needed:** A service like Signadot lets you select multiple PRs to work with. So if Team A and Team B have synchronous changes, both can be tested together before QA gets involved.
- **No bug filing:** This soft, intangible benefit is really one of the biggest boosts to developer productivity. Without needing to manually document each problem and send it to another team, the dev who first wrote the feature can work on fixing the bug right away.


A newer decentralized testing approach that puts more accurate clusters in the hands of developers has a number of advantages that lead to a better development velocity and a happier team.


## The Goal Isn’t to “Fix” Testing, but Incrementally Improve Quality


While there’s a lift to implementing a system like request isolation, it has one huge advantage over making changes to a cluster architecture or environments where code is tested and run: it’s possible to adopt incrementally. Think about it: before production, every engineering team has a cluster that’s highly accurate but they don’t want to break by pushing experimental code to the services. With request isolation and smart request routing, it’s possible to test PRs in this cluster, even if only your team has access to such a system. You won’t be breaking the underlying cluster with your experiments, so small groups can try this system before it rolls out to the whole team.


## How Signadot Can Help


Signadot allows you to validate every code change independently. By connecting to PRs in your source control, every PR can get a request-isolated space within your cluster to test how this new version will interact with the rest of the cluster.


If you’d like to learn more, share feedback or meet the amazing community already using request isolation to accelerate their developer workflows, join the[Signadot Slack community](https://signadotcommunity.slack.com/ssb/redirect#/shared-invite/email) to connect with the team.
