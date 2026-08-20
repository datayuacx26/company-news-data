---
schema_version: "1.0.0"
document_id: "800776dc8a5765e5d6e6c5ae6cb923c397957028e2d341e309089fb72fb51feb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-over-rest-with-node-heroku-and-apollo-engine-fb8581f8d77f"
published_at: "2018-03-14T21:21:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:74b49b2403782f82c166470f94f5568b2dd9f6193fb0404e6883bcbc367a520b"
---

# GraphQL over REST with Node, Heroku, and Apollo Engine

Wrapping your REST endpoints in GraphQL is a quick way to make it easier to use the data you have in your existing backends. In just a few hours, you can implement a thin translation layer using Node and Apollo Server and start getting the main benefits of having a GraphQL API without having to dive in to the backend implementation.


Why wrap your REST API with GraphQL? You’ll get…


1. Easier data querying and frontend developer productivity
2. A self-documenting API that makes it easy to query for what you want
3. More efficient networking where you only load the data you need


The only question left is: How do you deploy that API so that you can scale it easily and understand how it’s working?


In this article, we’ll go over one simple option: Deploying to the Heroku platform as a service with Apollo Engine, an API gateway that provides GraphQL-specific features like caching and performance monitoring.


GraphQL organizes your endpoints into one flexible unified API.


## Wrapping a REST API with GraphQL


In this example, we’ll use a simple example of wrapping a REST API with a GraphQL schema. We’ll go with the Ticketmaster API. This API allows us to get details about artists and find their upcoming concerts.


### The complete example server


You can find the code for the server we’re looking at in the repository below:[apollographql/ticketmaster-rest-api-wrapperticketmaster-rest-api-wrapper – Simple wrapper for the Ticketmaster REST APIgithub.com](https://github.com/apollographql/ticketmaster-rest-api-wrapper)


You can also try out the[live endpoint](https://ticketmaster-rest-api-wrapper.herokuapp.com/graphiql) to test out some queries. We’re going to look through the main parts of that server code, but we won’t go over every part in detail. For that, check out our[tutorial about building GraphQL servers](https://dev-blog.apollodata.com/tutorial-building-a-graphql-server-cddaa023c035) .


### The schema


First, we have a GraphQL schema that describes the shape of our data:


```text
// Construct a schema, using the GraphQL schema language
const typeDefs = gql`
type Query {
myFavoriteArtists: [Artist]
}
type Artist @cacheControl(maxAge: 60) {
id: ID
name: String
image: String
twitterUrl: String
events: [Event]
}
type Event @cacheControl(maxAge: 60) {
name: String
image: String
startDateTime: String
}
`;
```


The GraphQL schema language is awesome, since you can use it to communicate about the shape of your API and even[mock data for tests](https://www.apollographql.com/docs/graphql-tools/mocking.html) . We’ve often found the step of describing your existing API with GraphQL to be a really illuminating moment for teams switching from REST.


The concise format gives you a chance to really think about what your API should look like, so it’s a great opportunity to convert your backend data formats into the shape you want to use as a frontend developer building an app. We call this approach schema-driven development, and it enables you to get a better API while taking advantage of a clear abstraction layer to build different parts of your app in parallel.


### The resolvers


Now that we have described the schema, the only thing left is to wire it up to the underlying REST API by calling` fetch` in our resolvers — these are functions attached to fields in the schema that define where to get the data:


```text
const resolvers = {
Query: {
myFavoriteArtists: (root, args, context) => {
return Promise.all(
myFavoriteArtists.map(({ name, id }) => {
return fetch(
`https://app.ticketmaster.com/discovery/v2/attractions/${id}.json?apikey=${
context.secrets.TM_API_KEY
}`
)
.then(res => res.json())
.then(data => {
return Object.assign({ name, id }, data);
});
})
);
}
},
Artist: {
twitterUrl: artist => {
return artist.externalLinks.twitter[0].url;
},
image: artist => artist.images[0].url,
events: (artist, args, context) => {
return fetch(
`https://app.ticketmaster.com/discovery/v2/events.json?size=10&apikey=${
context.secrets.TM_API_KEY
}&attractionId=${artist.id}`
)
.then(res => res.json())
.then(data => {
// Sometimes, there are no upcoming events
return (data && data._embedded && data._embedded.events) || [];
});
}
},
Event: {
image: event => event.images[0].url,
startDateTime: event => event.dates.start.dateTime
}
};


// A list of artists for whom I want to know about upcoming events
const myFavoriteArtists = [
{ name: "Kansas", id: "K8vZ9171C-f" },
{ name: "Lil Yachty", id: "K8vZ9174v57" },
{ name: "Jason Mraz", id: "K8vZ9171CVV" }
];
```


There are a couple things to notice here:


1. We’re getting the Ticketmaster API key from the` context` — this is a special argument to every resolver that can be passed from a central place. You’ll see it in the` context` option to` graphqlExpress` below.
2. Sometimes, we need to reformat the data to match our desired schema. In particular, we sometimes unwrap complicated REST responses to match the simpler shape we desire. The GraphQL API is a great place to do this since it moves that logic out of your UI code.


To learn more about resolvers and what you can do with them, check out the[Apollo docs page about resolvers](https://www.apollographql.com/docs/graphql-tools/resolvers.html) .


Those are basically all of the parts you need to convert a REST API into a GraphQL schema! Now, let’s look into wiring it up to HTTP.


## Apollo Server and Engine


To attach our newly-created schema to a Node HTTP server that we can deploy, we’ll use Apollo Server and Apollo Engine. We can easily add the GraphQL schema to an Express server with a middleware:


```text
// The GraphQL endpoint
app.use(
"/graphql",
bodyParser.json(),
graphqlExpress({
schema,
tracing: true,
cacheControl: true,
context: {
secrets: {
TM_API_KEY: process.env.TM_API_KEY
}
}
})
);
```


Note that here we have enabled the` tracing` and` cacheControl` options, which expose some GraphQL extensions that allow Engine to integrate with your schema. If you look at the schema above, we use the` @cacheControl` schema directive to report cache expiration hints to Engine.


Finally, we start the server with Engine, like so:


```text
const engine = new ApolloEngine({
apiKey: process.env.ENGINE_API_KEY
});


// Start the server
engine.listen({
port: PORT,
expressApp: app
}, () => {
console.log(`Go to http://localhost:${PORT}/graphiql to run queries!`);
});
```


At this point, we have all the parts we need to run the server locally.


### Running queries locally


You’ll need to get two API keys:


1. [Ticketmaster API key](https://developer.ticketmaster.com/products-and-docs/apis/getting-started/)
2. [Apollo Engine API key](https://engine.apollographql.com/)


Then just pass them in when you run the server:


```text
npm install
export TM_API_KEY=...
export ENGINE_API_KEY=...
npm start
```


You’ll be able to run some queries in your browser locally. At this point you can already look in the Engine UI to see how our request timing breaks down based on which underlying REST calls we make. For example, when we run the following query:


```text
query UpcomingEvents {
myFavoriteArtists {
name
twitterUrl
events {
name
startDateTime
}
}
}
```


We’ll see a trace like the below screenshot in the Engine UI. If you’re just following along, go to the[public demo page](https://engine.apollographql.com/demo/engine-up-demo) for this app and[run some queries](https://ticketmaster-rest-api-wrapper.herokuapp.com/graphiql) .


This tells us that basically all of the request duration came from the two REST API calls to Ticketmaster for the` myFavoriteArtists` and` events` fields, which took 477 and 663 ms. Wrapping this API in GraphQL is a good idea, since it allows us to avoid a waterfall of requests from the client, which is especially slow over a mobile network.


### GraphQL result caching in Engine


You’ll note that in the schema code snippet we covered above, there are some special` @cacheControl` annotations. These directives work together with Engine to enable the gateway to cache your results for the appropriate time.


The entire operation result is cached as one unit, but you can specify expiration times based on the types and fields in your schema. Engine reads the separate cache hints, and selects the most conservative time to apply to the whole query. You can read more about this feature[in the Engine docs](https://www.apollographql.com/docs/engine/) .


Now, let’s look at an example of caching in action. Go to GraphiQL again for your app, and run the same query over and over again. You’ll notice it returns super fast — not the almost one second time we were seeing for the first query execution. We can confirm this in the Engine performance UI:


As you can see in the chart, cached requests resolve almost instantly. This enables us to work around the performance of the underlying API for data that doesn’t always need to be up to date.


If you’re just following along, you can see a[demo version of the Engine UI for this app](https://engine.apollographql.com/demo/engine-up-demo) without logging in. Try[running some queries](https://ticketmaster-rest-api-wrapper.herokuapp.com/graphiql) and seeing the data show up. Give your query a unique name so it’s easy to find.


## Deploying to Heroku


Now, we just need to get this API on the internet. Heroku is a great place to host a Node app, so let’s give that a shot. Thankfully, we don’t need to do anything special for Engine or GraphQL to run on Heroku!


It’s just a few simple steps:


1. Go to your[Heroku dashboard](https://dashboard.heroku.com/apps) , and log in if necessary
2. Click “New” > “Create New App” in the top right
3. Name your app and hit “Create”
4. Select your desired deployment method — for our app we used GitHub deployment, which just required selecting the right repository
5. Go to the “Settings” tab
6. Click “Reveal Config Vars”
7. Put in the environment variables for` TM_API_KEY` and` ENGINE_API_KEY` , just like we had previously


You should pass any API keys you’re using through environment variables, not in your code


### Test your deployed API


Now your API should be live — click “Open App” in the top right, and you should be able to run queries at` <your app name>.herokuapp.com/graphiql` . If you’ve just been reading along, ours is deployed at[https://ticketmaster-rest-api-wrapper.herokuapp.com/graphiql](https://ticketmaster-rest-api-wrapper.herokuapp.com/graphiql) .


You can also call that API via cURL to test that it is indeed accessible from the internet. Try the following command:


```text
curl \
-X POST \
-H "Content-Type: application/json" \
--data '{ "query": "{ myFavoriteArtists { twitterUrl } }" }' \
https://ticketmaster-rest-api-wrapper.herokuapp.com/graphql
```


This should give you some Twitter profile URLs.


## Go wrap your own API!


Now hopefully you’re closer to wrapping your own REST API with GraphQL. If you run into any questions, ask in the Apollo Slack and someone will help you out. There are some more advanced topics we’ll write about soon that will help you better wrap your REST API, but it’s always good to start simple and add complexity as you go.


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
