---
schema_version: "1.0.0"
document_id: "8d64cebff9306c0bfadda3f82853c96febfd942fd47f48c3a25ce08ca6051781"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-sandbox-an-open-graphql-ide-for-local-development"
published_at: "2021-06-15T09:00:44+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7b5f6ce8cae23111343268fd173a6fa8e6596c0d31ee88f5f87e2b0cb27a739b"
---

# Apollo Sandbox: an open GraphQL IDE for local development

Today, we’re introducing[Apollo Sandbox](https://sandbox.apollo.dev/) – the quickest way to navigate and test your GraphQL endpoints. At the end of 2020, we launched development graphs with the goal of making Apollo tools more accessible and valuable for every GraphQL developer. Sandbox takes us another step closer to that goal by making the Apollo Explorer openly available to any developer, with no login required. We’re excited to give back to the GraphQL community with Sandbox, and we think you’ll love using it. Here are a few reasons why:


## 1. One-click query building in Explorer


Typing out GraphQL queries by hand is time-consuming at best and a frustrating minefield of confusing paths, missed brackets, and incorrect variables at worst. Explorer offers no-code query-building to eliminate syntax errors and help you be more productive. Of course, if you still need to add hand-written code, you can do just that with Explorer’s editor with query linting and autocomplete.


## 2.` ⌘ + K` field and path search


Your graph may be very deep and have thousands of fields and types. You know what data you’re looking for but the more your graph scales, the more difficult it becomes to navigate the graph to get there. Explorer’s` ⌘ + K` intelligent search shows you all of the possible paths to a field or type. Choose the path you want to use, and our one-click query builder will add your query to the sandbox without writing a single line of code.


## 3. Flexible response formatting


Once you’ve built your query, Explorer shows you the response in an interactive JSON or table format with collapsible sections and data sorting by clicking on table headers. You can also copy the response to your clipboard, download it as a CSV, or download the JSON.


## 4. Explore and test your local GraphQL server


Just because you’re working on your schema locally doesn’t mean that you shouldn’t have the best tools for the job. You can point Sandbox at any local GraphQL server and use it just as easily as other tools like GraphiQL or GraphQL Playground with zero setup. If you want to, you can even use Sandbox with production GraphQL APIs, although introspection needs to be enabled, and APIs that enforce CORS will need to[allow our URL](https://www.apollographql.com/docs/studio/explorer/#cors-policies) . That being said, here are a few public endpoints that you can try with Sandbox:


- [sandbox.apollo.dev/?endpoint=https://rickandmortyapi.com/graphql](http://sandbox.apollo.dev/?endpoint=https://rickandmortyapi.com/graphql)
- [sandbox.apollo.dev/?endpoint=https://api.spacex.land/graphql/](http://sandbox.apollo.dev/?endpoint=https://api.spacex.land/graphql/)
- [sandbox.apollo.dev/?endpoint=https://spotify-graphql-server.herokuapp.com/graphql](http://sandbox.apollo.dev/?endpoint=https://spotify-graphql-server.herokuapp.com/graphql)


## 5. No login required


You can use the Schema Reference and Explorer in Sandbox with your GraphQL endpoint just by going to[sandbox.apollo.dev](http://studio.apollographql.com/sandbox) . You’ll never have to create an account or log in, though if you do we can show you your query history and open up some additional customization settings.


## Let us know what you think!


We’re excited for you to get hands-on with Sandbox and start using all of the awesome features we highlighted in this post and others that we didn’t mention yet like like Traces, editor hints with response previews, a` graphql-lodash` integration, variable auto-creation, data mocking, and more! You can also get even more Sandbox features like query history and advanced Explorer settings by logging into Apollo. We deeply value your feedback, so as you try it out, don’t hesitate to let us know what you think. You can leave feedback on Sandbox through[this form](https://forms.gle/qnfPc5n2vhm1JsS58) , and if you have any questions or open-ended thoughts, drop them in our community forum.


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
