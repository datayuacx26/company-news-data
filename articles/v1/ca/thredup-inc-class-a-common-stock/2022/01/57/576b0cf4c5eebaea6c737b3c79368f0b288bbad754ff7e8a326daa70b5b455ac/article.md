---
schema_version: "1.0.0"
document_id: "576b0cf4c5eebaea6c737b3c79368f0b288bbad754ff7e8a326daa70b5b455ac"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/re-introducing-thredup-engineering-85da8d6e78f7"
published_at: "2022-01-05T21:56:41+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:d875b29a94d5da7b1f012697f4bf83c43b6413bc716daac189dade6daf35c457"
---

# (Re)introducing thredUP Engineering

# (Re)introducing thredUP Engineering


[David Grumm](https://medium.com/@dgrumm?source=post_page---byline--85da8d6e78f7---------------------------------------)


6 min read


·


Jan 5, 2022


--


## It’s been a minute, so let’s get reacquainted


ThredUP Engineering has a long tradition dating back to[2010](https://labs.thredup.com/a-beginners-journey-with-ruby-on-rails) of writing about our technology, how our team works, and what we’ve learned in our pursuit to build the world’s largest online thrift store. But in the past few years we’ve gotten completely out of the habit. Let’s be honest, it can be difficult for engineers who are really busy and generally more accustomed to writing code than prose to come up with a steady stream of compelling content to share… but that’s what we endeavor to do. To get things (re)started, I thought it would be helpful to share an overview of our team, our approach and some of our tools to give context for the variety of engineering-related posts to follow.


## First, a bit about who we are


Our engineering team, like all teams at thredUP, is made up of a diverse collection of big thinkers; truth seekers; infinite learners; outspoken collaborators. We’re solving big problems in novel ways in order to do good for our customers, our planet and our business.


We’re currently a growing group of more than 100 talented and passionate engineers aligned on a mission to inspire a new generation of consumers to think secondhand first. We’ve built a custom ecommerce platform consisting of robust, distributed services and client applications spanning web, iOS and Android to serve the unique needs of our two-sided marketplace, and we’re scalingUP to meet the ever increasing demand of suppliers and shoppers of thrift.


thredUP Engineering team members, Jan. 2022


Engineering is primarily distributed among two development hubs located in Oakland, California and Kyiv, Ukraine with an increasing number of remote team members spread across the US, Europe and Asia. Since the first COVID19-induced shelter in place orders in March 2020, we’ve been fully remote. We’re organized into four broad groups:


**Product Engineering** : Delivering compelling digital customer experiences across web and native mobile applications, powered by robust, custom ecommerce services.


This group is obsessed with thredUP suppliers, shoppers and resale partners. They strive to make the experience of using thredUP fast, engaging and easy at every step of a user’s journey from Awareness to Conversion to Loyalty, and all points in between.


**Operations Engineering** : Designing, building and operating enterprise applications powering our supply chain and corporate functions (e.g. financial reconciliation services), enabling thredUP to deliver its revolutionary service at scale.


The proprietary systems and technologies this group creates run our distribution and processing centers and enable optimization of the flow of secondhand items from suppliers to shoppers.


**Data Engineering & Data Science** : Advancing thredUP’s mission by extracting value from our data.


Our data group brings our data to life in the form of machine learning solutions, deep analytics, and interactive dashboards. Data scientists and engineers brainstorm ideas with company leaders, work closely with business stakeholders to scope projects, develop technical plans, and execute high-impact products with a wide audience.


**Cloud (Infrastructure) Engineering** : Providing platforms and tools that enable thredUP engineering teams to rapidly build and deploy scalable, reliable and secure solutions.


This group applies DevSecOps practices and principles to provision and manage Kubernetes clusters and AWS resources which host all thredUP services. They install, configure and integrate tools, often creating custom solutions in order to improve engineering productivity (i.e. CI/CD, observability, etc.) And they maintain security and compliance of the systems.


Among all these groups, engineers from various disciplines are organized into cross-functional delivery teams we call “pods”. Taking inspiration from[Spotify’s Squad model](https://blog.crisp.se/2014/03/27/henrikkniberg/spotify-engineering-culture-part-1) , Amazon’s two-pizza team rule, and key concepts from[Inspired](https://svpg.com/inspired-how-to-create-products-customers-love/) ,[Accelerate](https://itrevolution.com/accelerate-book/) ,[Team Topologies](https://teamtopologies.com/key-concepts) , and others, the structure of our pods is guided by the following principles:


- A team should be small (e.g. 5–9 people)
- A team should be long-lived (e.g. team lifespan is multiple years, member rotations are infrequent, after 12–18 months preferred)
- A team should be autonomous (i.e. full-stack capable, loosely coupled to other teams)
- The size and complexity of the team’s code base must not exceed the team’s cognitive capacity
- Each team should have a mission and “north star” KPIs with agency to achieve them
- Every subsystem (a.k.a. service, app, etc.) should be owned by exactly one team; contribution to that system by other teams is permissible


The majority of our pods are considered “stream-aligned teams” focused on creating optimal experiences for thredUP customers, while others are “platform teams,” which provide common services that stream-aligned teams leverage. Small “enabling teams” anchor our Guilds to ensure that we continuously stay abreast of and assimilate relevant trends in emerging technologies, architectures and techniques. Together, all of these teams are building the future of resale.


Press enter or click to view image in full size


thredUP Engineering team structure, focus on Product Engineering


## A glimpse into how we work


We don’t prescribe one way for all our teams to function, just as we don’t prescribe one set of technologies they can use to get their job done. Teams have agency to use the technologies, techniques and methods that will help them achieve their goals most effectively, and where they can find leverage by using something we already have, all the better. We experiment often and share the results across teams; things that work well for one team are often adopted broadly. *We are focused on outcomes, and aren’t afraid to do things differently to enable our engineers to perform their best.*


“Maker Days” exemplify this. Years before the pandemic, Engineering experimented with weekly meeting-free days, dubbed “Maker Days”, intended to give people the space and time to focus, uninterrupted, in order to make real headway on challenging tasks. During these days, folks could opt to work from anywhere — office, home, a local coffee shop, you get the idea. It worked so well for Engineering, that it was adopted company-wide and helped prepare us all to work effectively while remote throughout the pandemic.


More recently, we adopted a 4-day work week to give people greater flexibility in balancing work and personal obligations. Results of this experiment have been very positive so far, with no dip in organizational productivity and positive qualitative feedback from team members. So now we have a 4-day work week, with Tuesday Maker Days.


Currently, we’re taking a fresh look at what “return to office” means for thredUP. We’ve hardly skipped a beat while working remotely for the past 20+ months, but we also know that there are unique advantages to working together in the same physical space. In order to reap the benefits of in-person collaboration while maintaining the flexibility of remote work, we’ve decided to try a different kind of “hybrid” approach — the choice to work remote or in-office on any given day is left up to each individual. In our model, the office becomes something like a co-working space where teams can come together ** on occasion for high-fidelity collaboration such as brainstorming, group problem solving, off-sites and relationship building activities. To streamline coordination overhead and encourage a regular cadence of in-person interactions, we’re reserving the third week of each month for “in office week” for things like all-hands meetings, out-of-town guest hosting, and team celebrations. Keep an eye out for a future post on this subject where we’ll share results of this experiment.


## Some of the tools we use


As noted previously, we don’t prescribe a single set of technologies or tools for our teams so we employ quite a few. Here is a sampling to give you a sense of our technology landscape. Expect to see future posts which will dive deeper into these and others we come across.


### Native Mobile


- MVVM
- (iOS) Swift, ObjectiveC
- (Android) Kotlin, Java


### Web Front-End


- Progressive Web Application
- Typescript, React, Apollo, Express
- Webpack, Service Workers, Babel
- Jest, Cypress


### APIs and Services


- Microservices over Monoliths (the journey is long)
- Node.js, GraphQL
- Ruby-on-Rails, Grape
- Kotlin, Java
- Elastic, Split.io, Temporal


### Data Platform


- Lakehouse
- Snowplow, Databricks, Looker


### Cloud Platform


- Cloud Native
- Amazon Web Services (E2C, S3, EKS, Aurora, Redshift, etc., etc.)
- Kubernetes, Istio, Helm, Terraform
- Redis, RabbitMQ, Kafka
- CloudFlare, Auth0, OneLogin
- Datadog, Teleport, HackerOne,


### Additional Tooling


- CI/CD: Github, Jenkins, Github Actions
- Observability: Datadog, Rollbar, Fullstory


## Until next time…


That’s it in a nutshell — a brief re-introduction to thredUP Engineering at the beginning of 2022. As we look forward to the rest of this year and beyond, we are excited by the technical, organizational and business challenges ahead. We’re glad to be back and ready to share more about our journey, which we hope you’ll find insightful and informative. And if you think thredUP Engineering might be the right fit for you, please check out our[Careers page](https://www.thredup.com/careers) ordrop us a line , we’d love to hear from you.
