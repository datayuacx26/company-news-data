---
schema_version: "1.0.0"
document_id: "b80c08c47e8f851328978d45f7baf5c6f36b415e6cf647fe5b531361fecdb06f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-is-hiring-for-the-engine-team-58c87ef208b5"
published_at: "2018-05-09T19:40:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:a2727b2d9ece546ce0759c6447c13d6ee26579b73b6abd7359590b5a426fdc2f"
---

# Apollo is hiring!

Last updated: October 10th, 2018


**tldr;** We’ve just hired a VP of Engineering,[Claire Hough](https://www.linkedin.com/in/claire-hough-a9345/) , and we’re growing our engineering team with a specific focus on full stack and backend infrastructure developers. You can find more information about our roles and apply[here](https://www.meteor.io/jobs/) . We look forward to meeting you 🙂


Hi everyone! I’m Danielle, an Engineering Manager at Apollo, and you may not know this but **we’re hiring** 😃 We have a number of positions open across the company, and on the engineering side we’re in particular looking for developers to join us and help build our Apollo platform: *our family of tools for developers that helps them to build, query, and analyze their data in graph form.*


There are many tools in the Apollo platform, some of which are open source and have a thriving global contributor community, some of which are proprietary technologies that we’ve developed in house for advanced Apollo users and customers.


We’re well known for our work in open source JavaScript tooling, but we’ve also built a distributed cloud system that processes massive amounts of performance metrics data from GraphQL servers and schemas which our advanced users and customers use to gain insight into their API’s utilization, history, and management.


We’ve built our platform using modern technologies across the stack. Our analytics interface is built on React, D3, Apollo Client, GraphQL, Tailwinds, etc. and our backend systems are built on top of Kafka, TypeScript, Kotlin, Postgres, and Druid. If you’re interested in helping shape the future of developer tools you may already even be using, using some of the most modern and popular tools available, please check our our jobs website and consider applying at[apollographql.com/careers](https://www.apollographql.com/careers) . We’d love to meet you! 👋


## Our company


We’re a small team of ~35 people distributed across the globe and we’re on a mission to improve how people develop software. We work hard to build tools that make application development easier, more powerful, more enjoyable, and accessible to more people. We believe in the impact we’ll have on the world when more people can develop software to solve their own problems, and when more software developers can focus on solving the world’s problems rather than solving their app’s configuration, deployment, testing, data fetching, and other boilerplate problems.


We’re building the future of GraphQL. We’re ***the*** leaders of GraphQL tooling across the stack, and we’re all-in on GraphQL because we‘ve seen the benefits that its workflow has brought to product teams across the industry and we’ve reaped the rewards ourselves in our own application development. We were lucky to discover the transformational benefits of switching to a GraphQL workflow early on and we’ve been building tools to help others do the same since January 2016 — more than two years now 🙀.


The Apollo team, distributed across the world and headquartered in San Francisco.


Over that time we’ve seem tremendous growth in the GraphQL ecosystem and in our Apollo community. As such, our team is also growing 😃


We’re hosting the third annual[GraphQL Summit](https://summit.graphql.com/) in San Francisco in less than a month *(the biggest event of the year for us)* and we sponsor our team members to travel, represent, and give talks about GraphQL and Apollo at meetups and conferences around the world. We’re driving GraphQL adoption forward across the stack with our platform, its three main components being:


- **Apollo Client** — the most popular GraphQL client and the second most popular frontend state management tool for React after Redux.
- **Apollo Server** — the #1 choice for writing GraphQL servers with JavaScript.
- **Apollo Engine** — the only tool for schema management, schema versioning, and GraphQL-specific performance monitoring and API usage insights.


Our company’s roots started in open source tech with the[Meteor JavaScript framework](https://www.meteor.com/) , so it’s worth mentioning that building, maintaining, and contributing to open source projects is a core part of our development culture — right alongside building and growing the projects that our customers love and that bring revenue. We’re proud of our flexible and remote-first work culture. Our team is distributed across 5 continents and 7 timezones and we fly everyone to San Francisco once a quarter for our “Everyone in Town” weeks, which are hands-down my favorite weeks at the company personally.


## What we’re building — the Apollo platform


What I’m particularly excited to share with you in this post is more information about[Apollo Engine](https://www.apollographql.com/engine) , the tool that we build for advanced GraphQL users who need critical insights and governance over their APIs. Engine is our flagship product for GraphQL and we’re solving some challenging underlying problems in data processing and data visualization to make those features happen.


The first feature we ever built in Engine: traces of resolver execution timing for GraphQL requests.


At the core of Engine is a set of performance monitoring tools that help you understand how your GraphQL server is running and how your API is being utilized in a GraphQL-centric way. To offer live usage metrics in-app, we’ve built a data ingestion pipeline in Engine to accept, process, store, and quickly serve metrics data that Engine is constantly receiving from the GraphQL servers of our users around the world.


Engine’s Performance tab, showing request volume and server latency metrics over the last day.


To bring that data to life, we’ve built a set of complex and highly interactive data visualizations. We’ve focused on bringing every detail of server execution to the fingertips of our users, so that our users are empowered to solve their own problems and can focus on making their apps the best they can be — not on tracking down edge cases, finding rogue performance problems, ensuring that they’re running valid queries, etc.


As developers ourselves, one of the most fun things about building developer tools is that we’re essentially building tools to make our own lives easier. And with that, our tools end up helping our friends, our community, and large companies with large teams in our greater industry as well. There have been several instances while developing Engine where the feature we were building actually helped inform how to build the feature itself. I know that sounds confusing — it took me a while to wrap my head around the inceptive properties of building devtools — but that’s all part of the fun of working in the devtools space.


## The tools we use — our tech stack


In our open source work we pride ourselves on being at the cutting edge of JavaScript development. We follow the best practices in JavaScript and TypeScript closely and drive a lot of them ourselves. We’re among the first people to try out new features and APIs for tools like React when they’re released, and are often able to secure pre-releases that we can build upon. This mentality of keeping up with and contributing to tech innovation applies not only to how we build our open source tools, but how we build the rest of our platform as well. Our analytics and interface runs on[React](https://reactjs.org/) ,[D3](https://d3js.org/) ,[Apollo Client](https://github.com/apollographql/apollo-client) , and[GraphQL](http://graphql.org/) . Our cloud system that powers it runs on[Apollo Server (Node)](https://www.apollographql.com/server) ,[TypeScript](https://www.typescriptlang.org/) ,[Kotlin](https://kotlinlang.org/) ,[Kafka](https://kafka.apache.org/) , and[Druid](http://druid.io/) .


Apollo Client growth over the last year — May 2017 to May 2018


We have an internal culture of dogfooding all our own technology. That means that we’re the first to run the newest versions of Apollo Client and our other open source GraphQL tools in the other parts of our platform that we build. Our data visualizations are built in D3, often with custom chart code because the interactions we build are more nuanced than what’s typically offered in reusable charts libraries.


We use Engine in Engine to help us better understand how our own schema is being used by our customers. Though seldom talked about publicly, our desire to run our product on modern technologies extends to our backend stack as well. For stability and scalability, we undertook a significant project this year to overhaul our infrastructure which was initially built on Kotlin and Bigtable to run on Apollo Server, Kafka, Kotlin, Postgres, and Druid — a project that’s now on the cusp of finishing. We’ve built our new Druid infrastructure in an innovative way that will not only improve our system’s ability to run smoothly, but will open doors for us to build a multitude of new features that were infeasible on our old architecture.


We’re constantly seeking ways to improve the software development experience of engineers across the stack — no matter if it’s improving the ergonomics of JavaScript development with a new Apollo Client feature, testing out and providing the very first set of internal feedback to the implementation of schema stitching in the[graphql-tools](https://github.com/apollographql/graphql-tools) package, opening issues and offering pull requests to fix bugs we’ve run into in[graphql-java](https://github.com/graphql-java/graphql-java) , contributing to the[Druid open source project](https://github.com/druid-io) to build support for some of our custom use cases, or building a product (Engine) to help developers understand exactly what’s happening with everything related to running their GraphQL layer 😃


---


I put this article together is because I know there are members of our Apollo community out there that are interested in helping us push forward and develop everything I’ve mentioned. If you follow our blog, you’re probably already familiar with and maybe even using some subset of our Apollo tools. I want to make sure that you know that **we’re hiring and that we’d love to meet you** 😃 We’re a hard working and talented group, but ultimately we’re just a small team of really normal people that have big dreams and aspirations.


If you’ve made it this far, thank you for reading 🙏 It’s been a privilege and an exciting ride to be on this GraphQL journey with the Apollo team, and we’d be honored if you considered joining us as well. If what I’ve written has resonated at all with you, if you’re interested in what we’re working on, or if you want to learn more about who we are and what we’re building here, please consider applying to one of our open R&D positions:


- [Backend Services Engineer](https://www.meteor.io/jobs/#b8ec842e-e79a-455e-a665-b312892d946e)


We look forward to meeting you! 👋 For even more information, you can visit our company website —[apollographql.com/careers](https://www.apollographql.com/careers) .


Our team! You can learn more about us by visiting[apollographql.com/careers/team](https://www.apollographql.com/careers/team/) .


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
