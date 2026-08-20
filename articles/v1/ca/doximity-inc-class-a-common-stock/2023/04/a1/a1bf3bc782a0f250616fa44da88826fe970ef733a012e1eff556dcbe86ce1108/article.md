---
schema_version: "1.0.0"
document_id: "a1bf3bc782a0f250616fa44da88826fe970ef733a012e1eff556dcbe86ce1108"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/doximity-mobile-releases-streamlined-with-automation"
published_at: "2023-04-12T14:19:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:f00348b749c447318a2644c0e9af7ccd379519240546112db354592c04f0e504"
---

# Doximity Mobile Releases Streamlined with Automation

Releasing mobile apps to the store is inherently slower than web updates due to manual process overhead, such as versioning, packaging, and submitting to the App Store for review. While web developers have long enjoyed full buzzword-compliant Continuous Deployment, mobile teams generally use a defined release cadence, typically 2-4 weeks.


The[DORA metric](https://codeclimate.com/blog/dora-metrics/) , which stands for DevOps Research and Assessment, evaluates performance using four key metrics: Deployment Frequency (DF), Lead Time for Changes (LT), Mean Time To Recover (MTTR), and Change Failure Rate (CFR). These metrics are commonly used to gauge an organization's agility, efficiency, and ability to implement changes quickly. Organizations that excel in these areas tend to perform smaller, more frequent deployments. A few years ago, our mobile team was classified as a medium performer for deployment frequency and reported 47% for change rate failure, which should be from 0% to 15%. We knew we could do better.


Doximity mobile teams historically used a three-week release cycle. However, the pace wasn’t fast enough to quickly get new features out the door. As a result, almost every release would be delayed due to some “very important thing” that had to get in. We also had to do more patch releases to fix issues introduced in production, which were even harder to debug with so many changes in a single version.


One of the essential features that forced us to accelerate our release cycle was the Dialer Video product. We launched Dialer Video soon after the beginning of the pandemic. We wanted to get something out and to iterate quickly to meet the needs of our doctors while they were going through a difficult time and learning to work remotely. In April 2020, we attempted to shift to a weekly release cycle to meet this demand. While some automation was in place, there was also a lot of overhead with each release, and the developers spent a significant amount of time just managing the process. We compromised on a two-week cycle until we could streamline the process to something more sustainable. That elusive one-week release was our ultimate goal, and we finally achieved this in November 2021. Since then, we have continued to enhance the automation and are now at the point where our weekly releases require zero developer overhead, and QA fully controls the process. This change shifts the responsibility to the appropriate team, and it has worked without issue for over a year.


### What are the benefits of a shorter release cycle?


There are several benefits beyond getting features and bug fixes out quickly. With the ability to do weekly releases, the pressure to deliver by a specific time has decreased significantly. We no longer need to manage each item that gets into a release. Suppose a story is merged, then great. If it doesn’t make this release for any reason, it will be in the one next week, and it’s not a big deal. Not all chunks of work can be finished in a week, so we still have long-running projects that may take months to complete. However, when they finally merge, those features get onto users' devices much more quickly.


Releasing more frequently has also increased the stability of our application. The more code changes that go into a single release, the more likely things will break unexpectedly. By including a smaller set of changes with each version, we can quickly narrow down the cause of any issues found to only a week's worth of updates. The chart below represents the number of iOS releases per year vs. patch releases *(a patch is required to fix a newly introduced issue in production)* . In 2020, when we averaged a three-week release cycle, we had a 47% change failure rate! That means almost half of our planned releases required a patch. Last year, with our weekly cycle, we had 0 patches. In addition to more frequent releases, there are other strategies we have implemented to improve our app stability that fall outside the scope of this article, such as robust UI test automation and enhanced error logging to catch new issues early.


### Mobile release automation process


After all of our refinements to the process, the following is the extent of the manual input required for each release:


1. A QA team member enters the Slack command,` doxbot create-release \[repository name\]` , on Monday morning.
→ This command creates the release branch, sends the build to TestFlight/Google Play, and bumps the version on the main branch.
2. The QA team tracks their manual regression testing in a Jira task.
3. When regression is complete, the QA team member changes the task status to “Passed QA.”
→ This sends the build to the app store and submits it for review.
4. Once approved for the store, QA folks move the Jira task to “Done.”
→ This tags the release in GitHub.


That’s it! QA controls all manual steps at this point without added overhead.


A lot of magic is happening behind the scenes to make this happen, made possible with the efforts of our dedicated Infra Automation Team. Below is a high-level overview of the automation we have put in place with manual steps in yellow. All of this involves a combination of GitHub Actions, CircleCI, Fastlane, Jira Automation, and our internal Slack chatbot that we call Doxbot.


Speaking of CircleCI, it’s also worth calling out that our ability to promptly execute CI workflows for our iOS application was a very helpful milestone on our journey to weekly releases. CircleCI’s[self-hosted runner feature](https://circleci.com/docs/runner-overview/) proved instrumental - it allowed us to set up and manage our own MacOS hardware to serve as CircleCI runners and opened up many efficiencies that were otherwise unavailable to us. With self-hosted runners, we reduced our CI workflow duration from an average of 45 minutes to an average of 15 minutes. This shortened the feedback loop for developers when making code changes and enabled us to get builds into the hands of our QA team much sooner.


Going from no automation to where we ended up is daunting for any team, but the efforts have paid off. On the DORA scale, our mobile team has gone from medium to high-performing, as well as significantly decreased our change failure rate from 47% to 0%. If you want to build this for your mobile app, we recommend starting with the easiest areas to automate and slowly expand from there. While the goal may seem overwhelming initially, as you begin to see the benefits of automation, you'll have more ideas on what to turn over to your CI system and how to do it. The journey of a thousand miles begins with a single script.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech/) if you'd like to be notified about new blog posts.
