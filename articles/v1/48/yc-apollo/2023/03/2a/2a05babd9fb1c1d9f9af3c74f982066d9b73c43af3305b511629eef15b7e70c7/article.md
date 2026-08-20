---
schema_version: "1.0.0"
document_id: "2a05babd9fb1c1d9f9af3c74f982066d9b73c43af3305b511629eef15b7e70c7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/why-isnt-apollo-explorer-open-source"
published_at: "2023-03-29T10:22:31+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:b5334d7abd8306f89fe66b7459dddad181e5fc1dc7b56a7fc1cd6159d983f556"
---

# Why isn’t Apollo Explorer open source?

A question I get a lot from folks within Apollo and outside the company alike is: Why isn’t[Apollo Explorer](https://www.apollographql.com/docs/graphos/explorer/explorer/) open source?[GraphiQL](https://github.com/graphql/graphiql) is open source and is the tool that helped a lot of us write our very first GraphQL queries. Explorer is like a super-charged GraphiQL, so therefore it should be open source too, no?


In the spirit of open source, I thought I’d write down my philosophy on this topic here in a quick blog post for y’all.


As the creators of Apollo Client, Apollo Server, *many* open source tools for GraphQL, and MeteorJS before that, we know exactly what it looks like and what it takes to do open source well. We have had eras at Apollo where we were really proud of our OSS status and reputation, and eras where we couldn’t keep up maintenance and just became flat out embarrassed at how we let our repositories languish.


When we were a younger company, we tended to open source most things by default. We were so excited about GraphQL that we wanted to help it spread by building as many tools for the ecosystem as we could muster. Along the way, we’ve realized that when something becomes popular, and people start relying on it, it becomes increasingly more work to continue to maintain and evolve. It’s a big responsibility to maintain a popular open-source repository, and we bear this responsibility with heart and pride, but it also takes a lot of work. There were many GraphQL projects that we started but couldn’t keep up with and ultimately decided to give away to other maintainers in the community for that reason.


That said, Apollo has always been and will always be fully committed to the health and betterment of our core open-source projects: Apollo Client, Apollo Server, and many others. To make this commitment, we employ several teams of engineers to work on these projects full-time. We make no direct revenue from this work, but we fully recognize the importance these projects hold to our customers and the GraphQL community which is why we continue to make this commitment. It’s a full-time job for multiple people to maintain a popular repository, let alone continue to innovate within it.


…let’s bring this back to Apollo Explorer now 😃


We set out to build Explorer because we had a vision for how easy we wanted it to be for the world to write GraphQL queries, and we wanted to pursue that vision as quickly and directly as possible. We decided to build Explorer within GraphOS Studio, the UI for our SaaS platform, because this is where we already wrote code day-to-day and we had everything we needed with respect to our design system, build system, deployment pipeline, CI/CD, etc. already set up.


We’ve discussed at many different times what it would look like to extract Explorer’s code into its own package that we could publish on NPM and then use in Studio, and to be completely frank, it’s just too much work. Also, to be even more frank, we really like the flexibility of what a small team can accomplish when all it takes to roll out an update globally is a simple web app deploy, and we aren’t bogged down with version management, backwards compatibility, and publishing to NPM.


That said, we firmly believe that Explorer is the best GraphQL IDE in the world and we want it to be just as accessible to the world as an open-source IDE like GraphiQL. So the route we’ve opted to achieve this is to publish Explorer’s code to a CDN every time we deploy a new version of Studio. This gives us the best of both worlds:


1. It lets you embed Explorer on your website or in your developer portal, so your users can use it to access your GraphQL API.
2. It lets you fully customize the network requests sent from Explorer to your GraphQL endpoint, in case you want to add custom headers or include a Login button to authenticate.
3. It lets us continue to maintain Explorer at the highest level of quality and roll updates out to you all as quickly as possible.


There are several ways to embed Explorer in your website:


1. Set the preference in your[Apollo Server config](https://www.apollographql.com/docs/graphos/explorer/embed-explorer#embedding-on-the-apollo-server-landing-page)
2. Embed[in a React app](https://www.apollographql.com/docs/graphos/explorer/embed-explorer#embedding-on-an-arbitrary-webpage) using our @apollo/explorer package
3. Embed[using vanilla Javascript](https://www.apollographql.com/docs/graphos/explorer/embed-explorer#embedding-on-an-arbitrary-webpage) via our <script> tag


For us, Explorer is a true labor of love and our expression of how we want the world to interact with GraphQL queries. If you ever have suggestions or feature requests for it, please bring them to our[Studio community repo](https://github.com/apollographql/apollo-studio-community) . We read everything that comes in even if we can’t respond or act quickly.


This has been my quick 2 cents. Thanks for reading! Here’s an embedded Explorer with the GraphOS Platform API just for fun:


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
