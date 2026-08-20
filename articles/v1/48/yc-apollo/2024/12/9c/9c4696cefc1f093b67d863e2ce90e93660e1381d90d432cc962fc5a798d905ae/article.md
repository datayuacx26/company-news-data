---
schema_version: "1.0.0"
document_id: "9c4696cefc1f093b67d863e2ce90e93660e1381d90d432cc962fc5a798d905ae"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/simplify-odata-api-orchestration-with-apollo-connectors"
published_at: "2024-12-18T07:30:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:3be3bc3e8db6cc8cffc9b0d9938447cc72088436ed1dbca8d643a5f244696b33"
---

# Simplify OData API Orchestration with Apollo Connectors

OData (Open Data Protocol) is a standardized protocol for building and consuming RESTful APIs, enabling developers to query and manipulate data using a uniform, SQL-like syntax across diverse platforms. While it simplifies data access with features like filtering and pagination, developers often struggle with its complexity in implementation, over-fetching or under-fetching data, and scaling for real-world applications.


These complexities often arise from the need for developers to coordinate API requests across business domain entities to deliver a new feature. OData introduced features that enable relationships within the API, but this still required application developers to have to understand this complexity to build their new feature. The complexity of making those relationships with foreign keys was more difficult than just writing some client code to orchestrate requests across the entities. This leads to bloated client code or additional service layers like backend-for-frontend (BFF).


It was never supposed to be this complicated! Apollo Connectors makes orchestrating your REST API calls easy. You simply connect your REST endpoint to portions of your schema and a plan will be built and executed based on the client’s request. Let’s see how that works using the OData[Basic](https://www.odata.org/getting-started/basic-tutorial) /[Advanced](https://www.odata.org/getting-started/advanced-tutorial) tutorials and the API they provide: https://services.odata.org/V4/TripPinServiceRW.


## Using your OData API as a source


Apollo Connectors enables you to connect to any REST API by writing a GraphQL schema with metadata for the details of your API. You can do this using the` @source`[directive](https://www.apollographql.com/docs/graphos/schema-design/connectors/http#using-baseurl-in-source) and provide a` baseURL` for where your OData API lives:


```text
extend schema
@link(url: "https://specs.apollo.dev/federation/v2.10", import: ["@key"])
@link(url: "https://specs.apollo.dev/connect/v0.1", import: ["@source", "@connect"])
@source(
name: "TripPin"
http: {
baseURL: "https://services.odata.org/V4/"
headers: [{ name: "Authorization", value: "Bearer {$config.apiKey}"}]
}
)
```


## Connecting an Individual Entity by ID


OData has the concept of an[entity](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part1-protocol.html#_Toc31358841) based on a single ID and our first step in exposing these entities in the connector we’re building. You’ll want to define the entities you want to orchestrate requests across. First you’ll need to define the shape you want to return, which is typically the entity returned by your` ODataController` :


```text
type Person {
id: ID!
firstName: String
lastName: String
emails: [String]
}
```


Now we need to connect the defined` Person` to our API endpoint using the` @connect` directive and expose our` GET` endpoint. We typically place read-only functionality in the root` Query` of our schema. The OData tutorial provides a` People` route, but it’s important to note that we don’t want to just use` /TripPinServiceRW/People` when you have versioned your entity models in your OData API. For this scenario, you should be using a specific` $schemaversion` to the url structure:


```text
type Query {
person(id:ID!): Person
@connect(
source: "TripPin"
http: { GET: "(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People('{$args.id}')" }
)
}
```


With our schema connected, we need to provide a selection of the data coming from our API and how to transform the data to the shape we defined. If you opened up[https://services.odata.org/V4/(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People(‘russellwhyte’)](https://services.odata.org/V4/(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People('russellwhyte')) , you’ll see the full shape of the` People` entity. The` Username` field is the id and I’ll want to rename the fields to use camel case. Below is an example of the schema next to the actual results returned from the API so you can see how they map together:


Now we have a full defined connector for our` Person` and the last step is to define it as an entity by setting` entity: true` :


```text
type Query {
"""
Get a Person
http://services.odata.org/V4/(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People('russellwhyte')
"""
person(id:ID!): Person
@connect(
source: "TripPin"
selection: """
$.value {
id: UserName
firstName: FirstName
lastName: LastName
emails: Emails
}
"""
entity: true
)
```


And that’s it! The nice thing here is that this pattern can be applied to any entity you have defined in your OData API!


## Connecting an Entity Collection


OData controllers provide basic list functionality for entities defined and we can connect to that endpoint just like we did for the entity, except omitting` entity: true` and the arguments:


### Adding pagination for Entity Collection


OData provides[system query options](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part1-protocol.html#_Toc31358953) that can be used for pagination or filtering. We can quickly expose the pagination rules by adding` skip` and` take` to our arguments and mapping that to OData’s` $top` and` $skip` :


```text
type Query {
"""
Get a list of people that can be paginated
http://services.odata.org/V4/TripPinService/People?$top=1&$skip=2
"""
people(take: Int = 3, skip: Int = 0): [Person]
@connect(
source: "TripPin"
http: { GET: "(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People?$top={$args.take}&$skip={$args.skip}" }
selection: """
$.value {
id: UserName
firstName: FirstName
lastName: LastName
}
"""
)
}
```


## Creating an Individual Entity


Now that we’ve seen how to expose reading an entity, we need to add the “create” functionality for our entity as a` POST` request. This is going to look very similar to how we read an entity, but we’ll need to create argument inputs that are mapped into the` body` we define in` @connect` :


```text
type Mutation {
"""
Create a Person
http://services.odata.org/V4/TripPinService/People
"""
createPerson(username: ID!, firstName: String, lastName: String): Person
@connect(
source: "TripPin"
http: {
POST: "(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People/"
body: """
$args {
Username: username
FirstName: firstName
LastName: lastName
}
"""
}
selection: """
$.value {
id: UserName
firstName: FirstName
lastName: LastName
}
"""
)
}
```


## Updating an Individual Entity by ID


Updating an entity looks almost identical to creating an entity, but in this case we have a` PATCH` request instead of a` POST` :


```text
type Mutation {
"""
Update a person
http://services.odata.org/V4/TripPinService/People('miathompson')
"""
updatePerson(username: ID!, updates: PersonUpdates): PersonUpdatesResponse
@connect(
source: "TripPin"
http: {
PATCH: "(S(unfmciiartlvloowd54bozof))/TripPinServiceRW/People('{$args.username}')"
body: """
$args {
Username: username
FirstName: updates.firstName
LastName: updates.lastName
Emails: updates.emails
}
"""
}
selection: """
success: $(true)
message: $("User deleted")
"""
)
}
input PersonUpdates {
firstName: String
lastName: String
emails: [String]
}
type PersonUpdatesResponse {
success: Boolean
message: String
}
```


You can either create a generic update field for each entity (i.e. updatePerson) where you update any fields in a grouped ** input *,* or you can have more granular names that describe what is being updated vs a generic update (i.e.` updatePersonName` ):


```text
type Mutation {
"""
Update a person
http://services.odata.org/V4/TripPinService/People('miathompson')
"""
updatePersonName(username: ID!, firstName: String, lastName: String): PersonUpdatesResponse
@connect(
source: "TripPin"
http: {
body: """
$args {
Username: username
FirstName: updates.firstName
LastName: updates.lastName
}
"""
}
selection: """
success: $(true)
message: $("User deleted")
"""
)
}
type PersonUpdatesResponse {
success: Boolean
message: String
}
```


## Invoking a Singleton


OData provides a capability where you can expose singletons on the models you define in your API. This is typically used for “experience” APIs that provides some functionality for an entity or model. Depending on what your Singleton does will determine if you want it to be a “read” operation (in the` Query` ) or a “write” operation (in the` Mutation` ). For the tutorials, they expose a` GetNearestAirport` that returns the closed airport based on a latitude and longitude. We’ll put this read functionality into our` Query` for this scenario and it will look very similar to our` Person` connections:


## Connecting it all together


By combining OData with Apollo REST Connectors, you can quickly build an API orchestration layer that leverages everything in OData but leaves the complexity behind. We covered a pattern on how you can connect entities you have defined in your OData APIs and you can apply this pattern to any OData Entity.


If you want to get started with Apollo Connectors, head over to our[quickstart documentation](https://www.apollographql.com/docs/graphos/get-started/guides/rest-quickstart) and then you can recreate everything in this post. We’ll also be hosting our second episode of Connectors Live on January 15, 2025 where we’ll build out this OData orchestration layer live from scratch to hosted instance. We’re working on the registration link for the next episode and will update here once it’s up. Until then, you can also checkout our last episode that covers a checkout flow for the Stripe REST API if you want to see more connectors examples.


Happy building! 🛠️


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
