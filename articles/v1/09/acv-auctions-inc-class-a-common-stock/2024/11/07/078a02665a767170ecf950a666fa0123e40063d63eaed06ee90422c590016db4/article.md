---
schema_version: "1.0.0"
document_id: "078a02665a767170ecf950a666fa0123e40063d63eaed06ee90422c590016db4"
company_key: "acv-auctions-inc-class-a-common-stock"
company: "ACV Auctions Inc."
source_id: "acv-auctions-inc-class-a-common-stock-rss-d00f1f014d5a"
canonical_url: "https://acv.engineering/posts/canary-releases/"
published_at: "2024-11-01T05:00:00+00:00"
first_seen_at: "2026-07-20T23:17:32.344046+00:00"
fetched_at: "2026-07-28T21:59:50.514445+00:00"
content_hash: "sha256:0de295034c648cf1b0d1918a521247f7a55c56d56de53f34a58bccb795a54564"
---

# Canary Releases

As more companies start to adopt continuous delivery (CD) processes, the ability to deliver features quickly while maintaining reliability becomes crucial. At the same time software engineering organizations often have hesitations when adopting CD best practices. These hesitations tend to center around the risk of instability, quality assurance, and customer impact. At ACV we utilize canary releases to help assuage these fears. This article will explain what canary releases are, how they work, and how ACV leverages them to support our continuous delivery process. Whether you are a seasoned developer or new to the world of DevOps, understanding canary releases can significantly improve your approach to continuous delivery.


## What Are Canary Releases?


Canary releases are a technique used to cautiously roll out new software to a subset of users before a more general rollout. The name of this kind of release is a reference to canaries in coal mines.[Until 1986](https://www.smithsonianmag.com/smart-news/what-happened-canary-coal-mine-story-how-real-life-animal-helper-became-just-metaphor-180961570/#:~:text=Throughout%20much%20of%20the%2020th,canaries%20in%20the%20coal%20mines.) , miners would bring canaries into mineshafts to function as a warning system for carbon monoxide, which is a clear, odorless, and poisonous gas common to the environment. If the canary collapsed, it was an indication that there was poisonous gas in the air and the workers needed to evacuate. In this way canaries, and the phrase, “canary in a coal mine” became synonymous with impending disaster.


With software development, the idea is that there are two versions of the software in production, the old stable version, and the new, potentially poisonous, version. The team directs a small group of users, the canaries, to the latest version of the software while the majority stay on the previous version. If there is a problem with the release, the problem only impacts or “poisons” the canaries and the team can easily switch the canaries back to the previous version while the team works out the bugs.


*Canary Releases by Posthog*


Software organizations may also call this technique a phased rollout or an incremental rollout and it is similar to blue green deployments. It is important to note that canary releases are different than A/B tests. While canary releases are an effective way to detect problems and regressions, A/B tests are a way to evaluate hypothesis using variant implementations. Canary releases should be short lived, while A/B tests may last months to collect enough data to determine a clear answer.


## How to Setup a Canary Release


At ACV we use feature flags to control our canary releases. The team deploys the latest version of the software in an off state. This means that the code is on production but there is no path to executing that code. Feature flags function as a bypass when deactivated. When the code executes, it skips the newly deployed code and blocks any execution paths to that code.


The cool thing with feature flags is that a percentage of users can have the feature flag on while the rest have it off. This allows us to redirect a percentage of users to the new code. If the new code has bugs, all we do is turn the feature flag off and all users go back to the previous version. This gives us confidence that even if our suite of automated tests fails to detect bugs, and we release bad code to production, we can minimize the impact while also having a simple rollback strategy.


We find that implementing canary releases at ACV helps us address the hesitations around continuous delivery. The team has an effective strategy to minimize customer impact when they keep the canary group sufficiently small. In addition, our canary users end up contributing to our monitoring and feedback loops. This builds closer relationships between the developers that are writing the code and the users that use the code.


One of the greatest benefits of the canary release strategy is that it does not require a perfect quality assurance program or high-risk tolerance for instability. Instead, the strategy puts the focus on being able to recover quickly. Software developers can even automate this recovery by tracking business metrics. A statistically significant regression in these metrics can trigger the deactivation of a feature flag.[IMVU](https://engineering.imvu.com/2010/04/09/imvus-approach-to-integrating-quality-assurance-with-continuous-deployment/) pioneered this technique and called it cluster immune systems.


## Areas of Caution


While we have found the canary release strategy extremely helpful at ACV, there are areas of caution. First, canary releases require teams to manage multiple versions of their software at once. Another challenging area is for teams that have distributed software such as mobile deployments. These teams cannot control when users will install the software on their computer or mobile devices, and this can lead to a decrease in active canaries during the testing period, leading to an insufficient sample size.


Another area of concern is the managing of the feature flags themselves. If teams do not delete feature flags, and their corresponding code, after full adoption of the new software, then it can lead to code bloating and increased complexity for future changes. One last area is databases. If different versions of the code produce and store different versions of the data, the underlying services and databases must be able to support all active versions and be able to translate between versions. This can become quite challenging, especially if the number of active versions of the code grows.


The simplest way to address these areas of concern is to keep the number of versions to a minimum. Canary releases should be short lived and rollout strategies executed quickly. For every feature flag created to support canary releases, there should be a corresponding task created to delete the feature flag. Once the team has fully rolled out the change, the team can then delete the feature flag. The key here is a disciplined approach to using the technique.


## A Tweet Ending


ACV has found that canary releases have helped us address hesitations with our continuous delivery process by reducing risk. By rolling out changes of our software to a small group of users first, we can minimize the impacts of bugs in production while not slowing our deployments down with a heavy QA process. We also have a quick and straightforward way to restore users to a working version of the code. Software organizations should consider integrating canary releases into their deployment strategy to take a confident step towards more reliable software delivery.


-
-
-
-
