---
schema_version: "1.0.0"
document_id: "239f97492e3eea624795019834182b22c25e59a243bdd77b761e3ba05be74ca4"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/from-rest-to-graphql-in-minutes-with-prebuilt-connectors"
published_at: "2025-06-13T07:15:23+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:d5062872f9dba7274dec9d9a6e11abea772686395ffe56d7c70a7c5a9943798a"
---

# From REST to GraphQL in minutes with prebuilt Connectors

We’ve all been there: vague API docs, an outdated OpenAPI spec, and a half-buried list of endpoints that leave you guessing. With prebuilt[REST Connectors](https://www.apollographql.com/docs/graphos/connectors) , you skip the guesswork. Just download the schema files, run your graph locally, and start querying live data with GraphQL.


In this guide, we’ll use the[Space Devs Launch Library 2](https://thespacedevs.com/llapi) in the[Connectors Community repo](https://github.com/apollographql/connectors-community/tree/main) as an example to show how easy it is to integrate a REST API into your graph in minutes and how you can tailor it to meet your needs.


## Prerequisites


- Create an[Apollo Studio](https://studio.apollographql.com/) account. This allows you to create and manage your graph, providing the necessary credentials, APOLLO_KEY and APOLLO_GRAPH_REF (more on those later…) which the Apollo Router uses to fetch the supergraph schema and run locally.
- [Install](https://www.apollographql.com/docs/rover/getting-started#installation-methods) and[authenticate](https://www.apollographql.com/docs/rover/configuring) Rover CLI which we will use for configuring our graph and running Apollo Router locally.


## Setup


In your working directory, initialize your schema with:


```text
rover init
```


Select the option to create a new graph.


Then “Start a graph with one or more REST APIs”.


Next you will name your graph and Rover will generate you a new ID for your graph and APOLLO_KEY. Once your APOLLO_KEY and GRAPH_REF are generated,you can start the router with the command provided, but I would recommend putting these values in an VSCode > settings.json. The configuration below will ensure these environment variables are loaded into any new VSCode terminal windows you open:


```text
{


"terminal.integrated.profiles.osx": {


"graphos": {


"path": "zsh",


"args": ["-l"],


"env": {


"APOLLO_KEY": "<YOUR_KEY>",


"APOLLO_GRAPH_REF": "<GRAPH_REF>"


}


}


},


"terminal.integrated.defaultProfile.osx": "graphos"


}
```


Rover generated a supergraph.yaml file for you. You can delete this.


## Using thespacedevs prebuilt connector


Next, navigate to[thespacedevs folder](https://github.com/apollographql/connectors-community/tree/main/connectors/thespacedevs) in the Connectors Community repo. This Connector is an implementation of the[Space Devs Launch Library 2](https://thespacedevs.com/llapi) . Download all of the graphql files here and the supergraph.yaml. You can add these to the root of your project folder. Rover generates test schema files and a supergraph. Anything that is duplicated or you do not need can be removed.


Next, open your terminal and run the following.


```text
rover dev --supergraph-config supergraph.yaml
```


That’s it! Your graph is live at http://localhost:4000. Pop it open in your browser to explore the queries in Apollo Sandbox.


## Modifying a prebuilt connector


You can use a prebuilt Connector as is or modify it for your application. For example, I’d like to add more information about what a valid search string looks like for upcoming launches.


In **launches.graphql** find the upcoming launches query and place this comment above it.


```text
"""
Fetches a list of upcoming launches by agency name.
Try Starlink, Nasa, or Exa for example.
"""
upcomingLaunches(search: String, limit: Int = 5, offset: Int = 0): LaunchConnection


@connect(


source: "llv2"


http: {


GET: "/launches/upcoming/?search={$args.search}&limit={$args.limit}&offset={$args.offset}&format=json"


}
...rest of code
```


Save your file and you will see your new comment as context in the query builder.


## What’s Next?


Once you have the prebuilt Connector ready for your use case now you are ready to integrate it to a client. To continue exploring where to use this Connector, take a look at[this tutorial](https://www.apollographql.com/blog/simplify-your-rest-api-logic-in-react-with-connectors-for-rest-apis-and-graphql) to learn how to replace traditional REST API calls in a React and Next.js application. Or write your very first MCP tool using the[Apollo MCP server](https://www.apollographql.com/docs/apollo-mcp-server) . There’s even a[ready to go example](https://github.com/apollographql/apollo-mcp-server/tree/main/graphql/TheSpaceDevs) using The Space Devs Api for MCP you can use.


There are many prebuilt Connectors ready to use or extend including[Stripe](https://github.com/apollographql/connectors-community/tree/main/connectors/stripe) ,[OpenAI](https://github.com/apollographql/connectors-community/tree/main/connectors/openai) ,[AWS Lambda](https://github.com/apollographql/connectors-community/tree/main/connectors/aws) , and more. Motivated to build your own Connectors to share with the community? We welcome contributions! Learn how to submit your Connector in the[Connectors Community repo](https://github.com/apollographql/connectors-community) .


Written by


Amanda Martin


[Read more by Amanda Martin](https://www.apollographql.com/blog/author/amanda)
