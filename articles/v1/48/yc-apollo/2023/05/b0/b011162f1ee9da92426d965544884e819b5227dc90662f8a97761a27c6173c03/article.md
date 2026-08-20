---
schema_version: "1.0.0"
document_id: "b011162f1ee9da92426d965544884e819b5227dc90662f8a97761a27c6173c03"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/office-hours-apollo-ios"
published_at: "2023-05-05T13:40:48+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:00d69a1eb74a7941985596033b1f6200b790ab9600b4939086329a797dcf16cd"
---

# Office Hours: Apollo iOS

We recently kicked off a new live series in Discord: office hours! Here, the maintainers of Apollo projects will answer the community’s most burning questions.


We have a recording of the first stream below, where Jeff Auriemma and Anthony Miller discuss the hottest questions about Apollo iOS.[Our next stream](https://discord.gg/NEPmWUdKn7?event=1103357411680600124) will be all about Apollo Kotlin on May 10, 13:30 UTC—[join the Discord server](https://discord.gg/NEPmWUdKn7) now so you don’t miss it!


Without further delay, here’s the recording of office hours for iOS Client—check out the chapter markers for the topics most interesting to you!


Office Hours: iOS, streamed live in Discord April 20, 2023


## Transcript


**Jeff Auriemma**


All right. Well, let’s get started then. I’m here with Anthony Miller. This is really exciting. We’ll kinda introduce ourselves, I guess, and say what we work on at Apollo.


If you could have any animals as a Pet, what would it be? Let’s go with that. We’ll start with Anthony first. I’ll hand you the mic, Anthony.


**Anthony Miller**


Hi, I’m Anthony Miller. I’m a lead iOS maintainer for Apollo GraphQL, I’ve been maintaining the Apollo iOS library.


Yeah, really great to be here today. So yeah, name’s Anthony. My location, I am based in Las Vegas, Nevada.


And I’m not a huge pet person in general, but growing up, I always said that if I had, you know, crazy money, if I was a millionaire, I would build a house with a moat and a drawbridge.


And it became a running joke with my friends that I had to have a biome in my moat. So what I want is sharks and flesh eating piranhas as pets. That is like the craziest, most ridiculous thing you could do, but it’s just been a running joke since I was a kid that that’s got to be my dream home.


I have to have a moat with piranhas.


**Jeff Auriemma**


A moat with piranhas, piranhas being that I was not expecting that Anthony. Well, remind me to visit sometime and, you know…


**Anthony Miller**


don’t fall in!


**Jeff Auriemma**


Good time that day. Oh, that’s pretty rad. All right.


I’m Jeff, Jeff Auriemma. I live in southwestern Connecticut in a town called Monroe, which is near New York City. I am the engineering manager, serving the open source engineers that maintain the Apollo clients, including Apollo iOS.


If I can have any animals of pet, mine is definitely not along those same lines. I chose a capybara.


I don’t know. I just think capybara is… my youngest son has a capybara stuffed animal that I find adorable.


One of my favorite integration testing libraries in the Ruby side of things is capybara.


And so I guess I just want a capybara as a pet. I feel like I would be a good capybara.


**Anthony Miller**


Great answer. I was in Brazil in December and we took a river tour. And while we were on the river, there was a capybara just eating some grass, literally standing on the shore, right by a big highway.


There’s just cars going by and this capybara just chilling looking at us.


He was the cutest little thing.


**Jeff Auriemma**


Well, cool. Do you have that geotag? Can I go back to that location?


**Anthony Miller**


I’ll find it for you. I’ll send you a link.


**Jeff Auriemma**


Well, yeah, we’re here. talking Apollo iOS today at the Maintainer office hours. This is the first stream of this kind, which is super exciting. And it’s just a way for the maintainers of these open source clients to chat about the things that the community is asking about, to talk about the project and why it’s awesome.


And hopefully empower some folks with the knowledge they need in order to use this technology to the greatest impact. So I’ll be kind of in the asker seat here, just asking Anthony a few questions on behalf of the Apollo iOS community.


I see we got a few folks in the audience right now. And if folks want to ask questions in chat, that’s fine. And we’ll see if we can kind of find a natural place for that. This is again, the first stream of its kind.


So we’re kind of feeling out the format and figuring out what works best. So your patience is always appreciated.


But I don’t know Anthony, you ready for kind of the first question here?


We want to talk about versions, major versions. So Apollo iOS has been on, or had been on version zero since the first commit in 2016.


But that changed in October of 2022 with the release of 1.0.0. And so we’ve gotten a lot of questions and feedback regarding that major version bump, right?


That big major version bump.


Have any like kind of common themes emerged? And what do you want developers to know who are looking to upgrade from zero to one?


**Anthony Miller**


Yeah, I think that the biggest common theme with the 1.0 release has been a lot of genuine concerns about how sizable the migration path was. And I fully admit the migration path is pretty significant to get to 1.0.


But 1.0 is the first major version release.


Up until 1.0 release of any project, you are able to break APIs with reckless abandon. Every single version that you’re, you know, 0.5, 0.6, 0.50, 0.51, you can continue to break APIs and cause minor breaking changes or major breaking changes all the time.


And we knew that getting a 1.0 meant a stable API that we felt comfortable with, that we felt we could continue to adapt and grow and include new features and provide what the community needs in a way that isn’t going to continue to make breaking changes constantly.


So what we decided to do was rather than continuing the pattern of every single minor version bump having a couple of little breaking changes and people constantly having to migrate, we really wanted to rip off the Band-Aid and come to a point where we say, okay, there’s a lot of things with the APIs that we don’t like, that aren’t really, you know, allowing us to do what we want to do going forward.


Let’s rethink these things.


Let’s take a step back and take a wide scope view of what should a GraphQL client look like? What should a GraphQL client be doing? We became a bit more opinionated on quite a few things that have definitely led to some pain points for people who were doing the migration.


But we really do believe that though the work and the effort to migrate to 1.0 is really going to be worth it because it’s going to allow you to, with much more confidence, stay updated and continue to be up to date with the feature work and the bug fixes that we are providing going forward.


Up until 1.0, it was very common for us to get a GitHub issue and there’d be some bug report or some question about something and people were using a version that was six months old of the library. And that’s because the upgrade path for each incremental minor version was a little bit of breaking changes in a lot of companies and a lot of people just didn’t have the time or didn’t want to take the effort to do that and consistently be upgrading all the time.


So if you can get to 1.0 and you can rip the band-aid off and do that migration once, going forward, breaking changes should be… The goal is that they never happen, right? But being honest, occasionally there were very, very minor breaking changes but we try to make them minor breaking changes that only affect a small portion of users who are using one specific advanced feature or working in some specific edge case that is very rare. Sometimes you have to fix a bug or add something that is going to be a little bit of a migration path but it’s been dramatically easier to upgrade through all of our patch releases and the 1.1 release since then.


**Jeff Auriemma**


That’s awesome to hear. So like the story is sort of like, yes, you know, 0 to 1 is can be a big undertaking but once you’re there in 1.x-land, right now we’re at 1.1.2 if I’m not mistaken. You’re following a more predictable release pattern. Your breaking changes should be very few and far between once you’re kind of up to date.


That’s kind of the where you’re trying to drive home.


**Anthony Miller**


Absolutely. The goal and so far we’ve been successful in that going from 1.0 to 1.1 and the 1.2 and 1.3 releases that are slated for this year that we’ve already got a bunch of ideas for what we’re going to be adding there.


I see Dylan’s asked in the chat here, will there ever be a 2.0 and absolutely we already do have on our roadmap right now for the 2.0 release.


That’s not going to be coming for a while and by design we don’t want to be making major version bumps too often. A major version bump indicates that there is going to be a migration that you’re going to have to do and we don’t want to have to do that too often but the 2.0 release is going to be when we get there eventually. I’m not going to make any guarantees on a date but the 2.0 release is going to be all about restructuring the networking APIs.


The 1.0 release was really focused on the generated models and the way that you generate the models, the way that you consume the generated models, adding more customization and robust feature sets to the generated models and restructuring the other APIs to consume and create and initialize those models in a well-structured way.


2.0 is going to be about allowing more advanced configuration and customization of the networking layer, trying to add in more of the modern Swift syntax with the Swift Concurrency features for async and await and combine and that is going to definitely be a major breaking change because the networking APIs we have are pretty rigid and have some fragile areas right now that we really want to improve upon and in order to do that it’s going to need to be a significant breaking change.


**Jeff Auriemma**


I feel like I’m excited to hear about that though. I know I’m super excited for 2.0 and we don’t even really have a date for that mind yet but I know it’s on the horizon which is super exciting.


**Anthony Miller**


Yeah.


**Jeff Auriemma**


So yeah, those major version bumps are always interesting. What can folks expect do you think? We’re on the 1.0x path right now and we’re shipping these great features for Codegen and other parts of the library.


When we think about that that jump from 1 to 2, do you expect it being kind of similar to the effort that it took to get from 0 to 1 or will there be maybe like what can users expect there?


Are we not sure yet?


**Anthony Miller**


Yeah, I think that it’s really is going to depend. Likely it’s going to be a relatively easy migration. You’re going to need to change some call sites out and use the new APIs and structures.


For anyone who is a general close to the default setup, it’s going to be really easy. At least I anticipate that it’ll be really easy. If you have a really robust set of complicated customizations, if you’re using your own custom request chain, likely you’re going to have to take all of your custom code and migrate that too.


The amount of your networking stack that is customized is going to probably be directly correlated to how significant the migration path is there. Because if you’re using the default ways of doing things that we’re going to re-implement all those things and the new APIs for you.


Hopefully it shouldn’t be too bad for most people.


**Jeff Auriemma**


Cool.


**Anthony Miller**


Yeah, I would like to add to that that the 1.0, the areas of your code base that you’re needing to migrate are the areas that are surrounding the actual generation of your code, your build steps and things that do the code generation and your consumption of the generated models.


So any of your code that’s in your UI components or your business layers that are consuming the generated models, writing them to the cache, reading them from the cache, using them in your UI, that’s the stuff that’s being migrated for 1.0 because the focus of 1.0 was the model layer.


For 2.0, the parts of your code, the generated models we anticipate should have very little to no breaking changes and the breaking changes will be in the networking layer. So even though it was a big like rip off the band-aid for 1.0, it didn’t affect the networking layer and incrementally with 2.0, now you’ll want to migrate your networking code and then 3.0, we want to do some point in the future, we’ll be focused on the caching layer.


So then you’ll only have to migrate, hopefully your caching code mostly. So it’s sort of like this, each major release is focused on one central component of the functionality we provide.


**Jeff Auriemma**


It’s pretty cool that we’re thinking that far ahead to two major versions ahead, I think that gives the community a lot of comfort in knowing what’s coming up on the horizon.


**Anthony Miller**


Definitely, it also gives me a lot of lost sleep and anxiety because I’ve got ideas. It’s exciting, but at the same time I have so many ideas and we have such a huge scope of things we know we want to achieve and things that we know we can achieve. And even though we’re proud of the library now and we think it provides a ton of value to people, when you look at the value it provides now and the things it does compared to the ideas we have, there’s just so much room for growth and exploration.


And it’s really exciting, but also, it keeps me up a night.


**Jeff Auriemma**


Your passion is enthusiastic for sure and definitely contagious here. You said something close to the end of what you just said earlier. You talked about generated models, right? You were talking about all that capability.


And one question we get a lot from the community is why are generated models immutable in version one? It’s a big difference for version zero because version zero does those generated models could be mutable. So what do you want developers to know about this?


**Anthony Miller**


So the first thing I will preface by saying this is one of the things we’ve gotten a lot of feedback in since the 1.0 version. And it’s caused us to rethink some things. We are always here to listen to the community, to get feedback and to take that into account and make changes that enable you to do what you want to do with our library.


So it’s something that we’re considering and we’re having some discussions internally about opening that up and making it easier to generate more mutable models on an opt-in basis rather than it being the default in the future.


But I’ll talk about why we decided to make that decision and try to justify it a little bit. I think this plays into a lot of the philosophical concepts around making the 1.0 release. What we really wanted to do is we wanted to provide a more principled API that encourages best practices and discourages anti-patterns.


That’s why we made this change.


So when you have mutable data structures, they can be changed at runtime within a view component. You can edit them, you can change values on them and then continue to pass them around. But those changes are not being persisted to the caching layer. Those changes aren’t being made, you know, posted as a mutation to your data on the server. And it becomes a more difficult to track mutable data.


It’s a common thing that’s taught in, you know, in intro to programming courses that you should make models immutable and make everything as immutable as possible and only make things mutable when they need to be. Because you are able to more safely reason about the scope of things. And when changes, when mutations are made, you know they’re not affecting other pieces when they are made in a very principled instruction way.


So when you want to make changes to the data, the way that it works now is you define a specific operation that is a local cache mutation that generates a model that is mutable. You can edit that model, you can, and then write it back to the cache. And then all of your other operations that fetch the data will get updated with that new data. So then you get a new version of that model when you fetch it and you can use a watcher so that it automatically gives you updates when your data in the cache changes and you get propagated up this new piece.


So rather than having some UI component that fetch an operation and then it’s mutating data in memory somewhere, this makes it so that the best practice is to write mutate data, write it to the cache, the cache is your source of truth.


And then all of the other parts of your application that are fetching data are getting changes from the cache and they’re getting new immutable models with the new updated values.


We think that that is the best way to structure an application in GraphQL. And that’s why we made this change.


We’ve seen some really great feedback on some use cases where people, it’s making their lives a lot harder and they don’t want to have to go through that process. And so we’re going to probably be creating a new option for CodeGen where you can opt in on an opt for operation basis or on a full project basis to make your models immutable. But we don’t want that to be the default.


And we want it to be something that you have to think about, choose to do and understand the implications of it.


**Jeff Auriemma**


Sure. Like a sensible default, but like kind of being able to sign a little bit of an opt in to a practice that we don’t necessarily, yeah, not ideal, right?


**Anthony Miller**


Yeah!


**Jeff Auriemma**


That’s interesting.


Yeah, thanks for walking us through that. And you know, yeah, I think it just speaks to the power of community input, right? Folks out here using Apollo iOS talk to us, tell us what your experiencing there.


Let’s figure out what we can do in order to empower you and and also keep you, you know, give you the ideas and tools you need to succeed.


**Anthony Miller**


Yeah, I mean, the last time that I built an application myself was before Swift UI and combined even were, I think they were just announced at WWDC. They weren’t stable enough to build an app with. I was using UIKit. I wasn’t using a async and await. I was using Apollo client at the time, but the way that people are building things has changed dramatically. And I try to keep up our whole team tries to keep up with the new technology and have an understanding of it.


But from my past experience, as I know that unless you are building a large stale production application that using the advanced features of this stuff, it’s really hard to read blog posts and make some like dog shooting example projects and builds a little prototype things and get a real scope of where the hard challenges come in and where the difficult parts are protecting and engineering.


And application using modern frameworks where those challenges are. And so we really rely on feedback from the community to help give us a sense of where people are struggling, where our library is making their lives harder, where we have opportunities to make people’s lives easier.


And we try to keep as much of our hands on the on the what’s going on in the world there. But it is really hard when we’re not working on a large application that we’re pushing to production with lots of users and a team of 20 or 30 or 150 engineers working on it and changing different components of it all the time. It’s really, really hard to understand what challenges people are facing there.


I have a really good understanding of that from like four or five years ago. But the ecosystem is constantly changing. So we really do rely on a lot of community feedback.


**Jeff Auriemma**


Yeah, thanks for walking me through that. And I was kind of planning on asking this a little later on, but I might make it skip the line a little bit because I think we’re on the subject.


The community on the subject of community feedback, right? We learn a lot from developers and a lot of times, one of the most typical ways we get feedback from developers is when they encounter an issue and they need help with it. So a developer might think it’s a bug, not sure if it’s a bug or not. What should they do and how should they go about letting us know about that or I don’t know, what’s your take on that, Anthony?


**Anthony Miller**


We have so many different channels of communication.


Right? So obviously,[GitHub issues](https://github.com/apollographql/apollo-ios) are there. We have this. I always confuse Discord and Discourse.


So we have this[Discord](https://discord.gg/graphos) now, but we also have a[Discourse forum](https://community.apollographql.com/) , which is the same forum software that the Swift.org community uses that runs the Swift library and language itself.


That is a great place to ask questions. There’s obviously for more hyper-specific questions Stack Overflow is a great community that has a lot of other people that aren’t Apollo employees that are answering questions and helping people out there a lot.


So whatever your opinion on Stack Overflow personally is. But if you think something is genuinely a bug in our library, the best place to put that is in a[GitHub issue](https://github.com/apollographql/apollo-ios) .


If you want us to answer some questions about why we’ve done something or made some decisions, if you have general purpose feedback for us, I think that probably this[Discord](https://discord.gg/graphos) or the[Discourse forum](https://community.apollographql.com/) are a better place to have those kind of discussions that aren’t themselves an action item, something that is directly actionable.


**Jeff Auriemma**


This Discord is the front end channel I think that we’re currently using for client questions, right?


**Anthony Miller**


That’s right. Yeah.


**Jeff Auriemma**


So yeah, keep them coming. Yeah, everybody.


**Anthony Miller**


Yeah, please, please. We’ve started to get a few. This Discord is a new thing, but we’re excited to have another place for community interaction. As far as bug reports go, if you think something’s a bug in Apollo and it ends up not being our bug or, you know, there’s just a configuration change or something you need to make, we’re happy to look at it, give you an answer really quick and close the ticket app.


If it is a bug, having an issue that we can add to our projects, tracking and create an app, you know, uses an action item is awesome and makes it. It’s the most likely way that you’re going to get that bug fixed. On top of that, if you really want to get a bug fixed, the number one thing you can do is give us an actual reproduction case.


And there are degrees of that. The best way of doing that is like, when we get an issue that has a zipped up project that is a whole on Xcode project that is narrowly scoped to just highlight this bug and give us a reproduction, it makes our lives so much easier. I can run it, I can attach the debugger, I can look at exactly what’s happening and figure it out and fix it.


**Jeff Auriemma**


And you do that, right? Like when somebody sends you that, you absolutely right?


**Anthony Miller**


Absolutely. Yeah. When it’s here is the reproduction steps. It’s a little bit harder. Usually, if the reproduction steps are accurate and are detailed enough, we can figure it out. And a lot of times we have to ask for more clarification. If you can’t come up with a specific reproduction case or you’re not even sure exactly what is causing it. So that’s why you can’t reproduce, then providing us as much information as possible is really helpful. What I’m always looking for there is a schema and a operation definition that I can run codegen against and then see that it’s not working.


**Jeff Auriemma**


An operation definition being like query mutation, whatever.


**Anthony Miller**


A GraphQL query or mutation, yeah. And very often, we also want your codegen configuration file that is used to determine how the codegen looks for your files. Some of the shape of how the output of your codegen is structured because a lot of times there’s little edge cases in the way that your project is structured and the way that codegen works for specific projects that we don’t always catch because there’s so many nearly infinite different ways that people can configure this to work with their projects. So if you are using CocoaPods, we have to do certain things with our import statements.


If you are taking the generated models and embedding them right in your application target, we have to name space things differently than if you’re going to take all the generated files, put them in their own module. And then if that module itself is a CocoaPod or if that module itself is an SPN package or if that module that you’re putting your generated models into is a library that you’ve created directly in an Xcode project yourself and linked the files to that changes and informs the way that the generated models have to be shaped and how they have to reference each other for namespacing and things like that.


And so sometimes we get issues and bugs where it’s like, hey, I can’t, this files not being imported properly or getting a build error because a generated file saying it doesn’t recognize like, you know, it can’t find some other file.


Often times it’s because we make changes to fix one bug and we don’t realize that the generated code that we’ve changed in certain specific situations doesn’t link properly anymore. So having an example of here’s the schema, here’s the operation, here’s what our codegen configuration looks like tells us the information we need to know to usually be able to figure out the root calls.


**Jeff Auriemma**


Nice, so details. Good.


**Anthony Miller**


As many details as you can, yes.


**Jeff Auriemma**


So the less guesswork we have to do, the easier everybody’s lives get, including the folks who are looking to get us to give a second set of eyes on an issue and see if there’s a bug.


**Anthony Miller**


Definitely.


**Jeff Auriemma**


Nice. And you know, I think you mentioned before, you know, the feedback and you talked about like specifically back being, you know, given through these different, these different places. So I heard, I heard the old[discourse forums](https://community.apollographql.com/) . I heard this[discord](https://discord.gg/graphos) , which I think is getting a lot of traction and really excited about that.


So definitely, you know, if you’re here, we got a place for you here. And like in the front end channel, and then, um,[GitHub issues](https://github.com/apollographql/apollo-ios) , it sounds like too. If there’s something like actionable, you want us to take a look at or if there’s like a pretty thought through requests that you have for us.


**Anthony Miller**


Yeah.


**Jeff Auriemma**


Well, thanks for walking us through that. That’s, that’s really great information. I mean, details like reproductions are great, especially something that we can just download, but next good, it’s going to be, you know, that’s, that’s, that’s the gold standard for sure.


But it’s nice to know there’s a bit of a sliding scale there. Um, awesome. So, um, changing gears a bit, we’ve gotten a lot of requests over the years to make the generated models conform to the codable protocol.


In 1.0, codable conformance was left out of the generated models as a deliberate decision, as I understand it. Um, are there any plans to add this feature in the future? Or why was it left out, I guess, in the first place?


**Anthony Miller**


So, short answer, there are no plans to add this feature in the future. Um, and, it was made as a deliberate decision for a reason.


And I want to talk about that and explain it again to people. Um, because there’s a couple of like, GitHub issues that I’ve kind of given a little bit of an explanation, but those may be hard to find.


So I want to address this one. Um, we continue to get requests for this, but my question is always why? Why do you want codable conformance? What do you think you’re doing with codable conformance? What value are you getting out of it?


So, codable is a protocol that allows objects to be serialized and deserialized into some format. It can be JSON. It can be XML. It can be whatever format you want, but codable just allows objects to understand what fields need to be serialized with what values for any type of coder, an XML code or a JSON decoder, whatever.


What this is used for is persisting data generally. Well, in Apollo client, we have the normalized cache. We have a SQLite cache that is a persistence mechanism that is highly specific and catered to the shape of GraphQL data. It doesn’t take your models and persist them as each individual operation definition.


It normalizes all of your models across all the different operations to provide the best performance and the most robust feature set for looking at mutating that data, updating things.


Like I talked about earlier, you can make a mutation right into the cache and then have all of your operations propagate that change immediately. That is all powerful functionality that comes from the normalized cache. We want the normalized cache to be your source of truth. If we’re providing codable, what we’re doing is we’re enabling people to again, do something that we don’t believe is a best practice, something that we believe is an anti-pattern to persist this data arbitrarily in some other file to do something else with it and hold it somewhere else so they can persist it and reread it back in the future on a different run of the application.


Why are you doing that? Why aren’t you using the normalized cache? I’m not saying there aren’t actual good use cases for that. If you want to make extensions to make your objects conform to codable, if you want to do that in some way that’s specific for a use case you have, I say go ahead and do that. We’re not preventing you from doing it. But for us to provide it as an option in code gen means that we have to maintain it. As the models change and evolve, it’s something we have to continue to work on and maintain and make sure it continues to work. As our APIs change and as the Swift APIs evolve, and we don’t recommend you to use it for 99% of the use cases that it actually people are asking for us to have it for. What I found is generally when I explain this to people, then they go, “Oh, yeah, okay, we’ll use the normalized cache. We’ll use a SQLite cache, we’ll persist the data there.”


For us, the effort and energy required to do this and then continue to maintain it, for the use case almost always being something that we highly recommend you don’t do, makes it something that we don’t feel comfortable adding. That doesn’t mean that you can’t make your models conform to codable. In fact, because the code generation has been rewritten in Swift, you can fork the code gen library, add your own codable template in there, and build your own version of the codegen, CLI, that generates that with codable.


It’s just not something that we want to maintain and it’s not something we want to actively promote to the community and advertise the community as a feature of Apollo iOS.


**Jeff Auriemma**


It makes total sense. Yeah, that does not sound easy to maintain too. That seems intuitively, that resonates a lot with me.


**Anthony Miller**


Yeah, I mean, one thing you have to understand is if you persist models as JSON data or XML data separately and then you get new data from the server, those models that are persisted somewhere else that you’re doing manually are not going to get updated.


They’re not going to get the changes. If you make a change to that persisted model and it affects fields on another object that is also part of another operation, that other operation is not going to know about those changes. It’s not going to be updated. So propagation of changes between your different operations doesn’t happen. That’s what the normalized cache does for you.


It is structured to be performant. And in fact, in the future, we aim to add a lot of new features and functionality for querying things from the normalized cache with very dynamic ability to filter and sort lists of data and grab entities based on types and based on fields and say, give me all objects of this type whose name field starts with the letter A. That kind of stuff you couldn’t do if all of your models are persisted in JSON files.


**Jeff Auriemma**


Yeah, normalized caching is one of those things that I just find is such a superpower of GraphQL. It just lends itself so well to it. So yeah, I wanted to double underline that. That sounds yeah. Lean into it, embrace it, if your GraphQL application. And we’ll be there to support you in a lot of different ways.


Cool. Thanks. I know that’s always a frequent flyer of ours. And speaking of frequently asked questions, another one that keeps showing up on the radars.


**Anthony Miller**


I know where this is going.


**Jeff Auriemma**


` GraphQLNullable` ! Yeah. So, you mentioned in GitHub, you talked before about explaining it again. And for those of you who aren’t familiar, who’ve been in the GitHub issues, Anthony has made some really clear and detailed posts and GitHub comments and everything talking about these decisions. So, if you got a chance to click through those and finding those pieces of writing, definitely do check it out.


There’s a lot of great code samples in there and examples of why we do things. And Ditto for Calvin, he’s gone to great lengths to champion this concept. And so, talk to us about` GraphQLNullable` . What is it? And why is it part of the API?


**Anthony Miller**


So, we have found, time and again, that some of the foundation library for the Swift standard library structures just aren’t clear to the way that GraphQL does things. GraphQL is opinionated. It’s not only type safe, but it’s type safe in some very specific ways. And there’s a lot of complicated pieces of the semantics here.


GraphQLNullable is a great representation of that. In Swift, you have optionals. And optional is an enum that has two cases, Some and none. None means nil. Some means there’s a value there. The problem is, in GraphQL data, there’s three different versions of this. There is Some, there is none, and there is null. When you are — and in Swift, you can use the object NSNull to indicate null, but you can’t have an optional that has these three different options. When you are inputting data into a variable for an operation parameter, there is a semantic difference in GraphQL between nil, meaning you don’t — you omit it.


And null, meaning you explicitly include the field with a null value. This is part of the GraphQL spec. It says that there is a semantic difference here, and clients should expect to be aware of and indicate nil or null. Now, certain backend servers and a lot of people commonly just treat them the same way on their backend. And so those are the people that we get most of the comments about why` GraphQLNullable` is making our code more cumbersome.


But we are a general purpose GraphQL library, and we have to conform to the GraphQL spec. And the fact is that we want to make it clear that if you omit a field versus including it as null, you are doing something functionally different and your server may interpret that different. And that also includes our cache has to understand the difference. Our cache is taking the data that comes back from the servers, so if you are writing manual data to cache, we also need to know was this data written because of a nil or a null field.


The classic example of this is if you have a mutation and you omit a parameter and you make it just nil, then that field on the object that you’re mutating just doesn’t get changed. So you can change the fields that you want to change and keep the current value of the fields that have it changed. But if you include that field as null, what you’re saying is delete that field’s value on the server, on the backend. So there’s a big difference there between knowing I want to delete this value versus just leave it as is and change these other ones.


You have to be able to have the functionality for a client to be able to say, “Hey, this data is dead. I want it to be deleted.” And you have to have the ability to be able to keep it where it is. So this is a really important thing to have. We used to do double optionals for this and the problem with a double optional, meaning the first, the outermost optional indicated null or indicated nil and the innermost optional, it was an optional with a value of nil.


That would indicate null. That was really confusing for people. People didn’t understand why their things were wrapped in double options and we got a ton of questions about that.


Now we have` GraphQLNullable` that makes this very explicit and you have to make the decision and we get a bunch of questions and complaints about that. This is really a lose-lose situation. But it’s something that’s necessary and we needed to keep the semantics of GraphQL.


Now I want to talk a little bit about the design of this.


This is an enum that works just like optional except it has three cases. And the way that I would love for this to work is that you could still do all of the same optional and wrapping. So` GraphQLNullable` is necessary for the input of values. But when you’re reading your values, it’s very rare that you actually care on the client if they’re null or nil. Most of the time you just want to know if you have the value or not. But when you’re when you’re reading the values client side.` GraphQLNullable` makes that more cumbersome because it’s an enum that has these cases you have to switch over cases. Swift’s enums are very powerful but they also are I think probably the most cumbersome, ugly API piece to use.


It’s really cumbersome to switch over all the cases. They just are a thing that makes things hard to do. And that’s why the Swift language made the optional enum as a very special case that has its own reserved operators. The question mark operator that you can use for null unwrapping is awesome and optional chaining is awesome.


But because our enum isn’t part of the Swift standard library, it doesn’t get that special syntactic sugar, which makes it a lot harder to do. You can’t just omit it or type a nil. You have to type in dot like GraphQL enum dot nil or GraphQL enum dot null. When you initialize values, you have to wrap them in this GraphQL enum. And we really tried everything we could to make this better.


We conform to the expressible by string literal and integer literal protocols so that if your value is a string or an integer, you can just pass the literal value and it wraps it in the GraphQL enum for you. But unfortunately, there’s no way to make expressible by object literal for some other arbitrary object. So if your` GraphQLNullable` is wrapping another piece of structured data like one of our generated models, you have to initialize the enum case itself.


And that’s where people get into some ugliness around this. So I have done everything I can to try to make the API for using this as close to a regular optional as possible. But there is just some limitations that are part of the Swift language that we’re not happy about, but we do feel like this is the more principled structured way to do things.


And it forces you to make that decision because very often with double optionals, you could just put in nil when what you actually meant was no, and you’ll never get a compiler, you’ll never get any indication that you’re not you’re not sending this field until you in production get a bug.


So by doing this, we’re forcing you to make that decision.


**Jeff Auriemma**


Yeah. And then we make the compiler your friend.


**Anthony Miller**


Yeah. Exactly. And adding on that, people have asked for a way to like make the API a little bit easier by adding a default value. So basically, making it so that you can pass in an optional to the GraphQL enum initializer.


And if the optional value that you pass in is nil, then the default value would be nil, or if the value is nil, the default value would be null. The problem is that based on your use case, we don’t know what’s that default value should be. Should that default value be null, or should it be nil?


So what we’ve done is we’ve put out a snippet that says, here’s a function, it’s an extension, you can copy and paste into your project and change whatever you want that default value on this convenience and this initializer to be. But we don’t want to include that convenience initializer in the library, because again, it is allowing people to accidentally do something in a way that isn’t what they actually intended.


And one of our core principles here is to make things very explicit and make the developer have to make the decision about what they want things to do, not to have implicit things that are happening and developers don’t understand that a decision was made that was unintended.


**Jeff Auriemma**


And where’s that snippet? Is it in like a GitHub issue?


**Anthony Miller**


It is in a bunch of GitHub issues. I am happy to pick it up somewhere and put it somewhere. Actually, I think that it probably should be added to the documentation for` GraphQLNullable` in our in our documentation site. So perhaps I’ll do that today.


**Jeff Auriemma**


All right. Yeah, well, added to the show notes too. That sounds good. Cool.` GraphQLNullable` . Yeah. And you know, if folks who are listening, either during or after the fact, haven’t done so already in your GraphQL practitioner, you’re doing GraphQL stuff, definitely do find some time to read this back if you haven’t already. You’d be surprised at how quickly you can get through it. It’s probably like an afternoon’s worth. But it’s definitely worth going through and getting these kinds of nuances because it definitely pays off. It definitely pays off.


**Anthony Miller**


Yeah, there are a lot of things you can do with your operation definitions that if you don’t know are part of the GraphQL language, you’re not able to take advantage of.


And we’ve created the code generation engine in a way that is very heavily informed by the shape of your operation definitions. So you can achieve the same fields being selected in a to number of different ways.


And the way that you decide to structure that informs the way the generated models are shaped and the way that you’re able to consume them. So understanding the difference between, you know, putting an inclusion condition on an in line fragment versus putting it on each individual field itself, using a named fragment on a type versus using an inline fragment on that type. Do you make an inline fragment on the type and then include the name fragment inside of it? Or do you make a name fragment on the type and just include that named fragment itself directly?


That actually wraps it inside of an inline type case fragment anyway. But it does it in a way that changes the generated models and changes the way that you can consume them. So I heavily suggest people to read the GraphQL spec, be of informed about that and then play around with their codegen of their operations when something feels cumbersome in your generated models and doesn’t work the way you want it to.


Very often there’s a way to restructure your operation to make it a little bit cleaner and easier for you. And I think named fragments are one of the biggest ways to do that, especially when you want to reuse data across operations.


**Jeff Auriemma**


Nice. So when you’re like taking a look at a pull request or something and it has generated code in it, you definitely want to take a close look at that. It sounds like too. That just assumed that the generated code is what you want, right?


**Anthony Miller**


Yeah, it will certainly do the job and allow you to access all the fields. But if it’s if it’s cumbersome and you’re having to like dig into a bunch of different type cases to access the fields you want, you can put those things together in a wrapped overall type case that merges all of those fields together and makes it easier to read your data or to consume your data.


**Jeff Auriemma**


Switching away from from this topic, came across an issue recently that I thought it might be cool to chat about a little bit. And it was the issues in GitHub, that’s number[2863](https://github.com/apollographql/apollo-ios/issues/2863) , improve @_exported import Apollo API and generated files. What’s next for @_exported? I’m really curious to hear what your take is on that.


**Anthony Miller**


I hope what’s next is that the Swift community removes the underscore and makes this an actual stable public API.


Because it’s an awesome feature and it’s been something that hasn’t gone through the entire regime process and become a stable feature for quite a long time now. So just a general overview of what this is, we’ve broken the Apollo library up into two primary pieces.


There’s the Apollo client, Apollo module that holds onto the networking client, the execution layer and the caching’s Apollo store and all the things you need to operate on GraphQL data.


And then we have Apollo API. And Apollo API contains a set of models and protocols that your generated models conform to and use. So Apollo consumes the or links to the Apollo API project and your generated models link to the Apollo API project. This allows you to use your generated models from modules in your library that aren’t doing GraphQL operations. So you can have one module that you define your network operation in, and then a UI module that you fetch the code and you fetch the models and then you give those models to your UI module.


And your UI module only needs to import your generated models. Well, your generated models, since they need those files from Apollo API, and in order for you to access and use the fields on those generated models, you need Apollo API. What we’ve done is we’ve at exported import Apollo API in your generated code. What that does is it takes all of the signatures from Apollo API, and it links them to your code if you import your generated models. So it’s a transitive import statement that when you import this file, it also imports this other module’s headers. Well, headers, that’s an old Objective C thing. We don’t really do that in Swift.


There’s no headers anymore but that allows you to easily do this. Otherwise, what we found before we did that is that people were making bug reports because they would import their general model, their generated models, they’d try to use them, and they’d get a compiler error that like GraphQL, enum, or GraphQL, nullabe is not found in this, you know, is an unrecognized type. Well, that’s because you needed to then also import Apollo API, and there was no clear way of knowing that. If you’re using the generated models, I’d say 99% of the time or more, you need to import Apollo API anyway. So we just exported and imported for you. The feedback we’ve gone from the community, I think, was firstly just concerned because it is an underbar API. And so people are afraid of the experimental APIs.


We have done a lot of due diligence. I’ve looked very deeply into the way that this works and the structure of what it does. And we’re pretty confident that it doesn’t have straws, any serious side effects, or doesn’t cause any other problems for your project. There are a lot number of other underbar API. Some of them are more or less stable than others. And every time we try to use any sort of underbar, underscore API, we are very diligent about understanding what the current status of implementation is, how likely it is to change and break things in the future.


What other side effects or edge cases are not addressed yet, how likely this thing is to actually get included as a stable feature in the future, or be obsolete, or deprecated. And we are very, very diligent about doing that.


@_exported is something that is going to stick around, and it’s pretty much going to stay the same way. The only thing that there is actually a problem right now, we have a bug that we’re trying to find a workaround for, but I think this is related to @_exported import, not being a fully finalized API. There’s an edge case where if you use something like GraphQL enum, and you pass that object that was exported import through to something else that then needs to check a protocol conformance on the type, it doesn’t always have that protocol conformance metadata. And so the cast to that protocol will fail when it shouldn’t.


You have a type that does conform to a protocol, but because you’re importing it transitively, the compilers in lining code in a way that it doesn’t know about that conformance. And that’s causing some weird awkward issues. The workaround for that right now is unfortunately link your other code back to Apollo API, which is the thing we were trying to avoid by having exported import in the first place. So it doesn’t cause you to have to do anything you wouldn’t have already had to do anyway.


But in those cases, we’re trying to find a workaround for it. And I think that’s one of the things that at exported import needs to be needs to have corrected in the Swift standard libraries before it becomes a stable feature.


**Jeff Auriemma**


Well, I learned something new today. Thank you.


What do you love new stuff actually? I’m really appreciative of this. I hope all of you out there who are listening also are learning something new. It’s pretty cool. We’ve, oh, well, we have a question in chat from Dylan. Oh, yeah. The question is, can I use defer in Apollo iOS? If not, when is it coming?


**Anthony Miller**


It’s not there yet. It is coming. Calvin is the product owner on that and is working diligently on it. We don’t have an exact due date yet, but our deadline or anything. But it is coming soon. Yeah. Yeah, we’re looking soon. It is actively being worked on. And we’re just trying to make sure that we get it right.


Because we’re now in a world of a one point of where we want to keep our API stable, we really want to make sure we get things right the first time. And yeah, there’s going to be bugs sometimes that we need a fix, but we don’t want those bugs to affect the public API.


We are okay with fixing things internally in the implementation code, but we don’t want to realize that there was some oversight in the way that you used defer in certain situations that is going to cause a problem that we need to restructure or rethink the way that you consume the API.


So we’re being really, really principled and diligent about making sure we do these things in a way that makes sense.


**Jeff Auriemma**


Yeah. Thanks for the update there. And if folks are interested, in following along to our roadmap does have our best guess at what we think it’ll be ready for use, which is really exciting. So that you can get that in the repo. You just go to the[roadmap document](https://github.com/apollographql/apollo-ios/blob/main/ROADMAP.md) .


We try to keep it up to date and keep you informed of when things are going to be dropping. So keep a lookout for that. Defer is coming soon to follow us. It sounds like we were definitely dotting the “I”s crossing the “T”s.


**Anthony Miller**


Very excited for that one.


**Jeff Auriemma**


Yeah. Yeah. It’s been such a joy to use honestly and such a great part of GraphOS. So having client support in iOS is just going to be such a great, such a great affordance for the users.


Well, time flies when you’re having fun. We’re coming close to the end of the hour, which is like super cool.


**Anthony Miller**


We were worried about not having enough stuff to talk about for now.


**Jeff Auriemma**


Yeah. But I do have one more prepared question and I was hoping that you could talk about Anthony, which is, well, speaking of the GraphQL spec, which we were talking about earlier, scalars, right?


You know, GraphQL spec defines some scalars for use, but we’ve done some questions recently where we’ve suggested that folks use a custom scalar.


What is a custom scalar? Why should people use it?


**Anthony Miller**


Yeah.


Custom scalars are a really great and powerful tool in GraphQL. And it’s, when you read it in the GraphQL spec, it doesn’t, it’s not apparent how valuable they are and what they really do for you. What a custom scalar is, is in your schema, you can define a scalar with an arbitrary name and then you can make the type of any field be that scalar.


That just indicates that this is some sort of type that is a specific special type. What that type is, obviously has to be implemented by each individual server and client in their under, you know, in their code base. Swift has an implementation of string and kotlin has an implementation of string, bringing in Java does and they all are different implementations of how it works in that language, but they all kind of have the same general semantic for a custom scalar.


What you’re doing is you’re indicating, hey, this is a new type and every instance of this type can be represented by the same sort of data structure with the same semantics and usage. That usage and semantic has to be defined by your Swift team, your client team who’s making your Apollo iOS library, but it allows you to indicate that all fields of this type are going to be that custom scalar. If you didn’t have custom scalars, what you’d have to do, the great example of this is date time, right? When you serialize date times in JSON and you send them over the wire, you send them as a string.


So if you didn’t have custom scalars, you would have to make your time or date field be of type string. But what you’re saying by using a custom scalar is, hey, we know that the data we’re sending you over the wire is a string, but this string actually represents a scalar called a date time. And so on in Kotlin and in iOS and Swift and in Java and every other library, you have to choose how to serialize that string into a date time and represent that data structure and consume it in your client.


But you’re getting this hint that this field is a date time, not just a raw string. And what we do with the generated models is we, whenever you reference a field that is a custom scalar, we create a type alias that we point to.


So your fields in your generated models show date time or whatever the name of your custom scalar is. Now, by default, the generated type alias we’ve created is just a type alias to string, because that’s the default underlying data that we’re going to get over the wire. But what you can then do, and we have some really, really great documentation on custom scalars with some great examples of how you can do this, is you can take that type alias and instead make it a struct or a class of your own that is initialized with a string value and converts that transforms it into whatever data structure you want.


This can be used for you UID, it can be used for date times, it can be used for really anything that you can imagine as a scalar value. And you write the serialization code that takes the raw string, turns it into your scalar, and turns it back.


And the the Apollo client automatically passes that data, serializes and de-serializes it. And if you change that type alias from string to a date object of some sort, your generated models will then spit that out when you access that field. So when you access the field on the generated model, you’re going to get a real swift date or swift UUID or whenever custom structure that you have defined, rather than getting back a string and then having to wrap it in some sort of view model that transforms things.


The transformation happens because you defined this custom scalar and we know the field is that custom scalar. Super powerful, super valuable and makes your code a lot and your generated models a lot more useful and easy to use. What we wanted to do was make it so that you don’t have to take our generated models and then wrap them in some other view model that then does a bunch of transformations on them. We wanted our models to be able to, in as many cases as possible, be really robust and useful as your model objects that you use in your UI and your business layers.


**Jeff Auriemma**


Yeah, a lot of folks think about GraphQL as like a networking technology or things like that, but it really is a really powerful way of storing fetching and expressing data in a way that’s reliable and then elegant.


**Anthony Miller**


Well, the GraphQL spec isn’t any of that. The GraphQL spec is a definition of a network transport, but all of the tooling that it enables and that Apollo and other awesome developers around the GraphQL community are building take advantage of that specification in such incredible ways to make just really robust feature sets that make your entire persistence and models and networking layer work like magic as much as possible for you and let you focus on the business logic of what your products do.


**Jeff Auriemma**


I love that example that you gave the date time scalar too. I think that’s a pretty common use case. It just reminded me this article


I read recently that they’re talking about giving the moon its own time zone, which made the programmer in me cringe. I’m afraid.


**Anthony Miller**


I feel like the moon has to have multiple time zones, right?


**Jeff Auriemma**


I think so. I don’t know. Yeah, seems unfair to the moon to just get one time zone, but I guess we’ll cross that bridge when we get there and maybe another custom scalar in the works for that.


But anyway, I think we’re getting close to wrapping up. Anthony, thanks so much for sharing your time with the community today. I don’t know. I thought there are any


parting questions. Great, but it’s got to be real quick. What’s your favorite color?


**Anthony Miller**


It’s purple.


**Jeff Auriemma**


I think we got a rule.


**Anthony Miller**


Thank you, Jeff, for your time today. All of the awesome questions. I really, really appreciate it. I really appreciate the opportunity to get some of this information out there in another format that’s more consumable for people. We got to answer some really commonly asked questions and give some really good perspective on the way we think about doing things and why we’re building what we’re building and how you can take advantage of it. I’m really, really glad that we got to do this today. Thanks so much for making this happen.


**Jeff Auriemma**


Thank you! No one I’d give a half tip to Dylan who really helped us get our stuff together today, as well as Watson and Patrick who are super involved with streaming here and to the Apollo iOS team. Anthony, as you see here, and Calvin and Zach who just joined us on Monday, we’re really excited to see what all of you are doing next.


I think that’s it for now. We’re going to guess leave the stage and call it a call it a day.


**Anthony Miller**


Thanks, everybody.


Written by


Dylan Anthony


[Read more by Dylan Anthony](https://www.apollographql.com/blog/author/dylan)
