---
schema_version: "1.0.0"
document_id: "5f0483686d7d3350b10e593f2dbcb879093ccc45afb3d2d19b204bb850542099"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/limitations-in-measuring-platform-engineering-with-dora-metrics/"
published_at: "2024-04-06T22:45:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:bfc0d6a354d1c6a986bfc88ee4d11ad29c457b8c8224d54d90d1f8b09b5e5786"
---

# Limitations in Measuring Platform Engineering with DORA metrics

While helpful, DORA metrics don’t capture your team’s performance on improving Developer Experience


Author: Nočnica Mellifera


The problem of measuring developer enablement — how well our platform engineering and[developer experience](https://www.signadot.com/blog/how-to-improve-and-measure-devex-in-your-organization/) is making it easy to add features and maintain our service — crops up in every large organization. Good tools specialists, enablement engineers and operations people can get great results for the right team, but measuring what they do is notoriously difficult.


It would be nice if we had some simple metrics that worked across teams and organizations to understand how well we were enabling developer velocity. In my[previous article](https://www.signadot.com/blog/the-wrong-way-to-use-dora-metrics/) , I discussed some of the pitfalls of doing just that with DORA metrics used incorrectly. In this piece we’ll drill down into evaluating platform engineering with DORA metrics.


To clearly discuss the role of DORA metrics in evaluating team health, we must confront the most thorny question: whether DORA metrics are an effective tool for evaluating platform engineering. My conclusion is that you can’t evaluate your platform engineering team based on DORA metrics.


In a discussion of DORA metrics,[Hazel Weakly](https://www.linkedin.com/feed/update/urn:li:activity:7137437870444744704/) , head of infrastructure and developer experience at Datavant, references[Mordecai’s concept](https://rambling.malignat.us/2022-08-27/lowstakes-and-highstakes-metrics?trk=public_post_comment-text) of low-stakes metrics:


> “Low stakes metrics are useful for self reflection and self improvement; you want them, but you shouldn’t be graded on them. DORA metrics are definitely that, which makes me a little frustrated sometimes that there’s so much benchmark and assessment language around DORA metrics because that’s precisely not what they’re for.”


Platform engineering encompasses a large amount of work that is outside the measurement of DORA metrics. While they may help the platform team identify how much work they have to do and how much progress has been made, it makes no sense to punish or reward teams based on how many times per day they deploy code.


## **Platform Engineering Is More Than Software Delivery**


Tech debt is invisible to DORA metrics. Unless that tech debt causes failed software releases, all the cruft in the world won’t hurt your observed metrics. For platform engineers, this means that a lot of the work they do to improve the developer experience doesn’t matter to DORA metrics. DORA metrics target software delivery performance above everything else. However, platform engineering encompasses a wider range of responsibilities, including infrastructure management, security, scalability and maintainability, which are not directly measured by DORA metrics.


To give just one theoretical: If platform engineers spend a six-week spike standardizing configuration management and environment variables across all teams, leading to a more secure and standardized experience across the stack, all that work might not affect DORA metrics whatsoever. Even as developers suddenly find it so much easier to work with environment variables and stop sending critical keys in unencrypted config files, the platform engineering team will look ineffective if they’re judged by DORA numbers.


## **Platform Engineering Is User-Centric**


The 2023 DORA report emphasizes focusing on the user, whether external customers or internal platform users. Traditional DORA metrics don’t capture this user-centric approach, which is essential for high-performing teams.


It’s worth examining whether most of the evaluation of a developer enablement or platform engineering team could be covered by simply asking engineers, “Are we doing a good job at platform engineering?”


When developers are feeling good about their tools and process, it’s a very strong indication that platform engineering is doing their job well. Finally, platform engineering needs to consider[how easy it is to adopt](https://www.signadot.com/blog/how-to-be-an-effective-platform-engineering-team/) their platform; this user-centric metric is absent from DORA metrics’ measurement.


## **Generative Culture and Psychological Safety**


Google is famous for popularizing the need for “[psychological safety](https://www.nytimes.com/2016/02/28/magazine/what-google-learned-from-its-quest-to-build-the-perfect-team.html) ” in high-performing teams. While a number of factors can affect psychological safety, there is little doubt that the most frequent destroyer of a[feeling of safety on a team](https://www.signadot.com/blog/7-reasons-developer-experience-is-a-strategic-priority/) is misapplied or unfairly applied performance evaluations.


When we talk about evaluating the performance of our platform engineering team, using DORA metrics is not a way to encourage trust within that team. At a glance, DORA metrics:


- Don’t capture everything the team does.
- Are strongly affected by code quality and work done by other teams.
- Will rise and fall stochastically depending on the features currently in development.


Imagine going to work every day knowing your continued employment hinges on numbers you don’t control, that could either go way up or way down if the road map changes, and you have no power to improve on your own!


The importance of a generative organizational culture and psychological safety, as highlighted in the 2023 report, is not directly measured by DORA metrics. These aspects are crucial for team performance and innovation in platform engineering.


## **The Impact of a Changing Tech Landscape**


The 2023 report also underscores the significant potential impact of AI in enhancing developer productivity. While the overall promise of large language models for transforming every aspect of business are hotly debated, the undeniable ability of LLMs to describe code’s function and generate code from simple guidelines means we have to explore how AI will enable our development life cycle.


Essentially, the questions being asked here are:


- Is the platform engineering team exploring how these tools can transform our development and deploy process?
- Are we exploring better ways to compose, test and document our code?
- Are those tests exploring practical process improvements, and can we generalize successes on one team to the broader organization?


Platform engineering has long been a “skunk works” of new ideas and tools. Even Backstage, one of the largest projects in the developer enablement space, remains a relatively obscure topic. But exploring new tools and techniques will only hurt DORA metrics in the short term and aren’t guaranteed to affect any measure directly.


These factors are outside the scope of traditional DORA metrics but are vital for the efficiency and effectiveness of platform engineering teams.


## **DORA Metrics Are Part of the Story**


Human factors are the most important criteria for success of platform engineering. When we know that the platform engineering team is interfacing well with all of engineering, we have a great idea that things are working well. At best, DORA metrics offer a check value on that assessment. If everyone loves the new developer platform but releases have slowed to a crawl, we should investigate the cause. But it’s likely that this check will get us just the context we need.


While DORA metrics provide valuable insights, their misuse and narrow focus can lead to a skewed understanding of developer productivity and platform engineering effectiveness. It’s essential to use these metrics as part of a broader strategy that includes qualitative assessments, focuses on user-centric development, acknowledges the importance of a generative culture, and leverages the potential of AI and quality documentation. This holistic approach is crucial for truly understanding and improving the multifaceted nature of platform engineering.
