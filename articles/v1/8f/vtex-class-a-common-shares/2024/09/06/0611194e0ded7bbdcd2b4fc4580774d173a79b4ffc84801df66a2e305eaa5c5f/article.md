---
schema_version: "1.0.0"
document_id: "0611194e0ded7bbdcd2b4fc4580774d173a79b4ffc84801df66a2e305eaa5c5f"
company_key: "vtex-class-a-common-shares"
company: "VTEX"
source_id: "vtex-class-a-common-shares-rss-fffff04a1e70"
canonical_url: "https://vtextech.substack.com/p/productivity-in-software-engineering"
published_at: "2024-09-18T13:20:23+00:00"
first_seen_at: "2026-07-20T23:19:02.969240+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:8036ddaac0e9dbd5cbff7597ad794b496a193e7e275c87dfd840a3e76b0ec6e3"
---

# Productivity in Software Engineering: Beyond lines of code, the importance of experience in the development cycle

# Productivity in Software Engineering: Beyond lines of code, the importance of experience in the development cycle


### A practical approach on how VTEX improves productivity with experience


[VTEX Tech Blog](https://substack.com/@vtextech)


,[Bruno Dias](https://substack.com/@managementinanutshell)


,[ROd Zaccara](https://substack.com/@rodzac)


, and[Gustavo Franco](https://substack.com/@grfranco)


Sep 18, 2024


In any industry, measuring productivity is essential for identifying areas that need improvement and implementing effective changes.


When discussing software development, some still say that the number of lines of code written or how many commits you did in one day is an adequate measure of productivity.


It is still expected that the productivity of a team or a software engineer will be associated with the number of lines of code. This is a simplistic and potentially harmful idea for a team's software quality, dynamics, and culture. Software development is a creative and complex process that involves much more than just typing code. Efficient solutions often require fewer lines of code, while long codes can be confusing and ineffective.


**Code quality, ease of maintenance, efficiency, and functionality are much more meaningful metrics** . At VTEX, we believe that instead of demanding productivity, we choose to understand how to make people more productive.


### The Challenge at VTEX


At VTEX, we understand that a lousy development experience impacts productivity. Therefore, we saw the need to create a group dedicated to supporting our product engineering teams; we needed a way to measure and improve the satisfaction of our software engineers regarding the processes and tools they used to develop systems. This led us to first look for a solution to this experience problem and then measure and refine productivity if necessary.


It is important to highlight that our People Operations also monitors the qualitative experience of all VTEX employees. We work together to cross-reference information from the two qualitative analyses and enrich our understanding of the development experience at VTEX.


In this article, we focus on how we built a program for measuring the development experience qualitatively and quantitatively and, subsequently, on how building a holistic view of experience and productivity is key to evolving the business.


#### Choosing Quality First


After evaluating several measurement options, both quantitative and qualitative, we decided to start with quality—there is nothing like hearing directly from those who are experiencing the problem. We believe that by putting voices and experiences at the forefront, we could gain rich, relevant insights to improve our practices and tools, thereby positively impacting productivity. It is no wonder that the name of this initiative came to be called


**Voice of Developer (VoD)** .


### The Voice of Developer program


The VoD goal is to foster


**collective ownership** among developers and leaders in improving their productivity and experience through transparent feedback and shared experiences. Our Developer Experience team will talk with teams and share their initiatives to solve problems globally to avoid the


*[tragedy of commons](https://en.wikipedia.org/wiki/Tragedy_of_the_commons)* , where we fall into scenarios with different tools for solving the same issues.


We currently conduct a quarterly survey for qualitative and quantitative analysis, in which we seek to understand better our software engineers' experiences, needs, and challenges.


The research we adopted is a partnership between the


[DX platform](https://getdx.com/) and is based on studies published by researchers specialized in the area, with main references based on


**[DORA](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance)**[(DevOps Research and Assessment)](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) ,


**[SPACE](https://queue.acm.org/detail.cfm?id=3454124)**[(Satisfaction, Performance, Activity, Communication and Emotion in Software Engineering)](https://queue.acm.org/detail.cfm?id=3454124) and


**[DevEx](https://queue.acm.org/detail.cfm?id=3595878)**[: What Actually Drives Productivity](https://queue.acm.org/detail.cfm?id=3595878) . These articles can be used to seek more information about the principles behind this research model.


The results of this program have allowed us to act centrally on improvements and locally between teams. Together, we implemented improvements in both our software engineers' work experience and productivity. Some examples:


From the first survey to date, engineering reports a substantial increase in


***deep work*** -- which means being able to concentrate on work without interruptions. Based on initial responses, we instituted avoiding meetings on Wednesdays, which we internally called No Meeting Wednesday (NMW).


People with more experience report that deadlines are more realistic and the work direction is more precise, with two categories mapped as


***realistic timelines*** and


***clear direction*** .


In several teams, we saw improvements in the quality of the shift (


*on-call* ) and more time for code refactoring.


We also noticed that engineering reported a drop in productivity when we revisited our change management processes in production to increase our resilience. This made us plan even more investments in maintaining resilience and increasing productivity in the continuous software delivery flow.


#### Incorporating Quantitative Measures


While our initial approach focused on quality, we recognize the importance of including quantitative measures in our assessment. We are in the process of adding this data to our Voice of Developer program.


The objective is to correlate the qualitative analysis, that is, responses from engineering people, with quantitative data obtained directly from our systems based on


[DORA (DevOps Research and Assessment)](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) and not in lines of code.


We believe this combined approach will allow us to better understand how changes we make to our practices, tools, and processes affect the software engineering experience and productivity.


#### **Productivity Score**


In this journey, the global efforts of the Developer Experience team through delivering tooling for teams and local efforts of engineering teams to improve their workflows have impacted several metrics collected in VoD. With this, a natural question arises:


**How does improved experience impact developer productivity?**


When we defined the productivity score as a high-level indicator, it informed us if we were on the right path to enable our teams to have time to market and fulfill the business needs with the proper tooling.


**NOTE** : In this context, productivity refers to the percentage of developers’ time gained by reducing obstacles or inefficiencies in their work environment.


We have made quite progress since we started, reaching 11.3 in the productivity score, which means the organization's productivity was boosted by


**11.3 virtual engineers** .


*We can explore this methodology in another article.*


### Measures, objectives, and competition


Whatever methodology is used in your company, it is necessary to remember that


[Goodhart's "law"](https://en.wikipedia.org/wiki/Goodhart%27s_law) often applies in practice. A generalization of what Goodhart said is:


*"When a measure becomes a goal (or target), it ceases to be a good measure."*


At VTEX, we intentionally avoid competitive comparisons between teams or any type of gamification of metrics and even directly link them with rewards. Given that a fair comparison would require us to consider the maturity of the services, the number of people per team, and more.


In short, we saw in practice that


**a more holistic approach focused on the developer's experience generates increased productivity** and more impact on our business. With our qualitative and quantitative approach, we have a powerful tool for continuously measuring experience and productivity in software engineering.


---


If working in this environment interests you,


**[VTEX is hiring](https://job-boards.greenhouse.io/vtex) .**


A guest post by


[VTEX Tech Blog](https://substack.com/@vtextech?utm_campaign=guest_post_bio&utm_medium=web)


Inside stories of VTEX Tech


A guest post by


[Gustavo Franco](https://substack.com/@grfranco?utm_campaign=guest_post_bio&utm_medium=web)


Sr. Director of Engineering
