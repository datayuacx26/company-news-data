---
schema_version: "1.0.0"
document_id: "6da7748fd289cc1cfd0af237815de9ef90c6b695e22472502799ecb5c65c879a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/see-you-next-year-graphql-summit-recap"
published_at: "2020-08-14T08:40:35+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:1604e5c54efc2947ae7343975cb43fca6d157c2b6269bb8f684cf242fbac28e0"
---

# See You Next Year, GraphQL Summit [Recap]

Last week, we wrapped up the **best GraphQL Summit Worldwide** yet. Over 6000 developers from around the world tuned in *online* to learn about the exciting things happening in the world of GraphQL.


While we were sad we couldn’t see you in person like last year’s GraphQL Summit, we were blown away by the response we got from attendees.


If you weren’t able to make it out to GraphQL Summit this year, that’s totally cool — we’ve put the entire conference (minus the topic rooms — ya had to be there)[on YouTube for you to watch](https://www.youtube.com/watch?v=ImudUVWINXo&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H) .


In this post, I’ll give you the rundown on GraphQL Summit, the major themes that arose, and how we made this year’s Summit the best one yet.


## Federation adoption is steadily rising


Even though it’s only been less than a year since Federation was announced, we’re already hearing great stories of how engineers at large enterprise companies are championing this approach to simplify serving a GraphQL API at scale.


For the uninitiated, Federation lets teams maintain their own separate graphs, then collaborate by composing those graphs into a **single graph endpoint for the entire organization.** That’s awesome for a lot of reasons, but one of the big ones is that it makes it *much simpler* for UI developers to build apps on top of a single GraphQL API.


The[GraphQL in Production panel](https://www.youtube.com/watch?v=Nh8lPL0pKVM&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=9) from GraphQL Summit Worldwide. From left to right, top to bottom — Matt Derbiglias (Apollo GraphQL), Shruti Kapoor (Paypal), Mike Byrnes (Priceline), Andy Vajda (American Airlines), and Marc-Andre Giroux (GitHub).


### The Federation and GraphQL in Production Panels


At Summit, we heard from engineers from GitHub, Netflix, American Airlines, StockX, Priceline, and PayPal about how they’re using Federation in the real-world.


There’s a lot of valuable knowledge in these panels. One of my favorite quotes from Stephen Spalding (Engineer @ Netflix). He says,


When we talk about scale, we normally think about it in terms of scaling up users, like how many users are binging the latest season of Umbrella Academy. But on the other end, metrics like **the number of developers** and **the complexity of the domain** are important dimensions of scale, and it’s actually these latter dimensions that are addressed by a federated architecture. Integrating the work of hundreds of engineers can be the hard part.


— Stephen Spalding (Engineer @ Netflix)


Check out both panels for advice on things to watch out for as you scale up and how to champion Federation at your company! 💪


- Watch the “[GraphQL in Production Panel](https://www.youtube.com/watch?v=Nh8lPL0pKVM&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=9) “
- Watch the “[Federation Panel](https://www.youtube.com/watch?v=JFARux-92b0&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=3) “


### The Future of Federation: Project Constellation


*Project Constellation* is a project led by[Ashi Krishnan](https://twitter.com/rakshesha) (from Apollo) that aims to make Federation **faster, stronger** , and **more flexible** .


How?


By introducing a **compile step** for Federated graphs.


With this, new directives, and new syntax, we’ll be able to get:


- More explicit control over query planning
- Immediate feedback, type safety, and validation as to if your composition will work by compiling your graph locally,
- and a much faster developer experience, because it’s completely written in Rust!


As we watch Federation , we believe this is something that’s going to be vital in the future.


So get ‘yer shades out. The future for Federation is lookin’ bright 😎.


- Watch “[Project Constellation; the future of Federation](https://www.youtube.com/watch?v=MvHzOwdLb_o&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=8) “.


## A new GraphQL Community Discord


Without a real-life venue to physically meet up with other developers, start hallways conversations, and make new friends, we had to get crafty about how we create a space for community members to have fun and connect with others at Summit.


Taking a page out of the[Party Corgi](https://www.partycorgi.com/) handbook, we invited attendees to hang out, watch the stream, and participate in topic rooms in the[new GraphQL Community Discord](http://go.apollo.dev/discord) .


Check out[this tweet](https://twitter.com/kurtkemple/status/1289219911235989506) that shows you just how PACKED the topic rooms were.


Check out[this Tweet from Kurt](https://twitter.com/kurtkemple/status/1289219911235989506) showing off the topic rooms.


Honestly, big shoutout to Discord for making a great tool that helped make Summit a huge success 🙏.


### Topic room discussions


There were rooms for everything.


From Rust, caching, Android, iOS, and even livestreaming on Twitch, attendees had the opportunity to break their text convos out voice channels so that others could join in on deep conversations about a particular topic.


Two of my favourite topic rooms to be a fly on the wall in were:


1. Michael Waton’s *Schema Design* room, and
2. the *Caching and GraphQL* topic rooms


In Watson’s room, we learned about a nested mutation pattern that seemed to solve a lot of problems developers were running into, and in the caching rooms, some of my favourite moments were spent reflecting on the stellar progress we’ve made as a community improving client-side caching over the last 3-5 years.


## Lessons Learned from GraphQL Developers


The talks were great. Kudos to every speaker. Each talk and panel was jam packed with actionable things you could take and start putting into practice in your GraphQL projects.


- [Kamilah Taylor](https://twitter.com/kamilah) from Gusto taught us how to design for evolvability using a modular iOS application architecture, how to prevent duplicated business rules by encoding them within your GraphQL API, and how to separate the concerns into application and infrastructure layers. Watch “[Lessons learned while going from 0 to 1 with Mobile and GraphQL](https://www.youtube.com/watch?v=NLEk_EOjAn4&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=2) ” here.
- [Mandi Wise](https://twitter.com/mandiwise?lang=en) , who just joined us at Apollo (yay 🎉), gave a talk called “[How to Auth: Secure a GraphQL API with Confidence](https://www.youtube.com/watch?v=dBuU61ABEDs&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=13) “. Nuff said. Check out this talk to see what your options are next time you have to think about how you’re going to secure your GraphQL API.
- [Sasha Solomon](https://twitter.com/sachee) from Twitter taught us how to strictly type errors, handle them as domain concepts, and give them the ♥️ they deserve using the *result* pattern with GraphQL Unions.
- And if you’re just getting started with GraphQL, check out the[Learning GraphQL panel](https://www.youtube.com/watch) to hear from other developers’ paths learning and working with GraphQL, and their favorite resources when they first started out.


## Conclusion


### The best Summit yet


We’re really happy about the way Summit turned out this year, despite the changes. I’m just really proud of my team, all the panelists, speakers, staff, and everyone who played a part in making this the best Summit yet.


### There’s so much more to take in


There were so many more great talks from Summit like:


- Tanmai Gopal (@[tanmaigo](https://twitter.com/tanmaigo) )’s “[Approaches for caching public and private GraphQL APIs](https://www.youtube.com/watch?v=HJPYnUT5unw&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=5) “,
- Kim Crayton (@[kimcrayton1](https://twitter.com/KimCrayton1) )’s talk, “[Profit Without Oppression](https://www.youtube.com/watch?v=XKNPA_8DO_U&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=6) “,
- The exciting work around “[The Future of Custom Scalars](https://www.youtube.com/watch?v=SRGTaYL3h9c&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=7) ” by Andi Marek (@[andimarek](https://twitter.com/andimarek) )
- and the “[Defer and Stream Directives in GraphQL](https://www.youtube.com/watch?v=icv_Pq06aOY&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=11) ” from Liliana Matos (@[lilianammmatos](https://twitter.com/lilianammmatos) ) and Rob Richard (@[rob_richard](https://twitter.com/rob_richard) ) from 1stdibs


Last but certainly not least, watch the[Apollo Keynote](https://www.youtube.com/watch?v=ImudUVWINXo&list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H&index=1) to hear more about the new capabilities of Apollo Studio and Apollo Client 3, each of which were released less then a couple of months ago.


You can also read the blog posts here:


- “[Announcing the Release of Apollo Client 3.0](https://www.apollographql.com/blog/announcing-the-release-of-apollo-client-3-0/) “
- “[Introducing the Apollo Explorer](https://www.apollographql.com/blog/introducing-the-apollo-explorer/) “
- “[Graph Manager is now Studio](https://www.apollographql.com/blog/graph-manager-is-now-studio/) “


### Watch


You can watch[all the talks and panels from GraphQL Summit](https://www.youtube.com/playlist?list=PLpi1lPB6opQxkOv1H0j4JHhHx9bydEk8H) here on YouTube.


### Resources


You can download the slides from GraphQL Summit right here!


- Apollo Studio (Danielle Man):[https://summit.graphql.com/slides/studio.pdf](https://summit.graphql.com/slides/studio.pdf)
- Apollo Client (Khalil Stemmler):[https://summit.graphql.com/slides/client.pdf](https://summit.graphql.com/slides/client.pdf)
- Apollo Federation (Jesse Rosenberger):[https://summit.graphql.com/slides/federation.pdf](https://summit.graphql.com/slides/federation.pdf)
- Lessons learned while going from 0 to 1 with Mobile and GraphQL (Kamilah Taylor):[https://summit.graphql.com/slides/mobile.pdf](https://summit.graphql.com/slides/mobile.pdf)
- An approach to automated caching for public & private GraphQL APIs (Tanmai Gopal):[https://summit.graphql.com/slides/caching.pdf](https://summit.graphql.com/slides/caching.pdf)
- Profit Without Oppression (Kim Crayton):[https://summit.graphql.com/slides/profit.pdf](https://summit.graphql.com/slides/profit.pdf)
- The future of custom Scalars (Andreas Marek):[https://summit.graphql.com/slides/scalars.pdf](https://summit.graphql.com/slides/scalars.pdf)
- Defer and Stream Directives in GraphQL (Rob Richard, Liliana Matos):[https://summit.graphql.com/slides/directives.pdf](https://summit.graphql.com/slides/directives.pdf)
- 200 OK! Error Handling in GraphQL (Sasha Solomon):[https://summit.graphql.com/slides/200ok.pdf](https://summit.graphql.com/slides/200ok.pdf)
- How to Auth: Secure a GraphQL API with Confidence (Mandi Wise):[https://summit.graphql.com/slides/auth.pdf](https://summit.graphql.com/slides/auth.pdf)


Written by


Khalil Stemmler


[Read more by Khalil Stemmler](https://www.apollographql.com/blog/author/khalil)
