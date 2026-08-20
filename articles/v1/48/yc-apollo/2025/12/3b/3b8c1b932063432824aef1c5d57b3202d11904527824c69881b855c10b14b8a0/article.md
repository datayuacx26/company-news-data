---
schema_version: "1.0.0"
document_id: "3b8c1b932063432824aef1c5d57b3202d11904527824c69881b855c10b14b8a0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-apps-for-chatgpt-with-apollo-mcp-server-and-apollo-client"
published_at: "2025-12-18T17:02:39+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:fa07dd46f0f56beeb3f4b257cb44a60525b50e8b80df5ae0c9625f2e6a02b905"
---

# Building Apps for ChatGPT with Apollo MCP Server and Apollo Client

In early October, OpenAI introduced the ChatGPT Apps SDK with a handful of launch partners. These “apps” could be invoked by ChatGPT via Model Context Protocol (MCP) and would be embedded directly into the ChatGPT interface. In their intro video, they showed use cases like asking for flights, hotel locations, house listings, and more, including teasing the idea of asking follow up questions to ChatGPT.


Imagine your application being part of the conversation, allowing the user to ask follow up questions, and having your app be responsive to those questions. These are action-oriented conversational apps where the LLM can take action on behalf of a user with rich interactive UIs.


This idea quickly caught fire, and now the[MCP apps spec](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/draft/apps.mdx) is proposing a standardized approach to create these kinds of experiences, with other AI vendors sure to follow suit. We’re excited about the potential here, and are working on tools to abstract away all of the AI provider details from you with a great developer experience, allowing you to focus on building your app instead of worrying about vendor idiosyncrasies and evolving standards.


We want this to be a true “build once” solution where you can deploy your app anywhere that supports one of the protocols that we will support. This way, you don’t need to host a separate MCP Server, and build a separate app, for every provider and sub-protocol that emerges.


You can instead focus on your customers.


This post will introduce a tutorial on how to build an app for ChatGPT using[Apollo Client](https://www.apollographql.com/docs/react) and[Apollo MCP Server](https://www.apollographql.com/docs/apollo-mcp-server) . By the end of it, you’ll have what you need to get started with our opinionated stack for building these apps. If you prefer to dive into the template and follow along, you can[find the repo here](https://github.com/apollographql/ai-apps-template) .


## Building a conversational app the “hard way”


Based on the[OpenAI documentation](https://developers.openai.com/apps-sdk/) and examples, to build one of these apps you would:


1. Create an MCP server
2. Setup a resource that returns HTML
3. Create tools which return some _` meta` information and some data via` structuredContent` , pointing at the resource in step 2
4. Repeat step 3 for as many unique “actions” you want in your app


We don’t want you to have to do *any* of this. You shouldn’t have to focus on any of the inner workings of this sub-protocol, or even think about MCP. Your front-end engineers shouldn’t have a new dependency on your platform engineers. Instead, you should be able to focus on building exciting and compelling experiences for your customers.


Instead of this feeling like learning 100% how to build apps from scratch, this should be 90% reusing what you already know about building apps, and 10% learning about the specifics of this new paradigm.


## Building a Conversational App with Apollo MCP Server and Apollo Client


We wanted to make this as easy as possible and decided to reach for tools that are already familiar to many of our users: Apollo MCP Server and Apollo Client. If you want to follow along in this tutorial, you can find all the code in our[OpenAI Apps SDK demo](https://github.com/apollographql/ai-apps-template) .


To build this app we have two components: a React app and Apollo MCP Server. This solution works without any additional MCP Server configuration. The client gets to focus on their application, not the MCP server configuration.


## Basic React Setup


Starting in our` main.tsx` file, we create an` ApolloClient` instance and` ApolloProvider` , very similar to how we would on a traditional React app, but we’re going to import them from` @apollo/client-ai-apps` instead of the normal location. This is an Apollo Client integration package, similar to` @apollo/client-nextjs` . This allows us to do a lot of setup and details behind the scenes of dealing with data being exchanged over MCP.


```text
import { StrictMode } from "react";
import {
ApolloClient,
ApolloProvider,
InMemoryCache,
ToolUseProvider,
type ApplicationManifest,
} from "@apollo/client-ai-apps";
import { createRoot } from "react-dom/client";
import "./index.css";
import App from "./App.tsx";
import manifest from "../.application-manifest.json";
import { MemoryRouter } from "react-router";


const client = new ApolloClient({
manifest: manifest as ApplicationManifest,
});


createRoot(document.getElementById("root")!).render(
<StrictMode>
<ApolloProvider client={client}>
<MemoryRouter>
<ToolUseProvider appName={manifest.name}>
<App />
</ToolUseProvider>
</MemoryRouter>
</ApolloProvider>
</StrictMode>
);
```


Now we can use the normal` useQuery` and` useMutation` hooks as we normally would!


For example, I can write a component that gets my` TOP_PRODUCTS` :


```text
const TOP_PRODUCTS = gql`
query TopProducts @tool(name: "Top Products", description: "Shows the currently highest rated products.") {
topProducts {
id
title
rating
price
thumbnail
}
}
`;


function App(){
const { loading, error, data } = useQuery<{ topProducts: Product[]; }>(TOP_PRODUCTS);


// ... etc ...
}
```


Looks pretty normal, but what is that` @tool` directive on the operation? **That is allowing you to declare in your app a tool that will be exposed to the LLM** , including a name and description. At the same time, we are registering the operation that will be executed when this tool is called (and therefore the data that should be delivered as part of this tool), and the graphql variables become the input schema for the tool!


A mutation works the same way:


```text
const ADD_TO_CART = gql`
mutation AddToCart($productId: ID!, $quantity: Int!)
@tool(name: "Add to Cart", description: "Adds a product to the users shopping cart.") {
addToCart(productId: $productId, quantity: $quantity) {
id
}
}
`;


function ProductDetail() {
const [addToCart, { loading: addingToCart }] = useMutation(ADD_TO_CART);
// ... etc ...
}
```


This is really exciting because we’re able to declare so much with so little code. Also, these are on-the-fly tool declarations. I don’t need a platform team to create these tools for me on an MCP server!


## Tool Routing


Another important aspect of this solution is showing the right component based on what tool was called by the LLM. It turns out we’ve had this problem solved for years now with React Router!


To do this, we provide a` useToolEffect` hook, which works the same way as a` useEffect` , but allows you to run the effect based on which tool was executed.


```text
import { useToolEffect } from "@apollo/client-ai-apps";
import { useNavigate } from "react-router";


const navigate = useNavigate();


useToolEffect("Top Products", () => navigate("/home"), [navigate]);
useToolEffect(["View Cart", "Add to Cart"], () => navigate("/cart"), [navigate]);
```


Using this hook, and a very familiar` navigate` function from` react-router` , I can express that when the “Top Products” tool is called, I should navigate to the` /home` view.


## The Magic Glue: Vite Plugin and Apollo MCP Server


The magic of this solution really comes from a custom Vite plugin called the` ApplicationManifestPlugin` which extracts all the operations, tools, and metadata from your React app and generates a` .application-manifest.json` file:


```text
import { defineConfig } from "vite";
import { ApplicationManifestPlugin } from "@apollo/client-ai-apps/vite";


export default defineConfig({
plugins: [
ApplicationManifestPlugin(),
],
});
```


This plugin runs during` dev` and` build` time and generates a file that looks something like this:


```text
{
"format": "apollo-ai-app-manifest",
"version": "1",
"name": "the-store",
"description": "An online store selling a variety of high quality products across many different categories.",
"operations": [
{
"id": "45766620db4342c46ee9c3eff9c362e58c0790ce8ec16d89458ca7a74088e778",
"name": "AddToCart",
"type": "mutation",
"body": "mutation AddToCart($productId: ID!, $quantity: Int!) {\n  addToCart(productId: $productId, quantity: $quantity) {\n    id\n    __typename\n  }\n}",
"variables": { "productId": "ID", "quantity": "Int" },
"tools": [{ "name": "Add to Cart", "description": "Adds a product to the users shopping cart." }]
},
{
"id": "cd0d52159b9003e791de97c6a76efa03d34fe00cee278d1a3f4bfcec5fb3e1e6",
"name": "TopProducts",
"type": "query",
"body": "query TopProducts {\n  topProducts {\n    id\n    title\n    rating\n    price\n    thumbnail\n    __typename\n  }\n  categories {\n    image\n    name\n    slug\n    __typename\n  }\n}",
"variables": {},
"tools": [{ "name": "Top Products", "description": "Shows the currently highest rated products." }]
},
],
"resource": "http://localhost:5173",
}


```


What you’ll see contained in this manifest is everything that the Apollo MCP Server needs to automatically generate the resource and tools for your app. There’s no need to create or configure any of this on the MCP Server. It will automatically pick it up based on your manifest file.


And that’s it! You can build your React app and declare tools alongside the data declarations and the tooling will do the rest. At the time this post was written, you would then test your app in ChatGPT using developer mode. OpenAI outlines how to try the app in their[documentation](https://developers.openai.com/apps-sdk/deploy/connect-chatgpt) .


## The Future: Build Once, Deploy to Any Agent


With the[MCP apps spec](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/draft/apps.mdx) hot off the press, and likely other providers working on an answer to ChatGPT’s AppsSDK, we have a very important goal: To abstract away all of the provider details from you with a great developer experience, allowing you to focus on building your app instead.


You can instead focus on your customers.


## Summary


This solution is exciting for platform engineers because it doesn’t require them being in the loop and becoming a blocker for the frontend teams they are looking to empower. A single Apollo MCP Server powers apps across providers and accelerates platform engineering. For frontend engineers, this removes the burden of sub-protocol concerns and lets them focus on building high-quality user experiences.


It’s important to note that these apps are still very, very early.


Remember many, many years ago when “apps” first appeared on the iPhone? Many people didn’t understand why they needed a mobile app when they already had a website. It’s kind of funny looking back now because we had no idea what we were even looking at or how much it would change and shape our future.


That’s about where we are now with these conversational chat apps. Customers and companies alike don’t yet “get” these apps. The app store just launched. But once these things fall into place, and we hit an industry mind share, we believe we’re going to see an explosion of conversational, chat-based applications.


Try out building apps for ChatGPT today with Apollo Client and Apollo MCP Server by going to our[Template Repo](https://github.com/apollographql/ai-apps-template) and following the README. We’d love to hear what you think.


Written by


Andrew McGivery


[Read more by Andrew McGivery](https://www.apollographql.com/blog/author/amcgivery)
