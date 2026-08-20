---
schema_version: "1.0.0"
document_id: "3a13576144ab87ddcdc216e210b0fbe5f31af4b4f16848875188944a28900fd4"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/the-wrong-way-to-use-dora-metrics/"
published_at: "2024-04-01T10:02:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:277d07b1d34ab21cd162b20fc9a47c04cd9f199e01336879520da3ad1b56bb22"
---

# The wrong way to use DORA Metrics

Are you using DORA metrics as intended? Google says you might not be!


Author: Nočnica Mellifera


In the last few years, a lot has been said in favor of DORA metrics for measuring the success of[developer enablement](https://www.signadot.com/blog/how-to-improve-and-measure-devex-in-your-organization/) within your organization: how well your platform engineering, operations, and developer experience efforts are making it easier for developers to deliver features and maintain services. These five metrics (up from four in the original[2013 State of Devops](https://www.puppet.com/resources/history-of-devops-reports#2013) report from Puppet) are:


- **Deployment frequency** — How often an organization successfully releases to production
- **Lead time for changes** — The amount of time it takes a commit to get into production
- **Change failure rate** — The percentage of deployments causing a failure in production
- **Time to restore service** — How long it takes an organization to recover from a failure in production
- **Reliability —** Broader than availability, reliability is a measure that includes availability, latency, performance and scalability to represent operational performance.


I agree that measuring these is vital. But it must be said that the intent of these metrics was always to give an indicator of[how well your team was delivering software](https://www.signadot.com/blog/how-to-do-dora-metrics-right/) , not a high-stakes metric that should be used, for example, to hire and fire team leads. While that mission has always been clear, the original metrics report asked leaders to determine whether teams were “elite performers” and strongly implied that better teams would always have better DORA metrics.


That conflict, between whether DORA metrics are an interesting stat that can show progress or a critical stat that represents success or failure for a team, has polarized opinion on DORA metrics. The reality is that DORA metrics are a strong indicator of the health of developer experience, but like any observed statistic, the information can be misused and misinterpreted.


### **High-Stakes and Low-Stakes Metrics**


There should be a clear distinction between high-stakes metrics and low-stakes metrics. This isn’t my distinction, rather I’m cribbing from[Mordecai’s great post on the topic](https://rambling.malignat.us/2022-08-27/lowstakes-and-highstakes-metrics?trk=public_post_comment-text) :


“When metrics are low stakes, when they stay inside the team, they are beneficial. They are instituted, monitored, and acted upon by the people that are subject to them. This is the *Diagnostic* or *Improvement Paradigm* .


“On the other side, where stakes for the metrics are high, there is the *Accountability Paradigm* . Here, measures and metrics are not necessarily for improvement or finding issues, they are for making sure that people do what they are supposed to.”


While many writers, myself included, have encouraged leaders to use the DORA metrics to assess their teams’ development velocity and ease of deployment, they can be misused, lead to poor optimizations and even perverse incentives.


## **Ways that DORA Metrics are Misused**


When I shared my last piece on how to measure and calculate DORA metrics with the platform engineering communities on Slack, Reddit and Discord, I often got strong responses along the lines of “I hate DORA metrics.” Digging into that feeling, the response came from too many experiences of the metrics being misused and misinterpreted. What follows are five ways that DORA metrics, or really any tightly focused performance metric, can be misused.


### **1. Teams Pursuing Performance Metrics over Business Goals**


Many organizations focus narrowly on the four main DORA metrics (deployment frequency, lead time for changes, change failure rate and time to restore service). The danger in this focus is that we lose sight of the organization’s goals. This can lead to neglecting other critical aspects like organizational performance, team dynamics, reliability, burnout, productivity and job satisfaction. In a conversation on the Platform Engineering slack,[Bryan Ross](https://bryanross.me/) put it well:


“Many of the teams I work with have a fleet of metrics to show benefit but they’re unable to then communicate those back in a way that relates to ‘The Business.’ In the words of Rod Tidwell, ***“show me the money”*** ! How can we relate DORA metrics to financial gains — cost avoidance, savings, etc?


To correctly use DORA metrics, we must constantly tie the overall goals of greater reliability and developer velocity to the overall business goals and show how an improved developer experience also improves things like retention, work quality and overall productivity.


### **2. Using DORA as a Comparison Between Teams Rather than Across Time**


Software isn’t a homogenous industry, and it’s not right to compare DORA metrics between teams. Every software team isn’t going to have the same ideal release cadence, the cost of a single downtime incident will be different, and teams’ ability to work on out-of-band fixes will be different. On the Platform Engineering Slack, a discussion about DORA metrics got this great comment from[Thomas](https://platformengin-b0m7058.slack.com/archives/C02DF3KKNES/p1701184461390309) :


“I’m really confused about the ‘change failure rate.’ The best teams have less than 5% and the worst more than 64%. But the best teams deploy many times every day, and the worst team less than once per month. In my previous job, we released 50 times per day. If we had a change failure rate of 5%, we would have 2.5 incidents per day!!! If you release once per month and have a 50% change failure rate, you would have an incident every second month or so. Sounds like the worst teams have a much more stable environment.”


And Thomas has a fair point. How would it be “ideal” to have an incident a few times per day? While it’s possible to explain this with some other metrics — high-performing teams also have very short interruption times, meaning incidents are handled in less than an hour — the 2023 State of DevOps report has a line in its introduction that’s applicable here:


“The best comparisons are those performed over time on the same applications rather than between different applications, which will always have different contexts.”


While it’s statistically quite meaningful to say that your team had a massive increase in its release cadence, it’s not terribly meaningful to note that you release 10 times as much as another team in a different organization. It’s better to go faster than your team used to.


### **3. Misinterpretation and Misapplication**


As highlighted in[The New Stack article](https://thenewstack.io/google-says-you-might-be-doing-dora-metrics-wrong/) , there’s a common misunderstanding of what DORA metrics represent. They are often seen as end goals rather than indicators of underlying processes and practices. This misunderstanding can lead to practices that superficially improve metrics but don’t contribute to genuine improvements in software delivery or team well-being.


Let me share an extreme example of metrics over real goals. In 2020, Hacktoberfest organizers offered a free T-shirt to anyone who had submitted four or more pull requests. Intended to encourage new contributions to open source projects, instead maintainers were flooded by thousands of frivolous pull requests from people who had seen videos on a “cool hack to get a free T-shirt.” By setting a simple metric target, the organizers had encouraged unhelpful, disruptive behavior,[the opposite of their goals](https://en.wikipedia.org/wiki/Goodhart%27s_law) . This perverse incentive situation is a specific example of[Campbell’s Law](https://en.wikipedia.org/wiki/Campbell%27s_law) : When we set a simple metric target, there’s a high temptation for behavior that meets the metric while hindering the overall project.


While using DORA metrics shouldn’t generally be considered explicitly corrupt, if we over-focus on metrics goals, you will see pressure to distort them. In my own career I’ve seen long discussions about how the site failing for thousands of users wasn’t really an outage. The motivation to misclassify the incident was the concern about what a reported outage would do to performance metrics.


Beyond misreporting numbers, the big issue with misinterpretation is that DORA metrics in and of themselves tell you nothing about the health of a team. They indicate a good developer experience, but if features are slow to release, if business goals aren’t being met, if overall the product teams can’t do what they need to get done, all the daily deployments and super-fast rollbacks mean very little.


### **4. Neglecting Human Factors**


So what is a good goal for improving developer experience? The best metric for DevEx is and will always be your developers’ self-reported satisfaction with the process.


Focusing solely on the quantitative aspects of DORA metrics can lead to overlooking the human elements of tech organizations, such as burnout, productivity and job satisfaction. These aspects are crucial for a sustainable and effective work environment.


### **5. Cultural Misalignment**


DORA metrics are not just numbers; they represent a culture. If the leadership fails to communicate and embody the principles behind these metrics, their implementation can become counterproductive.


As[Martin Thwaites](https://www.linkedin.com/feed/update/urn:li:activity:7137437870444744704/) put it in a LinkedIn post, DORA metrics aren’t what really matter:


“Always think about ‘why’ you want those metrics to improve, as I guarantee you that the people who are paying money to use your product don’t care about whether a team is at the top or bottom of your DORA Metrics Leaderboard. If your ‘why’ is that you want to be the best at DORA metrics, you may be, and just a guess here, measuring the wrong things.”


## **Conclusions: Statistics Conceal as Much as They Reveal**


In my early years working with application performance management and observability tools, I remembered a classic trap that would result in undetected downtime. With tons of alerts set to buzz all engineers whenever response times dropped, a monitoring system would fail to catch significant failures of backend services. The problem? When the database service failed, it responded with error messages, which were much faster than actual responses. During an outage, response times fell, leaving all dashboards green.


This is an object lesson in how pursuing a few simple measurements can lead to failures. As demonstrated above, an over-focus on a small set of measurements can also lead to optimizations to improve metrics without improving real-world performance.


In my next article, we’ll discuss how DORA metrics may or may not help evaluate the quality of platform engineering within your team.


To join a group of engineers and leaders who are trying to build a better developer experience for their teams using an easier, faster way to let developers experiment with new code,[join the Signadot Slack](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ) and say hi!
