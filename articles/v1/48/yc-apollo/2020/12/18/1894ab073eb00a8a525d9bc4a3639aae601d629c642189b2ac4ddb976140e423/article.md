---
schema_version: "1.0.0"
document_id: "1894ab073eb00a8a525d9bc4a3639aae601d629c642189b2ac4ddb976140e423"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-studio-a-graphql-ide-for-every-environment"
published_at: "2020-12-08T07:04:15+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:cf21298121c517a742e9ba1d6b36044daceb3568fd8e0c0cc06f0caeea72d05a"
---

# Apollo Studio: A GraphQL IDE for every environment

Developers love GraphQL because of its amazing developer experience.


Yet even the best software has rough edges. This year, we embarked on a mission to identify challenges with current GraphQL tools in hopes that we could make an amazing developer experience even better.


We chatted with dozens of developers, and the pain points and workarounds we heard were all too familiar. One web developer with over 20 years of experience avoided rebooting their computer while using GraphiQL so they wouldn’t lose their queries. A technical product manager described searching through their graph with 4,000 types as a scavenger hunt, even if they knew what they were looking for. And engineers of all levels lamented the repetitive, error-prone nature of typing out GraphQL syntax for variables and fragments.


There had to be a better way! So we put our heads together to imagine the GraphQL IDE of the future.


Our first major milestone was releasing the free **[Apollo Studio Explorer](https://www.apollographql.com/blog/introducing-the-apollo-explorer/)** back in June, which only supported registered production graphs. Today, we’re thrilled to announce **development graphs** , which enable you to use all of Apollo Explorer’s powerful features (like intelligent search and variable extraction) with your locally running GraphQL server! 🚀


## Apollo Studio Explorer


[Apollo Studio](http://studio.apollographql.com/?referrer=blog) is our cloud service for developing and delivering a data graph. While Apollo Studio also includes tools for GraphQL CI/CD and observability, we’re going to focus on the **Apollo Studio Explorer** in this post.


The **Apollo Explorer** is a GraphQL IDE in the cloud specifically tailored to GraphQL developers. Connecting your GraphQL IDE to the cloud unlocks a whole new world of possibilities that aren’t achievable with existing open source tools. Worried about losing your query history? Apollo Explorer saves your query history to the cloud automatically, so you can seamlessly pick up where you left off even after you shut down your computer for the day.


Up until now, the Apollo Explorer was only usable with registered graphs in production. With development graphs, you can take advantage of all the cool features Apollo Explorer offers (like extracting variables, one-click query building, and intelligent search) in your local development environment. Just like the rest of Apollo Explorer, development graphs are free to use forever with no limitations, whether you’re just getting started or have a graph with 4,000 types.


## Get started!


Setting up Apollo Explorer with your local graph should be faster than making a pot of coffee. No code changes are required. If it’s not,[let us know](https://github.com/apollographql/apollo-studio-community/issues) where you get stuck so we can help you through it.


First, you’ll run your GraphQL server locally. Development graphs work great with Apollo Server, or any spec-compliant GraphQL server in other languages like Java, Rust, and Python. If you don’t have a graph handy, you can[clone our tutorial graph](https://github.com/apollographql/fullstack-tutorial) and run` cd final/server && npm install && npm start` .


Next, you’ll head to[studio.apollographql.com/dev](http://studio.apollographql.com/dev?referrer=blog) . If you don’t have an Apollo Studio account, you will need to create one by signing up with your GitHub account or email address.


Finally, choose a name for your graph, select the “Development” graph type, add your localhost endpoint (Apollo Server’s default is` http://localhost:4000` ), and click “Create Graph”.


And that’s it! You’re ready to start querying.


When you make changes to your schema, Apollo Explorer will pick them up automatically with no reloading required on your end. The graph you query will always reflect what’s on your local machine.


Since development graphs run locally through introspection, they are completely private to you. Development graphs enable you to build, test, and document your schema changes locally, as well as preview graph changes in local branches and PRs.


## Features to save you time


We think you’re going to love developing your graph locally with Apollo Explorer. Our team has been using Apollo Explorer to build the graph powering our cloud service for a couple months now.


Spoiler alert: the developer experience has been awesome! Apollo Explorer development graphs have given our engineers a major productivity boost, which is something we all need in 2020. 😂 Here are just a few of our team’s favorite time-saving features:


### One-click query building


Typing out GraphQL queries by hand is error-prone, even for GraphQL experts. Not only do missing brackets and incorrect variables present a formidable learning curve for newcomers, they also slow experienced developers down. Apollo Explorer’s no-code query building experience helps you fetch your data faster without the headache of GraphQL syntax errors.


If you need to augment your queries with hand-written code, Apollo Explorer’s editor, based on Monaco, has rich autocomplete and IntelliSense so you can intuitively find the fields you’re looking for.


One-click query building saves you time writing GraphQL queries


### Intelligent search


As your graph grows, search becomes critical for quickly discovering fields and types. To save you the scavenger hunt, we’ve designed an intelligent search experience for navigating graphs of any size, even graphs with thousands of types.


Type ⌘ + K to open the search window, then enter what you’re looking for. Once you find your desired result, one click will show you all possible paths so you can add the field to your query immediately without writing any code.


Intelligent search helps you navigate large graphs


### Extract variables and fragments


Nearly every GraphQL query we write uses variables or fragments. To save time typing out the syntax, we built a neat shortcut that extracts inline variables and fragments.


Extracting variables reduces frustrating syntax errors


## But wait, there’s more!


There are so many other features we built to help save you time, like *inline hints* , *operation history* , *mock responses* , *inlined performance traces* , and *response display modes* , but we’ll leave it to you to try them out and let us know how you like ‘em!


You can also[read the docs on the Explorer](https://www.apollographql.com/docs/studio/explorer?referrer=blog) for a full list of what you can do.


## We want to hear from you


Thanks so much for embarking on this journey with us to build the GraphQL IDE of the future! If you enjoyed this post, we encourage you to[test out Apollo Studio with your local graph](http://studio.apollographql.com/dev?referrer=blog) . Ultimately, we want to make your experience building and querying your graph more productive and enjoyable so you can focus on shipping features.


As you try things out, please know that we deeply value your feedback, and that we read and consider everything you share with us:


- [Please use this form](https://forms.gle/hhfA72JPC3fw43Wx5) to share feedback on Apollo Explorer
- Submit open-ended suggestions and bug reports on our[feedback repository](https://github.com/apollographql/apollo-studio-preview-docs#try-studio-with-the-github-graph) .


We’re excited to hear from you. Happy querying! 🚀


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
