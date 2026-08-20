---
schema_version: "1.0.0"
document_id: "87e06a83ebdcd1ac831eb63596a764444eb6809d614cb4f109642014766595dc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-the-apollo-explorer"
published_at: "2020-06-30T13:30:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:856e6d30a63c2e3d9c944812ffb1adc8a296561ab07ab034266c805fdb779ca4"
---

# Introducing the Apollo Explorer

Early this year, the Apollo team began development on a new tool for navigating your graph and building GraphQL queries. Unlike many tools that seem more focused on server developers, our goal was to create a powerful, accessible, and intuitive experience for *graph consumers* —the product engineering teams that spend their days working with a production data graph.


Today, we’re beyond proud and excited to reveal the result of that work: the **Apollo Explorer** .


The Explorer is a full-featured query builder and runner that’s part of **Apollo Studio** (which we renamed from Graph Manager[just yesterday](https://www.apollographql.com/blog/graph-manager-is-now-studio) 🙂). This is the first Studio feature that’s first and foremost built for graph consumers. You can use the Explorer with every graph that’s part of your Studio organization, and with every environment those graphs run in (staging, production, and so on). Powerful features like dynamic field selection, intelligent search, and built-in documentation help your team members compose exactly the query they need.


From the outset, we wanted the Explorer to just “feel right” to use. It includes a ton of small design touches that someone might not consciously notice, but together they create a genuinely magical user experience. We’ll cover a few of those touches on our tour.


Let me show you around!


## The operation editor


The Explorer’s operation editor is built on[Monaco](https://microsoft.github.io/monaco-editor/) , so it’ll immediately feel familiar to anyone who’s used VS Code. Naturally, it includes all the features and conventions we’ve come to expect from query-building tools (such as` ⌘+Enter` to run, state persistence across sessions, and panels for providing headers and variables). The editor also provides full IntelliSense support for GraphQL, including query linting, autocomplete, peek definitions on hover, jump-to-definition with a command-click, and more.


And when you’re working with multiple operations, the editor follows along, shifting its focus to whichever operation you’re working on:


We built the operation editor with one goal in mind: zero frustration. After all, we’re GraphQL developers too, and we use the Explorer all the time! When something bugs us about the editor, we fix it. If something bugs *you* , please[let us know](https://docs.google.com/forms/d/e/1FAIpQLSfy9sH9KN3eZZ2Z5yA9XvFq74ITbWOvrFB-iAeMRKf-nWWQeQ/viewform) so we can fix that too.


## Building a query


GraphQL’s syntax is a big part of its learning curve. On top of learning a new mental model for requesting data, you need to learn a new language to actually execute a request! Well, we believe that syntax should *never* get in the way of exploring and querying your graph.


One of the beautiful things about GraphQL is that a query is pretty much just a collection of the schema fields you want to fetch. So hypothetically, if you could **just click the fields in your schema that you want** , you could build a query without even typing. Speaking of which, the Explorer lets you do exactly that:


The Explorer enables you to “step into your schema.” As you step further, it keeps track of your current path. This path helps you navigate back up your schema via breadcrumbs, *and* it tells Explorer exactly how to reach your current location from the root. This means that at any point, you can click the **(+)** button next to a field, and the Explorer knows exactly how to add that field to your query. It also knows exactly which arguments the field requires, and it automatically creates variables so you can specify values for those arguments:


Variables are often scalar types like strings and numbers, but they can be just as complex as any other object. By automatically creating variables for you, the Explorer helps prevent common errors like JSON bracket mismatches.


This query-building flow becomes more and more essential as your schema grows in size and complexity. In a graph that includes unions, interfaces, and a large variety of arguments, being able to abstract away the GraphQL and *just query your data* makes a huge difference. Once you get used to it, you’ll feel like you’re moving in slow motion without it.


## Searching for a field


Have you ever been positive that a field is somewhere in your schema, but you just can’t remember where? And even if you *do* know where, a large schema might include thousands—or even *millions—* of different paths you could take to query that field. Manually identifying the ideal path from a schema’s root down to a field takes a *lot* of mental energy, and it really shouldn’t need to. This is what search algorithms are for!


To help you save that mental energy, the Explorer comes with a two-step, Spotlight-style search. First, it helps you identify which field you’re looking for. Then, it helps you identify which path you want to use to add that field to your query.


### Step 1: Identify a field


It’s very common for multiple fields in a schema to have the same name, like` id` or` status` . This first step helps you identify exactly which type-field pairing you’re looking for.


If you know you’re looking for a field named` email` , the Explorer helps you identify whether you want` User.email` or` Organization.email` . And if you’re less sure about the field’s name, Explorer uses a fuzzy search, so you can get it a little wrong. You can also include a` .` modifier to search both type and field names simultaneously, which can help you narrow in on the perfect match.


### Step 2: Find a path to the field


After you identify which type-field pair you want, the Explorer runs a breadth-first search from your schema’s entry points (` Query` ,` Mutation` and` Subscription` ) to the chosen field. It lists all of the paths it finds, ordered by length. After you select which path you want, the Explorer opens it for you in the Documentation panel, and you can add it to your query with a click.


The Explorer’s search feature removes tons of time and context switching from the query-building experience, especially for larger schemas. Queries that previously took minutes to design now often take just a few seconds.


## Operation history


The Explorer includes a history tab that allows you to view a previously executed query, including all of its variable values:


This comes in handy for all the usual reasons: it’s easy to recover previous work; it helps you keep your workspace clean while retaining operations that took a lot of time and care to craft; and it’s easy to experiment and iterate with different values for variables.


## Light mode / Dark mode


Last but not least, the Explorer comes with both light and dark themes, because what developer tool is complete without both? We even defined a new Apollo brand color named “midnight” specifically for our dark theme. 🌙


## Try it out!


The Explorer is available for **free, unlimited use** for every team using Apollo Studio. If you’re a Studio user, head on over to[studio.apollographql.com/explorer](https://studio.apollographql.com/explorer) . And if you’re new to Studio,[get started here](https://www.apollographql.com/docs/graph-manager/getting-started/) .


You can also try out a demo of Explorer right now at[demo.apollo.dev](https://demo.apollo.dev/) . Just create your Studio account and open up the Explorer tab. You can explore both Apollo’s demo graph (named Acephei) and GitHub’s public GraphQL API. Note that to explore GitHub’s graph, you’ll also need to[set up a personal auth token.](https://github.com/apollographql/apollo-studio-preview-docs#try-studio-with-the-github-graph)


Thanks so much for reading, for exploring, and for continuing on this journey with us. As you try things out, please know that we deeply value your feedback, and that we read and consider everything you share with us:


- [Please use this form](https://forms.gle/hhfA72JPC3fw43Wx5) to share structured feedback on the Explorer, and
- Submit open-ended suggestions and bug reports on our[feedback repository](https://github.com/apollographql/apollo-studio-preview-docs#try-studio-with-the-github-graph) .


As ever, happy querying!


Magic shipping cat


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
