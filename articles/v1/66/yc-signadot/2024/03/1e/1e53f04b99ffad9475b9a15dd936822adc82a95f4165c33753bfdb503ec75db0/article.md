---
schema_version: "1.0.0"
document_id: "1e53f04b99ffad9475b9a15dd936822adc82a95f4165c33753bfdb503ec75db0"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-improve-and-measure-devex-in-your-organization/"
published_at: "2024-03-09T23:50:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:c80318c0009cb005fcc6a1462d8ddbfef6d7cfeddb54c08dd2bf56b4bbdc90bc"
---

# How to Improve and Measure DevEx in Your Organization

Developer experience (DevEx) — the overall environment, tools, practices and culture that software developers encounter in their daily work — is one of the greatest influences on your ability to hire and retain top talent. Things like development environments, workflows, tools and processes, and the overall work culture play a critical role not only in how happy your developers are but also their productivity, efficiency and output — and your bottom line.


In my previous article, I shared seven reasons it’s essential for IT leaders to prioritize developer experience. In this article, I’ll explain some of the ways you can improve and measure DevEx in your organization.


## **9 Ways to Improve DevEx**


There are several processes and practices that can significantly enhance the developer experience, especially for backend engineers working in cloud-native microservices environments.


### **1. Environment Standardization and Containerization**


You can’t really talk about DevEx without talking about the area that takes the most work and yields the best results: standardizing environments. Using containerization tools like[Docker](https://www.docker.com/?utm_content=inline+mention&utm_source=the+new+stack&utm_medium=referral&utm_campaign=tns+platform) and orchestration systems like[Kubernetes](https://roadmap.sh/kubernetes?utm_source=the+new+stack&utm_medium=referral&utm_content=inline-mention&utm_campaign=tns+platform) can standardize development environments. This reduces the “it works on my machine” problem, ensuring consistency across development, testing and production environments.


However, standardization isn’t easy. Every team will have reasons they don’t want to give up their way of deploying production containers, not to mention their technique for development and test environments. One reason for strong pushback is that individual teams will lose expertise with standardization: As you standardize, some teams’ expertise in[Microsoft](https://news.microsoft.com/source/?utm_content=inline+mention&utm_source=the+new+stack&utm_medium=referral&utm_campaign=tns+platform) Azure functions or self-hosted Kubernetes won’t be applicable, and it’s natural to resist learning something new when the only reason is “we want every team to be the same.”


### 2. Comprehensive Documentation and Onboarding


Having well-structured and up-to-date documentation for codebases and systems is crucial. This includes having a streamlined onboarding process for new engineers to make it easier for them to understand and contribute to projects quickly.


It’s a good idea to keep documentation and onboarding linked in your strategic plan because any improvement in documentation is actually an improvement in team culture.[Large language model](https://thenewstack.io/llm/) (LLM)-based documentation or self-documenting code will never solve the problem of poor developer experience because the problem with your current documentation isn’t its word count. As[Vincent](https://vple.me/posts/documentation-culture/) put it in an article on doing documentation right:


“I don’t think that writing is the real challenge when it comes to documentation. I think the real challenge is building a culture around documentation.”


In the same way developers should be thinking about testing as soon as they start developing a feature, a[good documentation culture](https://thenewstack.io/an-engineers-best-tips-for-writing-documentation-devs-love/) thinks about how any code will be understood by others, and how communication via documentation can help.


### 3. Toolchain Optimization


Regularly reviewing and optimizing the set of tools and software used by developers can significantly improve their efficiency. This includes IDEs, version control systems, issue-tracking tools and communication platforms.


Note that this doesn’t necessarily mean implementing a developer portal. All-in-one tools like developer portals only make sense at certain scales. As[Hazel Weakly](https://hazelweakly.me/blog/so-you-want-to-hire-for-developer-tooling/) put it: “Implementing Spotify’s Backstage before you’re at 500–1,500 engineers is a mistake.”


### 4. Automated Code Quality Checks


Implementing automated code quality tools, like static code analyzers and linters, helps maintain code quality and standards without extensive manual review, thus reducing the cognitive load on developers.


### 5. Regular Code Refactoring


Encouraging and allocating time for regular code refactoring helps in maintaining a clean, efficient and understandable codebase, which in turn makes development work more enjoyable and less cumbersome.


### 6. Collect Feedback From Product Engineers


It’s critical to treat DevEx as a product within your team. If you make great tools for developers but, for very basic reasons, the developers never adopt them, you’ve failed. In a piece on treating developers as your target market for DevEx,[Romaric Philogene](https://romaricphilogene.substack.com/p/platform-engineering-2-treat-developers) said:


“When we start seeing our platform as a product, our focus shifts to user-centric development. This shift means understanding the pain points, needs and desires of our ‘customers’ — the developers. It’s about building a platform that not only works but works in a way that delights those who use it.”


While compliance, monitoring and management are all potential benefits of an effective developer experience, they can’t be your first focus. You must collect feedback from engineers and build tools that reduce their toil; without that you can’t succeed.


### 7. Effective Incident Management


Implementing robust incident management processes, including clear escalation paths and post-mortem analyses, can reduce stress and improve the overall development experience. You may read this and think, “Isn’t this a site reliability engineer’s (SRE’s) job?” Their focus is on executing plays effectively during an incident. DevEx should help create the systems for handling all incidents. With a focus on treating the engineering teams as a market for our ideas, DevEx should help get all teams to adopt better practices.


### 8. Developer Autonomy and Empowerment


Allowing developers to have a say in decision-making processes, tool selections and project directions can lead to a more engaged and motivated team. Ownership is an absolutely critical component of the developer experience. The job of DevEx engineers is not to fix things, but to empower other engineers to do what they need done as efficiently as possible. As[Tobias Mende put it](https://unblocked.engineering/blog/developer-experience-platform-team/) :


“As a platform unit, one of the highest goals is to get out of the way of other units. Instead of doing work for them, you should enable them to do their work themselves.”


In short, things like tweaks to your deploy pipeline, which might be easy for you to implement for another team, are really an opportunity to examine how you can empower other engineers to make these changes themselves.


I don’t want to sugarcoat it: empowering engineers to fix their own problems means more work in the short term. But the positive results are much bigger than the time spent on tooling. Allowing all engineers to tweak the deploy pipeline will unlock superpowers for your entire engineering team.


### 9. Cross-Functional Collaboration and Knowledge Sharing


Something a lot of military historians seem to agree on is that Napoleon was great at reinforcing victory. If three armies were fighting and one was breaking through, he’d reinforce the successful army and press an advantage. OK, OK, so how does this relate to DevEx? When you have one team that’s adopting new tools fluidly and doing great things and another team that’s struggling, it’s natural to focus on training and helping the team that’s struggling. But it can be more effective to see how far you can go with a more successful team. Once they’ve achieved something great, you’ll have an easier time convincing other teams to learn from them. Beyond knowledge sharing, there’s also the importance of collaboration between stages of the software lifecycle. Encouraging collaboration between different teams (like development, operations and quality assurance) promotes a broader understanding of the project lifecycle and reduces silos.


These practices, when implemented thoughtfully, can create a more productive, efficient and enjoyable environment for developers, which is essential for innovation and long-term success in the competitive field of software development.


## Measuring Whether DevEx Is Working


If the purpose of developer experience is to enable developers, then part of the answer to measuring its success is the circular: “We know DevEx is working well if dev is working well.” Thankfully, you don’t have to measure the whole team’s performance to know when DevEx is worth the effort. It isn’t possible to use 100% quantitative metrics to measure DevEx success. While you should be able to measure success in areas like better dev retention, things like more cohesive dev culture are not so clear-cut.


### Quantitative Metrics


Key performance indicators (KPIs) like deployment frequency, change lead time and mean time to recover offer insights into the efficiency and resilience of development processes. Regularly tracking these metrics can help identify areas for improvement in DevEx. One helpful measurement framework is the DevOps Research and Assessment ([DORA](https://docs.gitlab.com/ee/user/analytics/dora_metrics.html) ) metrics (which[I’ve written about](https://www.signadot.com/blog/how-to-do-dora-metrics-right/) before), but it’s important to realize that DORA metrics primarily measure the health of your developer experience, not the work of your DevEx team.


I don’t have the space to fully explore the distinction, but to take one example: DORA metrics measure the time between finding a problem in production and fixing it. While a good developer experience can help respond to incidents, it’s not fair or reasonable to say, “We had more outages year-on-year, so the DevEx team must be doing a bad job.”


### Qualitative Assessments


Quantitative data should be complemented with qualitative feedback, and it probably makes sense to treat qualitative assessments as more important. Regular surveys, one-on-one interviews and retrospectives provide valuable insights into the developers’ subjective experiences and challenges. Surveys and interviews should also be the starting point for any significant work by DevEx engineers: You have to identify developer pain points before you implement solutions. By starting with developer surveys and conversations, you can help provide the tools developers will want to adopt.


While DevEx isn’t solely responsible for any DORA metrics outcome, you can expect DevEx to directly affect survey responses to questions like:


- “How easy is it to release code in our current deployment pipeline?”
- “Is our developer tooling more advanced than other development teams you’ve worked with?”
- “How satisfied are you with the developer environment you use to experiment with new code?”


### Continuous Improvement


DevEx is an evolving process. Regularly revisiting strategies, tools and practices based on collected metrics and feedback helps ensure continuous improvement and adaptation to emerging technologies and methodologies.


## How to Focus on Developer Experience


In many of the scenarios above, the process of developers testing code and releasing it to production with confidence can be a major frustration that contributes to a poor developer experience. When teams worry that their code, which they’ve already run in dev, test or staging environments, will break when trying to run in production, they have trouble rating their developer experience as “good.”


Signadot supercharges the developer experience by improving how developers test their code in a shared environment. With request isolation, your teams can deploy their experimental code to a shared, high-accuracy environment.


If you’d like to join a community that’s enabling DevEx at a high level and talk about the right strategies for a smooth testing and deployment experience, join the[Signadot Slack community](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) to connect with others trying to increase developer velocity.


## Conclusion


Investing in DevEx transcends improving technical processes; it is a strategic decision that impacts a company’s competitiveness, financial health and innovation capacity. For engineering leaders, prioritizing DevEx is essential to nurturing a creative, efficient and satisfied engineering team capable of driving business success.
