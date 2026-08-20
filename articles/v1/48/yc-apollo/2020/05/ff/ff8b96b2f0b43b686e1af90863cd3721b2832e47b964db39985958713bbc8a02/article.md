---
schema_version: "1.0.0"
document_id: "ff8b96b2f0b43b686e1af90863cd3721b2832e47b964db39985958713bbc8a02"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/a-deep-dive-on-apollo-data-sources"
published_at: "2020-05-07T08:39:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:48.304506+00:00"
content_hash: "sha256:02000711d1d1b4fb5b7c80e92ff299cff29ccf582a31c8e4670dfeace3173ec6"
---

# A Deep Dive on Apollo Data Sources

GraphQL makes it easy for clients to fetch data from the server. But where does a GraphQL server source that data from in the first place?


Thanks to the flexibility of GraphQL, we have many options (back-end services, databases, external APIs, and so on). Certain cloud platforms provide automatically generated GraphQL access to a particular set of data sources, and if you create your own GraphQL server, you can combine any set of data sources you like.


#### Using a GraphQL PaaS (Platform-as-a-Service)


Several PaaS products provide automatically generated GraphQL APIs that fetch data from a provisioned database:


- [Hasura](https://hasura.io/) sources data from Postgres databases, GraphQL services, and REST APIs.
- [AppSync](https://aws.amazon.com/appsync/) sources data from DynamoDB, Elasticsearch, and Aurora.
- [MongoDB Stitch](https://docs.mongodb.com/stitch/graphql/) sources data from a linked MongoDB cluster.


#### Coding our own server


If we choose to code our own GraphQL server, the first thing we need to decide on is a programming language. Luckily, there are well-maintained server libraries available for a large number of programming languages like Python, Ruby, and Java (see[here](https://graphql.org/code/) ).


By far, the most popular language for writing a GraphQL server is JavaScript, and the most popular library is Apollo Server.


From within our own Apollo Server instance, there are lots of different ways to source data. A few examples are:


- [Prisma](https://www.prisma.io/) , an ORM for[SQL databases](https://www.prisma.io/docs/more/supported-databases) (and others in the future), which efficiently performs operations against databases using batching and memoization.
- [Join Monster](https://join-monster.readthedocs.io/en/latest/) fetches data from relational databases by converting GraphQL queries into SQL statements, including selecting specific fields and joining tables.
- **Data sources** —the topic of the rest of this article 😊.


### Data sources


Apollo Server **data sources** are a collection of classes that encapsulate the code that’s specific to interacting with a particular type of data service. They take care of challenging data-fetching logic like caching and batching, while also providing access to the GraphQL **context** .


Check out an example of a data source in practice in the blog post,[Easy and performant GraphQL over REST](https://blog.apollographql.com/easy-and-performant-graphql-over-rest-e02796993b2b) .


We can write our own custom data source by extending the base` DataSource` class provided by` apollo-datasource` , or we can use an open-source data source. Here are the current official and community-maintained open-source data source libraries:


- [RESTDataSource](https://www.apollographql.com/docs/apollo-server/data/data-sources/#rest-data-source) : Fetch data from a REST API. Includes caching based on the HTTP response’s` Cache-Control` header and lifecycle methods.
- [SQLDataSource](https://github.com/cvburgess/SQLDataSource) : Fetch data from a SQL database. Includes caching of SQL[Knex](https://knexjs.org/) queries.
- [MongoDataSource](https://github.com/GraphQLGuide/apollo-datasource-mongodb/) : Fetch data from a MongoDB database. Includes caching and batching when fetching objects by ID.
- [GraphQLDataSource](https://github.com/poetic/apollo-datasource-graphql) : Fetch data by sourcing from an *existing* GraphQL API.


### Data source classes


Before we use or build our own data sources, we need to understand the two primary data source classes and their responsibilities.


#### 1 — The parent class


The generic parent class **defines the policy for how we interact with a particular data source** .


If a new database comes out, we’ll first need to write a parent data source class to encapsulate *how* to talk to it.


In more detail, the data source parent class holds the following responsibilities:


- It extends the` DataSource` class from the` apollo-datasource` library.
- It has some way of receiving information about the database or API (either a constructor argument or an instance variable, like` RESTDataSource` ’s` this.baseURL` ).
- It contains an` initialize()` method that receives the context and an optional cache.
- It decides when to call lifecycle methods that can be defined by the child class, like` RESTDataSource` ’s` willSendRequest()` and` didEncounterError()` .
- It contains generic methods for fetching data, which may use` DataLoader` and/or the cache.
- It contains generic methods for altering data, which may invalidate cached data.


#### 2 — The child class


The child class **extends our** **parent data source, and performs operations specific to our application** . The child class:


- Extends the parent class.
- Knows specific database details, such as the API URL or connection string.
- Defines lifecycle methods. Common use cases are adding authorization headers to requests and reporting errors.
- Defines *application-specific* methods to be called by resolvers. These methods use the parent class’s *generic* data-fetching or data-altering methods.


### Creating a custom data source


Now that we understand the structure of data sources, let’s see all of the pieces in action as we create a parent/child class pair to source data from a new imaginary document database called “Foo”.


#### Parent class


We’ll call our parent class` FooDataSource` , and we’ll have its constructor take a Foo DB client, which has these fields:


- ` dbClient.connectionURI` : the URI of the database server
- ` dbClient.getByIds(ids)` : given an array of IDs, returns the associated documents from the database
- ` dbClient.update(id, newDoc)` : updates the document with the given` id` to be the` newDoc`


Here’s a skeleton for our class that we’ll fill in:


```text
import { DataSource } from 'apollo-datasource'
import { InMemoryLRUCache } from 'apollo-server-caching'
import DataLoader from 'dataloader'


class FooDataSource extends DataSource {
constructor(dbClient) { ... }


initialize({ context, cache } = {}) { ... }


didEncounterError(error) { ... }


cacheKey(id) { ... }


async get(id, { ttlInSeconds } = {}) { ... }


async update(id, newDoc) { ... }
}
```


Here’s the constructor definition (or the[full class](https://gist.github.com/lorensr/5250fd77779fcc14466bcc5cd5f69a72) if you’d like to see it in context):


```text
import { DataSource } from 'apollo-datasource'
import DataLoader from 'dataloader'


class FooDataSource extends DataSource {
constructor(dbClient) {
super()
this.db = dbClient
this.loader = new DataLoader(ids => dbClient.getByIds(ids))
}


...
}
```


The constructor saves the DB client as an instance variable to be used later. It also creates an instance of` DataLoader` to use for this request (a new data source object is created for each GraphQL request).` DataLoader` needs to know how to fetch a list of documents by their IDs. Here we’re assuming the array of documents that` getByIds()` returns are the same order and length as` ids` (a requirement of` DataLoader` ); otherwise, we’d need to reorder them.


[DataLoader](https://github.com/graphql/dataloader) is a library that does batching and memoization caching for the queries our data source makes during a single GraphQL request. **Batching** converts multiple database requests for a single document into a single request for multiple documents, and **memoization caching** deduplicates multiple requests for the same document.


```text
import { InMemoryLRUCache } from 'apollo-server-caching'


...


initialize({ context, cache } = {}) {
this.context = context
this.cache = cache || new InMemoryLRUCache()
}
```


The` initialize()` method is called automatically by Apollo Server. If Apollo Server has been configured with a global cache, we use that; otherwise, we create an in-memory cache.


```text
didEncounterError(error) {
throw error
}
```


When an error occurs, we call` this.didEncounterError()` , which a child class can overwrite if they want.


```text
cacheKey(id) {
return `foo-${this.db.connectionURI}-${id}`
}
```


We include the` connectionURI` in the cache key to avoid collisions, which can occur if multiple Foo data sources are connected to multiple Foo databases, and those data sources are all using the same cache.


```text
async get(id, { ttlInSeconds } = {}) {
const cacheDoc = await cache.get(this.cacheKey(id))
if (cacheDoc) {
return JSON.parse(cacheDoc)
}


const doc = await this.loader.load(id)


if (ttlInSeconds) {
cache.set(this.cacheKey(id), JSON.stringify(doc), { ttl: ttlInSeconds })
}


return doc
}
```


We provide a` get(id)` method to use in our server’s resolvers, with an optional` ttlInSeconds` if the caller wants the result to be cached.


1. First, we check if the doc is already in the cache. If it is, we parse it (cache values are always strings) and return it.
2. If it isn’t, we ask` DataLoader` to get the document.` DataLoader` takes all of the calls to` load(id)` (a single GraphQL operation often involves multiple calls to` .get()` ), deduplicates the set (in case` .get()` is called multiple times with the same ID), and puts all the distinct IDs into an array for a batch request (the call to` dbClient.getByIds()` in the constructor).
3. When the batch request completes,` DataLoader` returns on this line the one document we need:


```text
const doc = await this.loader.load(id)
```


Then if` ttlInSeconds` was provided, we cache the document for that length of time. And finally, we return it!


```text
async update(id, newDoc) {
try {
await this.db.update(id, newDoc)
this.cache.delete(this.cacheKey(id))
} catch (error) {
this.didEncounterError(error)
}
}
```


We also provide an` update(id, newDoc)` method to use in resolvers. After a successful update, it deletes the old document from the cache. Another possible implementation would be to overwrite the previous cache entry with` newDoc` (in which case we’d need a value for` ttl` , so perhaps we’d add a third argument to` update()` with a` ttlInSeconds` ).


#### Child class


Once we have the parent class complete, we can use it to create one or more child classes (in this example case, we create one for each database, but with some data sources we might create one for each table or collection). Here’s an example child class:


```text
import FooDataSource from './FooDataSource'
import { reportError } from './utils'


export default class MyFooDB extends FooDataSource {
async updateFields(id, fields) {
const doc = await this.get(id)
return this.update(id, {
...doc,
...fields
})
}


didEncounterError(error) {
reportError(error)
}
}
```


This class overrides` didEncounterError` to use its own error-reporting service instead of allowing the error to propagate. It also adds a new method that calls the parent’s` .get()` and` .update()` methods. When we create the data source, we give the database client to the constructor:


```text
import FooClient from 'imaginary-foo-library'


import MyFooDB from './MyFooDB'


const fooClient = new FooClient({ uri: 'https://foo.graphql.guide:9001' })


const server = new ApolloServer({
typeDefs,
resolvers,
dataSources: () => ({
myFoos: new MyFooDB(fooClient)
})
})
```


Now inside our resolvers, we can use` context.dataSources.myFoos` and call all of the methods defined in both the parent class and the child class:


```text
const resolvers = {
Query: {
getFoo: (_, { id }, context) =>
context.dataSources.myFoos.get(id, { ttlInSeconds: 60 })
},
Mutation: {
updateFoo: async (_, { id, fields }, context) => {
if (context.isAdmin) {
context.dataSources.myFoos.updateFields(id, fields)
}
}
}
}
```


These example resolvers use` .get()` from` FooDataSource` and` .updateFields()` from` MyFooDB` .


---


That’s how to create and use your own data source! 💃 If you create one that’s applicable outside your own application, I encourage you to open-source it so that others may benefit from your work 🤗. If you do, you can open a PR to add it to the[list of community data sources](https://www.apollographql.com/docs/apollo-server/data/data-sources/#community-data-sources) in the Apollo docs.


If you’d like to keep learning about Apollo, I recommend subscribing below and using your company’s education budget to get a copy of the[Guide](https://graphql.guide/) , an extensive book on GraphQL and Apollo 🚀. (Or if you’re experiencing hardship due to C-19,email me for a free copy!)


May you be well 🖖 *️← the compassionate alternative to 🤝 in the age of ☣️*


—[Loren](https://twitter.com/lorendsr) 🤓


Written by


Loren Sands-Ramshaw


[Read more by Loren Sands-Ramshaw](https://www.apollographql.com/blog/author/lorengraphqlguide)
