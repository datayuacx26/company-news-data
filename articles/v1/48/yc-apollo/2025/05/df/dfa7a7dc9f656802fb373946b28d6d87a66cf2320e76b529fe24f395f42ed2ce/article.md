---
schema_version: "1.0.0"
document_id: "dfa7a7dc9f656802fb373946b28d6d87a66cf2320e76b529fe24f395f42ed2ce"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/getting-started-with-apollo-mcp-server-for-any-graphql-api"
published_at: "2025-05-15T09:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:25ab62bffb8259f1a54dac8d1585ea7b9b0ba3db255d55d64cb6cf6ec5adb889"
---

# Getting Started with Apollo MCP Server for any GraphQL API

As a developer, you’re likely familiar with the challenges of integrating AI systems into your existing infrastructure. This might have entailed implementing your own custom integration, or using framework like LangChain. These approaches may have worked, but can quickly lead to large maintenance overheads. While we’ve long-held the belief that[AI is best with API Orchestration](https://www.apollographql.com/blog/the-future-of-mcp-is-graphql/) , making it a reality was not practical for most development teams.


Enter the[Model Context Protocol](https://modelcontextprotocol.io/) (MCP) and the[Apollo MCP Server](https://github.com/apollographql/apollo-mcp-server) . With the Apollo MCP Server, we’ve made it easier than ever to bring your data and services together in a secure and scalable way. This guide will walk you through setting up the server, which provides a standardized interface for AI systems like Claude and GPT to interact with your graph-based API orchestration. Whether you’re building a new application or looking to scale existing initiatives, the Apollo MCP Server is designed to simplify the process of integrating the graph API into your AI workflow. In this guide, we’ll cover the basics of setting up the server, including installation, configuration, and best practices for getting started with GraphQL and AI integration.


## Setting up` apollo-mcp-server` from source repository


We have been working on multiple development workflows for the` apollo-mcp-server` and will release future content outlining how to get started without cloning the repository. This blog post will outline how to get started from the[source GitHub repository](https://github.com/apollographql/apollo-mcp-server) .


The` apollo-mcp-server` is written in Rust and in order to build the source code, you must[install Rust](https://www.rust-lang.org/tools/install) . *We also release the server binaries as a docker image that you can use. Most of the content around commands and options for the server in this post are the same for the docker image* .


After you’ve installed Rust, clone and build the repository:


```text
git clone https://github.com/apollographql/apollo-mcp-server
cd apollo-mcp-server
cargo build
```


Building the repository will create the` apollo-mcp-server` binary located at` target/debug/apollo-mcp-server` . You can invoke the binary and see the usage options by running` target/debug/apollo-mcp-server --help` in your terminal window:


## Using The Space Devs example MCP server


The repository contains an example MCP server for[The Space Devs](https://ll.thespacedevs.com/) Launch Library v2 API (API with spaceflight information) with pre-defined tools located in the` graphql/TheSpaceDevs` folder. There is also the standard weather example with` get-alerts` and` get-forecast` tools found in the[MCP server developer quickstart](https://modelcontextprotocol.io/quickstart/server) , but this post will focus on The Space Devs example.


You can use the[MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector) to view and test out tools as you develop. This post will use screenshots from the tool to show the constructed tools in the` apollo-mcp-server` . To start the inspector, run` npx @modelcontextprotocol/inspector` in a new terminal window and open up[http://127.0.0.1:6274](http://127.0.0.1:6274/) to access the tool.


### Running` apollo-mcp-server` with STDIO


Inside the inspector tool, configure the “Transport Type” to be STDIO and set the command to be the full path to your binary. It should contain the absolute path to where you cloned the repository and the` target/build/apollo-mcp-server` binary. For example, running on a Mac, my path is` /Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/target/debug/apollo-mcp-server` .


For the arguments, set the following:


- ` --directory /Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/graphql/TheSpaceDevs` : This is the directory where our MCP server example is. You can think of this as the folder you would be working in while building MCP tools for your GraphQL API.
- ` --schema api.graphql` : By default, pre-defined operation tools have a description generated for them based on the comments in the schema and input argument details.
- ` --operations operations/ExploreCelestialBodies.graphql` : You can provide any number of operations, but to start we’re just going to look at the` ExploreCelestialBodies` tool.
- ` --endpoint https://thespacedevs-production.up.railway.app/`[:](https://thespacedevs-production.up.railway.app/:) You must provide the GraphQL API endpoint for the MCP server to tools to execute operations for. In this example, The Space Devs GraphQL API is built with[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) and exposes the[Launch Library v2 REST API](https://ll.thespacedevs.com/docs) . If you don’t have a GraphQL API, you can get started in minutes using[Apollo Connectors](https://www.apollographql.com/docs/graphos/connectors) and start creating any MCP tool with your API.


The full argument you put in the MCP inspector should be similar to this *(be sure to change your*` --directory` *to the full path you cloned the repo in):*


```text
--directory /Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/graphql/TheSpaceDevs --schema api.graphql --operations operations/ExploreCelestialBodies.graphql --endpoint https://thespacedevs-production.up.railway.app/
```


With the command and argument populated, hit the “Connect” button and you should should see a` ExploreCelestialBodies` tool:


Congratulations! You just got your first GraphQL MCP server running


. Now let’s take a deeper dive at how the tools are being created for the MCP server.


### Pre-defined operation tools


For each` --operation` provided, a tool is generated for the MCP server. If you click on the` ExploreCelestialBodies` tool, you’ll see the tool description with inputs that were defined in the operation. By default, we try generating a description for the tool and inputs based on the return type. This example demonstrates what is generated when the API schema has no documentation provided in it:


Not every GraphQL API has documentation added to them, but you have the ability to add context to your tools in a couple of ways. One way is to add documentation to the schema, or in our case the` graphql/TheSpaceDevs/api.graphql` file. To test this out, find the` celestialBodies` field on the` Query` type and add a description above the defined field like below:


```text
type Query {
...
"Paginate through celestial bodies. Search by name, type, or diameter."
celestialBodies(
search: String, offset: Int = 0, limit: Int = 20): CelestialBodyConnection
...
}
```


Now restart your MCP server in the inspector and list the tools out. You should see the newly added comment in the tool description:


This means you can add additional context to the schema without having to change the actual API giving you the control you need for any schema. There are other times you might want to override the entire description with a custom value for your tool. You can do this by adding a comment to the top of any operation. The` graphql/TheSpaceDevs/operations/SearchUpcomingLaunches.graphql` operation contains an example of this and the easiest way to see this in action is to add this tool to the argument string we defined in the MCP inspector by appending it after the first operation we defined:


```text
--directory /Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/graphql/TheSpaceDevs --schema api.graphql --operations operations/ExploreCelestialBodies.graphql operations/SearchUpcomingLaunches.graphql --endpoint https://thespacedevs-production.up.railway.app/
```


Restart the MCP server and expand the` SearchUpcomingLaunches` tool to see the comment *“Fields searched – launch_designator, launch_service_provider__name, mission__name, name, pad__location__name, pad__name, rocket__configuration__manufacturer__abbrev, rocket__configuration__manufacturer__name, rocket__configuration__name, rocket__spacecraftflight__spacecraft__name. Codes are the best search terms to use. Single words are the next best alternative when you cannot use a code to search”* as the tool description:


But what if you don’t want to have pre-defined operations as tools? Some use cases for AI assistants require a more “curious” LLM that has the ability to explore the graph and execute dynamic operations that are authorized by a trusted user. You may not be able to pre-define all of the tools for your use case which is why we added the` --introspection` argument with some automatic tools.


### Schema aware tools using` --introspection`


Adding the` --introspection` option to our arguments will include two tools: a` schema` and` execute` tool. The` schema` tool can provide the schema to the LLM for the user’s question by adding it to the context window. The introspection mechanism in the MCP Server also allows an LLM to progressively acquire type information as needed with a depth parameter to allow traversing the type tree with fewer or more calls to the tool. This helps shake large schemas to a more concise context window reducing the overall token count.


The` execute` tool is a simple tool that the LLM can use to execute any GraphQL operation against the provided schema.


To see this in action, add` --introspection` to the Arguments defined in the MCP inspector. After restarting the MCP server, you should see the additional tools populate:


### Running` apollo-mcp-server` with Server-Side-Events (SSE)


In addition to STDIO transport,` apollo-mcp-server` also supports Server-Side-Events (SSE). To use SSE, the command and arguments defined in the MCP inspector will need to be executed in a terminal with the` --sse-port` option. This will start up the` apollo-mcp-server` on the desired port that the MCP inspector can be configured for.


Inside the inspector tool, configure the “Transport Type” to be “SSE”. In the terminal that cloned the` apollo-mcp-server` repository, run the command and arguments with` --sse-port 5000` appended on the end.


```text
target/debug/apollo-mcp-server \
--directory <absolute path to this git repo>/graphql/TheSpaceDevs/ \
--schema api.graphql \
--operations operations/ExploreCelestialBodies.graphql operations/SearchUpcomingLaunches.graphql \
--endpoint https://thespacedevs-production.up.railway.app/ \
--introspection \
--sse-port 5000
```


Now disconnect and reconnect in the MCP inspector to see the same list of tools running. We’ve explored the minimum configuration options for the` apollo-mcp-server` to begin building tools for our GraphQL API. There are additional options like support for persisted query manifests that you can explore and we’ll outline those workflows in future posts.


## Using` apollo-mcp-server` in an Assistant UI


Now that we’ve explored how to run the` apollo-mcp-server` and develop MCP tools using the inspector to view them, everything is setup to start bringing our MCP server into an assistant experience. Deciding on whether you use` STDIO` or` SSE` as the transport type will primarily depend on the UI you are planning on using to test the tools.


In this post, we’ll be using Claude Desktop which is easiest to configure using` STDIO` transport. To include our MCP server, we need to modify the` claude_desktop_config.json` to include our command and arguments. Below is an example configuration that loads all of the tools in the repository example *(be sure to change your*` --directory` *to the full path you cloned the repo in):*


```text
{
"mcpServers": {
"thespacedevs": {
"command": "/Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/target/debug/apollo-mcp-server",
"args": [
"--directory",
"/Users/michaelwatson/Documents/GitHub/apollographql/apollo-mcp-server/graphql/TheSpaceDevs",
"--schema",
"api.graphql",
"--operations",
"operations/ExploreCelestialBodies.graphql",
"operations/GetAstronautDetails.graphql",
"operations/SearchUpcomingLaunches.graphql",
"--endpoint",
"https://thespacedevs-production.up.railway.app/",
"--introspection"
]
}
}
}
```


Now you should be able to start the assistant and see the tools available in the UI:


Now asking questions should start to invoke tools based on the LLM and assistant UI you are using. Notice how this flow actually doesn’t find results but Claude-3.7 re-tries the tool with the Vandenburg base because it knows that is the main launch site for California:


## Get started with MCP tools for your GraphQL API today


Implementing tools for your GraphQL API can be as simple as writing GraphQL operations, not writing custom tool code. Graph-based API orchestration means anyone can create an abstraction layer for their APIs that powers these tools with a single operation.


You don’t need to modify your APIs. You don’t need to rewrite your applications. You don’t even need to commit to a full platform shift. You can simply start by mapping your existing services onto a graph, explore how they connect, and build from there.


[Join me](https://www.apollographql.com/events/get-started-with-mcp-tools-for-your-graphql-api-today) for a special Apollo livestream on 5/21/2025 to help you get ready for[Code with Claude](https://www.anthropic.com/news/Introducing-code-with-claude) . In the livestream, I’ll dive into developer workflows like hot-reloading with your MCP server with plenty of time for live Q&A for any MCP questions.


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
