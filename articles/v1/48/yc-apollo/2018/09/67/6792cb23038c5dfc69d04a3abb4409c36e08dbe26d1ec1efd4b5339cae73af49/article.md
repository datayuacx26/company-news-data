---
schema_version: "1.0.0"
document_id: "6792cb23038c5dfc69d04a3abb4409c36e08dbe26d1ec1efd4b5339cae73af49"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/layering-graphql-on-top-of-rest"
published_at: "2018-09-05T23:50:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:4d4ee9949756e0a755124dbd7093424e5dd9c4e49e4259fa0300667b4b9cb2b2"
---

# Layering GraphQL on top of REST

GraphQL is quickly replacing REST as the standard for which data is queried, fetched and transformed between the frontend and backend. And developers are looking to explore and reap its benefits in their brown-field projects.


The benefits of GraphQL are numerous, some of which are:


1. Querying many resources and retrieving the exact data you want in one request.
2. A new generation of ergonomic developer tools from API to the client-side with platforms like Apollo ([Client](https://www.apollographql.com/docs/react/) *,*[Server](https://www.apollographql.com/docs/apollo-server/) *,*[Engine](https://www.apollographql.com/engine/) ).
3. Improved performance, especially on devices with slow network connections via reduced payload size and less round trips to the server.


In our experience, the best way to integrate GraphQL into your product architecture is to deploy it as a layer between your apps and existing APIs.


GraphQL isn’t about throwing away existing infrastructure, but leveraging it to the fullest. Without much ado, I’ll show you to how to layer GraphQL over existing REST APIs and services.


## Getting started with MVRP API


I’ll use the REST API of a Motor Vehicle Registration Information Portal, MVRD, as a case study. The API gives us access to the details of the cars that are registered immediately when they cross the borders into the country.


The REST API has two endpoints:


1. [https://mvrp.herokuapp.com/api/cars](https://mvrp.herokuapp.com/api/cars) — Fetch all the cars at once
2. [https://mvrp.herokuapp.com/api/car?plateNumb](https://mvrp.herokuapp.com/api/car?plateNumber=EPE68ET) er=xxxx — Search for a car by ***plateNumber*** , where` xxxx` is the plate number.


Check out the source code for the API if you’re interested in how it was built: Framework of choice — **Lumen** (A PHP micro-framework).[apollographql/mvrpmvrp – REST API for Motor Vehicle Registration Portalgithub.com](https://github.com/apollographql/mvrp)


## Layer GraphQL over MVRP API


To layer GraphQL over our existing PHP API, we will use Apollo Server. It has built-in support for GraphQL subscriptions, powerful error handling tools, schema stitching, file uploads, mocking, schema directives and easy monitoring integration. Let’s dive into building our schema!


### Schema-First Development


This is a[development approach](https://www.apollographql.com/docs/fundamentals/tips.html#schema) for designing and building modern UIs that involves the frontend and backend teams agreeing on a schema first, which serves as a contract between the UI and the backend before any API engineering happens. Now, let’s construct a simple GraphQL schema that describes the shape of the data from the REST API.


When you are building your first GraphQL schema, there’s a huge temptation to create literal mappings on top of existing database collections or REST API data response fields. While this mapping may be a fast way to get up and running, we strongly suggest building the schema based on how the GraphQL API will be used by the front-end. This way, you can design a schema that reflects how frontend developers are querying it, without constraining yourself to the exact shape of data returned by your existing APIs.


The schema below establishes a contract that describes exactly how we should fetch data into our UI. Worthy of note are the two schema fields, **vehicleStatus** , and **yearOfManufacture** above that are named differently from the fields: **status** , and **productionYear,** returned from the REST API.


```text
import { ApolloServer, gql } from 'apollo-server';


const typeDefs = gql`
type Car {
id: Int!
plateNumber: String!
color: String!
model: String!
chasisNumber: String!
vehicleStatus: String!
yearOfManufacture: Int!
issueDate: String!
expiryDate: String!
}
type Query {
car(plateNumber: String!): Car
cars: [Car]
}
`;
```


Our GraphQL schema reflects how it’s used on the user interface. Now, check out the Query fields:


1. **car —** Get one car based on the plateNumber parameter.
2. **cars —** Get all cars.


The schema is ready. What next? How do we efficiently fetch data from the REST API endpoints using GraphQL?


### Data Sources


When deploying GraphQL as a layer between your apps and existing APIs and services,[Apollo Data Sources](https://www.apollographql.com/docs/apollo-server/features/data-sources.html) provide the best experience for fetching and caching data from REST endpoints. It’s a new pattern for loading data from a particular service, with built-in support for caching, deduplication, and error handling. Apollo Server serves as *the GraphQL layer* which ships with data sources out of the box!


Let me show you how straightforward the implementation is. Go ahead and install the` apollo-datasource-rest` package from npm.


```text
npm install apollo-datasource-rest
```


Create a` datasource.js` file and extend the` RESTDataSource` class like so:


```text
import { RESTDataSource } from 'apollo-datasource-rest';


export class MvrpAPI extends RESTDataSource {
constructor() {
super();
this.baseURL = 'https://mvrp.herokuapp.com/api/';
}


async getAllCars() {
return this.get('cars');
}


async getACar(plateNumber) {
const result = await this.get('car', {
plateNumber
});


return result[0];
}
};
```


Let’s break down this code:


1. The` baseURL` is assigned the root domain of the API in the constructor of our data source class.
2. **getAllCars** and **getACar** methods fetch data from the` /cars` and /` car?plateNumber` endpoints respectively.
3. The` get` method on the` RESTDataSource` makes an HTTP` GET` request. Similarly, there are[methods built-in to allow for](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods)` <a href="https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods" target="_blank" rel="noreferrer noopener">POST</a>`[,](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods)` <a href="https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods" target="_blank" rel="noreferrer noopener">PUT</a>`[,](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods)` <a href="https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods" target="_blank" rel="noreferrer noopener">PATCH</a>`[, and](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods)` <a href="https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods" target="_blank" rel="noreferrer noopener">DELETE</a>`[requests.](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#HTTP-Methods)


**Note:** *Data Sources* also allow you to[intercept fetches to set headers, query parameters, or make other changes to the outgoing request.](https://www.apollographql.com/docs/apollo-server/v2/features/data-sources.html#Intercepting-fetches) This is most often used for authorization or other common concerns that apply to all requests.


### Data Sources — Partial Query Caching


With data sources, the HTTP requests are automatically cached based on the caching headers returned in the response from the REST API. ***Partial Query Caching is unlocked*** — a design pattern that lets the GraphQL layer cache responses exposed by the underlying APIs and then assemble them into arbitrary query results without refetching from the server.


When using` RESTDataSource,` the caching depends on the` Cache-Control` header. However, you can override this default behavior by setting the` cacheOptions` and specify the time you want to cache user responses in your data source as shown below:


```text
async getAllCars() {
return this.get('cars', null, {
cacheOptions: { ttl: 60 }
});
}
```


Apollo data sources guarantees that you don’t need to rewrite your caching logic when crafting a GraphQL layer on top of your existing APIs.


**Note:** Apollo Server uses an in memory cache by default, but you[can configure it to use Memcached or Redis instead.](https://www.apollographql.com/docs/guides/performance.html#setup)


### Write the Resolvers


The resolver implementation for the **Query** fields are shown below:


```text
const resolvers = {
Query: {
car: (root, { plateNumber }, { dataSources }) => dataSources.mvrpAPI.getACar(plateNumber),
cars: (root, args, { dataSources }) => dataSources.mvrpAPI.getAllCars(),
},
Car: {
vehicleStatus: ({ status }) => status,
yearOfManufacture: ({ productionYear }) => productionYear,
},
};
```


The resolver functions in the example above call the methods that fetch data via the data source class and ensure that the **vehicleStatus** , and **yearOfManufacture** fields are part of the data response. Wait,` dataSources` as an argument, and` dataSources.mvrpAPI` show up in both methods? How? Where did they come from? Don’t fret, I’ll show you soon!


Go ahead and instantiate` ApolloServer` . Now, pass the **MvrpAPI** data source as an option to` ApolloServer` constructor in ***server.js:***


```text
import { ApolloServer, gql } from 'apollo-server';
import { MvrpAPI } from './datasource';


...


...


const server = new ApolloServer({
typeDefs,
resolvers,
dataSources: () => ({
mvrpAPI: new MvrpAPI()
})
});


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`)
});
```


On line 10, in the example above, the` dataSources` option is passed to the` ApolloServer` constructor and set with an instance of the` MvrpAPI` data source class. Apollo Server puts the data sources on the context for every request, so you can access them from your resolvers. It will also give your data sources access to the context.


The complete ***server.js*** file should look like this now:


```text
import { ApolloServer, gql } from 'apollo-server';
import mapKeys from 'lodash/mapKeys';
import { MvrpAPI } from './datasource';


const typeDefs = gql`
type Car {
id: Int!
plateNumber: String!
color: String!
model: String!
chasisNumber: String!
vehicleStatus: String!
yearOfManufacture: Int!
issueDate: String!
expiryDate: String!
}
type Query {
car(plateNumber: String!): Car
cars: [Car]
}
`;


const resolvers = {
Query: {
cars: (root, args, { dataSources }) => dataSources.mvrpAPI.getAllCars(),
},
Car: {
vehicleStatus: ({ status }) => status,
yearOfManufacture: ({ productionYear }) => productionYear,
},
};


const server = new ApolloServer({
typeDefs,
resolvers,
dataSources: () => ({
mvrpAPI: new MvrpAPI()
})
});


server.listen().then(({ url }) => {
console.log(`🚀 Server ready at ${url}`)
});
```


The complete code for the GraphQL server is here. Clone, install and run:[apollographql/mvrp-graphql-servermvrp-graphql-server – MVRD GraphQL Server: A thin layer over MVRD Rest API with Data Sourcesgithub.com](https://github.com/apollographql/mvrp-graphql-server)


**Note:** Alternatively, you can simply run it in the browser via[https://apollographql-mvrp-graphql-server.glitch.me](https://apollographql-mvrp-graphql-server.glitch.me/) .


Now, we need to……No, nothing more. That’s it!


The next logical step is to[deploy](https://www.apollographql.com/docs/apollo-server/deployment/heroku.html) and[monitor the app](https://blog.apollographql.com/apollo-server-2-0-performance-and-error-reporting-built-in-baa6158f3c09) in production with[Apollo Engine.](https://www.apollographql.com/docs/engine/)


**Note:** You can[build a client app to fetch and display data](https://www.apollographql.com/docs/react/essentials/get-started.html) from our new GraphQL layer. Feeling lazy? I already[built you a minimal React app for this example.](https://codesandbox.io/s/llvlmk1kj7)


---


## Conclusion


Building a universal GraphQL API on top of your existing REST APIs and backends allows you ship new features without writing new endpoints or waiting on backend changes. We’re eager to hear your input and learn from your experiences. Get in touch on[Twitter](https://twitter.com/apollographql) or[Apollo Slack](https://www.apollographql.com/slack) if you’d like to help.


Written by


Prosper Otemuyiwa


[Read more by Prosper Otemuyiwa](https://www.apollographql.com/blog/author/prosper)
