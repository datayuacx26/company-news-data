---
schema_version: "1.0.0"
document_id: "5c7f56bbbf61363faa904f9e0bc5d3a2bd4c98f874b888b29b035ccc033c45b8"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-client-office-hours-with-alessia-bellisario"
published_at: "2023-11-08T17:46:38+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:1bc6c4325be44873dc6c9d56af38c83f7d94cff01fd842d77b269e3a7fef9f4f"
---

# Apollo Client Office Hours with Alessia Bellisario

Alessia Bellisario and Jeff Auriemma talk about Apollo Client. Originally streamed in[Apollo’s Discord](https://discord.gg/M6FT5cNR5B) on October 6, 2023.


## Transcript


**Jeff:**


Alright. We’re back at office hours after a bit of a hiatus. This is really exciting. Welcome back to everybody who’s joined us before, welcome to anybody who’s joining us for the first time. This is Apollo Maintainer Office Hours, focusing on Apollo Client today, and we’re really excited to talk to you about that. I’m Jeff, Jeff Auriemma. I’m an engineering manager here at Apollo GraphQL serving the Apollo Client, Apollo iOS and Apollo Kotlin maintainers. And I’m joining you out of Connecticut in the USA. Speaking of Apollo Client, I am thrilled to introduce Alessia Bellisario. Alessia, why don’t you tell us a little about yourself too.


**Alessia:**


Hey Jeff and everyone else out there on the internet. My name is Alessia Bellisario. And as Jeff mentioned, I am a maintainer of Apollo Client.I work at Apollo as a staff engineer. And I am coming to you live from New York City right now. A nice, overcast, kind of drizzly day. And I think I’ve just covered the basics. Jeff, should we… where should we go from here?


**Jeff:**


All right. Well, let’s talk Apollo Client, and what is Apollo Client without being able to have folks be able to download it? And I’d love to kind of pick your brain a little bit on something you started working on almost a year ago, I guess, at this point, which was a release process. Can you tell me more about how we release Apollo Client today? Because it’s pretty involved.


**Alessia:**


Sure. Yeah. And this was something that I, you know, it was a task that I gravitated toward after I joined the team, because, right after I joined, I was sort of the second person to join the team or be on the team at that at that point in time. The team had gotten a little bit smaller, and just naturally Apollo Client, the project’s been around for many years, as, as many of you know, so, I worked on a couple of things out of the gate, like defer support, which we’re probably going to chat about later. But, I was just like, okay, I see this opportunity to make releasing the library easier so that when our team does have a couple other folks join us, as was the case in the months after I joined, I now work with the two great colleagues, Jerel and Lenz, and we’re now shipping lots of different bug fixes, features, and just overall improvements. And so I think a lot of engineers know that when you have a larger team, even just coordinating with, like 3 is not a large engineering team, but just coordinating with other engineers and trying to keep up a release cadence, automation is your friend. And so we’re programmers, we want to automate away the tasks that are tedious, that just take up our time and are appropriate for computers to handle for us. So that’s where I just reached for changesets, which is a great library. It’s a CLI, it has its own GitHub action as well that you can pull in. So it has some really great capabilities, and it was the right tool for the job for us and allowed us to not have to spend our time handcrafting release notes, manually tagging releases on NPM and all of that. And so I think it’s helped with just our velocity, making sure that we’re able to keep up a release cadence that is going to bring all the features we’re working on to you as quickly as possible. So you can


use them right away.


**Jeff:**


That’s so cool. I know when the first time I saw it in action, I was mystified. It was just so magical because I remember that process taking a long time and you had to memorize a lot of steps and now we’re able to just hit the merge button in a lot of ways and a lot has gone into that. But every time we do that, it still feels magical to me. And one of the things that folks might have noticed and I’ve noticed this too when we have an open pull request or if we are working on a fix and it hasn’t been merged yet, sometimes we’ll see these NPM commands that have a version number that’s a bit longer. Like right now we just released 3.8.5. But there’s like a 0.0 point something and then a tag. Can you tell us more about what that is and how that’s useful?


**Alessia:**


Sure. Yeah. So that’s called a snapshot release and we’re using changesets to generate those releases. And those are ad hoc. We create them using a command. You can trigger it in many different ways using GitHub actions, but in our case, a maintainer is able to drop a comment on a specific PR that will trigger a snapshot release to be generated. It has a long, you know, the command to install it is a little bit longer because we don’t want any collisions in the version numbers that we use for these snapshots. So it just tells us what PR number and then there’s like a date stamp, essentially a timestamp in there. And that just allows us to very quickly, you know, as we’re triaging issues, if we see a report come in, we can, you know just drop a bug fix of what we imagine would address that issue. And oftentimes if a bug report or issue is opened with a runnable reproduction, creating that snapshot release of what we are pretty sure is the fix for, you know, a user’s problem that they’re encountering, it just gives us that release to be able to say, okay, we saw your code sandbox. We generated this snapshot release. We plugged in this new version, this one-off version with the fix that we thought was the right one here and allows us to confirm and see it for ourselves that yes, indeed, this does address this. We will merge the PR right away and get that out in a stable release. So just helping to close the loop there and runnable reproductions really help with this. All open-source maintainers will tell you that’s the key and that is really the work that comes from, you know, the community. So yeah, that helps.


**Jeff:**


Yeah. Like having that scenario where you can pull it down and see what they see and then be able to just npm install a potential fix that just sounds like common sense. So everybody, if you ever have a question or experience an issue, make sure one key part is being able to reproduce it on our end. Otherwise, it’s really hard otherwise. So cool. As I understand it too, we tag things like there’s the semantic tagging and npm too, right? And I think that lately we’ve been using that. Like I think we have “next” and “latest” and things like that. How should people be tagging their install stuff if they’re


using Apollo Client?


**Alessia:**


Yeah, great. That’s a great question. So “latest” is always going to be, and I’m talking about npm tags here as Jeff said, but “latest” is always going to be the latest stable version of the client. So for example, right now that’s a 3.8.5. And so yeah, npm install @apollo/client@latest that will get you the most recent stable version, and we’re using the npm tag “next” for the latest release in our upcoming minor release. And so in this case, if you install @apollo/client@next, you’ll get the latest alpha release of 3.9, which we’re currently working on. And so those are the two main tags I would recommend that folks track. And if you’re, we have all of the pre-releases as well for the next minor series tagged on GitHub. So if you’re curious about what changes are in a specific alpha version of 3.9, for example, you can find all of that tagged and clearly indicated on in our GitHub releases.


**Jeff:**


That’s awesome all right. And as I understand it now, we’ve invested further in changesets too, right? And our browser extension, Apollo Client DevTools, you recently took that on. Obviously, we’re tagging and everything like that, but that’s a little bit of a different flow, right? Like what are we doing for the DevTools here?


**Alessia:**


Yeah, so the Apollo Client DevTools are a really interesting project that have been around for a while. And our core Apollo Client team of maintainers has sort of taken that on and we’re just modernizing the code base, preparing to add some more features there, which is really exciting. But again, to anyone who’s ever maintained a browser extension, the release processes can be even more arduous and involve a lot more manual work than even just releasing a library on NPM. So, depending on how many extension or add-on stores you want to upload that to, there are nuances for Chrome versus Firefox versus any others that you’re supporting as well. Those are the two that we support in the Apollo Client DevTools. But yeah, with GitHub actions and changesets and some GitHub actions that the community has built for uploading assets to the Chrome extension store and Firefox add-on stores, we’re now able to really increase the cadence of those releases of DevTools as well. So yeah, it’s pretty cool. In like one minute, whenever we want to cut a release of the DevTools, we press the merge button for our changesets PR, GitHub action kicks off and literally it’s like a minute, 30 seconds and it’s done. And then you’ll see it live in the case of the Chrome extension store like 30 minutes later. That used to take, I don’t know, like probably 20 to 30 minutes of clicking around before. So it’s really nice to not have to worry about that.


**Jeff:**


Yeah, I’ve done one of those releases and it’s just, you know, there’s a lot and having had done it in the past with other extensions, it’s so much nicer to be able to do that. And it makes getting, we’ve already, I think, shipped several different iterations of the extension now using that process, which I mean, for a while it wasn’t releasing very frequently. So it’s great to see that, you know, we’re putting more and more stuff into the user’s hands there and I think that’s a big part of the picture, really exciting stuff. And for those of you who haven’t used the DevTools in a while, give it a try. Or if you’re not aware of the Apollo Client DevTools, give it a whirl. It can really make a big difference not only in getting things done, but also understanding, I think, different parts of Apollo Client, like the cache in particular. That’s how I learned about it when I was just an Apollo Client user. Awesome. All right, well, let’s change gears a little bit. Talk less meta about Apollo Client. I want to get my hands into a feature here. You spent a lot of time recently implementing federated subscription support in Apollo Client. And I would love to dive into that a little bit. But maybe first for folks tuning in who might not be aware or might need a refresher, what are GraphQL subscriptions?


**Alessia:**


Yeah, that’s, I think, the best place to start, definitely. So subscriptions in GraphQL, subscriptions are one of the three main operation types. So we have queries, mutations, and subscriptions. And subscriptions are best for having clients subscribe to updates that are pushed from the server, where the server can push data to clients that opt into that by subscribing, right? So not the best to use in every single situation. You might think, “Oh, I should just have my whole app subscribe to all its data.” And as we’ll get into, there are different mechanisms and ways for routers and subgraphs and GraphQL servers to be able to performantly provide that data to clients. But there’s certainly a cost there, right? Subscriptions, we want them to be like really low latency and highly performant. And so, you know, it really makes the most sense to use them in cases where, like, let’s say you want small incremental updates on a large object that your client is needing updates on, right? Or you want real-time updates, like in a chat application or something like that. That should be really low latency. But in other cases, you might be fine just having a query that is polling intermittently, or you’re doing some manual refetching on your query occasionally. So I think that’s just worth calling out that not, you know, it really depends on the UI and your individual application. But certainly those two use cases of incremental updates or something that needs to be real-time low latency, those are perfect use cases for GraphQL subscriptions.


**Jeff:**


That’s awesome. And so, like, you know, subscriptions and now, like, what makes that federated piece special? What makes federated subscriptions kind of different or unique?


**Alessia:**


Yeah, so when Apollo first started looking at, you know, how we were going to offer subscriptions in your Supergraph, in GraphOS, at enterprise scale as, you know, that’s sort of the quality bar and the type of usage that, you know, we need to be able to offer and support. And so federated subscriptions just means that your super graph can serve data from all of its subgraphs. So you’ll have, still the great developer experience of having individual backend teams own their own services, be able to contribute whatever data or fields to that unified API that clients are going to have to access, you know, all the data that they need to serve a single subscription on a client. But that the Apollo router will be able to handle, do query planning, query deduplication, and serve that from multiple subgraphs in a hyper, hyper performant way. So that’s, that’s what federated subscriptions are in a nutshell, I’d say.


**Jeff:**


I know a lot of folks when they think about subscriptions or any kind of real time data, especially when you’re talking about the web platform, a lot of them think to think about like web sockets first. But I understand there’s a new protocol that underlies the client-server or the client-router in this case, communication here. I’m curious if you could tell us a little bit more about it. It seems really cool.


**Alessia:**


Yeah, so that protocol, as Jeff just said, is used for clients to talk to the router. And the protocol is called multipart subscriptions. And it’s, it’s really based on or actually incorporates the incremental delivery over HTTP specification. And it is, it has a little bit on top of that, but I won’t go into like describing the full nitty gritty details of this spec as Apollo as we’ve devised it and as we’re using it, but it is all in our docs. So if you’re curious, you can read all about it there. All about the payload field and how transport errors are handled for anyone who is really curious. But I think the important thing to really note is that it’s using a long lived HTTP connection from the client to the router. And again, the multi-part response format is really similar to and is using the same transport mechanism as we’ve used with features like defer. So we’re using that multi-part over HTTP communication, we’re getting individual chunks of data, individual messages in the case of multi-part subscriptions. Sometimes we’re getting heartbeat messages from the router to keep that connection open. And at the end of the day, it functions very similarly to like web sockets or server sent events. There’s just no like limitation in terms of the number of open connections. And so, yeah, that’s part of the reasoning of why we’re using this protocol.


**Jeff:**


That’s super cool. Yeah, that’s really interesting. Whenever I see a new protocol, I got to learn about it, so thanks. Any special configuration that Apollo client users should know when they’re setting this up or


trying it out?


**Alessia:**


No, the main thing is we introduced multi-part subscription support in Apollo Client in version 3.7.11. And so that’s the minimum version that you’ll need to use. But otherwise, you obviously need to be talking to a backend service, a router instance that can speak multi-part subscriptions. But aside from that, if you’re using Apollo Client’s HTTP link, then there’s nothing else you need to do. You don’t have to set up a sidecar sort of web socket link as you would typically have to do with subscriptions if you’re using


subscriptions over web sockets. So yeah, just use HTTP link and you’re good to go.


**Jeff:**


Nice. And would I just go, if I wanted to use like the router for this, would going to like the Apollo website and signing up there be the place to go to try it out? Like maybe would that be the place to check it out?


**Alessia:**


Yes, definitely. You can try it out by signing up for a free serverless plan and yep. And all of that is on apollographql.com.


**Jeff:**


Cool. Okay. So you mentioned defer and I’d love to switch gears and talk about defer. You’ve spoken about that, you’ve written about it and I’m curious, what is it? What’s defer? Or I should say @defer, the defer directive.


**Alessia:**


Yeah, the defer directive is, it’s near and dear to my heart now because it was really the first feature I worked on after I joined. And it was really interesting. I had to learn a lot about the history of the defer and stream proposal, which is a single proposal to amend the GraphQL spec, which has its own history and so there was a lot to ramp up on and learn in terms of how the GraphQL working group works as well. And all of that was interesting for me, but in terms of what I think is interesting to our audience, what I really love about features like defer, it just speaks to, I think the power of the underlying shared SDL, having a GraphQL shared specification where we have so many different apps, so many different use cases, and in this case, like, I think, defer and stream were originally conceived of and tested out and used at Meta, which they have their own internal implementation of, and so there’s just like learnings that can be contributed back to the shared community where we can say, okay, this is a really interesting idea, we have these kind of proof points of how GraphQL itself can make it easier to deprioritize parts of your query or certain fields, and that’s just such a powerful lever that everyone should have access to that. So, yeah, so you know, it’s been, defer and stream have been in the works for a long time, and it seems like we’re really getting pretty close to them finally being ratified in the spec, in the text of the spec, and that’s really exciting. So, yeah, I hope I answered your question there, but in terms of what defer is for and why I think it’s interesting,


**Jeff:**


Yeah, I don’t think I have an elevator pitch like that, so I appreciate it. And so like when people consider using it, you talked about like subscriptions being a bit like, you know, you want to focus in and make sure it’s matching the use case, like where is defer the most useful, do you think, when folks are looking at their JavaScript apps?


**Alessia:**


Yeah, so I think defer can be really powerful when, you know, like I think you really have to consider your application first, right, because the main challenge that defer helps to solve is the idea that if you have a single query, a large query with like many, many fields on it, that you may have certain parts of that query that are just, you know, the weak link in the chain essentially, like they’re the slowest part of the query, and the thing that is usually GraphQL’s strength, being able to have your UI describe the shape of the data that it needs, and make a single network request, then becomes a liability when you have just one part of that query, one resolver, one, you know, just like one set of fields or field that is just taking longer to resolve. And the idea that like, you shouldn’t have to do a ton of work to split apart your query to jump through all these additional hoops, make a second separate network request, just to be able to show something to the user sooner, because most of the fields would be ready to present if only you could access them, right? So I think that, again, it’s a tool in your toolbox. So should you slap defer on everything? Definitely not. That would cause chaos, for sure, in your app, right? But, like, let’s say you have, you have some, like a UI where you show some comments below the fold or something, you know, definitely not fields that are going to cause a ton of like, layout shift, right? If your if your app is you’re saying like, okay, I’m going to defer all the fields that the user is first going to be presented with, that’s not going to create a pleasant UX. So I think it’s really just another tool in the hands of client developers that can use it to really improve the performance and user experience when it’s wielded in the right way. But it’s just like any other tool, you really have to find those use cases. So again, like the fields that are below the fold, fields that are maybe data that users can kind of double click into, where you they can be lazily loaded, they can be deferred until they’re ready. And you don’t have to write all kinds of spaghetti code to achieve that.


**Jeff:**


Pretty cool. I’m linking also in the chat or Patrick actually just posted a link to the chat to[your talk at GraphQLConf](https://www.youtube.com/watch?v=b0iNvXu5ZlU) , which I think got into a little bit more detail on that, on what what happens under the hood. So further reading for folks who are listening here or viewing it in this case. Awesome. And if I’m the developer using Apollo Client, is it just using, you know, just add the defer directive to the field that I want to see? Is that is that all I need to do as long as I’m on the 3.7 or higher, right? Is that it?


**Alessia:**


Yes, that’s right. That’s the minimum version 3.7 and defer the the only sort of catch there is that it can’t be deferred or applied to individual fields. But those fields just need to be wrapped in a fragment. So a named fragment, an inline fragment, but it has to be a fragment.


**Jeff:**


Got that. Okay, interesting. Any like kind of I know, like defer is its own directive, and it can be like kind of in there. But like, when you’re talking about React, how do we is there any like special patterns that are supported now or maybe in the future that that folks using defer might be able to take advantage of?


**Alessia:**


Yeah, so when it comes to the fact that defer relies on the use of fragments, the line I’m drawing to React here is that we have a useFragment hook, which was experimental in 3.7. And we removed the experimental label in the 3.8 minor version. And that hook is really useful for just listening to changes in Apollo Client’s cache for just a specific fragment. But the reason I mention it is because we are looking at working on a suspenseful version of useFragment in the 3.9 minor release. That would really follow from a lot of the great work that Jerel, my colleague, did to spearhead and bring suspense support in our React hooks to our new set of React hooks to Apollo Client. So, that’s useSuspenseQuery, useBackgroundQuery, and useReadQuery as well. All of this is to say that because defer is really driven by this use of fragments, we see many reasons to really move the ball forward in terms of patterns for and ways to use fragments in Apollo Client, be that the fragment registry, which was also added recently, or nonreactive, which is a fairly new directive that complements useFragment. But all of this is to say that I think there will be some really powerful patterns of deferring fragments that can then be listened to essentially in all these leaf node React components with a suspenseful version of useFragment that can suspend and show fallbacks when those deferred fragments are pending. And that can happen in many places across your app, wherever that fragment is shared. So yeah, I hope that answers your question there, Jeff.


**Jeff:**


Yeah, for sure. And I understand defer, you were mentioning earlier, being in the spec conversations isn’t quite 100% included in the spec, right? It’s still being, I think the final touches are still being put onto it. What are the conversations like in the working group at the moment? Like, what’s the status of defer right now?


**Alessia:**


So there have been some exciting developments even in the last year. And fairly recently, within the last week or two, I want to say, the reference implementation in JavaScript is graphql-js on GitHub, or just on NPM, it’s the graphql package. So that project has been really just so important in demonstrating and as a reference implementation, like allowing the entire ecosystem to test out the sort of algorithms, the response format on the client and the algorithm in a server implementation for how deferred queries should be resolved on the backend, allowing that to be tested out. And so very recently, there was a change to the proposed spec incorporating a lot of, I think, really positive changes to the spec proposal. And so that was shipped in graphql-js’s 17.0 alpha three in like a week or two ago. So the community is now starting to begin adopting what may be the final form of the proposal. I think there’s a decent chance of that. But again, I’m just an observer along for the ride as well. And have been really interested to see all of the changes and improvements that have come from community feedback and testing it out in projects like Apollo Client and so many other projects in the ecosystem. So yeah, just hat tip to all the maintainers out there.


**Jeff:**


That’s awesome. Let’s stick with directives now. You mentioned earlier the nonreactive directive. And as I understand it, that’s not a GraphQL spec directive, but a client side directive that’s in Apollo Client. Can you give us a brief overview? What is the @nonreactive directive?


**Alessia:**


So @nonreactive came out of this idea that if you want to render what is, in this case, the use case we had in mind was rendering a list of items. And there’s a common saying in programming that most apps are just glorified lists. And I’ve built many a glorified list app in my day. It’s just like, you know, you’re just rendering…


**Jeff:**


I haven’t thought about that actually.


**Alessia:**


Yeah. Lists are super common, obviously. Right? We need to show our users collections of things all the time. Right? And so there was this rough edge or a bit of friction that we wanted to address in the sense that, okay, let’s say you have your parent component that’s going to fetch some data with useQuery or useSuspenseQuery. And then you get your list of people or list of users that you map over. And you want those child components, all of those individual user components that you render to only listen to changes for a given user object in the cache. So if I update user one, I really don’t want my parent to re-render. And I certainly don’t want a hundred other sibling components of a hundred other users


to re-render. And with useFragment, part of the story was there, right? Because you could listen to only the changes for user one. But you still had this issue of when user one changed in the cache, that user is part of the query that is going to be updated and trigger that rerender from the parent. And in React, we know the default is that rerenders trigger recursive rerenders throughout your entire React tree. And you have to actively fight against that.


**Jeff:**


Been there, I’ve felt that pain.


**Alessia:**


Yeah, exactly. So that was what we were trying to address with nonreactive. And so you can apply nonreactive to individual fields that you want to mark as fields that shouldn’t trigger a rerender that your useQuery call can essentially know that, okay, if the user’s name or any other fields that you do want to fetch initially, but on, you know, any subsequent update can be just discarded. That you can allow the child components to handle those updates by plugging in and listening just directly to that sub tree that individual user in the cache directly. And so you don’t have to worry about use memo or sorry, React memo, and wrap your components in that because then you would be receiving the updates in the parent, right, Apollo Client would be doing that deep comparison, finding the changes for individual users doing all that work, then attempting to rerender and then React would then have to do that comparison a second time and say, okay, let’s throw away all of the updates to the, you know, user components whose props haven’t changed. So instead of doing like, all of that work to change one user, instead, you can just tell Apollo Client, okay, just do the initial fetch for these fields, and then ignore updates to all of all of these nonreactive fields.


**Jeff:**


That’s awesome. Um, yeah, I was about to ask about about memo, and you answered my question. It’s an interesting thing. And I think Patrick just posted your[blog post](https://www.apollographql.com/blog/apollo-client/introducing-apollo-clients-nonreactive-directive-and-usefragment-hook/) on this that you wrote a few months back on the nonreactive. I think you talked about that distinction between memo. And that really painted the picture for me too, and so that’s rad. Can you use like nonreactive? So like, obviously, React is a big deal. But I’m curious if nonreactive can be used for folks who are writing JavaScript applications, but not using React?


**Alessia:**


Yes. So you can also, you can use nonreactive with the core, like client.watchQuery API. And, so, yeah, I imagine, like in other community-maintained integrations with other view libraries, that there are other API’s in those libraries that are built on top of watchQuery as well. And those would all work with with nonreactive.


**Jeff:**


Yeah, that’s cool. That’s one of the semantic hurdles I had to clear sometimes when I was when I first learned about like reactive variables before I joined Apollo, I’m like, oh, wait, this is actually something that we can use. And it’s just, you know, that “react” word is, has a lot attached to it now. That’s cool. I would love to pick your brain a little bit on testing, unit testing. As I understand it, you’re starting to take a look at how folks are testing, take a fresh look at how folks are testing their Apollo Client apps today. And I know we’re still kind of at the early points in that, but it would be great to get kind of a sneak peek as to like what you’re looking at there and how how we see our opportunities ahead. So if you’ve got anything to share there, it would be great to hear about it.


**Alessia:**


Yes. And I personally love the topic of testing. I don’t really know why I’ve just always kind of geeked out on whatever teams I’ve been a part of on, you know, like, trying to develop, use the best tools and find the best patterns to make it as easy as possible to write tests, because I think that’s one way that a really good developer experience can have an impact on end user experience if, and that’s a topic for another time. But as programmers, we love dev tools. And I love dev tools, obviously. But, but I think testing is so important, because the easier it is to write tests, the easier it is to, and not just any tests, but good tests. I’m definitely a member of the Kent C. Dodds kind of view on your test should be as close to emulating the behavior of your end users as possible, and all that good stuff. So for a very long time, Apollo Client has really focused on or supplied this one main tool called MockedProvider. And MockedProvider has, you know, been the workhorse of Apollo Client testing for many, many years. But there are some areas to add to the testing story. So for those who don’t know MockedProvider, it’s a context provider, and you have to wrap your components that you’re testing in it, and then you provide it, you give it some mocks. And, and those mocks have to match your query and variables perfectly. There can be certain kind of gotchas around, you know, your component updated, but you didn’t update your mocks, and there’s like missing fields and, and so it is just in that sense, it can feel a little bit brittle for certain testing needs or use cases. So what I’m looking at these days is something in the vein of more schema-driven testing approaches. I would really recommend, there was a great talk recently by Stephanie Saunders at Coinbase from GraphQLConf, all about testing and some of the challenges they faced at Coinbase. That was really just right up my alley of things I was thinking about right now. And so, yeah, so I will have more to say, to offer publicly in the way some more specifics here, but we definitely would love to also provide a schema-driven approach for testing where you can set some default values in terms of scalars in your schema, and you can write sort of resolver style mocks that are just a little more dynamic and flexible and also allow you to test the code in your link chain, ’cause that is one huge limitation of MockedProvider, yeah.


**Jeff:**


That’s exciting. Yeah, a lot of times I think, when I think of patterns that folks could leverage more in their apps, the link chain comes to mind and having something- increasing the testing surface area there seems really, really exciting to me personally as an Apollo Client user, so awesome. Well, thanks for the sneak peek there. Really looking forward to hearing more about what you have to say there in the coming weeks and months. And speaking of the coming weeks and months, I understand you’ve been very busy on the conference circuit this season, and I’m curious where folks might be able to catch your next public speaking engagements. Anything in particular you wanted to share about that?


**Alessia:**


Sure, yeah, I will be, next week I’ll be at GraphQL Summit in San Diego, so I’m excited for that one. I’ll be speaking about defer there. And then my colleague, Jerel and I, and Lenz as well, will all be at React Advanced in London. And we’ll be speaking there. Lenz is doing a workshop, I believe. We’ll be around at the conference, so we would love to meet anyone else who will be there. We will also be, Jerel, myself, and Jeff, I believe you’re gonna be attending React Summit US as well since it’s in our neck of the woods. Hopefully I’m not–


**Jeff:**


I’ll be there, yeah. it’s nice to be in driving distance there or train riding distance.


**Alessia:**


Yes, yeah, I’m looking forward to that one in November. And then Jerel and I will also be at the Richmond, Virginia JavaScript one-day conference, which we’re super excited for as well, also in November. But yeah, Jeff, you also have some speaking coming up, right, and when is that? Please remind me.


**Jeff:**


Yeah, I’m giving a lightning talk about GraphQL at Lead Dev West Coast in Oakland, the week of, which is held the 17th and the 18th of October. So looking forward to that. It’ll be my first Lead Dev conference, so pretty excited about it. And it’s nice to talk about GraphQL in that forum. So yeah, but we’re really looking forward to hearing more from you, more and more from you in the coming months and the whole Apollo crew. Awesome, in the meantime, is there any place in particular folks should find you? Is there a social media or anything like that that you wanna tell folks about, or just give a shout on the Apollo Client GitHub? Where’s the best place to find you?


**Alessia:**


Yeah, GitHub is great. I’m not super active on social media these days. Yeah, I have a website, aless.co, it has my email. So feel free to contact me, GitHub, email, whatever works. I’m sort of getting a, whatever, not full snail mail, but I’m sort of taking a step back from the always-on kind of social media. I’ve heard that a lot from people lately. I don’t know, what about you, Jeff? Any changes lately?


**Jeff:**


Oh, lately, not really. I think I had stopped most social media a few years ago, and then started using, I think, well, the site formerly known as Twitter. I think again, once like with a fresh account, like once I knew I’d be working in open source, I just thought, well, I’m used to hearing about open source from here. So I thought I’d start a presence again there, but I’m not super frequent on it, but it is, it’s one thing, I tried Threads too. And I think that that has promise. I don’t know, yeah. It’s always a question, right? You always have to find that line that you draw for yourself. But yeah, also give us a shout on the[Apollo Discord](https://discord.gg/M6FT5cNR5B) . That’s something where we’re always eager to help out and hear what folks have to say about things. All right, well, I think that’ll do it. Anything else for our audience before we say adieu for today?


**Alessia:**


No, just thanks for the conversation, Jeff. And I would, yeah, I’m usually on Discord. So that is also, I forgot to mention that, but thanks for bringing that up. We love to hear from folks on Discord. So yeah, hope to see everyone in person, online, and talk to everyone soon.


**Jeff:**


Awesome, and I feel like I should also mention this, the recording from this will be available on, in many places where you are used to getting podcasts now. There’s an audio version of this. So if you’re on Apple Podcasts or wherever you get your podcasts, is how I usually hear it said on shows nowadays. Go ahead and check it out, the Apollo Client or the Apollo GraphQL Podcast. Thanks everybody, and thanks Patrick for producing.


**Alessia:**


Thanks Patrick. Thanks everyone.


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
