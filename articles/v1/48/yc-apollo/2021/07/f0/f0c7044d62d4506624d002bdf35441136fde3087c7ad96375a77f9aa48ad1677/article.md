---
schema_version: "1.0.0"
document_id: "f0c7044d62d4506624d002bdf35441136fde3087c7ad96375a77f9aa48ad1677"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/federation-in-odyssey"
published_at: "2021-07-09T14:05:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:054747c96675bfc64c1f104d3ba3fb1b5cd429382e23004ed1fd400a5575fd80"
---

# Federation in Odyssey

One of the very best ways to improve your product is to use it yourself. We’ve been using Apollo Client and Server libraries internally at Apollo for ages. Still, it wasn’t until we released[Odyssey](https://odyssey.apollographql.com/) (the Apollo learning platform) that we realized we had the perfect opportunity to use Apollo Federation to consolidate our efforts with Apollo Studio. In this article, we’ll learn about what Federation is, walk you through our use case for Odyssey, show you how we set it up, challenges we overcame, and ultimately why Federation was a great solution.


## First, what is Apollo Federation?


[Apollo Federation](https://www.apollographql.com/docs/federation/) enables you to divide a single GraphQL API’s implementation across multiple back-end services. Each service has a different GraphQL schema that represents a different part of the combined API:


A **gateway** is responsible for receiving incoming queries and splitting them up across whichever services are required to resolve them. The gateway then takes each service’s response and combines them into a *single* response that’s returned to the querying client. From the client’s perspective, this works exactly like querying a *non* -federated API.


In addition to providing a clean API surface area, federation helps you control back-end data access. Each service only needs access to data stores related to its part of your API, and services don’t even need to be able to communicate with each other!


## Why adopt federation for Odyssey?


While designing Odyssey, we knew we wanted to persist users’ progress through courses to provide a better learning experience. We wanted to associate this progress with the` User` type from[Apollo Studio](https://studio.apollographql.com/) ’s GraphQL API, which we were already using for authentication.


But it didn’t make sense for a user’s progress to be stored in Apollo Studio’s database—it should be stored in the Odyssey service! That way, our team could maintain and evolve our implementation without impacting or relying on the Studio team. Enter federation.


In Odyssey, we read user data from Studio’s Explorer API for authentication, and then use that information to fetch the correct data from our separate progression tracking service. Even though both services are federated with the same gateway, restrictions are set up so that Odyssey can’t request anything and everything from Studio or make any modifications to its database.


This implementation gives us a **separation of concerns** that enables different teams to work on different products and features without interfering with each other. In this way, we can read the user data from the Studio subgraph to help us grab the right data from our Odyssey progression subgraph instead of storing duplicate user info.


## Setting up federation


Having never worked with federation before, the engineers on the Education team jumped into the docs. We created a local PostgreSQL database for progress tracking and built out our GraphQL server with Apollo Server.


Creating a federated service was surprisingly straightforward! To support[managed federation](https://www.apollographql.com/docs/federation/managed-federation/overview/) , we saved our schema in a separate` .graphql` file. We created a federated schema by invoking` buildFederatedSchema` (imported from` @apollo/federation` ) and passing in an array with an object containing our` typeDefs` and` resolvers` . The` typeDefs` were created by reading in our` schema.graphql` and wrapping it in a` gql` tag. Then we passed this federated schema as the value of the` schema` config option for our instance of` ApolloServer` .


```text
import {buildFederatedSchema} from '@apollo/federation';
import {ApolloServer, gql} from 'apollo-server';
import {readFileSync} from 'fs';
import {resolvers} from './resolvers';


const schema = readFileSync('schema.graphql').toString();


const federatedSchema = buildFederatedSchema([
{
typeDefs: gql`${schema}`,
resolvers
}
]);


const server = new ApolloServer({
schema: federatedSchema
})


server.listen({port: process.env.PORT}).then(({url}) => {
console.log(`🚀 Server is listening on ${url}`);
});
```


In order for our Odyssey subgraph to extend the User type, we submitted a PR to the Studio subgraph that added a` @key` directive to the User type definition. This created an[entity](https://www.apollographql.com/docs/federation/entities/) that could be shared between subgraphs.


In Apollo Federation, an **entity** is an object type that you define canonically in *one* subgraph and can then reference and extend in *other* subgraphs. Entities are the core building block of a federated graph.


In this case, we used the` id` field as the primary key by which the Odyssey subgraph would tell the Studio subgraph which user data we wanted.


```text
// example User type in Studio subgraph
type User @key(fields: "id") {
id: ID!
name: String!
}
```


Within the Odyssey subgraph, we created a` Task` type and extended the` User` from the Studio subgraph to add a` tasks` field that resolves to an array of` Task` s.


To extend the` User` type, we added a` @key` directive that matches the one on the` User` type in the Studio subgraph (see above example). The` id` field must be included since it’s part of the specified` @key` . It also has to be annotated with the` @external` directive to indicate that the field originates in the Studio subgraph.


```text
// example extended User in Odyssey subgraph schema
type Task {
id: ID!
value: String
completedAt: Timestamp
}


extend type User @key(fields: "id") {
id: ID! @external
tasks: [Task!]!
}
```


Then we wrote a resolver for the` User.tasks` field that finds all tasks that belong to a given user.


```text
// example resolver for an Odyssey user's tasks (in Odyssey subgraph)
{
User: {
tasks: user =>
Task.findAll({
where: {
userId: user.id
}
})
}
}
```


And finally, since we were tapping into the existing[gateway](https://www.apollographql.com/docs/federation/gateway/) at Apollo, we didn’t need to worry about creating one. To work on our federated service locally, we added our service’s local endpoint to the locally-running gateway’s` serviceList` .


```text
// example gateway config
import { ApolloGateway } from '@apollo/gateway';


const gateway = new ApolloGateway({
serviceList: [
{ name: 'odyssey', url: 'http://localhost:4001' },
// Define additional services here
],
});
```


## Challenges


With everything set up, we wanted to test it all locally before deploying anything to make sure everything worked as expected. The biggest challenge we faced while creating our federated service was navigating other subgraph and gateway codebases and figuring out how to run them locally. While not specific to Apollo Federation, stepping into unfamiliar tools and codebases is a challenge that teams can expect to face when implementing federation themselves. Pair programming sessions with engineers from those teams proved invaluable in our development process.


With our server up and running locally, it was time to test out some queries! But we had a problem. We just wanted to check to see if everything was set up correctly and working as expected without also spinning up another server locally for the Studio Explorer API. How could we run a query that needs to get the` User` data from a service that we’re not running locally?


We learned that you can run an` _entities` query to effectively stub the response for the fields that rely on the service you aren’t running.


```text
query ($_representations: [_Any!]!) {
_entities(representations: $_representations) {
... on User {
tasks {
id
value
completedAt
}
}
}
}


// query variables
{
"_representations": [
{
"__typename": "User",
"id": "1"
}
]
}
```


You can read more in-depth about resolving requests for entities in the[federation spec](https://www.apollographql.com/docs/federation/federation-spec/#resolve-requests-for-entities) .


## Benefits of our Federated graph


With our service up and running, let’s take a look at some of the benefits we got out of our federated graph:


- Having a separation of concerns with user data in one subgraph and task data in another reduced duplication of information. There’s no need to store the same user information in the Odyssey subgraph when it already exists in the Studio one.
- We can make changes to the Odyssey subgraph without having to worry about impacting the Studio subgraph since they’re independent of one another. If we have some downtime, it won’t cause any downtime for the Studio team. Each team is responsible only for the data that they need to use.
- We still only have one data graph to which we make requests! This helps clean up our frontend code by only needing to send requests to one source (the federated gateway) instead of two (one to the Studio graph and one to Odyssey).


## Conclusion


Coming into this project with no prior experience working with Apollo Federation, we found it surprisingly straightforward to configure our server and connect our service to the gateway. We had the opportunity to dig into another codebase and do some cross-team collaboration to get everything properly set up and deployed with how Apollo does things internally (these aren’t federation-specific steps, but were still part of our journey in creating our backend service). And we found some resources to help us in our local development that we can now use to improve Apollo’s own learning resources.


Now we’re excited to continue building and expanding our federated graph!


Written by


Janessa Garrow


[Read more by Janessa Garrow](https://www.apollographql.com/blog/author/janessagarrow)
