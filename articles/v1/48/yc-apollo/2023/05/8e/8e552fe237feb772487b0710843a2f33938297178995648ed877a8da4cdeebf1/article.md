---
schema_version: "1.0.0"
document_id: "8e552fe237feb772487b0710843a2f33938297178995648ed877a8da4cdeebf1"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/community-update-apollo-client-apollo-ios-and-apollo-kotlin"
published_at: "2023-05-23T12:58:59+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:eed02fabde1f98dcab10e5f16a085c210c315b3bbfd3c9918fb81486ae0004e0"
---

# Community Update: Apollo Client, Apollo iOS, and Apollo Kotlin

The client teams at Apollo GraphQL are committed to providing a welcoming, responsive, and helpful environment for GraphQL developers. Whether you’ve been an Apollo power user for years or hours, read on for some news and tips so you can get the most out of our community.


## Where to get the help you need


### The Apollo GraphOS Discord Server (new!)


Head on over to the[Apollo GraphOS Discord server](https://discord.gg/graphos) and say hello! All the Apollo client library maintainers are here, as are a growing number of Apollo developer community members. If you’re already a Discord user you can expect a familiar experience; we have forum channels for` frontend` and` backend` questions plus regular live streams from Apollonauts (our name for Apollo employees). Apollo Client, Apollo iOS, and Apollo Kotlin users have already asked a ton of great questions in the` frontend` channel. For more information about our Discord server, see[this post](https://www.apollographql.com/blog/announcement/join-the-apollo-graphos-community-on-discord-a-hub-of-support-collaboration-and-learning/) !


### Maintainer Office Hours (new!)


One of the biggest reasons we’re excited about the Discord server is our new[Maintainer Office Hours](https://www.apollographql.com/blog/mobile/ios/office-hours-apollo-ios/) series, where our open-source client maintainers will regularly stream to answer community questions live on video. These sessions will be recorded and shared afterwards as well!


### The Apollo GraphQL Forums


In addition to Discord, we also have a community forum powered by Discourse. All the client maintainers monitor this forum directly so whether you’re there or on Discord, expect the same level of helpfulness and support from Apollonauts and community members alike.


## Choosing the right place to post


### Feature requests


For any Apollo Client (web) feature request, please post an Issue in the[Apollo Feature Requests](https://github.com/apollographql/apollo-feature-requests/issues) repo on GitHub. Feature requests for[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin/issues) ,[Apollo iOS](https://github.com/apollographql/apollo-ios/issues) , and other Apollo projects should be opened as an Issue in their respective repos. To avoid duplicates, don’t forget to search existing Issues first!


### Questions, advice, bugs, and how-to


For general questions, advice, how-to questions, or to figure out whether a certain behavior is a bug, consider posting in our[Discord server](https://discord.gg/graphos) (` frontend` channel) or[forum](https://community.apollographql.com/) . For bug reports that are reproducible and actionable, post an Issue in the relevant client library’s repo.


### Roadmap inquiries


[Apollo Client](https://github.com/apollographql/apollo-client/blob/main/ROADMAP.md) ,[Apollo iOS](https://github.com/apollographql/apollo-ios/blob/main/ROADMAP.md) , and[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin/blob/main/ROADMAP.md) each have a` ROADMAP.md` document in their project root. For a high-level overview of what’s coming and approximate dates, please take a look! We’re really excited about the future and would welcome your feedback on our plans. If you think our roadmap should include a feature that you care about, see the “Feature requests” heading above.


### Pull requests


If you’re interested in contributing code to our open-source projects, we’d be eager to review your work! For small documentation adjustments, feel free to open a PR right away. For larger changes, we strongly recommend opening an Issue first with plenty of detail. That will give the maintainers an opportunity to share feedback and guidance before you start implementing. For further guidance, see the` CONTRIBUTING.md` document in the[Apollo Client](https://github.com/apollographql/apollo-client/blob/main/CONTRIBUTING.md) ,[Apollo iOS](https://github.com/apollographql/apollo-ios/blob/main/CONTRIBUTING.md) , or[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin/blob/main/CONTRIBUTING.md) repo.


## How to get the most out of the Apollo community


If you’re in search of answers to questions or think you may have found a bug, we’re here to help! Apollonauts are committed to responding quickly and helpfully to community members and no question is too small or too big. In order to get the most value possible out of our community, here’s some advice.


### First, search


Chances are your question is not unique. Taking a few moments to search the Issues of[Apollo Client](https://github.com/apollographql/apollo-client/issues) ,[Apollo iOS](https://github.com/apollographql/apollo-ios/issues) , or[Apollo Kotlin](https://github.com/apollographql/apollo-kotlin/issues) might pay off. Also try searching the[Apollo GraphQL Forums](https://community.apollographql.com/search?expanded=true) and the[Apollo GraphOS Discord](https://discord.gg/graphos) for context.


### Figure out what you need


Are you looking for open-ended, opinionated advice about a broad concept or topic? Do you need a discrete answer to a specific question? Action on a potential bug? Sharing a feature request? Knowing the desired outcome will help you figure out what a community member or client maintainer might need in order to resolve your inquiry.


### Share as many details as possible


I’d like to share of one of my favorite quotes:


“It depends.” – every software engineer


That’s the answer you’re likely to get unless you share plenty of information. Inquiries that are only a few sentences long without accompanying sample code are almost always too vague to resolve effectively. The gold standard is to prepare a runnable reproduction that demonstrates the problem. A runnable reproduction has two key aspects: (1) it’s a simple application that can be installed and run within a few moments and (2) it clearly demonstrates the problem or question to be resolved. This may seem burdensome at first but it’s the best way to get a helpful response in a relatively short amount of time. The less time community members and maintainers need to spend asking clarifying questions, the more time they have to assist you!


If you’re not sure where to begin, we have a few templates that you can use:


- ` <a href="https://github.com/apollographql/react-apollo-error-template">react-apollo-error-template</a>` (Apollo Client, React): Minimal bug reproduction app with no backend dependencies
- ` <a href="https://github.com/apollographql/rn-apollo-client-testbed">rn-apollo-client-testbed</a>` (Apollo Client, React Native, Expo): Minimal React Native app using Expo with debug and profiling functionality
- ` <a href="https://github.com/apollographql/spotify-showcase">spotify-showcase</a>` (Apollo Client, React, Apollo Server): Full-stack Spotify clone powered by Apollo Client + Apollo Server. Not designed as a bug reproduction template but can be used as such if you want to demonstrate an issue in a fairly complex app
- ` <a href="https://github.com/apollographql/next-apollo-example">next-apollo-example</a>` (Apollo Client, Next.js): Similar to` react-apollo-error-template` but for Next.js reproductions instead of vanilla React
- ` <a href="https://github.com/apollographql/apollo-kotlin-tutorial">apollo-kotlin-tutorial</a>` (Apollo Kotlin): Some Apollo Kotlin community members have cloned and modified our tutorial in order to show us how to reproduce bugs
- [Confetti](https://github.com/joreilly/Confetti) (Apollo Kotlin, Kotlin Multiplatform): This is an open-source KMM app that’s available in the Play Store for Android users and the App Store for iOS users. It’s probably not ideal for bug reproductions but for higher-level discussions about architecture or API design it could be useful. *Note: one of the Confetti maintainers is an Apollonaut but Confetti itself is not affiliated with Apollo GraphQL* .


Apollo Client (web) users can also consider sharing a[Replay.io](http://replay.io/) recording with us. Please make sure to create that recording from a development environment and make sure that it doesn’t contain any sensitive information you might not want to share.


If for some reason you are not able to share something runnable, please do include as many code samples and as much context as possible. It’s not as concrete as a runnable reproduction but can still help reduce the amount of time the community needs in order to fully understand the inquiry to the point where they can provide a helpful response.


### Follow our Code of Conduct


If you haven’t read our[Code of Conduct](https://www.apollographql.com/docs/community/code-of-conduct) lately, please do so! This applies to any space run by Apollo, including our GitHub repositories, the Apollo GraphOS Discord, the Apollo GraphQL Forum. The Code of Conduct reflects our commitment to making the Apollo GraphQL Community a welcoming and safe space in which individuals can interact.


## Let us know what you think


We want to make you successful! If you have any suggestions for our community please do contactcommunity@apollographql.com or reach out to an Apollonaut in one of our various channels. We would be very happy to hear from you!


Written by


Jeff Auriemma


[Read more by Jeff Auriemma](https://www.apollographql.com/blog/author/jeff)
