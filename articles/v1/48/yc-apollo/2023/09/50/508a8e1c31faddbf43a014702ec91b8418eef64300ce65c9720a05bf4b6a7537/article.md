---
schema_version: "1.0.0"
document_id: "508a8e1c31faddbf43a014702ec91b8418eef64300ce65c9720a05bf4b6a7537"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/office-hours-apollo-server-with-trevor-scheer"
published_at: "2023-09-07T18:00:48+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:f31791b688d1738de5994eabfa20cf15f88e9214d3b9872f457bdb4525c302b9"
---

# Office Hours: Apollo Server with Trevor Scheer

Trevor Scheer (Apollo Server maintainer) joined Jeff Auriemma (Engineering Manager) to discuss all things Apollo Server. Originally streamed live in the[Apollo Discord](https://discord.gg/M6FT5cNR5B) on August 25, 2023. Listen to the[Podcast](https://podcasters.spotify.com/pod/show/apollo-graphql) recording, or read the full transcript below.


### Topics


- Apollo Server 3 to 4 migration
- ` dataSources`
- Pagination
- Plugins
- Federation
- Federated Mutations
- Federated Subscriptions


### Links


- [Apollo Server 4 migration guide](https://www.apollographql.com/docs/apollo-server/migration/)
- [GraphQL pagination guide](https://dev.to/mandiwise/graphql-pagination-primer-offset-vs-cursor-vs-relay-style-pagination-1a60)
- [Creating Apollo Server plugins](https://www.apollographql.com/docs/apollo-server/integrations/plugins)
- [Federated Subscriptions](https://www.apollographql.com/blog/announcement/backend/federated-subscriptions-in-graphos-real-time-data-at-scale/)
- [GraphQL Summit](https://summit.graphql.com/)


## Transcript


Jeff: Hey everybody, welcome back to Office Hours. Office Hours at Apollo GraphQL. I’m Jeff, Jeff Auriemma. I am an engineering manager here serving the. maintainers of Apollo Client, Apollo iOS, and Apollo Kotlin. I live over in Connecticut, and that’s where I am joining you from today with my esteemed colleague, Trevor Scheer. Trevor, you want to tell us a little about yourself as well?


Trevor: Yeah, I’m Trevor Scheer. I’m an engineer, staff engineer. Been at Apollo for nearly five years now. Worked on a handful of projects, most notably Apollo Server and currently Apollo Federation.


Jeff: That is awesome. And so we are doing sort of a special episode of Apollo Office Hours today, talking through Apollo Server. And usually we’re talking about the client, so we’re really excited that you were available to join us, Trevor.


Trevor: Happy to join.


Jeff: The hot topic that I always hear about with Apollo Server is, of course, Apollo Server 4, which I think we’re farther along in that major version now. Last I checked, we’re in 4.9. Is that accurate? Am I getting my numbers right?


Trevor: That sounds right.


Jeff: All right. So Apollo Server 4, though, is still somewhat recent, I think, over the life of the project. We introduced it last year, if I recall correctly. And I was hoping that you could walk the audience through the big changes between Apollo Server 3 and Apollo Server 4.


Trevor: Yeah. As far as user facing goes, a lot of the changes were really internal so that we could deliver on a couple major user facing features, the top one being multi-part responses. So we were tasked with being able to implement` @defer` as part of AS4. And in trying to do so, we were pretty hamstrung by the existing architecture of Apollo Server 3 and decided that it was a good time to implement some previously suggested architecture overhauls that would allow us to do that. So as far as user facing goes, Apollo Server 4 was never meant to be super feature packed, but we did deliver on defer support, which is coming in GraphQL JS 17. It’s usable now in its alpha form, but but right now that’s not the latest release. Another thing that I was excited to ship was the context correctness throughout the internals of Apollo Server 4 and our plugin system. So there was an attempt to push that context type all the way through the Apollo Server internals in v3, but it was pretty riddled with errors. And we were kind of stuck with it because like fixing those errors would have been a breaking change. So Apollo server four kind of gave us the opportunity to fix that for our TypeScript users.


Jeff: Nice. Those are some big changes, obviously. One of the things that I remember reading about too in the release notes was about some of these third party integrations like AWS Lambda, Cloudflare, et cetera. And as I understand it, those integrations were removed from the library. I’m curious as to why that choice was made.


Trevor: Yeah, we had a few reasons for making the decision. Namely, we are GraphQL experts here, and not every other Node.js framework experts. So part of that was to kind of offload the maintenance burden of having to know the intricacies of each of these frameworks ourselves, and kind of lead that up to experts in the ecosystem. So hopefully someone that liked Next enough, as well as Apollo Server, would be willing to maintain one of those integration packages. We also, after the architecture changes, we ended up with a pretty straightforward API for integrating with Apollo Server. It was really entangled before, and so maintaining integrations was a lot more of a pain. So we decided that the level of lift required to maintain and write these integrations was a lot lower than it was before. And so we felt pretty comfortable leaving that up to community members to carry if they felt passionate about it. And if not, someone can come along and write that 100 line wrapper and maintain it themselves if they prefer in their own project.


Jeff: When I hear about those changes, I think, like, simplicity, that’s the word that comes to mind.


Trevor: Yeah.


Jeff: And I know features are often the first things people see, though. They don’t necessarily see simplicity right away. So like, paint a picture for us. How will this architecture benefit Apollo Server users down the road?


Trevor: Sure. I think it makes us… Well, it definitely lightens the maintenance burden for us, so we’re able to focus on the issues of Apollo Server, you know, not the issues of Next and Micro and all these various other things. I think that the outcome has been a success so far. Our previous lead, Glasser, has been able to step away and work on other projects while I maintain Apollo Server at about half time. We don’t have any big yak shaves that we’re looking at getting into a next major version. And this is after about a year. So I think so far, the V4 ship has been a success as far as our expected outcomes and goals were. So maintainability, simplicity. We also have like, we dropped. support for subscriptions- Rather, we dropped the built-in bolted on version of subscriptions and as well as GraphQL Playground in favor of our own. That was mostly to allow users to bring their own subscriptions rather than us be tied to specific versions of things. So removing some dependencies, it, it gives our users a more secure implementation because they’re not tied to those dependencies. That was something that we suffered from in version 3 with uploads and subscriptions, both seeing security issues then caused security issues for our users that we had to either backport fixes for or fork the library for, and that’s just not a good position for us to be in.


Jeff: Sure, yeah, makes sense. A lot of these things come in, like not everybody’s doing real time all the time, right, or these other features, right? So we can, it sounds more modular to me that I’m hearing this.


Trevor: Exactly. And if we do decide to support subscriptions in the future, it’ll be in a way that doesn’t expose our users to that same security problem.


Jeff: And to be clear, it’s still a pretty straightforward implementation to add subscriptions if you’re working on that. Like, folks who were using Apollo Server 4 are still running subscriptions in production.


Trevor: Right.


Jeff: It’s just the pathway there is a little different.


Trevor: Yep, and we provided in the migration guide a way to go about still implementing those.


Jeff: Yeah, let me get that[migration guide](https://www.apollographql.com/docs/apollo-server/migration/) into the chat too, for that, so that folks could follow along in case they’re hanging on to version three or a previous version and want to know what’s ahead here. I’m popping it into Discord and YouTube. If you are tuning in from either of those platforms, please do feel free to drop in something to the chat if there’s a topic that you want to hear covered. We’re here for it. So go ahead and let us know. Yeah, and I think that we can also, if you’re catching up with this later, I think we have feeds now on Spotify, if I’m not mistaken, and a few other platforms. So we’re really excited to be coming at you through those as well. All right. So one other thing that comes to mind about Apollo Server 3 versus 4 is the data sources API, which I understand changed quite a bit. And we’ll talk about the differences between major versions in a minute, First, I’m curious, could you just give listeners an idea of what are data sources?


Trevor: Sure. GraphQL is really just kind of a middle layer to our API. We know that the data is stored somewhere else in anything but the smallest cases. So data sources are a means to access those bits of data from your other services, whether it be a REST API or straight to a database, for example. So you can imagine data source as just a means to communicate to your actual back ends.


Jeff: Right. And it sounds simple, right? At least it sounds simple to me, but I’m curious, what are the big differences between versions 3 and 4 regarding the data sources API, which is a critical layer.


Trevor: Yeah. So fundamentally, there’s no difference in how the data source works or what a data source does. That’s all up to the user to write their data source, consume it how they want within the resolvers. Apollo Server 3 had this little magic constructor option called` dataSources` (camel case). Really all that did was on every request, called your data sources function, which recreated your data sources for that request. And it would initialize them with the context that your context function returned, as well as the cache, the internal Apollo Server cache. In Apollo Server 4, we got rid of that constructor option. There were a couple of reasons for that. And I can elaborate. It’s a little bit wordy, but yeah.


Jeff: I’m here for it. We’re here for the deep cuts.


Trevor: Yeah. One, we had a number of issues that would roll in every so often, same issue every time. Hey, why is my data source being constructed on every request? So a lot of people got the impression that the data sources function was called once on startup, which would knew your data sources, and that on every request it would reuse those data sources. That’s not the case. Because that cache is shared, you don’t want to be reusing those data sources across multiple requests. So eliminating the magic, that hidden layer of what’s actually happening with your data sources under the hood was one motivation for removing that function. The other was that getting the context typing correct in Apollo Server was actually really challenging with that` dataSources` function there. Because the` dataSources` ends up on the context, we have kind of this context creation in two different places, or rather like a context creation and a context update. And so being able to tell Apollo Server and its plugins, what like the type of context was, was actually pretty much impossible because we don’t know what’s going to happen in that` dataSources` function, what’s gonna end up there. And so removing that allowed us to know at the time of writing your code, at compile time, what the type of your context function actually is. Those are my words.


Jeff: Yeah, a lot of words. I love the words, though. Type safety has become such in the front of people’s minds. And we get a lot of questions about that on the client too about type safety and what things are possible and what are not, depending. I mean, runtime is so dynamic and there’s only so much we can do. So yeah, interesting.


Trevor: Yep. And, you know, being able to give people the context correctness in their resolvers, in their plugins and everything was a pretty big motivating factor for us. So getting that right this time was pretty exciting.


Jeff: Hmm. Yeah, well, I’m curious to talk about plugins later, because that is an interesting point there. And I think if we go back in time a bit to what we were talking about earlier, that notion of simplifying the amount of integrations we do, and then also at the same time giving the tools to write better integrations, especially through type safety, it sounds kind of exciting, honestly. Very cool. Nice. Well, as excited as I am about` dataSources` , we get a ton of questions about this, I think, everywhere. Well, it’s a common problem for any application developer, I think. It’s not just a GraphQL thing. But in GraphQL, I think a lot of folks pay attention to this a lot. And there are a lot of different ways to do it. So one of the most frequent ones is concerning pagination.


Trevor: Yeah.


Jeff: If you’re creating a list, right, what options are available for Apollo Server users who want to implement paginated lists?


Trevor: Yeah, we tend to steer people towards common approaches towards solving pagination, both cursor and offset-based. People who are familiar with GraphQL are probably also familiar with Relay, which Apollo Server doesn’t implement for you, but it definitely doesn’t stop you from using that. I think that’s a pretty well-loved approach to pagination in GraphQL. So yeah, those are, I guess the approaches I would look at and consider. And as far as within Apollo Server and GraphQL, if you’re not using Relay, if you’re using either the cursor or offset based approach, those can just be arguments to your resolvers. So often those, well, one or the other is passed along to, say, your REST API. you might pass your` limit=10` or whatever your pagination parameters are down to your REST service or as a part of your SQL query, if you’re querying the database directly. And so I would just pass those in as a field level arguments when I’m solving pagination at GraphQL level.


Jeff: And when we hear those terms like offset, cursor, and Relay, could we go to a little more detail there? When I hear offset, I think like a page number or an offset number and then a limit. Is that what you’re referring to?


Trevor: Yeah. Exactly.


Jeff: Like pages of 20, page 1, page 2, or whatever.


Trevor: Yep, I might say we’ve got 100 items. My offset tells me where I start on that list. So maybe I start on item 50. And then the limit would be how many I’m looking for in that request, so maybe 10. That allows me to determine, well, rather to request for certain sizes of data. Maybe if I’m just going to the next page, I only want 10 or so. Or if I’m prefetching a bunch of data in advance for the user. Maybe I can ask for 100 or some reasonable number of items that have them ready for as soon as they page through.


Jeff: There you go. And that question of what’s a reasonable number, I guess your mileage may vary.


Trevor: Your mileage may vary 100%.


Jeff: Cool. And when it comes to cursor, and I understand that we’ll get to Relay, which I think is a specific type of cursor. But I feel like cursor pagination and therefore Relay is kind of underappreciated, or at least it’s not as widely as understood as I wish. So I’m wondering if you can give us just a quick overview of what a cursor is and why pagination with cursor can be a good choice for developers.


Trevor: I might need, I think you’ve got something in mind here. And.


Jeff: Oh, just a quick overview. That’s all. Yeah, I know this is a bit off script. But when it comes to cursors, I think a lot of folks know that it exists. But I’m wondering what’s the practical impact of choosing that sort of paradigm as opposed to something like an offset. Like, the idea of kind of selecting, to me, when I was doing my first GraphQL backend, I remember implementing cursor in a way. And I enjoyed that, but I had to look it up a lot. And one of the things that I liked about it, though, it seemed like it was more precise in some ways, but it had some foot guns, too. But yeah, anyway. Are there like, do you see folks coming in talking about the tangible benefits of cursor? Or is there, I don’t know, I’m just curious as to what an application developer might consider with that choice.


Trevor: Interestingly, I’m not super exposed to the implementation or the regular application of pagination and these general server topics. As a server engineer, I am surprisingly unexposed to these.


Jeff: It just gives the arguments, right? It will give you the data.


Trevor: Yeah, exactly. So I’m afraid I don’t have a good answer for you there, unfortunately.


Jeff: We’ll find some articles to link people to, and we’ll put those in the show notes, folks. Rad. So speaking of lists, because we’re on the topic, one common complaint about GraphQL regards the N+1 problem. What is that for folks, and how do folks work around that in development?


Trevor: So the N+1 problem, I always get it mixed up. So I usually think of it as the 1+N problem. You usually start with asking for a list of something. But within that list, each of those objects also have something that they might want to query for. So if I have, say, users and their posts. My first query might be for users and each of those users then would want to query for their posts. So here we have the one, which is the users query. I get my list of users. The N is the number of posts queries that I’m about to make for each of those user. N users. We solve that with typically in GraphQL, there are other approaches to solving this problem elsewhere, but we have a data loader, which is a batching solution. So if you pass your data loader around in context and use the data loader to make your requests, you can then rely on the data loader to handle taking say 10 different requests for posts, batching them all together, resolving that one query under the hood, and then responding to each of those 10 queries individually once that one request is resolved.


Jeff: So there you go. Asked and answered.


Trevor: Did I do an okay job of explaining N+1?


Jeff: Oh, you’re the expert here. I’m generally aware of it. There are so many different user landways to do it, and I find it fascinating. So thanks. It’s always great to get perspectives on it. So let’s swing it around. I’d like to go back to the talk about plugins. You had mentioned before in type safety about making an interface for better plugins. And so let’s go back to that. Whenever I see a software with a great plugin API, and a great ecosystem too, because I know there’s a ton of plugins out there for Apollo Server, people just solving little problems, big problems on their own. And it just makes me smile. So what sort of plugins have you seen people create for Apollo Server? And in the course of what you’ve seen, without calling anybody out, of course, what mistakes do people often make? Any advice there?


Trevor: Yeah, I’ve got a few thoughts. One, I haven’t seen too many extremely useful plugin implementations. I would say that they’re generally good for solving problems quickly, working around little issues, maybe some infrastructure quirks. Maybe your requests come in a little funky or you need your response to go out a little funky. You can do that kind of modification on the fly, and get something working. Tracing, logging, observability, all good for those because you have insight into different parts of the request lifecycle, such as, like, GraphQL validation and parsing, maybe more interesting field resolution if we’re talking metrics and timing data. There’s not a whole lot that you can mess up, especially if you’re using TypeScript. Since we’ve got those types nailed down, TypeScript will tell you if you’re doing something wrong. Our plugin authoring story isn’t too complex, but there’s some gotchas. So maybe not gotchas, but some syntax funniness. Like we’ve got a` requestDidStart` , which is one of our hooks, which actually returns an object of hooks. And that might be just a little funny on the eyes. But if you are using TypeScript, it’ll, it’ll help you all the way.


Jeff: Nice. That’s awesome. Do we have like, we have a… I may have to follow up on this afterwards, but I’d love to share a… Oh yeah, here we are in the documentation,[creating Apollo server plugins](https://www.apollographql.com/docs/apollo-server/integrations/plugins) . I just wanted to make sure that folks had that in case they were interested in spinning up their own plugin. They’ve been meaning to solve a specific problem, didn’t know where to start. Well, I guess it’s today.


Trevor: You know, I might share that recently I implemented a plugin for our subscription callback protocol.


Jeff: Oh, nice.


Trevor: We’ll touch on that a bit later, but it turned out to be a pretty fast and easy way to get kind of a test run out the door that people can use today, rather than go and kind of embed everything into the Apollo Server internals just yet.


Jeff: All right. I got kind of a big idea question too that kind of zooms out a bit from Apollo Server. So I hope you won’t mind. And there’s a bit of backstory. I joined Apollo about a year ago. And when I first joined, I was already an Apollo Client user and fan and had dabbled in Apollo Server a bit on some personal projects, but we were a Ruby shop. We did Ruby on Rails. So we were using GraphQL Ruby on our backend. And we were also in the middle at this time of migrating kind of a Ruby on Rails monolith backend to smaller services. And all of our different kind of user facing backends, I guess you could say are sort of, for lack of a better word, backends for frontends, were… sourcing data either from our monolith or later on these smaller services independently. I came to Apollo and I learned that there was so much more we could have been doing. We threw around the term “supergraph” a lot and I’m wondering for folks who are in the shoes I was in not too long ago if you could maybe tell us a bit about what a supergraph is. Yeah, so I’ll give you the mic for that.


Trevor: Yeah, a supergraph is a way of… building out your services in discrete units and kind of merging them up into a single GraphQL graph. We have worked pretty hard on federation for the last four-ish years now. And…


Jeff: Federation being like how you get these together.


Trevor: Yeah. Composition would be the term we use for bringing various smaller subgraphs into a single supergraph. This was previously done most commonly via stitching in the past. And federation was kind of the next-gen stitching, if you will.


Jeff: Yeah, I think I remember reading about stitching. As I understood it, it was kind of a way to concatenate multiple schemas or something like that into something and then try to resolve any conflicts or whatever.


Trevor: Yeah, I think some of the problems that people faced there was in our case, well, I’m just going to call them subgraphs. That’s how we knowingly love them or lovingly know them. Subgraphs had to have knowledge of other subgraphs in order for, for stitching to work. And that was one of the main drawbacks there. Whereas with Federation, you can go and extend types. I might be speaking a little too far on what’s going on here, but you’re able to compose subgraphs together without them necessarily knowing about each other.


Jeff: Pretty cool. I think of that and I think of like, so there’s supergraphs and subgraphs and I think about smaller services, microservices architecture and things like that. So if I’m stitching together what you said, no pun intended, when folks think of a supergraph, they’re thinking a lot of smaller GraphQL servers that are composed together into- So when they are composed together, what is that thing that they are composed to? We call it a supergraph. What is the piece of software they need in order to run something like that?


Trevor: I don’t quite follow the question.


Jeff: Oh, like there’s like a gateway, right? Like a single kind of entry point that you query. So what are people’s options? Yeah, like what kinds of things could they consider installing?


Trevor: What do you do with your Supergraph once you have it?


Jeff: Yeah, what do you do with that thing?


Trevor: Yeah. We have the gateway, which has been the kind of long-standing node implementation for handling your Supergraph, handling federated queries, if you will. And nowadays it’s all about the Rust router. The router takes a Supergraph file-


Jeff: Apollo Router


Trevor: Yep, and it handles query planning exactly the same as Gateway and sends those queries out to your smaller GraphQL services.


Jeff: So you have this supergraph made of subgraphs that get together into a gateway or Apollo router, right? And if you have a bunch of, in hindsight, using that parlance, I see, like, you know, what I had in my last team as being a few different, I think we call them monographs, right, this idea of, like, they’re just standalone services. They stand on their own, a monograph. Plenty of people use Apollo Server every day and just stand alone and or they can use them to federate into a supergraph as I understand it. And I’m curious like does Apollo, I mean can people do both essentially into the future like does Apollo see Apollo Server and as having value both as a monograph server, like standing alone, and as a subgraph server?


Trevor: Yeah, I don’t think that- Apollo Server is certainly never going to stop supporting the single graph use case. You know, Federation is there to solve a problem for when you have it. Otherwise, you know, if you’re not having the scalability problem, then why introduce the complexity? Because there’s certainly an overhead to bringing in Federation and having to manage various subgraphs or another piece of infrastructure and all of that. So until you are feeling the pain of running a single graph, maybe you’ve got a handful of teams now working on the same graph, and that’s turning out to be cumbersome, that might be a good time to consider that. If you know you’re going that direction, you might start out federated. You certainly don’t have to be feeling pain to move on if you know that that’s the end goal anyway. And it might be simpler to do it earlier rather than later. But it’s certainly not a requirement to successfully run a graph.


Jeff: Yeah, like in hindsight, when I look at the team I was on, where we had two different independent services and in the middle of some larger initiatives that were taking shape in our backend, I wish I had known more about this architecture because we were definitely at that point where we were feeling the pain and as the number of services continued to grow, I wish I had known about it. So thanks for diving into that a little bit. And if you find yourself wondering whether that’s the case, it is something you can try out. And so we actually have a- we have a question coming in from Eran, and I hope I’m pronouncing your name correctly. And the question is, can you share how mutations work in a federated architecture? Mutation can only reach one subgraph, correct? How would I implement an operation that involves multiple services?


Trevor: I think, so I will make an assumption. Eran, please feel free to correct my assumption if it’s wrong. So I’m gonna say we’ve got a users posts service and we’re gonna, let’s say we need to make mutations across both of those. You now have a GraphQL server which needs to send a mutation to another GraphQL server. This does turn into a bit of a transaction problem where if your second step of the mutation fails, you wanna be able to roll that back and finally respond to the user with that failure because you might have success at the first level of the mutation, but if you don’t succeed at the second level, then… you need to handle that correctly. So if you do have that entanglement of writing twice or some number of times, it is important to handle those as transactions and carefully. The other thing that Eran might have meant is if you have multiple fields that are doing mutations, Federation handles that fine. Mutations by the spec are handled in sequence. And so whatever your first field of the mutation is will run before the second field. Both of those will run serially.


Jeff: As I understand it, with operations like this, and I’m going pretty far afield from my day-to-day experience, there’s also the query planner as part of Apollo router that can kind of give you, I think, compile time view as to how requests will execute against the various subgraph servers you have. And I think that includes mutations as well, but I’ll dig into the documentation.


Trevor: I recall years ago actually implementing that GraphQL spec, serial nature into the query planner because when we first implemented it, it didn’t respect that part of the spec. So when you send a mutation to the gateway or the router, it does run those top-level fields serially. And that’s part of query planning, yes.


Jeff: That’s really cool. One of my favorite things to dig into is those kinds of like query planning and brings me back to the days of SQL debugging and things like that, using the explain command. And whenever I look at our query planner, I kind of get vibes of that, which is a lot of fun. Keep the questions coming folks. Oh, and I see right above, and I apologize, I didn’t scroll up far enough. Onyishi asks if you can say something on the build subgraph schema. I was having issues with it recently do you have enough detail there? Do we need to get a little more?


Trevor: I don’t. I can share some current top-of-mind thoughts on build subgraph schema, but it would be helpful if you clarified a little bit Onyishi.


Jeff: Okay.


Trevor: Right now, build subgraph schema just accepts an array of type defs and resolvers, which is commonly used for building a schema that a Apollo Server can use. In a number of ways, this is… not quite what people need or it’s insufficient. A lot of people end up having to take that schema that we give them and run additional transformations on it for accommodating custom directives. So there are examples out on the internet of how people are going about doing this and solving that problem. So it can be done in userland. But I think we have room for improvement there. I actually have an open issue pretty recently on the Federation repo to improve the ergonomics there. There’s another equivalent, if you will, called` makeExecutableSchema` from GraphQL Tools, which has been the de facto way of building a schema. And I think we should support basically that API, that same API one-to-one in order to… solve the troubles that users face there. I think that ergonomics is not quite what it could be.


Jeff: Thanks Trevor.


Trevor: Onyishi, if that did answer your question, that’s great. If not, feel free to clarify in the chat and I can talk a little bit more.


Jeff: Yeah, and I’ll use this opportunity to plug our[Discord](https://discord.gg/chKx6T5bty) . There’s a[backend](https://discord.com/channels/1022972389463687228/1040407011734585404) channel, which is a great place to kind of post Q&A particularly if you want to share some technical details. We can always kind of had go ahead and zoom into the problem and try our best either community members or Apollo folks trying to engage with these issues and help you out as you build your app. All right. So I think about all this and federation, supergraph architecture, right? And I wonder, you mentioned subscriptions before, and how does it work with real-time data? It’s one thing to have a persistent connection to one, like subscriptions, for example. So I’m curious what you have to say about that.


Trevor: Yeah, this has been a subscription support at Apollo has been a long time ask. A lot of people want it. And it’s not always the right fit for the job. But for some people, it certainly is. And in the last six months, we’ve finally been able to prioritize it as an actual thing that we support, in the router anyway. This is all with respect to Federation. And Apollo Server supports the router’s form of communicating and handling subscriptions at the federation level now. So you can imagine where the difficulties might lie in real-time data across multiple services and managing that. You’ve got open connections with clients. The router itself becomes a client to the subgraphs, where it’s sending requests and expecting updates as they come in. So the router becomes a bit more stateful than it usually needs to be. It has to maintain those connections with subgraphs and expect updates coming in, being pushed to it. This is the callback protocol that we’ve implemented recently. So yeah, lots of challenges there for sure. I think we’ve got it all mostly working, so that’s pretty cool.


Jeff: That is rad.


Trevor: Yeah.


Jeff: Yeah, seeing some of that being lifted from afar was pretty inspiring. Yeah, linking in the chats to a. to a bit of an overview on how you can use subscriptions and federation, which was a pretty intractable problem for a while, but it’s pretty cool to see an innovative solution. And that’s also, if I’m not mistaken, from the client side at least, because our team’s built out the client side part of this, it’s not using WebSockets, curiously, it’s using HTTP. So I always thought- or multipart, a novel protocol over HTTP, which is pretty rad.


Trevor: Yeah, I haven’t been up to speed on those changes, but it’s cool to see because I know a lot of people have concerns with WebSockets and their performance, especially when it comes to mobile devices and battery life. And yeah, it’s nice to see that client teams have been able to do that with just HTTP.


Jeff: Yeah, yeah. Well, it’s definitely client and router, both.


Trevor: Yeah, fair enough.


Jeff: They both got to speak the same language. But yeah, it was still a lot of fun and interesting way to go. But anyway, pretty cool. So Trevor, where can people find you on the internet? Where do you prefer? People want to talk Apollo Server. Should they just at you on GitHub? What’s the deal?


Trevor: Add me on GitHub. I follow my GitHub notifications pretty religiously. I have too many projects, too many little projects to keep track of. So you can definitely get me on there. I’ve got my handle in my name. I don’t know if that’s on the stream. I’ll put it in the chat.


Jeff: Yeah, yeah, it’s` @trevor-scheer` , right?


Trevor: Yeah, so you can get me there. I am on and off on our Discord. Sometimes I’ll catch a question and respond to it right away. And if I don’t, then I usually end up forgoing it, missing it altogether. You can try @ing me there and see if you get a response. Otherwise, maybe someone who watches it more closely will let me know.


Jeff: A lot of helpful folks on there.


Trevor: Yeah, open an issue on Apollo Server or Federation. I’ll see it.


Jeff: All right. Well, and as I understand it, you’re going to be hanging out in, I think we’re calling it the Apollo Garage at Summit, right?


Trevor: I will be at Summit in the garage.


Jeff: So, for those of you who aren’t aware, GraphQL Summit is coming in October, October 10 through 12 in San Diego. So if you, if you like GraphQL, it’s a great place to be. I’m going to link it in both chats as well. So come say hey to a bunch of folks around the GraphQL ecosystem there. It’s going to be a great time. I’m on GitHub` @bigNimbus` and on Discord` @jeff.auriemma` and Twitter` @JeffAuriemma` without the dot. So if you ever want to talk clients or anything like that, let’s just let’s chat. Anything else? I think we’re popping to the end here.


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
