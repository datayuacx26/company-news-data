---
schema_version: "1.0.0"
document_id: "31a86188fceade9492be6454ab437eda90d0dd835835562ea0c5ce546727039e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-summit-2022-recap"
published_at: "2022-10-17T14:03:49+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:4c0e8e29a74344b80adf9745cd65725320c62d6697c29cb440fb162528cb1fb1"
---

# GraphQL Summit 2022 Recap: Highlights, Product Announcements, and Puppies

The Apollo team is still feeling the energy from spending three days in San Diego with over 1000 developers and engineering leaders. And, to the thousands of remote attendees, thank you. We are glad you were able to join in the action via Livestream! Summit participants attended workshops, panels, and sessions on all things GraphQL with a focus on how to successfully build supergraphs.


Monday was about helping teams level up their GraphQL skills with workshops focused on production best practices, schema design, Apollo Federation, and how to champion the supergraph.


GraphQL Summit 2022: Right before Tuesday Keynote


On Tuesday, Matt DeBergalis, CTO at Apollo GraphQL, kicked us off with a keynote announcing Apollo GraphOS and the new features we are launching. (Read on to learn what we launched at GraphQL Summit.) Attendees then split up across the GraphQL Summit pavilion to join compelling sessions from companies like Netflix, Fidelity, Wayfair, and Pinterest. Marc-André Giroux from Netflix’s talk on Building Great GraphQL Developer Experiences @ Scale was packed with insight on how Netflix helps build solutions to empower 100s of developers to collaborate on their supergraph. Attendees also raved about the practical advice for protecting your GraphQL API in Nick Aleks and Dolev Farhi’s talk on Offensive GraphQL Security Testing. Sam Scott from Oso’s talk Authorization Patterns in GraphQL and Ash Narcisse from Apollo’s Root of Auth things talks came in with high remarks from attendees. Tuesday’s closing keynote, How a Supergraph Powers The Front Page of the Internet, was by Reddit’s Savannah Forood and Jameson Williams.


Ananya Roy from Netflix spoke about how Netflix Studios use the supergraph


After a great day of sessions, we all gathered at Stone Brewing for the ultimate Graph & Brew networking event, complete with karaoke.


The scene from Graph & Brew at Stone Brewing


Apollo’s Ben Newman gave Wednesday’s keynote on The Future of Real-time Data with GraphQL, followed by compelling sessions from companies including Stripe, Expedia, Block, and TV2 Denmark. Attendees also called out Max Goodings’ talk, 7 Tips For Writing Better Resolvers, as providing concrete best practices on writing hundreds or thousands of resolvers. Two members of Walmart’s Global Tech team, Aditya Bakle and Ben Wilson, closed GraphQL Summit with the story of how they completely redesigned all of their digital properties using the supergraph in less than a year.


## **Product announcements**


## GraphOS


Tuesday morning, we introduced Apollo GraphOS, a complete cloud platform that lets anyone build, connect, and scale any supergraph. There are three key components of GraphOS:


1. **A global supergraph run-time** that is either hosted by Apollo or Enterprise users can self-host using Apollo Router
2. **A schema delivery pipeline** that includes a schema registry, observability, and schema validation to prevent downtime
3. **A supergraph development kit** a set of tools that give you a complete supergraph development environment on your laptop


GraphOS delivers advanced capabilities like @defer out of the box. @defer lets you mark slow parts of your query and tell the graph it can deliver those parts asynchronously, allowing teams to build more responsive, faster experiences.


Summit attendees could test out GraphOS at the supergraph stations across the venue.


Learn more about GraphOS[here](https://www.apollographql.com/graphos/) .


**Our supergraph development kit**


The supergraph development kit is a set of tools that lets you spin up a complete supergraph development environment on your laptop powered by Apollo Router. New Rover templates in popular programming languages allow new teams to onboard to the graph faster, by providing the skeleton for building out subgraphs.


Learn more about GraphOS and the supergraph development kit[here](https://www.apollographql.com/blog/announcement/platform/introducing-apollo-graphos/) .


**A peek into supergraph linking**


In Tuesday’s keynote, Matt previewed supergraph linking. The supergraph is a powerful way to allow independent teams within an organization to collaborate on a shared API. However, when organizations need to build experiences that require capabilities from other companies, this often results in writing tedious wrapping code around 3rd party APIs. The current approach creates a risk of experiences breaking if any underlying APIs change.


Matt showcased our conceptual approach to linking multiple supergraphs that would allow organizations to share capabilities more efficiently. Martijn Walraven went into more depth during his session, The Future of the Supergraph. He discussed how namespacing, dynamic schema composition, and multi-level routing could be used to build supergraph linking.


**Real-time data proposal**


For monolithic graphs, subscriptions are the most common approach for providing real-time data to GraphQL clients. However, implementing real-time solutions in federated graphs is challenging. As part of Wednesday’s opening keynote, Ben Newman, Apollo Open-Source Architect, proposed a real-time data protocol for federated graphs using @live directive and cursor-based query revalidation and diffing.


Watch the recap on demand and check out the slides from the talk. ( *Interested in helping shape our approach? We’re also looking for feedback.*[Take the survey here](https://bit.ly/real-time-graphql) *.)*


**Thank you to our GraphQL Summit Sponsors**


Our sponsors helped make GraphQL Summit an even more engaging experience with compelling talks and workshops from MongoDB, Grid Dynamics, Xolvio, Oso, and Moon Highway, along with experiences curated by Bethink Labs and Metronome. Visit the link below to hear their talks.


**Puppies at GraphQL Summit**


One last thing, we would be remiss not to post that puppies were in attendance at GraphQL Summit.


**Looking to relive key moments or attend sessions you missed? We’re gonna be posting these sessions over the upcoming weeks. Watch the keynote**[on-demand now.](https://www.apollographql.com/events/virtual-event/opening-keynote-with-matt-debergalis-cto-cofounder/)


Written by


David Isquick


[Read more by David Isquick](https://www.apollographql.com/blog/author/isquick)
