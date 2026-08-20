---
schema_version: "1.0.0"
document_id: "3c84f462611c5bc3bcd1567f73383655a9c71dc0d8448ec3c9be1f3abc47ed7e"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-03-20-askadeveloperyourquestionsanswered/"
published_at: "2020-03-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:a2af414bb48921c65e400f0e796624c4f213f59f5fbbc54b0d8b3cc13158830a"
---

# Ask a Developer: Your Questions Answered

We recently hosted a Q&A on our[trivago tech Twitter](https://twitter.com/trivago_tech) and[Life at trivago Instagram page](https://www.instagram.com/lifeattrivago/) , where we gave our followers the opportunity to ask our developers anything. Even though we tried to answer all of them on the day, we decided to compile some of the most recurring questions and their answers for you below!


## Getting a tech job at trivago


#### Do you offer any opportunities for React developers?


We don’t think in frameworks but in projects. So it helps to be able to adapt to other frameworks like Angular, Vue, Svelte, or Melody, too if needed. That said, we are always interested in developers with good React knowledge.


#### What do you look for when interviewing candidates?


We want to hire people who are a “culture add” – that means people who not only support the team’s values and professional ethics but also bring an aspect of diversity to the team. Alternative viewpoints, unique experiences, rare, specialized skill sets. And of course, an ability to do the job is fairly useful!


#### How would a new guy (newbie) in programming get to work alongside you?


You could actually start right away by contributing to one of our open-source projects at[github.com/trivago](https://github.com/trivago) . We also host a yearly[tech camp](https://techcamp.trivago.com/) for students. Apart from that, you can also participate in one of our Hackathons that we announce on our tech blog. But most of all, it’s essential to discover your passion, find an open position on our job page that interests you, and work towards that.


#### What are the ideal technical requirements for full-stack developers?


Every tech team at trivago has the freedom to choose their tech stack of preference, so it would depend on the team and role you’re applying for. In most of our job ads, the tech stack will be specified, so you’ll get the complete picture before you apply. Keep in mind that the roles are quite fluid, and switching between roles is also encouraged, so there is never an ideal set of skills we’re looking for. It’s also about being able to adapt and continuously learn new skills.


## Our tech stack


#### What’s the most common programming language used at trivago? Which stack are you using for frontend and backend?


By now, it’s mostly Kotlin, Go and PHP for backend, JavaScript for frontend, and Python and R for Data Science. Take a look at[this page](https://tech.trivago.com/about/) for our entire stack.


#### Which JavaScript frameworks are you using at trivago?


We use Angular, React, Vue, and[Melody](https://github.com/trivago/melody) (our own framework, which is also open source and a bit similar to React).


#### What kind of Machine Learning projects are you doing? Are you using R or Python?


We use both: R and Python (and a few more). One project we recently did was showing more relevant hotel photos when people were looking for Spa areas. We blogged about it[here](https://tech.trivago.com/2019/08/21/machine-learning-and-bathtubs-how-small-visual-changes-improve-user-experience/) . If you’re interested in that, we regularly speak at local meetups about current projects. Check out[this link](https://www.meetup.com/Dusseldorf-Data-Science-Meetup/) to stay in the loop!


#### Do you have jobs for C#, C++, or .NET?


Some of our teams do use C# and C++, but we don’t have specific roles dedicated to them. Overall, these languages are rarely found at trivago, though.


#### For your microservices communication, what tool(s) do you use to track and monitor the communication between the different services? And how can you easily detect a failed message even when the services are not down?


We’re using distributed tracing tools, Elasticsearch and Grafana plus Prometheus. Pretty standard stuff by now, but quite a high volume of metrics and logs. Our failover mechanisms depend on the criticality of the service. E.g. for booking transactions, we use a fallback queue that is stored in Kafka or persisted to disk for offline respooling in case Kafka is down. For less critical services, we might not need that, but we try to be upfront with our guarantees towards other teams.


## Working culture


#### Is it really cool to work there? Or are you being held hostage for positive blog posts?


Of course! We (H)appy (E)mployees (L)ove (P)rogramming… but seriously, it IS pretty cool to work here. As a working environment, trivago offers a lot more than many other employers can/do, so you actually enjoy being in the office. There are also many opportunities to learn and challenge yourself, which is appreciated by many who work here. A healthy work-life balance is also encouraged.


#### How can we learn more about your job? Process, time…


We regularly post articles on our tech blog and twitter that give insight into how we work and what we work with, so make sure to check back regularly.


#### Is the QA department fun at trivago, and does trivago offer the opportunity to change teams?


We don’t have a dedicated QA department – our QA engineers are spread across different pillars and teams. It’s a fun role because you work on the intersection between development and production. We have a big focus on test automation as well. Check out[this article](https://tech.trivago.com/2019/11/14/automation-first-approach-using-the-karate-api-testing-framework/) for more insights.


## How we work


#### What are the challenges you encounter when working closely with product management?


We try to work as closely together as possible to avoid big-bang moments. Developers are involved in ideation and don’t just do the implementation. This ensures early feedback on feasibility and leads to more pragmatic solutions. Bigger features get planned in sprints that we all agree on before starting. It’s important to clearly communicate the expectations from both sides on a regular basis.


#### Do you write micro-frontends?


No; or at least we don’t call it that. Different teams are still responsible for different parts of the website, though. The code however is in a shared repository.


#### What if development is not completed in sprint time? What extra do they do?


A sprint should help you plan ahead and get a clear picture of the expected workload. As far as we’re concerned, it’s a great way to communicate expectations to different teams. Unexpected challenges can always occur throughout a sprint and we account for that accordingly by adjusting those expectations, but not by lowering the quality of the final product. Keeping the quality up to our standards is the responsibility of each team and not every team at trivago is working in sprints. For example, many operations teams are using Kanban instead.


#### How is the integration and end-to-end testing integrated with a release?


Every release runs through a fully automated end-to-end test before a release. We do that with open-source tools like Selenium, Cucumber, or our own custom tooling. Some of it is open-source, like Cluecumber. (Check out[this article](https://tech.trivago.com/2017/11/16/cluecumber-report-maven-plugin-for-cucumber-test-reporting/) for more.) On top of that, we also run profiling tools like Blackfire or Lighthouse to test for performance regressions, and we have a custom tool called Hydra to test for accessibility issues. It’s not perfect, but we still find a lot of bugs this way.


#### What’s the approach while building a mobile app?


Good question! Our iPhone and Android apps are developed by separate in-house teams. We wrote a bit about our planning- and development methodology on our techblog[here](https://tech.trivago.com/categories/mobile/) . Generally, we try to keep processes lean and use a modern stack (Kotlin and Swift) to build apps.


We also try to avoid close coupling between the apps and the website and use a common data-layer based on GraphQL instead.


#### How do you deploy new features? A/B testing? K8s?


We are big fans of continuous integration. Deployment mostly happens automatically by now. For the most part, we’re using[Kubernetes](https://kubernetes.io/) on[Google Cloud](https://cloud.google.com/) for user-facing applications. We use some traffic replication with[istio](https://istio.io/) , too. We have a custom framework for A/B testing (we call them C-Tests) both frontend and backend code with quite a bit of machinery to get an analysis for every single test we run. At any given point in time, we run dozens of concurrent tests depending on user agent, location, and random assignment.


#### How do you get the pricing of the hotels by the minute? Do you use any advanced scraping methods too?


The secret sauce is direct, real-time connections to our partners like Online Travel Agencies (OTAs) or the hotel chains. No scraping needed! That said, it’s still a challenge to keep the prices up-to-date while keeping the load on our systems low. That’s why we have a custom-built price-cache called “Pricematrix” internally, which is a specialized, short-term key-value store, which takes user information into account.


---


Thanks to everyone who stepped forward and asked a question. Do you have any more questions for us about tech at trivago? Ask us on Twitter by using the hashtag #trivagotechtalk and tagging[@trivago_tech](https://twitter.com/trivago_tech) .
