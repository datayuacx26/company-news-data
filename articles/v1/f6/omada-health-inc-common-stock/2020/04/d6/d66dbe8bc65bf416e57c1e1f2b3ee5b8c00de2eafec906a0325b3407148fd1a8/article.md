---
schema_version: "1.0.0"
document_id: "d66dbe8bc65bf416e57c1e1f2b3ee5b8c00de2eafec906a0325b3407148fd1a8"
company_key: "omada-health-inc-common-stock"
company: "Omada Health Inc."
source_id: "omada-health-inc-common-stock-rss-9e1c3b1ccfca"
canonical_url: "https://medium.com/omada-health-tech/exploring-graphql-at-omada-f64133a50462"
published_at: "2020-04-28T19:01:21+00:00"
first_seen_at: "2026-07-20T23:18:22.449098+00:00"
fetched_at: "2026-07-28T22:26:48.304506+00:00"
content_hash: "sha256:856a9aab6a60a09c04e14785a8f30c527efd395571ef061cabbb0eca3ad51524"
---

# Exploring GraphQL at Omada

GraphQL


Graphql Vs Rest


# Exploring GraphQL at Omada


[Omada Health Engineering](https://medium.com/@OmadaEng?source=post_page---byline--f64133a50462---------------------------------------)


6 min read


·


Apr 28, 2020


--


*Written by*[Will Hastings](https://www.linkedin.com/in/willhastings4) *, Staff Engineer*


Gearing up to explore new technology at Omada! Photo by[SpaceX](https://unsplash.com/@spacex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on[Unsplash](https://unsplash.com/s/photos/space?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)


We build a lot of APIs at Omada. We build them to power the web, iOS, and Android apps our participants use to interact with their health coaches, take their lessons, track their data, and more. We build them to power the internal web app our health coaches use to understand what motivates our participants and help them make better decisions, as well as the internal UIs our support teams and operations managers use.


Traditionally, we’ve built APIs using a mostly[RESTful architecture](https://restfulapi.net/) and the conventions native to the Ruby on Rails framework that powers our backends. More recently, engineers from a couple of teams started exploring alternatives. After enumerating and considering the shortcomings of our current way of doing things, we decided to explore if adopting[GraphQL](https://graphql.org/) would help solve some of our challenges. We experimented with and evaluated GraphQL through incremental adoption, starting with a minor internal facing feature, and building up to it powering the next major feature for our participants.


In this post, I’ll cover:


1. The process we used to get started with GraphQL.
2. The strategies we used to overcome GraphQL’s learning curve.


In an upcoming post, I’ll cover the ways in which we discovered GraphQL solved some of our problems, including API documentation, data loading efficiency, mobile workflows, request correctness, and frontend state management.


## Adoption Process


Omada Engineering has long held a bi-weekly meeting where engineers from across all teams and levels can come together to discuss cross-engineering technical initiatives, shared challenges, and new ideas about technology and processes. For some things, a separate group of engineers will be assembled to dig deeper into a particular area. After discussing how we could improve the way we build APIs, we decided to form one such working group to analyze our current practices and investigate potential improvements.


When this group first met, we identified several shortcomings with our current API architecture and technology choices, including inconsistent documentation for client-side engineers, data loading inefficiencies, and complex frontend data management. We considered several solutions, including GraphQL and JSON API. We decided to go forward with GraphQL because of its robust tooling, growing community, and built-in type safety.


## Internal First


The first feature we used GraphQL for was a view in our customer support-facing internal app that displayed a paginated list of the blood pressure readings a participant has taken, which we were building out to support our program’s[expansion into hypertension](https://www.omadahealth.com/press/press-release-omada-health-adds-new-programs-fortype-2-diabetes-and-hypertension-self-management) . We chose this as a relatively straightforward feature for trying out a new technology (as opposed to the more complex UI health coaches get for reviewing blood pressure readings).


After releasing the customer support UI and confirming that it was bug-free, we continued over the next few months to roll out several more internal features using GraphQL, mostly new additions to our health coaching tools. During this process, the Coaching Engineering Team experimented with a few different ways of structuring GraphQL-related code on the backend and frontend, and with how best to leverage the libraries we’d chosen to get us going with GraphQL,[graphql-ruby](https://graphql-ruby.org/) and[Apollo Client](https://www.apollographql.com/docs/react/) .


We hope to share some of these learnings in an upcoming blog post on tips, tricks, and watchpoints we discovered while adopting and learning GraphQL.


## Mobile, the Next Frontier


Those of us on the backend and frontend web side who were advocating for GraphQL knew that if it didn’t work for our mobile teams, it wouldn’t work for our company. In order to find out how well GraphQL suited their needs, a couple engineers paired with or consulted with our lead iOS and Android engineers to help them prototype a simple feature as a test case for GraphQL. They investigated automatic code generation, security implications, and testability of Apollo’s GraphQL libraries for[iOS](https://www.apollographql.com/docs/ios/) and[Android](https://www.apollographql.com/docs/android/) . Though the prototypes were not meant to go to production, they gave us enough confidence that we’d identified the most important implications of GraphQL adoption on mobile.


## Making a Bet on GraphQL


After all the initial work to get to know GraphQL and try it out on our various platforms, we were left wondering what the next step was. So far, we had only shipped GraphQL usages to production that were part of our internal and coaching applications. In order to truly find out if GraphQL would work for us long-term, we needed to try it out on a participant-facing feature built in both the web and mobile apps. We needed to make sure we were really ready for our GraphQL API to take on the additional traffic and feature complexity, as well as to provide a level of stability necessary for serving our participants as a provider of digital care. We also wanted to be prepared to deprecate our GraphQL API without breaking mobile app versions still in use in the wild if we decided that GraphQL wasn’t for us.


The chance came along when our product team and one of our visionary engineers got together to propose a bold new feature, a new participant community experience based around shared interests or challenges. This feature was going to be sufficiently complex for both the backend/web and mobile developers that we believed it would serve as a true test of GraphQL. This also meant we needed to finish the infrastructure work needed to fully productize our GraphQL API, including:


- Limiting acceptable query complexity to avoid denial-of-service type attacks.
- Restricting[schema introspection](https://graphql.org/learn/introspection/) to authorized users.
- Integrating GraphQL API request-specific log data into our backend logging aggregation setup.
- Capturing performance data from our GraphQL API.


I’m happy to say we’re close to the initial launch of this new feature using GraphQL, and we remain satisfied with it as a tool for improving the way we build APIs.


## Scaling the Learning Curve


As awesome as GraphQL is, it and its associated libraries and tooling definitely come with a learning curve, especially for engineers used to a RESTful architecture and the default Rails conventions.


We leveraged the following strategies to get our engineers up-to-speed:


1. Tech Talk: Omada engineers frequently prepare and deliver tech talks to their fellow engineers, usually on Tuesdays during lunch. Earlier this year, I gave a talk to introduce GraphQL to the rest of Omada Engineering.
2. Pair Programming: Engineers at Omada frequently utilize pair programming to help us come up with better solutions, share knowledge, and decrease the need for separate code review. It provides a natural setting for one engineer to teach another about a new technology, and it helped us to spread GraphQL knowledge amongst the teams using it.
3. Learning Time: Engineers at Omada are encouraged to take a couple of hours each week to invest in their professional development, whether it be through doing tutorials, watching conference talks, or something else. For us engineers who wanted to delve deep into GraphQL, this dedicated time provided a great opportunity to level up. Additionally the Coaching Engineering team lead and manager arranged for the whole team to invest a day on working through a GraphQL tutorial together.


## Conclusion


While Omada still has a ways to go in our journey with GraphQL, we’ve made a solid start and are already seeing the benefits. I’m excited to see our first GraphQL-powered, participant-facing feature go live soon, and to see how our use of GraphQL evolves over the coming months.


Stay tuned for the next post on our GraphQL journey, in which I’ll detail how GraphQL helped us solve several problems with API development.


*Thank you to the following fellow Omadans for their help with GraphQL adoption and/or this blog post: Franck Verrot, Chris Constantine, Ethan Ensler, Alexander Garbowitz, Scott Stebleton, Austin Putman, Jonathan Wrobel*
