---
schema_version: "1.0.0"
document_id: "5ec2ff79af94ae5a2d0a8a043a069545511640c309f473de057301e8fb5eaff8"
company_key: "nextdoor-holdings-inc-class-a-common-stock"
company: "Nextdoor Holdings Inc."
source_id: "nextdoor-holdings-inc-class-a-common-stock-rss-10247875dc01"
canonical_url: "https://engblog.nextdoor.com/catching-anomalies-early-in-mobile-app-releases-ac95adf9da81"
published_at: "2023-01-11T15:27:14+00:00"
first_seen_at: "2026-07-20T23:17:36.074163+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:de91aa3e5379d6f6d9eaad1a6a7a29ee8f2155c2b7e9136c4de2d080b09bb33b"
---

# Catching Anomalies Early in Mobile App Releases

Data Science


Statistics


Software Development


Mobile App Development


Mobile Apps


# Catching Anomalies Early in Mobile App Releases


## *How Nextdoor catches mobile app release anomalies at 1% adoption*


[Walt Leung](https://medium.com/@waltleung?source=post_page---byline--ac95adf9da81---------------------------------------)


6 min read


·


Jan 11, 2023


--


At Nextdoor, our mobile applications on iOS and Android serve content to tens of millions of weekly active users. At this scale, we run a weekly release process for both iOS and Android, shipping hundreds of changes across multiple teams and dozens of mobile engineers.


Our team uses several observability processes and rollout strategies to keep these deployments safe and scalable. We most notably use phased rollouts to minimize the impact of a potentially bad release. Phased rollouts allow us to gradually increase the adoption of users for a new app version. For example, we can have a new app version be released to only 1% of users on the 1st day, 2% of users on the 2nd day, and so on. That way, if a new release were accidentally shipped with an uncaught regression, having it at 1% rollout means it affects fewer users, reduces its severity level, and gives us more time to react.


However, for many of our critical business metrics where a failure can sometimes be silent, most out-of-the-box observability approaches don’t work with phased rollouts. This is largely due to two problems:


1. Observability typically happens at an aggregate level. For example, we look at app sessions or revenue on a daily basis, across all users for a platform.
2. The behavior of early adopters on an app version differs from the median behavior of all users. Most importantly, early adopters are more active, almost by definition, to be in an early rollout of the new app version.


Press enter or click to view image in full size


*At Nextdoor, Daily Users are more likely to adopt releases over Weekly Users, Weekly Users over Monthly Users, and so on.*


For example, consider an app session regression on a hypothetical iOS version v1.234.5 released March 4. If we had unknowingly introduced a regression where we didn’t count an app session 5% of the time, at a 1% rollout, our aggregate impact would be expected to be roughly 0.05 x 0.01 = 0.05% of all iOS app sessions, which is practically impossible to detect (read: noise) with aggregate-level observability. Even worse, early app adopters skew more active, which means that maybe we should expect 0.06% of all sessions impacted. Or maybe 0.07% of all sessions impacted. In short, it’s hard to tell exactly what our aggregate impact should be.


However, when iOS release v1.234.5 reaches full rollout in a week, a 5% app session regression would be business critical. We can detect the app sessions drop once it reaches full rollout by looking at week-over-week or month-over-month metrics, but by that point, several days would have passed.


Press enter or click to view image in full size


*Stacked graph. Top trendline shows our app sessions, which has a clear regression starting March 7 with a low point at March 14. Bottom trendline shows the release adoption over time due to phased rollouts.*


**How can we detect these issues on day 1, at 1% rollout?**


A simple approach would be to normalize our business metrics to the total number of users on the release, and turn all metrics into relative metrics (e.g. on v1.234.5, app sessions per active user per app version). Unfortunately, as mentioned earlier, we can’t directly compare the app sessions from users who have adopted a release to those who haven’t as their underlying characteristics are too different.


What we’re trying to solve for these early adopters is: what is the difference between their actual app sessions after adoption compared with their hypothetical app sessions had they never adopted the release in the first place, or an unobserved counterfactual? In statistics, we can measure this through difference-in-differences analysis.


Press enter or click to view image in full size


*For iOS release v1.234.5, app sessions over time of users who adopted the new app release on March 4 (teal) vs app sessions over time of users who did not adopt (gray).*


## Get Walt Leung’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Difference-in-differences analysis is a simple causal inference method we can apply here to estimate this effect by accounting for the separate time varying effects of users that have and have not adopted a release:


1. For users who adopted the release, calculate the difference in their app sessions three days before and three days after the release period. In this case, we observed a **-0.02 decline** in app sessions.
2. Do the same for users that have not adopted the release. In this case, a **+0.20 increase** in app sessions.


Press enter or click to view image in full size


Assuming trends would have otherwise remained constant (pre-trend assumption), we would have expected app sessions of release adopters to increase by **+0.20** like we observed with non-adopters. However, they instead *decreased* by **-0.02** . We calculate the difference in differences to estimate a comparison against an unobserved counterfactual:


> ***-0.02–0.20 = -0.22 decrease in app sessions due to iOS release v1.234.5***


In practice, we don’t just calculate this in aggregate. We first make sure that our two cohorts exhibit similar behavior pre-adoption (pre-trend assumption). This is a critical step to difference-in-differences analysis. With a sample size over hundreds of thousands of users, we can achieve high confidence in similar pre-trend behavior with a simple standard deviation bound over the preceding few days to adoption. If this behavior holds, we then fit a linear regression model that estimates the average effect of a release for any particular metric:


***y = β0 + β1* Time_Period + β2* Treated + β3*(Time_Period*Treated) + e***


In the case of v1.234.5, we can measure statistically significant negative effects across multiple app sessions metrics.


Press enter or click to view image in full size


*Average % lift of metrics we ran App Release Anomaly Detection on for v1.234.5*


With this difference-in-differences approach, we are now able to flag the app sessions decline due to v1.234.5 on March 5th, **10 days earlier** than we normally would have been able to using week over week figures. We also mitigate the need to factor in external variables such as seasonality or day of week. This not only helps in diagnosing the source of the decline to a specific app release, it also isolates the regression to **less than 1% of iOS users** .


Press enter or click to view image in full size


*App Release Anomaly Detection allowed us to discover and fix the release regression at 1% rollout, before our aggregate observability even showed a drop.*


App Release Anomaly Detection is one of the many tools we’ve built at Nextdoor to give us observability into our releases while iterating quickly. It is one of the foundational elements that allows us to deploy major app releases on a weekly cadence and have confidence in our stability. Operationalized, App Release Anomaly Detection has helped us prevent nearly all severe critical client-side regressions and gives us peace of mind to release bigger changes at a more rapid cadence.


If this type of cross-functional work between platform engineering and data science at scale interests you, we’re hiring! Check out our[Careers page](https://boards.greenhouse.io/embed/job_board?for=nextdoor&b=https%3A%2F%2Fabout.nextdoor.com%2Fcareers%2F#51115) for open opportunities across all our teams and functions.


Written by[Walt Leung](https://www.linkedin.com/in/waltleungwbl/) and[Shane Butler](https://www.linkedin.com/in/shaneausleybutler/) , with support from[Hai Guan](https://www.linkedin.com/in/hai-guan-6b58a7a/) ,[Charissa Rentier](https://www.linkedin.com/in/charissarentier/) ,[Qi He](https://www.linkedin.com/in/qi-he/) , and[Jonathan Perlow](https://www.linkedin.com/in/jdperlow/) .
