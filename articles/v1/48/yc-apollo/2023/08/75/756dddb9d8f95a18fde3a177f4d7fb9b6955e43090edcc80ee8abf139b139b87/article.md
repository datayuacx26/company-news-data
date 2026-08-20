---
schema_version: "1.0.0"
document_id: "756dddb9d8f95a18fde3a177f4d7fb9b6955e43090edcc80ee8abf139b139b87"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/modernize-your-mern-stack-with-graphql-and-graphos"
published_at: "2023-08-28T12:50:29+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:fc5e09f13814ee49400e53c665ba426aa0a871fd5757333f91b475d4aa8a984b"
---

# Modernize your MERN Stack with GraphQL and GraphOS

How many routes do you have in your MERN stack? As your API grows with more routes, you may start creating multiple MERN stacks in a backend-for-frontend (BFF) architecture to help manage those routes. In creating additional MERN stacks, are you duplicating code in those BFFs? Using Federated GraphQL to have a single data access layer for your company can help you manage your MERN stack apps and provide an improved developer experience for everyone.


In recent years, GraphQL has gained significant traction in the tech industry thanks to its efficiency and flexibility. GraphQL provides a simple and more dynamic approach to data fetching than traditional RESTful APIs. If you’ve been building applications using MongoDB, Express, React, and Node.js ([the MERN stack](https://www.mongodb.com/mern-stack) ), it’s high time you considered using Apollo Server to make your stack speak GraphQL.


In this post, we will explore how you can modernize your MERN stack application to use GraphQL with Apollo Server. But before we jump into how to do that, why should you?


## **Why should you add GraphQL to your MERN Stack?**


GraphQL provides many benefits (read more about[why you should adopt GraphQL](https://www.apollographql.com/docs/intro/benefits/) ) but there are some specific ones for a MERN stack! GraphQL is declarative and provides an easy to understand the connection between your defined schema and your MongoDB queries in code. In your current server, you probably have multiple routes using` express.Router` that serve the API endpoints for your client app:


```text
import express from "express";
import db from "../db/conn.mjs";
import { ObjectId } from "mongodb";


const router = express.Router();


// This section will help you get a list of all the records.
router.get("/", async (req, res) => {
let collection = await db.collection("records");
let results = await collection.find({}).toArray();
res.send(results).status(200);
});


// This section will help you get a single record by id
router.get("/:id", async (req, res) => {
let collection = await db.collection("records");
let query = {_id: new ObjectId(req.params.id)};
let result = await collection.findOne(query);


if (!result) res.send("Not found").status(404);
else res.send(result).status(200);
});
```


This code can become hard to manage as your API grows and this could lead to duplicating code (like your db queries) between routes. It will also be difficult for any new developers coming on to the project. Imagine you have an app-server with hundreds of routes, a new developer onboarding to your team will need to understand how these routes are organized, how they serve different frontend components, and how these map to different database queries. The permutations between these three different pieces of logic could be dizzying to even a seasoned developer.


With GraphQL, you have the flexibility to define the schema and write resolvers that connect to your DB queries:


```text
//schema.graphql
type Query {
record(id: ID!): Record
}
type Record {
id: ID
name: String
position: String
level: String
}
```


```text
//resolvers.mjs
const resolvers = {
Query: {
async record(_, { id }, context) {
let collection = await db.collection("records");
let query = { _id: new ObjectId(id) };


return await collection.findOne(query);
},
async records(_, __, context) {
let collection = await db.collection("records");
return await collection.find({}).toArray();
},
},
};
```


Anyone who has worked on a GraphQL project can quickly make the connection and navigate your code structure. You no longer have to manage bespoke endpoints.


If you are using TypeScript in a MERN stack app or microservice, you have the client and server code in the same workspace enabling you to use[code generation](https://the-guild.dev/graphql/codegen/docs/getting-started) on your GraphQL schema. This can generate types that can be used on your server’s resolvers and types for your client’s operations, both work seamlessly with[@apollo/server](https://www.apollographql.com/docs/apollo-server/workflow/generate-types#setting-up-your-project) and[@apollo/client](https://www.apollographql.com/docs/react/development-testing/static-typing#setting-up-your-project) . In my opinion, this produces part of the ideal developer experience 😍


With GraphQL, you can have your client application make only one request which creates only one invocation of your API. This eliminates the need to manage custom API endpoints and gives a simple to understand pricing on your invocations (i.e. the page was viewed 1M times, so you have 1M invocations of your Function).


So if you are building a MERN stack app or microservice, you should upgrade it to speak GraphQL. Let’s learn how you can quickly do that!


## **Upgrading your server to speak GraphQL**


The first step to upgrade your server is to install the necessary GraphQL packages:


```text
npm install @apollo/server @apollo/subgraph graphql graphql-tag
```


*Note: We’re setting up this GraphQL API using[Apollo Federation](https://www.apollographql.com/docs/federation) . It’s important that you secure your subgraph to be used with the[Apollo Router](https://www.apollographql.com/docs/router) . For more information on securing your subgraphs with a router, see[this article](https://www.apollographql.com/docs/graphos/graphs/securing-a-subgraph) .*


Next, you’ll need to define a GraphQL schema based on the functionality you want to expose in your MERN stack. In this blog post, we’ll be using the[MERN stack tutorial](https://www.mongodb.com/languages/mern-stack-tutorial) as our example.e have a docs tutorial specifically for` @apollo/server` **LINK ONCE LIVE** if you want to jump straight to that.


To follow along, register for[MongoDB Atlas](https://www.mongodb.com/try?utm_campaign=devrel&utm_source=cross-post&utm_medium=cta&utm_content=apollo-mern&utm_term=stanimira.vlaeva) and deploy a forever-free database cluster. Atlas is MongoDB’s multi-cloud database integrated with a suite of data services. After deploying your free cluster, proceed by cloning the[MERN stack project](https://github.com/mongodb-developer/mern-stack-example) . Follow the instructions outlined in the repository to set up and run the Node.js and React applications.


Continuing from the MERN stack tutorial, we’ll start by defining a schema similar to the routes currently defined. We’re only going to focus on the Query/READ of the schema in this post:


```text
//schema.graphql
type Query {
record(id: ID!): Record
records: [Record]
}


type Record {
id: ID
name: String
position: String
level: String
}
```


The resolvers code is going to look similar to some of the code you have already written wherever you define your routes. From the schema defined above, we can pull the routes code into the resolvers:


```text
//resolvers.mjs
const resolvers = {
Record: {
id: (parent) => parent.id ?? parent._id,
},
Query: {
async record(_, { id }, context) {
let collection = await db.collection("records");
let query = { _id: new ObjectId(id) };


return await collection.findOne(query);
},
async records(_, __, context) {
let collection = await db.collection("records");
const records = await collection.find({}).toArray();
return records;
},
},
};


export default resolvers;
```


Lastly, we need to expose the` /graphql` route in our express app. In your server.js (or wherever you define your express app), we’ll use` @apollo/server` to expose the` /graphql` route:


```text
import express, { json } from "express";
import "./loadEnvironment.mjs";
import cors from "cors";
import gql from "graphql-tag";
import { ApolloServer } from "@apollo/server";
import { buildSubgraphSchema } from "@apollo/subgraph";
import { expressMiddleware } from "@apollo/server/express4";
import resolvers from "./resolvers.mjs";
import { readFileSync } from "fs";


const PORT = 5050;
const app = express();


app.use(cors());
app.use(express.json());


const typeDefs = gql(
readFileSync("schema.graphql", {
encoding: "utf-8",
})
);


const server = new ApolloServer({
schema: buildSubgraphSchema({ typeDefs, resolvers }),
});
await server.start();


app.use("/graphql", cors(), json(), expressMiddleware(server));


app.listen(PORT, () => {
console.log(`Server listening on port ${PORT}`);
});
```


Now you should be able to run your server project and visit` /graphql` to see Apollo Explorer 🎉. Next we need to upgrade the client project to use GraphQL.


## **Upgrading your client to use GraphQL**


The first step to upgrade your client is to install the necessary GraphQL packages:


```text
npm install @apollo/client graphql
```


Next, we need to add the` ApolloProvider` to our React app:


```text
// src/index.js
import {
ApolloClient,
InMemoryCache,
ApolloProvider
} from "@apollo/client";


const client = new ApolloClient({
uri: "http://localhost:5050/graphql",
cache: new InMemoryCache(),
connectToDevTools: process.env.NODE_ENV == "production" ? true : false
});


const container = document.getElementById("root");
const root = createRoot(container);
root.render(
<React.StrictMode>
<BrowserRouter>
<ApolloProvider client={client}>
<App />
</ApolloProvider>
</BrowserRouter>
</React.StrictMode>
);
```


` connectToDevTools` is what enables us to have the Apollo Client Devtools appear as an “Apollo” tab in your web browser inspector. You wouldn’t want to run this in production.


Now we’re ready to start utilizing the hooks` @apollo/client` provides to execute GraphQL operations. You’ll need to pick a UI to start with that is associated with the schema you designed. Since we are doing the query portion of the records, we can hook up the` recordList.js` to our GraphQL route:


```text
// recordList.js
import React from "react";
import { Link } from "react-router-dom";
import { gql, useQuery } from "@apollo/client";


export const GET_RECORDS = gql`
query GetRecords {
records {
id
name
position
level
}
}
`;
const Record = ({ record }) => {
return (
<tr>
<td>{record.name}</td>
<td>{record.position}</td>
<td>{record.level}</td>
</tr>
);
};
export default function RecordList() {
const { loading, error, data } = useQuery(GET_RECORDS);


if (loading) return <p>Loading...</p>;
if (error) return <p>Error : {error.message}</p>;


// This following section will display the table with the records of individuals.
return (
<div>
<h3>Record List</h3>
<table className="table table-striped" style={{ marginTop: 20 }}>
<thead>
<tr>
<th>Name</th>
<th>Position</th>
<th>Level</th>
<th>Action</th>
</tr>
</thead>
<tbody>
{data?.records.map((record) => (
<Record record={record} key={record.id} />
))}
</tbody>
</table>
</div>
);
}
```


Now you should be able to start your client application and the records will be loaded through your GraphQL route ✨


The next step is to start planning the next UI you want to build or migrate to your new GraphQL route with this iterative process:


1. Define the operation and schema – start with the ideal GraphQL operation for the UI you’re trying to build and write the schema to enable that operation.
2. In your server project, update the schema and write the necessary resolvers
3. In your client project, add the operation and add to your UI component


## **Conclusion**


In this blog post, we discussed some of the benefits of using GraphQL in a MERN stack application or microservice and how it simplifies the routes defined on the server side. Anyone who has used GraphQL in the past will be able to get up to speed quickly in your project and an upgraded developer experience. We also walked through how you can upgrade your project to speak GraphQL with` @apollo/server` and` @apollo/client` in just a couple of steps using the[MERN stack tutorial](https://www.mongodb.com/languages/mern-stack-tutorial) as our example. You can check out our docs that go into more depth on setting up the server portion of the project. Additionally, if you’d like to explore MongoDB in a cloud environment, you can register and deploy a free cluster in[MongoDB Atlas](https://www.mongodb.com/try?utm_campaign=devrel&utm_source=cross-post&utm_medium=cta&utm_content=apollo-mern&utm_term=stanimira.vlaeva) .


Now that you’ve started your journey into GraphQL, you can start building and iterating on your app faster. You can integrate GraphOS for[schema checks](https://www.apollographql.com/docs/graphos/delivery/schema-checks) or[routing requests across multiple GraphQL services](https://www.apollographql.com/docs/graphos/routing/cloud) , like a 3rd party GraphQL API you’re integrating with.


You probably will also have questions/choices come up as you build and we’re here to help! Come join us in the[Apollo Discord Server](https://discord.gg/cnfjXU92hZ) and ask your questions. You can ping me directly (@watson) or any of the Developer Advocates in there. We plan regular streams that turn into videos on our YouTube and it’s the best place to catch that content first. Hope to see you there!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
