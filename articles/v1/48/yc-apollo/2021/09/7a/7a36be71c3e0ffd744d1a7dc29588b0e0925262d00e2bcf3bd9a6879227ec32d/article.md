---
schema_version: "1.0.0"
document_id: "7a36be71c3e0ffd744d1a7dc29588b0e0925262d00e2bcf3bd9a6879227ec32d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/journey-of-a-graphql-query"
published_at: "2021-09-03T10:10:37+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:b01a0869d4f6de81cf5c9b56f14de7b60ad5813b925a81e3c18e7733cb28e176"
---

# Journey of a GraphQL query

There are a lot of moving parts in GraphQL and it can feel a bit overwhelming to piece them together, especially for beginners! Maybe you’ve gone through a couple of tutorials, written some code, but can’t quite wrap your head around how all these pieces work together. Or maybe you’ve tried out a few GraphQL queries to retrieve data, but want to know more about how the server is handling these requests.


You’ve come to the right place! In this blog post, we’ll take a step back to understand how a GraphQL query travels in a full-stack application (from the client to the server and back). We’ll learn how the schema, resolvers and data sources work together to return the exact data the client is asking for.


Grab a seat and get comfy, it’s story-time for the **Journey of a GraphQL query** !


## Starting with the schema


Before the query’s journey even begins, it needs a **schema** . A schema is like a contract between the server and the client. The schema defines what data the clients can request or change, and the server returns that data to the client. With a schema-first design, we’re going to structure our schema based on exactly the data the client application needs.


Let’s take the Catstronauts app as an example. This is the app we build step-by-step in the[Odyssey Lift-off series](https://odyssey.apollographql.com/lift-off-part1) . The app displays a list of learning tracks in a card-grid format, and each track card requires certain pieces of data.


*The homepage mockup, showing each track card with its required data* .


A closer look at a track card’s data.


Here’s what our schema can look like using the mockup. We write the schema using the[GraphQL Schema Definition Language (SDL)](https://www.apollographql.com/docs/apollo-server/schema/schema/#the-schema-definition-language) .


```text
type Query {
"Query to get tracks for the homepage grid"
tracksForHomepage: [Track!]!
}


"A track is a group of Modules that teaches about a specific topic"
type Track {
id: ID!
"The track's title"
title: String!
"The track's illustration to display in track card or track page detail"
thumbnail: String
"The track's approximate length to complete, in minutes"
length: Int
"The number of modules this track contains"
modulesCount: Int
"The track's main Author"
author: Author!
}


"Author of a complete Track"
type Author {
id: ID!
"Author's first and last name"
name: String!
"Author's profile picture"
photo: String
}
```


Without focusing too much on the SDL syntax, we can start to see how the schema’s fields connect back to our mockup’s required data! Using this schema, the client can definitely get the data it needs for its homepage.


## Sending a query from the client


It’s time to start the journey with a query! To retrieve the data for its homepage, the app uses a GraphQL query that defines the selection set of fields it needs. Using the types and fields outlined in the schema earlier, the client can craft a query that looks like this:


```text
query getTracks {
tracksForHome {
title
thumbnail
length
modulesCount
author {
name
photo
}
}
}
```


Then, the app sends this query to the server as a string. There are[a few ways to do this:](https://www.apollographql.com/blog/graphql/examples/4-simple-ways-to-call-a-graphql-api/) as an HTTP request with` fetch` or` axios` , GraphQL IDEs, or with packages such as Apollo Client.


## Server: parse, build, validate


In server land, when the GraphQL server receives the request, it needs to validate the query before it can move forward with retrieving and returning the data.


How does it validate the query? First, it extracts the query string from the request. Then, it parses the string and transforms it into something it can better manipulate: a tree-structured document called an AST (Abstract Syntax Tree). It’s much easier for the server to traverse the tree’s structure compared to the original query string! With the AST, the server can validate what the query is asking for against the types and fields specified in the schema.


### Handling errors


If anything in the query looks wrong, the server throws an error and sends it right back to the app! Here are a few scenarios in which the server might throw an error:


- a requested field is not defined in the schema (watch out for typos!)
- a requested field doesn’t exist on a given type
- the field is missing subfields
- the query is malformed (maybe a stray bracket?)


If the query is valid, the server can “execute” it! Meaning, the server can continue on in its process and actually fetch the data.


### Retrieving data with resolver functions & data sources


Starting at the top-level field in the query and working its way down, the server invokes that field’s **resolver function** . A resolver function’s mission is to “resolve” its field by populating it with the data from **data sources** , such as a database or a REST API. The resolver can mix and match any number of data sources to get the information the query is asking for, but the data needs to match the return type of the field, which is specified in the schema.


For example, the resolver for the` tracksForHomepage` field might look like this:


```text
tracksForHomepage: (parent, args, context, info) => {
return context.dataSources.trackAPI.getTracksForHome();
}
```


The resolver function is called` tracksForHomepage` because it needs to match the name of the field in the schema. Resolver functions have a specific signature with[4 optional parameters](https://www.apollographql.com/docs/apollo-server/data/resolvers/#resolver-arguments) :` parent` ,` args` ,` context` , and` info` . In the code snippet above, we use` context` to access the data source that takes care of retrieving all of the correct tracks data.


When all of the query’s fields are resolved successfully, the data is assembled into a nicely ordered JSON object with the exact same shape as the query. The server assigns this object to the HTTP response body’s` data` key.


### Sending the data back to the client


Then, it’s time for the return trip, back to the app in client land. The client receives the response with exactly the data it asked for! It passes that data to components to render them, and voilà, the homepage is live with data!


The full journey of a GraphQL query from client to server and back, with all the pieces involved.


On the client-side, when it receives the data, it can render those fields as shown below:


```text
const TrackCard = ({ track }) => {
const { title, thumbnail, length, modulesCount, author } = track;


return (
<CardContainer>
<CardImage src={thumbnail} alt={title} />
<CardTitle>{title}</CardTitle>
<CardFooter>
<AuthorImage src={author.photo} />
<AuthorName>{author.name}</AuthorName>
<TrackLength>
{modulesCount} modules - {length}
</TrackLength>
</CardFooter>
</CardContainer>
);
};
```


## Conclusion


And that’s the journey of a GraphQL query! At the heart of the journey is the schema. Then, the journey starts with a query sent from the client to the server. In server-land, the schema, resolvers and data sources all work together to validate and retrieve the data requested. If all goes well, the client receives the data in the exact shape it asked for.


We hope you enjoyed storytime!


If you’re hungry for more, or you want to test yourself on what you learned today, we’ve got a few quizzes about this particular topic (and more) in our Odyssey course called[Lift-off II: Resolvers](https://odyssey.apollographql.com/lift-off-part2/journey-of-a-graphql-query) . Enjoy!


Written by


Michelle Mabuyo


[Read more by Michelle Mabuyo](https://www.apollographql.com/blog/author/michellemabuyo)
