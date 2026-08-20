---
schema_version: "1.0.0"
document_id: "e711e821fbddee4931679c85873bc3d04703ad3dca625fdeda5b46307c81a9bd"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-do-dora-metrics-right/"
published_at: "2023-10-31T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:8356943fee8940cdd2846754e9f31aa980673526332dc7485a3aac73b1b8b0a7"
---

# How to do DORA Metrics Right

DORA metrics, or DevOps Research and Assessment metrics, offer insights into the performance and efficiency of software development and delivery processes. These metrics encompass aspects like deployment frequency, lead time for changes, change failure rate, and mean time to recover. These metrics matter for anyone managing an engineering team, from team leads to CTOs, because they provide a data-driven understanding of how well their teams are delivering software. I wanted to write about how these metrics are calculated, and what they really tell us about how our team is performing.


## **Deployment Frequency**


### **Definition**


Deployment frequency measures how often a team successfully releases code to production.


### **Importance**


High deployment frequency is often a sign of a mature CI/CD pipeline and effective collaboration among development, QA and operations. It enables faster feedback loops and quicker adaptation to market changes.


Note that out of the four DORA metrics only this one is better the higher it gets, so for ease of charting you may want to calculate 1/frequency, or a similar inverse metric, possibly “average time between deployments” where a higher value would mean a lower pace of releases.


### **Measuring Deployment Frequency**


The metrics in this article are listed in order of difficulty, from easiest to most difficult. Deployment frequency only requires that we know that a deployment occurred at a certain time. From there, we can calculate a histogram with daily, weeklyor monthly buckets. In the[DORA metrics project Four Keys](https://github.com/dora-team/fourkeys) the only complexity in calculation is creating rows for time buckets where there were no deployments.


### **Evaluating Deployment Frequency**


It’s hard to argue against more frequent deployments meaning a faster and more agile product team. The levels of performance are defined as follows:


**Elite performers -** On-demand (multiple deployments per day)


**High performers -** Between once per day and once per week


**Medium performers -** Between once per week and once per month


**Low performers** - Between once per month and once every six months


*Source: 2019 Accelerate State of DevOps, Google*


## **Lead Time for Changes**


### **Definition**


Lead time for changes is the median time it takes for a commit to be deployed into production. Calculate the time difference between when a commit is made and when it is successfully deployed to production. Take the median of these values over a specific time period.


### **Importance**


Shorter lead times often indicate streamlined development and deployment processes. It shows that the team can quickly deliver features, fixes or updates.


### **Measuring Lead Time for Changes**


The start of the time span when measuring lead time for changes should be straightforward: It’s almost certainly the time that a pull request (PR) is created or merged. To get time that a commit is deployed to production, we need the deployment information from deployment frequency. We also require that the start of a change process include an ID that will be carried through to the deployment step. This might look like a tag on deployment that includes the pull request ID. As long as ID is carried forward from a pull request to a deployment. Once we have an array of \`lead_times\` we can sum these lead times and divide by \`{length of time window}’


### **Evaluating Lead Time for Changes**


While things like an improved review process might increase this value, it’s still generally better to have changes happen shortly after they’re committed.


**Elite performers -** Less than one day


**High performers -** Between one day and one week


**Medium performers -** Between one week and one month


**Low performers -** Between one month and six months


*Source: 2019 Accelerate State of DevOps, Google*


## **Time to Restore Services**


### **Definition**


Time to restore services is the median time taken to restore a service after a failure has occurred. Remediation is considered complete when the associated bug or incident report is closed.


### **Importance**


A shorter time to restore services indicates effective incident management and a resilient system. It minimizes downtime and the impact on end users.


### **How to Measure Time to Restore Services**


Time to restore services is the hardest metric to measure. Unlike the three other metrics that should be measurable entirely from source control, we need to know when incidents began and ended, and this may not be a number that everyone agrees on. In some organizations incident times end up being manually entered for calculations of uptime, but this isn’t ideal. There are, in general, three paths to identifying the timespan of incidents:


- **Synthetic monitoring** — Sometimes called a “pinger.”’ If we send consistent requests to a set URL we should be able to define an exact timespan for the incident. The obvious downside here are false negatives, where the synthetic monitor doesn’t see the service as down because a 200 is being returned despite unexpected behavior. Synthetic monitors have gotten much more sophisticated in the last few years, so it’s possible to do something that looks more like end-to-end testing.
- **Logging errors, raising of exceptionsor otherwise direct code monitoring** — If internal errors are being raised, we can often assume we’re having an incident. This system can produce both false negatives and false positives: Sometimes a side effect from a function may raise an error but still give a satisfying response to the user. This might require a change in practice for what constitutes an error. For instance, we might have a user lookup service that raises an error when no matching record is found. When measuring incidents via logging, we’d need to change the level of flag being raised by noncritical failures.
- **Measurement by statistical threshold ( such as response time) —** It’s possible to infer incidents from statistical performance. If response time increases alarmingly, that might qualify as an incident even if the service is still working albeit at reduced capacity. This approach has the great advantage of closely representing the user’s expectations. A site that’s loading in more than 15 seconds will seem “down” to users even if the code is never raising an error, or the system is always sending “good” responses eventually. This statistic, however, requires the most in-depth APM measurement.


Unless you’re currently measuring incidents very closely, establishing time to restore services is likely to involve measuring new information with an observability tool. For smaller teams just exploring measuring developer velocity, it might be workable to manually document incident times as part of the postmortem process.


The final calculation of time to restore services statistic will just be \`sum(\[array of all incident lengths\])/{number of incidents}\`.


### **Evaluating Time to Restore Services**


This metric is likely to be a core competency for your operations team already, and the levels of performance from the DORA group are hard to argue with.


**Elite performers - Less than an hour**


**High performers - Less than one day**


**Medium performers - Less than one day**


**Low performers -** Between one week and one month


*Source: 2019 Accelerate State of DevOps, Google*


## **Change Failure Rate**


### **Definition**


Change failure rate is the ratio of the number of failed deployments to the total number of deployments.


### **Importance**


A lower change failure rate indicates a more reliable system and effective testing procedures. It shows that new changes are less likely to introduce issues.


### **How to measure Change Failure Rate**


By default in a project like Four Keys, change failure rate, like time to restore services, relies on counting deployments and incidents, and calculating the ratio between the two. This has some implicit assumptions: It assumes that the only failures that matter are those that affect users, and it assumes that all failed deployments go on long enough to raise incidents. One other concern is that the number of incidents are the critical measure here, not the length. So an outage that lasts 24 hours looks fine if there are a large number of deployments in the same week. But 20 five-minute outages look much worse. How do we get a more reliable change failure rate? Three possible paths:


- Define a standard rollback process — If you resolve that your incident response teams will always tag a failed PR or always use \`git rewind\`, you can directly measure when a change fails.
- Adopt a canary process, like[Argo Rollouts](https://argo-rollouts.readthedocs.io/en/stable/features/canary/) , and count rollbacks as failures.
- Define a standard for when an incident “counts” as a failure. For example, set a minimum length of an incident that counts as a failure based on deployment frequency.


In any of these examples, change failure rate will always be a statistic that feels less quantitative than the other three DORA metrics. The combination of at least two performance metrics will mean that spikes will often feel like they can be “explained away” with circumstantial information.


Final calculation of the statistic is \`{number of deployments in time window} / {number of failures in time window}\`.


Evaluating Change Failure Rate


The failure rate of change may include a high false positive value: If you’re using the final stage of deployment as a testing component, like doing final integration tests, it might be nothing to worry about if changes often fail. However the DORA group’s standards are:


**Elite performers** - 0-15%


**High performers -** 0-15%


**Medium performers -** 0-15%


**Low performers -** 46-60%


*Source: 2019 Accelerate State of DevOps, Google*


### **Special pleading -or- why we might explain these results**


It’s possible, for all four of these metrics, to come up with scenarios where an increment of the metric is actually a good thing. For example, if we increase the speed and ease of deploying code experimentally, it’s possible that the change failure rate will go up. With better and more reliable review processes, the time to deployment may increase. However, in all these scenarios an improvement in process should result in the other three metrics significantly improving. These very high-level metrics can help identify[Pareto Principal](https://en.wikipedia.org/wiki/Pareto_principle) benefits, where small changes result in big improvements to velocity.


## **What do these metrics really tell us?**


It’s important to recognize that DORA metrics are intended to tell you about the overall productivity of your development team. These metrics measure the ability of your developer platform to enable developer velocity; in other words how effective your developer environments, deploy systems and testing are at releasing code easily and reliably.


Your development team might be working extremely hard, and producing great code, but their DORA metrics can still be terrible because the test and deploy process is error-prone, high-effort and requires a lot of human intervention. This difficult developer experience will hurt your overall developer velocity, but the solution isn’t in getting your product engineers to work harder. The solution to poor DORA metrics is to take a serious look at the developer experience within your internal platform and make platform engineering a real priority for your team.


## **Conclusion: DORA matters for developer velocity**


If code is easy to test and release, and your development environment[very closely resembles production](https://www.signadot.com/blog/the-staging-bottleneck-why-your-engineering-team-is-slow-and-how-to-fix-it/) , you’ll have fewer rollbacks and a faster process to release to production. This speed isn’t just an indicator of technical excellence, it represents a paradigm of Agile Methodologies, and means your team is getting better at meeting the needs of users.


Understanding and implementing DORA metrics is not just a technical exercise but a strategic imperative for platform engineers and dev team leaders. These metrics offer a holistic view of your development pipeline, from code commit to deployment and incident resolution. They serve as key indicators of your team’s agility, operational efficiency and overall developer velocity.


While it’s tempting to focus solely on the development team’s output, DORA metrics reveal that the developer experience is equally crucial. A cumbersome, error-prone deployment process can significantly hamper even the most talented development teams. Investing in platform engineering and improving the developer experience are essential steps toward optimizing these metrics.


For those looking to improve their DORA metrics, Signadot offers tailored solutions that can help you achieve a more streamlined and effective development pipeline. Remember, in the fast-paced world of software development, standing still is not an option. Make DORA metrics a priority, and you’ll be well-equipped to adapt, innovate and excel.
