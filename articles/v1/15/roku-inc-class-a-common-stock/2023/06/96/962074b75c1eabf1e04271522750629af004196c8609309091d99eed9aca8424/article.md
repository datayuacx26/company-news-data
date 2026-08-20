---
schema_version: "1.0.0"
document_id: "962074b75c1eabf1e04271522750629af004196c8609309091d99eed9aca8424"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/unlocking-potential-rethinking-how-we-measure-developer-productivity"
published_at: "2023-06-28T13:02:15+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:1de04cb02cd12a7aa0a97ab852b93c0ef339d2ae1e9e7eb4641d72f17c3477e0"
---

# Unlocking Potential: Rethinking How We Measure Developer Productivity

Software development engineers play crucial roles at Roku. They are the architects behind the innovative applications, complex algorithms, and efficient systems that power our systems and products. However, as we strive to optimize their development processes and utilization of resources, measuring productivity becomes an increasingly challenging task.


In this blog post, we will delve deeper into the challenges faced when measuring the productivity of software engineers. We will explore alternative approaches, methodologies, and tools that can help any organization gain a more accurate understanding of the value and impact of their software engineering teams.


# Introduction


In today’s competitive and economically challenging environment, productivity is paramount, particularly in the field of software development. Developer productivity isn’t about just churning out lines of code but encapsulates the ability of developers or teams to effectively


**transform ideas and requirements into high-quality, bug-free software** that is production-ready


. This process should solve real problems, fulfill user needs, and add business value. Beyond creating deliverables, it is about generating significant, tangible results.


This productivity extends beyond individual performance, influencing the speed of product launch, product quality, user satisfaction, and a company’s agility in responding to market shifts. Thus, a highly productive team can significantly affect whether a company leads the market or lags. Rather than pressuring developers to produce more, **the focus should be on enabling smarter work** by reducing obstacles, optimizing workflows, and cultivating an environment that fosters continuous innovation and learning.


# The Connection Between Productivity, Job Satisfaction, and Tenure


While the primary focus of improving developer productivity might seem to be business-oriented – faster delivery times, higher quality outputs, and improved bottom-line results – there’s a human element that is equally important: **job satisfaction** . It’s the fulcrum in the powerful trifecta of productivity, job satisfaction, and job tenure, all critical to a software development team’s and organization’s overall success. Productivity is about meaningful achievements and impact, fostering motivation and engagement, thus driving a **positive productivity loop** .


In a **supportive and productivity-conducive environment** , developers are likely to feel a greater sense of accomplishment and satisfaction, reducing the chances of burnout and keeping morale high. They are also likely to feel valued and recognized for their contributions, which enhances their job satisfaction. According to research by Gallup, highly engaged teams show 21% greater profitability, underlining the connection between job satisfaction, productivity, and business results (Gallup, 2022). A happy and satisfied developer is more likely to stay with the company for a longer period, thereby increasing job tenure.


Investing in developer productivity is a win-win. It not only makes the software development process more efficient and effective but also enhances job satisfaction, reduces turnover, and nurtures a stable, highly skilled, and engaged development team. It’s essential to understand the **holistic impact of productivity improvements on individuals and the organization** . The subsequent sections will delve into measuring developer productivity and strategies to maximize it, keeping a keen focus on job satisfaction and long-term employee retention.


# Understanding Developer Productivity


Engineer productivity is a multifaceted concept with several interrelated elements. One of the critical components we value at Roku is the engineers’ **self-reported perceived productivity** . This is why we conduct **annual All Engineering Surveys** . By diligently running these surveys and meticulously analyzing all responses, we gain insights into areas where engineers self-report productivity challenges, allowing us to strategically invest in mitigating these issues.


For instance, build times for our embedded stack consistently emerged as a top concern in the survey responses. In response, we pivoted to[Bazel](https://bazel.build/) for our Build system, significantly improving build times. Another recurring issue was the difficulty in locating pertinent information in Confluence. To address this, we invested in the enterprise search tool[Glean](https://glean.com/)


to **streamline the process of information retrieval** .


While the importance of self-reporting cannot be understated, it’s important to remember that it is inherently subjective. The pitfalls of relying solely on self-reporting include potential biases and inaccuracies due to personal perspectives. As social scientists have found, there’s no such thing as a value-free observation in the social sciences. This underscores the fact that **personal perspectives can influence self-reported data** , potentially skewing results.


To mitigate this subjectivity, we complement self-reported data with objective metrics. We are currently developing **measures for the Cycle Time of development** , offering a more quantitative view of productivity. By balancing self-perception with data-driven insights, we aim to create a comprehensive and accurate understanding of developer productivity at Roku.


## Maximizing the ‘State of Flow’ in Engineers


A crucial aspect of understanding developer productivity involves recognizing the significance of the ‘ **state of flow** ‘—a concept introduced by psychologist Mihaly Csikszentmihalyi. This term describes a mental state of **complete immersion and energized focus** in an activity, often leading to heightened creativity and productivity. For software engineers, this state is frequently where their **most productive work** occurs, as they can deeply focus on complex tasks, solve problems more effectively, and generate high-quality code. Recently, A. Noda et al (2023) covered this subject of developer productivity trifecta in detail.


Unfortunately, this desired state **can be disrupted by a variety of factors** , such as constant interruptions like Slack messages, ad-hoc questions, meeting overload, system issues like network performance, process challenges like an inadequate CI system, waiting too long for system access or a convoluted approval process for merging branches, and tooling issues like bugs in internal tools or slow builds. These disruptions serve as significant obstacles to maintaining the state of flow, making it crucial for organizations to address these issues to optimize productivity.


At Roku, we prioritize creating conditions that enable our engineers to experience this state of flow more frequently. We seek to **understand the factors that promote or hinder this state** , which can inform strategies to maximize periods of flow and, consequently, boost overall productivity.


## Conducting Engineering Surveys


Engineering surveys play an integral role in capturing the pulse of the engineering team and understanding productivity issues. However, conducting impactful surveys necessitates careful consideration of various factors.


### Building Trust with Engineering


Developing a rapport with engineers is crucial for meaningful engagement. One effective way to do this is by involving them in the process. **Ask engineers about the questions they would like to be asked** , ensuring the relevance of these questions to each group. This inclusive approach not only fosters trust but also guarantees that the survey addresses the real concerns of the team.


### Transparency and Actionability


Once the survey is completed, openly share the results with the team. Not only does this validate the participants’ efforts, but it also promotes transparency. Equally important is to demonstrate that the organization **values the feedback and takes action on it** . This can be done by highlighting the changes or improvements implemented in response to the survey insights.


### Ensuring Anonymity


While transparency is important when we share the results, anonymity in survey participation **is vital** in creating a safe space for engineers to express their genuine thoughts and concerns. At Roku


, while we encourage openness, we also ensure feedback is actionable by summarizing the data along different dimensions such as:


- Tenure (e.g., <1 year, 1-4 years, >4 years), which can shed light on different challenges, like identifying onboarding issues among those with less than a year’s tenure.
- Senior Vice President (SVP)


organization size (e.g., groups with 100+ people), highlighting specific concerns within larger teams. For instance, perceptions of slow build times may vary significantly between teams working on different stacks.
- Location, as specific issues might be location-dependent , such as network performance.


- Role, to understand the unique challenges faced by different roles such as developers, QA, program management, and leadership.


### Avoiding Survey Fatigue


While frequent surveys can provide a steady stream of feedback, it’s crucial to **avoid ‘survey fatigue.’** Over-surveying can lead to reduced response rates and lower engagement levels. Therefore, **coordinating the timing** of surveys across different groups within the organization is essential to prevent overlap and ensure reasonable gaps between surveys. At Roku, the Developer Experience group collaboratively works with the People team and Enterprise Engineering to effectively **manage survey schedules** .


By conducting well-designed surveys, we can gain valuable insights into the state of developer productivity, enabling strategic decision-making to enhance efficiency, job satisfaction, and overall team performance.


# KPIs for Development Productivity


Quantifying developer productivity is an essential aspect of improving and managing it. Apart from using surveys to gain qualitative insights, we at Roku also rely on Key Performance Indicators (KPIs) to provide objective measurements of our team’s productivity. Our primary KPI is the ‘Cycle Time,’ which we believe accurately reflects our efficiency in delivering value.


## Cycle Time


We define Cycle Time as the period from the inception of an idea to its deployment in production. This overarching metric can be dissected further into distinct phases to identify bottlenecks and areas of improvement:


1. **Inner Loop:** This refers to the **individual developer’s workflow** . It commences with code-writing and local tests execution and extends through debugging and iterative refinement of the code. We’ve implemented a build audit tool that reports software build times on local environments to the cloud, which helps us quantify and optimize this phase.
2. **Outer Loop:** This encompasses the steps from **merging the code** into the main branch to getting it reviewed and accepted. We use a combination of in-house build data pipelines, dashboards, and data from our Source Code Management system, GitLab, to evaluate this phase.
After encountering numerous challenges in the Outer Loop for our embedded systems over the past two years of unstable Continuous Integration, we invested in the Pre-Merge validation system to improve the stability of this stage. This system ensures that the merge request doesn’t break the build or unit tests and passes preliminary functional tests. By doing so, it prevents ‘bad’ code from entering our Continuous Integration system. Through multiple iterations, we have chosen 40 minutes to be the target delay for a given merge request to go through the pre-merge pipeline. This change despite impacting the raw Outer Loop Cycle Time, has enhanced the ‘state of flow’ and improved developer satisfaction. See details of our recent[presentation](https://conf.researchr.org/details/icst-2023/cciw-2023-papers/2/Enabling-Pre-Merge-CI-on-your-TV) at CCIW summit.
3. **Pre-Production:** This phase includes **integration testing, load testing** , and other validation processes necessary before deploying to production.
4. **Production Rollout:** This final phase is the **actual deployment** of the code into the production environment.


Segmenting the Cycle Time and understanding each phase’s unique challenges has been pivotal in guiding our improvement efforts more effectively. This ongoing refinement of our metrics underscores our dedication to continuous enhancement and the optimization of developer productivity. It’s important to remember that alongside quantifiable KPIs, the engineers’ self-reported productivity also holds significant weight. Merging these two perspectives enables us to achieve a more holistic understanding of overall productivity.


## Anti-KPIs


While focusing on objective measurements is critical, it’s important to **avoid the trap of using easily measurable but misleading metrics** as proxies for productivity. We refer to these as ‘Anti-KPIs’. Examples include:


1. **Lines of Code Produced:** This metric often encourages **quantity over quality** . It fails to account for the complexity of the code or its maintainability.
2. **Number of Bugs Closed:** This might i **ncentivize quick fixes over comprehensive solutions** , or worse, encourage bug creation to inflate bug closure rates.
3. **Number of Commits Merged:** This fails to **consider the impact or relevance** of the changes made.


While these Anti-KPIs may provide some insights when viewed in conjunction with other factors, their standalone use can foster counterproductive behaviors and fail to offer a true measure of productivity and can **incentivize the wrong behaviors** .


Our approach merges both subjective perspectives through surveys and objective KPIs providing us with a more **comprehensive view of developer productivity** . As highlighted by Forsgren et al (2021), it’s critical to be mindful of common misconceptions in this field.


# Conclusion


The quest to understand and improve developer productivity is a fascinating journey, one that holds immense potential for **transforming the landscape of software engineering** . At Roku, we have laid down the groundwork with our defined metrics and methodologies. However, we recognize that this is an **ongoing process** . We are committed to continuously refining these metrics, learning from our experiences, and adapting our strategies to foster an environment that truly enhances productivity, job satisfaction, and tenure. The journey ahead promises exciting opportunities for growth and innovation, and we look forward to navigating it with our dedicated team of engineers.


## References / Additional reading:


1. Gallup 2022 State of Global Workplace 2022 Report[https://tinyurl.com/3seebya2](https://tinyurl.com/3seebya2)
2. Csikszentmihalyi, M. 2008. Flow: The Psychology of Optimal Experience. Harper Perennial Modern Classics.
3. Noda A, Sorey M, Forgren N, Greiler M. 2023 “DevEx: What Actually Drives Productivity” Communications of the ACM[https://dl.acm.org/doi/pdf/10.1145/3595878](https://dl.acm.org/doi/pdf/10.1145/3595878)
4. Forsgren, N., Storey, M., Maddila, C., Zimmermann, Houck B and Butler J. 2021 “The SPACE of developer productivity: There’s more to it than you think. ” Communications of the ACM[https://queue.acm.org/detail.cfm?id=3595878](https://queue.acm.org/detail.cfm?id=3595878)


The post[Unlocking Potential: Rethinking How We Measure Developer Productivity](https://engineering.roku.com/unlocking-potential-rethinking-how-we-measure-developer-productivity) appeared first on[Engineering Blog](https://engineering.roku.com/) .
