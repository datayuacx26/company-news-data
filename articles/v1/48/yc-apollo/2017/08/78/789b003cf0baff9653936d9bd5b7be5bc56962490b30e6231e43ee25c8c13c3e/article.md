---
schema_version: "1.0.0"
document_id: "789b003cf0baff9653936d9bd5b7be5bc56962490b30e6231e43ee25c8c13c3e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/courseras-journey-to-graphql-a5ad3b77f39a"
published_at: "2017-08-24T17:55:23+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:6776c5a8693433e7c1dfe1b79e1af699685f084149083a79a47c4ffaea46f47b"
---

# Coursera’s journey to GraphQL

Client developers at Coursera love GraphQL’s flexibility, type-safety, and community of support, and[we’ve made](https://building.coursera.org/blog/2016/11/23/why-ui-developers-love-graphql/)[that](https://speakerdeck.com/jnwng/going-graphql-first)[well](https://dev-blog.apollodata.com/graphql-just-got-a-whole-lot-prettier-7701d4675f42)[known](https://building.coursera.org/blog/2017/05/11/coursera-engineering-podcast-episode-one/) . However, we haven’t spoken much about how our backend developers feel about GraphQL — and that’s because most of them don’t actually *have to* think about GraphQL very much.


Over the past year, we’ve built tooling to dynamically translate all of our REST APIs to GraphQL, allowing our backend devs to continue writing the APIs they’re familiar with, while giving client developers full access to all of the data through GraphQL.


In this post, we’ll walk through our journey to GraphQL, and highlight a few of our successes and failures along the way.


## Initial investigation


Coursera’s approach to REST APIs was to build *resource-based* APIs (i.e. an API for courses, an API for instructors, an API for course grades, etc.). These were really easy to build and test, and provided a nice separation of concerns on the backend. However, as our product scaled and the number of APIs grew, we began to face many problems around performance, documentation, and general ease-of-use. On many pages, we found ourselves making four or five round-trips to the server to fetch the data that we needed to render.


I still remember the excitement on my team when Facebook first introduced GraphQL — we almost immediately realized how GraphQL could solve a lot of our problems, allowing us to fetch all data in a single roundtrip, and providing structured documentation for our APIs. But as much as we wanted to begin writing GraphQL endpoints for everything and stop using REST on our clients, that wasn’t an option that we were able to consider, because:


- At the time, we had over 1,000 different REST endpoints at Coursera (and now we have many more) — even if we wanted to stop using REST entirely, the migration cost to GraphQL would be astronomical.
- All of our backend services use REST APIs for inter-service communication, and we often expose the same API for use in both our backend services and our frontend clients.
- We also have three different clients (web, iOS, and Android), and wanted the flexibility to roll out slowly.


After some investigation, we found a great approach to help us get to GraphQL — we decided to add a GraphQL proxy layer on top of our REST APIs. This approach is actually pretty common and has[been](http://graphql.org/blog/rest-api-graphql-wrapper/)[very](https://medium.com/@raxwunter/moving-existing-api-from-rest-to-graphql-205bab22c184)[well](http://nordicapis.com/how-to-wrap-a-rest-api-in-graphql/)[documented](https://0x2a.sh/from-rest-to-graphql-b4e95e94c26b) , so I won’t go into details here.


## GraphQL in production


Wrapping our REST APIs was a pretty easy process — we built some utilities around making downstream REST calls to fetch data in our resolver, and wrote up some rules around converting our existing models into GraphQL.


Our first step was to build a few GraphQL resolvers and then launch a GraphQL server in production to make downstream REST calls to our source endpoints. Once we had this working (using GraphiQL to verify everything), we displayed this data on a demo page that we set up, and within a matter of days were ready to call our GraphQL pilot a success.


### Short-lived celebration


If there’s one thing I’ve learned from this project though, it’s not to celebrate too early.


Our GraphQL server worked perfectly for a few days. But suddenly, right before we were set to demo this page to the team, every GraphQL query started failing. This caught us off guard, because we hadn’t deployed any changes to our GraphQL server since we last verified that it was working.


After some investigation, we realized that our downstream course catalog service was rolled back to a previous version due to an unrelated bug, and the schema that we had built in our GraphQL service was now out of sync. We were able to manually update the schema and fix our demo, but we quickly realized then that as our GraphQL schema scaled to 1,000 different resources, backed by over 50 different services, keeping everything up to date was going to be impossible. If you have multiple sources of truth in a microservice architecture, it’s a matter of *when* , not *if* they’ll become out of sync.


### Automating the process


So we went back to the drawing boards, and tried to figure out a cleaner solution to get to a single source of truth — it made sense for us to treat our REST APIs as the source of truth, since they’re what we’re basing our GraphQL schema on. To do this, we’d need to *automatically* and *deterministically* build our GraphQL layer, reflecting what was currently running in our architecture, not what we thought was running.


Luckily for us (perhaps with a bit of foresight),[our REST framework](https://github.com/coursera/naptime) gave us everything that we needed to build this automation layer:


- Each service in our infrastructure was able to dynamically provide us with a list of REST resources running on it
- For each of those resources, we could introspect the list of endpoints and arguments (i.e. a course endpoint would have a fetch by id, or lookup by instructor)
- Additionally, we received[Pegasus Schemas](https://github.com/linkedin/rest.li/wiki/DATA-Data-Schema-and-Templates) , defined by our[Courier schema language](http://coursera.github.io/courier/schemalanguage/) for each model returned


Once we discovered the different pieces we’d need to build a GraphQL schema, we set up a task on our GraphQL server to ping every downstream service every five minutes, and request all of that information. We then were able to write a 1:1[conversion layer](https://github.com/coursera/naptime/blob/master/naptime-graphql/src/main/scala/org/coursera/naptime/ari/graphql/schema/FieldBuilder.scala) between the Pegasus Schemas and GraphQL types.


Next, we simply defined a translation between GraphQL queries and REST requests, using most of the logic from our earlier resolvers, and were able to generate a fully-functioning GraphQL server, never more than five minutes out of date.


### Relating the resources


One of the main reasons we wanted to adopt GraphQL was to fetch all data we needed for a page in a single roundtrip to the server. However, our initial approach only provided a one-to-one mapping between the models returned from our REST APIs and what we returned in GraphQL. Without actually linking our resources together, we’d still end up making just as many GraphQL queries to fetch data as we would while using our REST APIs. While there are definitely developer experience gains that come from fetching data about a user in GraphQL instead of REST, there aren’t actually performance gains if you have to wait for that query to return before fetching more data.


Our REST APIs each live in a silo — they don’t need to know about the existence of any other API. However, with GraphQL, models and resources *do* need to know about each other, and how everything is connected.


There’s no way to build the links between resources automatically though, so we defined a simple annotation that developers could add to resources to specify the relations between them. For example, we could say that a course resource should have an instructors field representing the instructors who teach that course. And to fetch those instructors, we should lookup instructors by id, using the instructorIds field already available on the course. We called these “forward relations,” because we knew exactly what instructors to fetch by id.


In the case where we wanted to go from one resource to another where there wasn’t an explicit link, we added support to do a reverse lookup to fetch the data — i.e. to get a user’s enrollments on a course, we could call the` byCourseId` lookup on the` userEnrollments.v1` resource, which could return the matching enrollment data for a given user on a given course.


The syntax that we developed looks something like this:


```text
courseAPI.addRelation(
"instructors" -> ReverseRelation(
resourceName = "instructors.v1",
finderName = "byCourseId",
arguments = Map("courseId" -> "$id", "version" -> "$version"))
```


Once these links were in place, our GraphQL schema began coming together— instead of lots of small data pieces that we could fetch with GraphQL, a web of all Coursera’s data and resources formed.


## Conclusion


We’ve been running our GraphQL server in production at Coursera for over six months now, and while the road has certainly been bumpy at times, we’re really able to recognize the benefits that GraphQL provides. Developers have an easier time discovering data and writing queries, our site is more reliable due to the additional[type-safety](https://github.com/apollographql/apollo-codegen) that GraphQL provides, and pages using GraphQL load data much faster.


Just as importantly though, this migration didn’t come at a huge cost of developer productivity. Our frontend developers *did* have to learn how to use GraphQL, but we didn’t have to rewrite any backend APIs or run complex migrations to begin taking advantage of GraphQL — it was simply available for developers to use as they created new applications.


Overall, we’ve been really happy with what GraphQL has provided our developers (and ultimately our users) and are really excited for what’s to come in the GraphQL ecosystem.


---


### Acknowledgements


- [Brennan Saeta](https://twitter.com/bsaeta) , who wrote the Naptime API library and helped write the initial GraphQL support in Naptime.
- Oleg Ilyenko, who’s incredible[Sangria library](http://sangria-graphql.org/) provides the backbone for all of our GraphQL work. If you’re doing anything with GraphQL and are currently using / planning on using Scala, you should definitely check Sangria out.
- The frontend infrastructure team at Coursera for helping get GraphQL from a test project to a production-ready implementation.
- And the entire Coursera engineering team for being patient and helpful guinea pigs as we worked out countless bugs and quirks in our GraphQL translation layer.


Written by


Brian Kane


[Read more by Brian Kane](https://www.apollographql.com/blog/author/brian)
