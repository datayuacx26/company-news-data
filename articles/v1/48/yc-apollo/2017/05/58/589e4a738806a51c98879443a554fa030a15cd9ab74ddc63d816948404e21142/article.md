---
schema_version: "1.0.0"
document_id: "589e4a738806a51c98879443a554fa030a15cd9ab74ddc63d816948404e21142"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-optics-is-now-free-for-small-projects-c23d70569913"
published_at: "2017-05-23T00:36:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:6f96d10dd153351e72bd36d77199de5872eec0b20818d521db5892ef90b0ac55"
---

# Apollo Optics is now free for small projects

Update: There is a new version of Optics![Apollo Engine](https://www.apollographql.com/engine/) has everything Optics does plus error tracking, query caching, and more. Engine is free for 1 million requests a month and it’s easy to switch from Optics to Engine using our[migration guide](https://www.apollographql.com/docs/engine/upgrade-optics.html) .


We want[Optics](https://www.apollodata.com/optics/) to be something every GraphQL developer can use to understand how their server works, even if it’s still in development. So today we’re excited to announce that we’ve added a completely free tier up to 10,000 requests a month. Now, you can instrument your API and see what it’s doing without worrying about running out of time in a free trial!


Developers from companies like Expo and Product Hunt have said that they like Optics because it provides a fine-grained view into the execution of their GraphQL queries. Last weekend at[GraphQL Europe](https://graphql-europe.org/) , Optics got mentions in several talks, including in Brooks’ talk about[launching GitHub’s public GraphQL API](https://graphql-europe.org/schedule/launching-githubs-public-graphql-api) .


Optics lets you see in detail how each query is resolved on your server, and shows aggregate statistics of how your schema fields are being used.


The Optics dashboard


If you haven’t heard about Apollo Optics yet, and want to learn more about what it does and why you might want a GraphQL-specific server performance tool, check out our[landing page](https://www.apollodata.com/optics/) !


## Optics quick start


If you have a GraphQL server, it only takes a few minutes to instrument your schema and see your data in Optics. The quick directions below are for instrumenting a Node.js Express server. If you’re using a different server technology, check out the docs for[JavaScript](https://github.com/apollographql/optics-agent-js) and[Ruby](https://github.com/apollographql/optics-agent-ruby) . An implementation for Sangria (Scala) is coming soon as well!


### 1. Install


Simple enough:


```text
npm install optics-agent --save
```


### 2. Import


In the file which connects your GraphQL schema to your Express server, add an import at the top:


```text
import OpticsAgent from 'optics-agent';
```


### 3. Instrument the schema, middleware and context


The Optics agent relies on a variety of data points, which are available at different stages in the execution cycle. Currently, you need to add code in three places.


First, wrap your GraphQL Schema object with` instrumentSchema` :


```text
// Before: graphqlExpress({ schema: schema })
graphqlExpress((req) => {
// other options...
schema: OpticsAgent.instrumentSchema(schema)
})
```


Second, add the HTTP middleware *before* your GraphQL middleware:


```text
expressServer.use(OpticsAgent.middleware());
```


Third, add an` opticsContext` field to your GraphQL context:


```text
graphqlExpress((req) => {
// other options...
context: {
// other context fields...
opticsContext: OpticsAgent.context(req),
}
})
```


That’s all the code we have to add. You’re almost done!


### 4. Sign into Optics and set the API key


Go to[optics.apollodata.com](https://optics.apollodata.com/) and create an endpoint to get an API key. Restart your server and pass your API key with an environment variable, like so:


```text
OPTICS_API_KEY=<my_key> npm start
```


The agent will automatically pick it up from there. Now, open up GraphiQL, run a query, and in a few seconds you’ll see data starting to show up in your Optics dashboard. You’re all set up!


---


We’re really excited to make Optics available to more people, and we hope you’ll find it a valuable tool to understand how your GraphQL schema and queries work! If you have any feedback or want to chat about Optics, please join the #optics channel in the[Apollo Slack](http://www.apollodata.com/#slack) !


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
