---
schema_version: "1.0.0"
document_id: "a1b03e5c825b96fdb6b3e087b17938675f73eac721ead80ec3c5292f22759111"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/10-years-of-graphql-celebrated-at-graphqlconf-2025"
published_at: "2025-10-02T13:13:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:fe7b20a16e5038beb006166ac0875e216d4fb96d75f0389e64252589a7ded4d4"
---

# 10 Years of GraphQL Celebrated at GraphQLConf 2025

GraphQLConf 2025 is a wrap! The Apollo team had a great time in Amsterdam celebrating a major milestone: ten years since GraphQL was open-sourced by Meta. What started as an internal tool for building mobile applications has evolved into the foundation for how thousands of companies orchestrate their APIs – and now, as the conference made clear, GraphQL is entering its most important era yet.


As we head into[GraphQL Summit](http://www.graphqlsummit.com/) next week, I wanted to share some reflections on what we saw in Amsterdam. The conference wasn’t just a look back at GraphQL’s first decade – it was a clear-eyed look forward at how the language that revolutionized API development is now at the center of how teams are building the next generation of AI-powered applications. Here’s what stood out.


## GraphQL still cooks


GraphQL was open-sourced ten years ago and since then, its spot on the Gartner Hype Cycle curve is past its initial spike. The creative energy in the movement, however, remains as strong as ever.


Matt Mahoney taught us about the new` @async` directive being prototyped at Meta. I’m really interested in how this progresses. As mobile and web GraphQL clients embrace fragment colocation, the utility of` @async` becomes apparent to me.


But innovation isn’t just limited to new directives; our own Derek Kuc (with support from Duckki Oe) showed how efficient semantic query comparison can make GraphQL query plan validation more correct and efficient:


The ecosystem continues to innovate as well. Benjamin Rabier showcased a novel approach to improving query planning in Grafbase’s federation gateway. The newest version of GraphQL Code Generator was put on display by Eddy Nguyen at the Guild, along with some recommendations on how to build an ideal workflow for GraphQL server development. And Matteo Collina’s (Platformatic) talk on stateless subscriptions was thought-provoking and relevant as more applications serve real-time data to end users.


## AI is better with GraphQL


Lingering over the conference was a prescient keynote by our CEO Matt DeBergalis that challenged the GraphQL community to make the language shine in the AI era. Bringing optimism and urgency in equal measure, Matt challenged the audience to imagine what it will take for GraphQL to be as transformative for agentic development as it is for traditional application development.


The keynote stage on Day One showcased the current state and future aspirations for GraphQL in the AI age. Sarah Sanders (Docker) walked attendees through the “Ask AI → Generate code → Iterate → Ship” loop that developers are settling into and what it means for GraphQL practitioners.


Our own Michael Watson joined Kewei Qu (Meta), Stephen Spalding (Netflix), Fredrik Björk (Grafbase), and Boris Bessemer (Vercel) on stage to discuss AI for APIs. Some “light bulb” moments definitely resulted:


MCP was also a hot topic throughout the conference. Michael also presented on schema design patterns for rapidly bringing REST APIs into your GraphQL schema – a critical capability as teams race to make their existing infrastructure AI-accessible in the new world of MCP:


Erik Wrede (Strawberry GraphQL) and Thore Koritzius rounded out the MCP programming with talks exploring how the protocol is evolving.


## Agentic, web, and mobile developers have new tools and architectures to work with


MCP remains a compelling protocol for GraphQL users, and Apollo is innovating heavily in this space with Apollo MCP Server. At our[post-conference MCP Server Builder’s meetup](https://luma.com/mcp-server) , we hosted speakers from Jetbrains, Grafbase, Block, Platformattic, and Apollo to dive deeper into practical MCP implementations.


Agentic development doesn’t exist in a vacuum, though, and tooling for traditional UIs is more robust than ever. It’s the golden age for GraphQL fragments. Mobile and web clients showcased evolving and progressive capabilities for fragments and fragment colocation to create better and more scalable applications. There was widespread agreement that colocating fragments is helpful for application developers and could result in more efficient workflows in an AI-assisted coding environment.


Lenz Weber-Tronic unpacked all the innovation that has gone into[Apollo Client 4.0](https://www.apollographql.com/blog/announcing-apollo-client-4-0) .


We learned how Relay is being used at both Meta and Pinterest, and Robert Balicki’s theatrical talk on Isograph underlined new capabilities of this exciting web development framework. Alec Aivazis showcased Houdini, a compelling and easy-to-use tool for full-stack GraphQL applications. And Jason Kuhrt unveiled Graffle, the evolution of the popular, minimalist graphql-request client.


Mobile development was the original use case for GraphQL, and the conference program didn’t disappoint on that front. Martin Bonnin’s talk on Apollo Kotlin Compose explored alternative ways to build Android apps, bridging web and mobile development and offering a simpler alternative to the MVVM architecture:


And Benoit Lubek showcased the improved new caching capabilities being added to Apollo Kotlin and Apollo iOS:


## GraphQL needs new capabilities


Lee Byron heralded the release of the September 2025 version of GraphQL. This version of the spec contains a lot of small-but-important changes and also introduces input polymorphism in the form of the` @oneOf` directive.


As developer and user needs change though, there was a broad undercurrent that GraphQL should evolve more rapidly. On Day One, members of the Technical Steering Committee speculated on the future direction of the language in a panel that discussed nullability, generics, error handling, incremental delivery, and more.


Martijn Walraven’s talk on GraphQL namespacing was an indication that the time has come for this long-discussed capability to move into users’ hands.


Benjie Gillam’s and Itamar Kestenbaum’s talks on nullability and Rob Richard’s deep dive on` @defer` and` @stream` picked up where the panel left off.


One perspective I hadn’t considered before was put forth by[Vanessa Johnson at The New York Times](http://youtube.com/watch?time_continue=8&v=ttmp_zkHH_0&embeds_referring_euri=https%3A%2F%2Fgraphql.org%2F&source_ve_path=MTM5MTE3LDI4NjY2) . The way that she proposed using directives to express accessibility metadata in GraphQL is fascinating and could be the basis for some common tooling or conventions.


## Federation has graduated from innovation to infrastructure


GraphQL Federation has reached an important milestone: what once was a provocative and novel idea has become so widely adopted, understood, and implemented that it’s talked about as casually as queries and mutations. This is exactly what should happen to successful architectural patterns – they become foundational.


On the implementation and specification side, Martijn Walraven joined ChilliCream’s Michael Staib on the state of GraphQL Federation and the nascent Composite Schemas specification.


The Guild’s Kamil Kisiela and Dotan Simha, along with Samuel Bernardo Vázquez Andalón, shared their implementation work across different technology stacks. Tom Houlé shared insights on federated subscriptions and authentication.


We also heard some interesting perspectives from folks deploying federation at scale. Mansi Mittal and her colleagues from Booking.com shared how they’re securing a federated graph handling 11 billion requests daily:


Benjamin Rabier, Spencer Kwok from LinkedIn, and Mark Larah from Yelp also shared stories on how they’re using GraphQL to orchestrate their APIs. Juan Carlos Blanco Delgado from RS Group demonstrated sophisticated event-driven caching patterns:


## The people are the best 🙂


A memorable highlight was seeing the energy that the community has been channeling into the GraphQL movement. Benjie Gillam and Jem Gillam from Graphile showed the impact of the GraphQL Locals meetup program, unveiled this year’s GraphQL Stars, and introduced the GraphQL Ambassadors program and its first cohort. Uri Goldschtein from The Guild called out the GraphQL Community Working Group, a new way for the GraphQL community to coordinate on advocacy, content, and partnerships.


I also had the privilege of giving the closing keynote about the GraphQL Foundation – an overview of how GraphQL’s governance works and a call to action for the community to get more involved:


Offstage, the hallway track could not have been more interesting. The energy and enthusiasm was palpable throughout the Pakhuis De Zwijger – our GraphQL all-hands meeting on Day 2, impromptu lunch outings, pair sessions, and even chess games were breaking out everywhere!


GraphQL didn’t stay confined in the venue’s walls, either. The GraphQL Stream Team raised money for ALS in the Amsterdam City Swim, where team member Emily Ruf had an impromptu encounter with Her Majesty Queen Máxima. The day before Conf, An Ngo and others coordinated an Amsterdam canal cruise for attendees.


## From Amsterdam to San Francisco: What’s next at GraphQL Summit


The themes that emerged in Amsterdam – GraphQL’s evolution for AI workloads, the maturation of federation as core infrastructure, and the continued innovation in client tooling – are exactly what we’ll be exploring at Apollo’s GraphQL Summit next week in San Francisco.


If you missed GraphQLConf this year,[Summit](http://www.graphqlsummit.com/) is your chance to see how these trends are playing out in production environments. We’ll have 600+ attendees and dozens of sessions showcasing how GraphQL and AI are being used at scale across multiple industries and use cases. We’re particularly excited to share what we’ve been building to make GraphQL the orchestration layer for agentic applications – the natural evolution of everything Matt challenged us to think about in his Amsterdam keynote.


Passes are selling quickly – we hope to see you in San Francisco, October 6-8!


Written by


Jeff Auriemma


[Read more by Jeff Auriemma](https://www.apollographql.com/blog/author/jeff)
