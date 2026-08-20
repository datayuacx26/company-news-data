---
schema_version: "1.0.0"
document_id: "7d9997ab6af6e90eb68b540e03eeddbb833ccfc31b3e5be748d693d87dd4563d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-mcp-apps-with-apollo-client-and-apollo-mcp-server"
published_at: "2026-03-02T12:00:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:02e1f2f39a2e4d3e37427ebbbdfe3eaf32b8ea6caeafcf703d626eb248dafb0e"
---

# Building MCP Apps with Apollo Client and Apollo MCP Server

Back in December, I shared a preview of how to[build apps for ChatGPT using Apollo MCP Server and Apollo Client](https://www.apollographql.com/blog/building-apps-for-chatgpt-with-apollo-mcp-server-and-apollo-client) . This post introduced the idea of building apps that live inside the conversation with an LLM. Users can ask follow up questions, trigger actions, and interact with rich UI components powered by your existing APIs.


Since then, the ecosystem has matured quickly. The ChatGPT App Store is open, the MCP Apps specification has launched, and many platforms have added support for MCP Apps. ChatGPT has also introduced a bridge for MCP Apps support. This space is rapidly evolving and there is a lot to keep up with.


Today, we are officially launching our framework for building MCP Apps using Apollo Client and Apollo MCP Server. This launch includes support for both OpenAI and any platform that supports the MCP Apps specification. With this framework, you can build using familiar Apollo Client patterns and serve your app through Apollo MCP Server, without standing up or maintaining dedicated MCP infrastructure.


In this post, we’ll dive into what we’re launching, why it matters, and what it unlocks for Apollo Client teams.


Join us on **March 3 at 9:00 a.m. PT** for a live walkthrough of building and deploying an MCP App with Apollo Client and Apollo MCP Server.


[Register now](https://www.apollographql.com/events/building-mcp-apps-with-apollo-client-and-apollo-mcp-server)


## Why Should You Care About MCP Apps?


AI chat interfaces like ChatGPT and Claude are quickly becoming the default way many users interact with the internet. Conversations in these platforms are now a primary way to discover products, evaluate options, and make purchases.


For companies that rely heavily on traditional search traffic and web-based lead capture, this shift can feel threatening. If more user journeys begin inside AI interfaces, some existing business models will face pressure.


This doesn’t mean SEO is dead. It means distribution is expanding.


In-chat apps represent a new surface area, similar to how mobile apps emerged alongside the web in the early 2010s. Companies didn’t shut down their websites when mobile took off. They added another channel. The same pattern is emerging here.


MCP Apps give you a way to:


- Reach users inside AI-driven interfaces
- Offer structured, interactive experiences instead of plain text responses
- Enable agentic workflows like booking, purchasing, or configuring directly within a conversation


This is not a replacement for your existing channels, it’s an additional channel where users are increasingly spending time. Having an MCP App is quickly becoming as strategically important as having a mobile app was a decade ago.


## Getting Started: Out-of-the-Box Template


You can quickly get started by using our[AI Apps Template](https://github.com/apollographql/ai-apps-template) . This provides a demo React app that includes the scaffolding for the framework, along with the @apollo/client-ai-apps integration library which provides the foundation for developing MCP Apps with Apollo Client and Apollo MCP Server.


The framework **automatically generates** a .application-manifest.json file. Apollo MCP Server utilizes this manifest at startup to automatically generate MCP Tools and Resources for your app.


Serving your new app over MCP is as simple as deploying the build artifacts next to a running Apollo MCP Server. All of the MCP plumbing happens automagically for you behind the scenes.


## Benefits: Self Service and Velocity


This framework allows you to utilize what you already know about building apps with Apollo Client and layer on paradigm-specific knowledge for MCP. It prevents you from having to dive deep into the MCP Apps specification or build and maintain a new MCP Server that effectively serves as a BFF for your MCP App. This greatly increases your velocity, both when getting started and as you develop new features over time.


For frontend teams, there’s no need to stand up an MCP Server or learn the inner workings of MCP. Your GraphQL queries become MCP server tools simply by adding an \`@tool\` directive. This provides a frictionless way to create new MCP server functionality without touching the server itself.


For platform teams, Apollo MCP Server empowers your frontend teams to self-serve. A single MCP Server can serve multiple MCP Apps which helps avoid the “MCP Server Sprawl”. Maintenance remains minimal as your MCP app footprint grows.


The end result is less infrastructure to maintain, fewer cross-team dependencies, and higher velocity by leveraging your existing knowledge and technology investments.


## What’s New in This Launch


A few things have changed since our original preview that are worth highlighting:


- Support for MCP Apps – Most of this is taken care of behind the scenes!
- Easier config using[cosmiconfig](https://github.com/cosmiconfig/cosmiconfig) – App configuration can now be loaded via a config file or an apollo-client-ai-apps property in your package.json
- Platform-specific utilities and conventions – React-native style file conventions allow hot-swapping components for a given platform, which is especially useful for ChatGPT extensions.
- Improved Typescript Configuration – The package now ships with extensible tsconfig to ensure you’re using the right tool for the right platform
- Improved Tree Shaking – Extensive work to re-organize the library to minimize bundle size, shipping only the exact code required for your app for a given platform target


Overall, this is now the build once, deploy anywhere solution that we aimed to release with an exceptional developer experience.


## Summary


With our framework, it’s now easier than ever to build an MCP App using the knowledge and skills you already possess using React and Apollo Client. Instead of focusing on MCP internals, platform specific meta values, or building yet another BFF-like service, you can focus on what matters most: building great experiences for your customers.


Try building an MCP App today with Apollo Client and Apollo MCP Server by going to our[AI Apps Template](https://github.com/apollographql/ai-apps-template) and following the README. We’d love to hear what you think. If you find any issues or bugs, you can open an issue on our[@apollo/client-ai-apps GitHub repo](https://github.com/apollographql/apollo-client-ai-apps) .


Written by


Andrew McGivery


[Read more by Andrew McGivery](https://www.apollographql.com/blog/author/amcgivery)
