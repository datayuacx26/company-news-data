---
schema_version: "1.0.0"
document_id: "3bac413eb5bba7901042cf3fe22c3d3f9a9bc9c0bd30ea207a45ca15505ac18b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-83888f119c8c"
canonical_url: "https://www.apollographql.com/blog/securing-your-graphql-api-from-malicious-queries"
published_at: "2018-02-21T21:45:32+00:00"
first_seen_at: "2026-07-24T16:23:53.794779+00:00"
fetched_at: "2026-07-28T22:26:53.665310+00:00"
content_hash: "sha256:add0c2bd2f633afaa273450d87a0301298aba123592c93ea03ad44b466cce1d8"
---

# Securing Your GraphQL API from Malicious Queries

With GraphQL you can query exactly what you want whenever you want. That is amazing for working with an API, but also has complex security implications. Instead of asking for legitimate, useful data, a malicious actor could submit an expensive, nested query to overload your server, database, network, or all of these. Without the right protections you open yourself up to a DoS (Denial of Service) attack.


For example, in our GraphQL API at[Spectrum](https://spectrum.chat/) we have a relationship like this:


```text
type Thread {
messages(first: Int, after: String): [Message]
}


type Message {
thread: Thread
}


type Query {
thread(id: ID!): Thread
}
```


As you can see, you can query both the messages of a thread or the thread of a message. This circular relationship allows a bad actor to construct an expensive nested query like so:


```text
query maliciousQuery {
thread(id: "some-id") {
messages(first: 99999) {
thread {
messages(first: 99999) {
thread {
messages(first: 99999) {
thread {
# ...repeat times 10000...
}
}
}
}
}
}
}
}
```


Letting this kind of query through is very bad, since it exponentially increases the amount of objects loaded and will crash your entire server. While there are certain mitigations at other layers which make sending the query in the first place a bit harder (e.g. CORS), they can’t fully prevent it from happening.


## Size Limiting


A first, naïve approach we considered was to limit the incoming query size by raw bytes. Since the query is sent as a string, a quick length check would suffice:


```text
app.use('*', (req, res, next) => {
const query = req.query.query || req.body.query || '';
if (query.length > 2000) {
throw new Error('Query too large');
}
next();
});
```


Unfortunately, that doesn’t work very well in the real world: the check might allow nasty queries using short field names or block legitimate queries using long field names or nested fragments.


## Query Whitelisting


A second approach we considered was to have a whitelist of approved queries we use in our own application, and telling the server to not let any query pass except for those.


```text
app.use('/api', graphqlServer((req, res) => {
const query = req.query.query || req.body.query;
// TODO: Get whitelist somehow
if (!whitelist[query]) {
throw new Error('Query is not in whitelist.');
}
/* ... */
}));
```


Maintaining that list of approved queries manually would obviously be a pain, but thankfully the Apollo team created[persistgraphql](https://github.com/apollographql/persistgraphql) , which automatically extracts all queries from your client-side code and generates a nice JSON file out of it.


```text
{
"scripts": {
"postbuild": "persistgraphql src api/query-whitelist.json"
}
}
```


This technique works beautifully and will block all vicious queries reliably. Unfortunately, it also has two major tradeoffs:


1. **We can never change or delete queries, only add new ones** **:** if any user is running an outdated client we can’t just block their request. We’d likely have to keep a history of all queries ever used in production which is a lot more complex.
2. **We cannot open our API to the public:** sometime in the future we’d like to open up our API to the public so that other developers can build their interpretation of what the Spectrum interface could look like. If we only let a whitelist of queries through that severely limits their options already and defeats the point of having a GraphQL API (super flexible system restrained by a synthetic whitelist).


Those were constraints we couldn’t work with, so we went back to the drawing board.


## Depth Limiting


One harmful aspect of the malicious query above is the nesting, classified by its *depth* , which makes the query exponentially more expensive. Each layer adds a lot more work for your backend, which can quickly add up when combined with lists.


We looked around and found[graphql-depth-limit](https://github.com/stems/graphql-depth-limit) , a lovely module by[Andrew Carlson](https://twitter.com/acarl005) , which enables us to easily limit the maximum depth of incoming queries. We checked our client and the deepest query we use has 7 levels, so we went with a (quite lenient) maximum depth of 10 and added it to our validation rules:


```text
app.use('/api', graphqlServer({
validationRules: [depthLimit(10)]
}));
```


That’s how simple depth limiting is!


## Amount Limiting


The second harmful aspect of the query above is fetching 99999 of an object. No matter what that object is, fetching a ton of it will always be expensive. (Although the database stress might be mitigated by DataLoader, the network and processing stress won’t)


Instead of setting the type of the first argument to Int (which allows any arbitrary number) we have a custom scalar created with[graphql-input-number](https://github.com/joonhocho/graphql-input-number) , that restricts the maximum value to 100:


```text
const PaginationAmount = GraphQLInputInt({
name: 'PaginationAmount',
min: 1,
max: 100,
});
```


This will throw an error, if anybody queries for more than 100 objects. We then use that across our API anywhere we have a connection:


```text
type Thread {
messages(first: PaginationAmount, after: String): [Message]
}
```


Now we’ve entirely prevented the malicious query from above! 🎉


## Query Cost Analysis


Unfortunately, there’s still a potential to overwhelm the server under the right conditions: there are certain app-specific queries that are neither too deep nor request too many objects but are still very expensive. For us at Spectrum, such a query could look like this:


```text
query evilQuery {
thread(id: "54887141-57a9-4386-807c-ed950c4d5132") {
messageConnection(first: 100) { ... }
participants(first: 100) {
threadConnection(first: 100) { ... }
communityConnection { ... }
channelConnection { ... }
everything(first: 100) { ... }
}
}
}
```


Neither the depth nor the individual amounts are exceptionally high in this query, so it would pass through our current protections. Yet, it potentially fetches tens of thousands of records, meaning it’s intensive on the database, server and network, a worst-case scenario.


To prevent this we would need to analyze queries before we run them to calculate their complexity and block them if were too expensive. While this is more work than both of our previous protections, it’ll make 100% sure no malicious query can get to our resolvers.


**Before you go ahead and spend a ton of time implementing query cost analysis be certain you need it.** Try to crash or slow down your staging API with a nasty query and see how far you get — maybe your API doesn’t have these kinds of nested relationships, or maybe it can handle fetching thousands of records at a time perfectly fine and doesn’t need query cost analysis!


I ran the above query locally on my maxed-out 2017 MacBook Pro and it took our API server a good 10–15 seconds to respond with a megabyte of JSON. We really need it since we never want anybody bombarding our API with that. ([The GitHub GraphQL API also uses Query Cost Analysis](https://developer.github.com/v4/guides/resource-limitations/) )


### Implementing Query Cost Analysis


There are a couple of packages on npm to implement query cost analysis. Our two front-runners were[graphql-validation-complexity](https://github.com/4Catalyzer/graphql-validation-complexity) , a plug-n-play module, or[graphql-cost-analysis](https://github.com/pa-bru/graphql-cost-analysis) , which gives you more control by letting you specify a @cost directive. There’s also[graphql-query-complexity](https://github.com/ivome/graphql-query-complexity) , but I wouldn’t recommend choosing it over graphql-cost-analysis since it’s the same idea without directive or multiplier support.


We went with[graphql-cost-analysis](https://github.com/pa-bru/graphql-cost-analysis) because we have a large discrepancy between our fastest resolvers (20μs) and our slowest ones (10s+), so we needed the control we got from it. That being said, maybe graphql-validation-complexity is enough for you, try it out for sure!


The way it works is that you specify the relative cost of resolving a certain field or type. It also has multiplication support, so if you requested a list any nested field within would be multiplied by the pagination amount, which is very neat.


This is what the @cost directive looks like in practice:


```text
type Participant {
# The complexity of getting one thread in a thread connection is 3, and multiply that by the amount of threads fetched
threadConnection(first: PaginationAmount, after: String): ThreadConnection @cost(complexity: 3, multipliers: ["first"])
}


type Thread {
author: Author @cost(complexity: 1)
participants(first: PaginationAmount,...): [Participant] @cost(complexity: 2, multipliers: ["first"])
}
```


This is just a snippet of our API types, but you get the point. You specify how complex a certain field is, which argument to multiply by and the maximum cost and graphql-cost-analysis does the rest for you.


I determined how complex certain resolvers are via the performance tracking data exposed by[Apollo Studio](https://studio.apollographql.com/signup?referrer=blog) . I went through the entire schema and assigned a value based on the p99 service time. Then we looked through all the queries on our client to figure out the most expensive one, which got ~500 complexity points. To give us a bit of leeway for the future we set the maximum complexity to 750.


Running the evilQuery from above, now that we’ve added graphql-cost-analysis, I get an error message telling me that the “GraphQL query exceeds maximum complexity, please remove some nesting or fields and try again. (max: 750, actual: 1010319)”


**1 million complexity points? Rejected!**


## Summary


To summarize, I’d recommend using Depth and Amount Limiting as the minimum protection for any GraphQL API — they’re easy to implement and will give ample safety. Depending on your specific security requirements and schema, you might also want to investigate Query Cost Analysis. While it is a bit more work than the other tools, it does provide full coverage in the defense against malicious actors.


---


[Discuss this post in the GraphQL community on Spectrum](https://spectrum.chat/thread/dc17eab7-fe14-44de-967b-79d5e6ab6e6a)


Written by


Max Stoiber


[Read more by Max Stoiber](https://www.apollographql.com/blog/author/mxstbr)
