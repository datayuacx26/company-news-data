---
schema_version: "1.0.0"
document_id: "3d9aca9318caf7f807a226ef2e47cf8397da5ec3284416552fcf3fe5285ff87d"
company_key: "s-p-global-inc-common-stock"
company: "S&P Global Inc."
source_id: "s-p-global-inc-common-stock-rss-ff630ac34bbe"
canonical_url: "https://engineering.global.com/graphql-apis-3cd082d76602"
published_at: "2024-05-22T09:27:29+00:00"
first_seen_at: "2026-07-20T04:36:47.908335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:91d39d2c2d78b1886cb8721a9e760bfcee24d69e18a265aa610db1a242cda738"
---

# GraphQL APIs

Press enter or click to view image in full size


# GraphQL APIs


[Global Engineering](https://global-engineering.medium.com/?source=post_page---byline--3cd082d76602---------------------------------------)


5 min read


·


Apr 23, 2024


--


*By Joe Maskery*


### What is an API?


As someone new to software development, one of the most important concepts I have been introduced to is APIs. API stands for Application Programming Interface; they are mechanisms that enable two software applications to talk to each other using a set of protocols and definitions.


The most common types of API used at Global are REST and GraphQL. Whilst REST is the most popular type of API used today, GraphQL is relatively new and offers some exciting features which this blog post will summarise.


### GraphQL


GraphQL is a data query and manipulation language and query runtime engine for APIs. The main selling point of GraphQL is that it allows clients to request only the exact data they need from the API, compared to REST where API responses are fixed and defined by the server.


### How it works


GraphQL APIs only have a single endpoint where clients send all requests; these requests are then validated, interpreted, and executed by GraphQL. This is very different to REST APIs which have multiple endpoints to each fulfil a different request.


GraphQL has two main operation types: *queries* and *mutations* . Queries are requests that retrieve data from the server, like the HTTP GET method, whereas mutations are any request which modifies a resource, like the HTTP POST, PUT, and DELETE methods.


To setup a GraphQL API you need to define a schema file. This schema file should contain the definitions for all *Object* types that will be transferred across the API as part of a client request or a server response. Additionally, the schema file should contain definitions for any queries or mutations that the API will perform including their names, required parameters, and response objects.


Any query or mutation defined in the schema file will also require a *resolver* . Resolvers are methods or functions that are assigned to each operation to perform the logic required to resolve the request, e.g. retrieve the requested data from a database. The way resolvers are implemented will vary depending on the language or framework that the application uses.


In summary, the GraphQL schema file contains the definitions for all object types that can be transferred across the API, and the templates for all operations that can be performed by the API. A key feature of GraphQL is that it allows multiple operations to be sent and resolved as part of the same request. As long as the request is valid against the schema file all operations will be resolved in the single request.


### Pros and Cons


***Advantages***


· **Eliminates over-fetching** : GraphQL allows clients to request only the exact data they need and nothing more. This reduces network traffic and improves the performance of client applications.


## Get Global Engineering’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


· **Eliminates under-fetching** : clients can retrieve all required data from a single request to the API, instead of making multiple requests to multiple different endpoints.


· **Flexibility** : clients have full control over the shape and structure of the data they receive and can retrieve nested or related data in a single request.


· **Strongly typed schema** : the GraphQL schema provides a clear contract between the client and the server, and clients can use introspection to see all available types and fields.


· **Versionless APIs** : because clients request only the specific fields they need, it is easier to add new types and fields without causing any breaking changes.


***Disadvantages:***


· **Complexity** : implementing a GraphQL API can be more complex than traditional APIs such as REST. It has a learning curve and developers need to pick up the key concepts such as the schemas, queries and mutations, and resolvers.


· **Performance overhead** : GraphQL servers need to parse and execute complex queries which can require more intensive processing when compared to REST APIs, especially for resource-intensive applications.


· **Caching** : caching is a challenge as GraphQL responses are generally unique to each query, especially compared to REST APIs where responses are more standardized.


### Example


To illustrate the above points we can look at an example. Taking an example of a Spring application which handles Order-Lines and Order-Line-Items, the schema may look something like this:


```text
type OrderLine {      orderId: ID!      company: String!      dateCreated: String!      numItems: Int!      totalPrice: Float!      orderLineItems: [OrderLineItem!]  }   type OrderLineItem {      itemId: ID!      orderLineId: ID!      orderLine: OrderLine      product: String!      startDate: String!      endDate: String!      duration: Int!      price: Float!  }   input AddOrderLineRequest {      company: String!  }   input AddOrderLineItemRequest {      orderLineId: ID!      product: String!      startDate: String!      endDate: String!      price: Float!  }   type Query {      getAllOrderLines: [OrderLine!]      getOrderLineById(orderId: ID!): OrderLine       getAllOrderLineItems: [OrderLineItem!]      getOrderLineItemById(itemId: ID!): OrderLineItem  }   type Mutation {      addOrderLine(addOrderLineRequest: AddOrderLineRequest!): String      addOrderLineItem(addOrderLineItemRequest: AddOrderLineItemRequest!): String  }
```


From the schema it is easy for us to see all available objects and operation types in the API and exactly how to call them.


Now we can see how easy it is to construct a request containing multiple queries and get the data back in a specific shape with only the required fields:


```text
query GetAllOrderLines {      getAllOrderLines {          orderId          company          dateCreated          numItems          totalPrice          orderLineItems {              product          }      }      getOrderLineItemById(itemId: "3") {          itemId          product          startDate          endDate          duration          price          orderLine {              company          }      }  }
```


From the response we can see how easy it is to work with related data, as we are able to easily see the relationships between the Order-Lines and the Order-Line-Items.


```text
{      "data": {          "getAllOrderLines": [              {                  "orderId": "1",                  "company": "Company 1",                  "dateCreated": "2024-04-03 14:05:24.927713",                  "numItems": 3,                  "totalPrice": 2178.99,                  "orderLineItems": [                      {                          "product": "Bus asset"                      },                      {                          "product": "LU asset"                      },                      {                          "product": "Bus asset"                      }                  ]              }          ],          "getOrderLineItemById": {              "itemId": "3",              "product": "Bus asset",              "startDate": "2023-11-13",              "endDate": "2024-04-25",              "duration": 165,              "price": 423.99,              "orderLine": {                  "company": "Company 1"              }          }      }  }
```


Both REST and GraphQL APIs have their pros and cons, and when deciding on which type of API to use it’s important to consider the requirements of your application. It’s also important to consider that GraphQL can be applied to an application on top of a REST API. In my team for example we have found it best to use a combination of GraphQL, for communication with the front-end, and REST APIs, for communication between back-end services, as each one has its benefits for slightly cases.
