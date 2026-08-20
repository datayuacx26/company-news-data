---
schema_version: "1.0.0"
document_id: "8bf01419917801c1529972226c191dc4c66707ebb4eabb58de334485596babea"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/office-hours-apollo-ios-16-june-2023"
published_at: "2023-07-20T16:35:49+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:1f7e255be7cb0bbb9a4a176e28a371a0ab4e96cb45cfd03f51ecc114b074b2e1"
---

# Office Hours: Apollo iOS – 16 June 2023

Here we are with another episode of our office hours!


We have a recording of the stream below, where Jeff Auriemma and Calvin Cestari discuss all about Apollo iOS![J](https://discord.gg/NEPmWUdKn7?event=1103357411680600124)[oin the Discord server](https://discord.gg/NEPmWUdKn7) , now so you don’t miss future office hours!


## Transcript


**Jeff**


Welcome back, everyone, to another maintainer office hours. What’s old is new again. We started this series with Apollo iOS, and now we’re back to Apollo iOS again. I’m Jeff, Jeff Auriemma. My pronouns are he and him. I live in Connecticut, and I’m the engineering manager for the Apollo client teams. That includes Apollo Client Web, Apollo Kotlin, and of course, the subject of today’s talk, Apollo iOS. And I’m here with Calvin. Calvin, why don’t you give us a quick intro as well?


**Calvin**


Sure. My name is Calvin Cestari. I am in Vancouver, BC. Been actually just gone past two years at Apollo. It’s gone by pretty quickly. So yeah, that’s me in a nutshell. Been doing mobile development for a while.


**Jeff**


And happy anniversary, Calvin. It’s hard to believe two years already. I’ve only been with you for part of that time, but it’s been a great part of that time for sure.


**Calvin**


It certainly has flown by. That’s for sure.


**Jeff**


Nice. All right. Well, it’s been tradition to start maintainer office hours with a bit of a a bit of a non sequitur, a bit of an icebreaker. This one’s kind of iOS-centric here. I got a question for you, Calvin. What was the first Apple device you ever used or bought?


**Calvin**


So the first one I bought was one of those white MacBook Pros, the white plastic shell case. I I think this was just after they transitioned from PowerPC to Intel. It must have been 2007 or something. So that was the first one I bought. But the first one that really got me into the Apple ecosystem was an original iPhone. And I actually still have it. I dug it up. I put it on to charge and it charges.


**Jeff**


That’s so rad.


**Calvin**


It unlocks. It’s still usable. It’s amazing. It’s been over a decade old. It’s stuck on some ancient version of iOS, though. Let’s see what this is. About 313. I can’t go any more than that, so it’s pretty terrible to use, but it still works.


**Jeff**


Not officially supported, I think, by Apollo iOS, but I wonder what we’d have to do in order to make it so.


**Calvin**


So I have just a mini. Look at the difference in the screen size already, just to do. It’s barely usable, the old one.


**Jeff**


How long did you use it for? How long did you hold on to the phone? Was it your daily driver for a while?


**Calvin**


It was. I was still living in South Africa at the time and they weren’t available there, but my brother used to go to Hawaii quite often for work. So I got him to buy me one and bring it back. And yeah, I used it for ages. I think I skipped the 3G and the 3GS and then I got a 4, so it lasted for a long time.


**Jeff**


I think the 4 was like a really big leap in design, if I’m not mistaken.


**Calvin**


Yeah.


**Jeff**


So you probably got off at the right time. Now the original iPhone still only worked on like 2G edge, right? Or something like that.


**Calvin**


Right. So it crawled.


**Jeff**


It did have a Wi-Fi radio, right? Or am I misremembering that?


**Calvin**


Yes.


**Jeff**


Okay. So there’s that. It’s interesting. You know, so many ways just to try and design. I guess my, the first device I actually bought was marketed as the iPod Video. I think it is now referred to as the fifth generation iPod. But it had a color– I think it was the first iPod with a color screen, but it still had the wheel. And I remember at the time I was at a university.


I went to the College of New Jersey, and I was a music major. And so I was working in the music technology lab as a lab assistant or whatever. I was there to make sure the computers were OK, I guess. And I remember loading up my iPod video. In addition to music, I would load it up with fresh Prince of Bel-Air episodes that I would get from my college’s whatever.


**Calvin**


Classic show.


**Jeff**


Yeah, Peer-to-Peer Network. I forget what he called it. And that was– that passed the time. That passed the time. So, fond memories of that device. And I remember unearthing it years and years later, interesting if it still worked and it did. So a testament to the build quality.


**Calvin**


Yeah, they are.


**Jeff**


I still think we’re missing something with the wheel interface too. There’s something to that.


**Calvin**


Those were very satisfying. The click they gave you as well. Yeah, these devices are amazingly resilient for being something that you throw in your pocket every day and gets taken around no matter where you are.


**Jeff**


I don’t know how they do it. Yeah, well, moving on to talking Apollo stuff. One of the things we’re excited about as a company, as a team, is the @defer directive. Could you give us a sense of what is the defer directive and when it’s coming to Apollo iOS.


**Calvin**


So defer is a way for clients to communicate to the server that’s going to send back the data to communicate the priority of the data that you want back. So through the use of this defer directive, you get to wrap a portion of the response data as saying that this can come back later. There’s parts of this that I want immediately you to send back in the first response, but then you could take a little bit longer to get the rest.


And you know the way people used to be able to do this before is you either like you split your queries so you you fetch the some things later that are not really important or you prefetch some stuff that you might need more than the rest. But having this a part of the spec is really like a definitive way for your server implementations to be able to to support this need, which is a lot better. It’s worth calling out though, this is not the same as subscriptions. It’s not meant to be like real-time data support. It’s still a single response, that single, sorry, request, but you’re stating that, yeah, take your time, give me this, like this is the important bit immediately.


**Jeff**


That’s interesting, and it is interesting too that we, that reminder about real-time, right? Because in some ways it’s like a challenge what we understand a request and response cycle to be. A lot of times we’re interested in fetching something discrete, like a discrete amount of data, and getting some sort of deterministic pile of data back. It sounds like that’s different, but not quite the same as real-time.


**Calvin**


Well, the real-time support for that is subscriptions. Subscriptions are like an endless stream, really. Stream being the important bit, you keep getting these events, and they keep updating you with the new values, whereas defer is still a regular request. It’s one request, and it will end once all your data has been received for you.


**Jeff**


When is it coming to Apollo iOS? Because I know we support it in different areas of the Apollo stack. I’m curious to hear more about the Apollo iOS side of things.


**Calvin**


We support it in all the other open source areas of the stack, Apollo Server, and then the two other clients. Apollo iOS has lagged a bit in its support for it, but we chose to prioritize a piece of the work implemented the transport protocol, the incremental delivery. Now that that has been in place for a little while, we can go ahead and do this. It is an H1 kind of feature for us. We have a couple weeks left to really grind away and get it done. It may stretch a little bit beyond that, but that’s like imminently is really the answer to when we expect to have it in.


**Jeff**


It’s really exciting. And for folks who might not be familiar with the term “directive” how does somebody use a directive in GraphQL? When we say that, what are you… Because I’m sure a lot of folks have used directives and they’ve typed it, but what does that mean? You’ve probably seen it before. It has the @ sign, or is it an ampersand? We always refer to them as @ signs, and you forget the actual term of what they are. Anyway, it’s an @ sign followed by a string of text. So you use these in your operations where you mock a named fragment or an inline fragment. You tag it with this directive, and then that signals how you want that piece of data to behave.


**Jeff**


So when folks add this to their query or something like that, I know one of the coolest parts about Apollo iOS is the thing I find to be the coolest part is code generation. we have these great, you know, these query documents and then they get translated into the model code that helps underpin our applications. What effect does that have on model code? Like what can folks expect when they type that differ in and what the output would be?


**Calvin**


So at the most fundamental level, like it has to become optional in Swift. Like Swift needs to understand that this data could be, well, it will be nil when the first response comes back. And then subsequently it will be filled in the rest of the response data. Some of the things we’re still fleshing out at the moment is how do we give you feedback that the response you’re receiving is empty or nil because there is no data, nil because it’s coming later, or nil because it is actually being marked as null.


It’s kind of three different ways that it can signal that. So we’re still trying to flesh out the details of that. If we do it at the response level, it doesn’t give you enough granularity to the different defer statements you may have in the operation. If we do it in the operation at each of the structures, the pieces of data that are going to be deferred, there’s a few technical details to work out there, but that’s kind of like the direction that we’re turning in.


**Jeff**


Sounds like an interesting problem to solve. And I’m glad this team’s solving it rather Than leaving it to users. I think that that’s a huge deal. Really cool. It’s exciting to hear about. I know Defer has been something that we’ve been really interested in as a company. And it’s great to see Apollo iOS getting that teed up. Next.


**Calvin**


Yeah, I’m pretty excited. I think it’ll be nice to have finally have parity with the other clients for that stuff.


**Jeff**


Yeah, just a side note, I just love the directives feature of GraphQL. It just enables so many things to be, I think logically placed in the structure of an application. It’s such a powerful abstraction when leveraged correctly, and I think defer is a great example of that.


**Calvin**


They’re really very powerful, especially when you tread into custom directives that are kind of specific to, like I mean Apollo will have its own directives, and then even more narrowly scoped than that, Apollo iOS may have its own custom directives. And we do actually have one already with the local cache mutations. So it will affect just the Swift-generated code, but no other parts of the ecosystem.


**Jeff**


Local cache. Yeah, that’s pretty cool. All right. Nice. Yeah, the custom directives are part of the spec too, which is great. So that gives a lot of space for folks to add new functionality to their apps and for libraries to really introduce these powerful concepts. More on that later, I guess, too.


**Calvin**


The beauty of them is that they’re also declarative within the schema and the operations, right? You don’t have to… It’s not going to be some config option that you have to set that’s buried away somewhere. It’s very easily readable within the human readable text of your operation or something.


**Jeff**


I know that’s coming soon, at least the defer bit, to an upcoming minor release. And I know we just shipped Apollo iOS 1.2 not too long ago. Can you give us kind of an overview? What’s new there?


**Calvin**


This one was a minor version bump, but it was a breaking minor version but we can come back later to the breaking bit. But there’s a couple pretty significant things in there. The performance improvements are a really nice win that came out of that. Anthony managed to squeeze out fairly significant performance when you look at the numbers. I think it was 15 to 20 percent in one part and like 75 to 80 percent in another part .


And a lot of this is done under the hood by changing the way that we store the data or how we we pass it around, and then a bit of trickery with generics and the source of data for the executor. And by doing that, you allow the compiler to unlock its own genius in how it’s– I’m amazed by compilers, but it can optimize a lot of the data structures around that for more efficiency.


**Jeff**


So performance improvements. It’s pretty exciting.


**Calvin**


I mean, none of these pieces of code really execute for very long in the context of a single operation. So if you look at it in a single operation, the time gain is really negligible, but it’s when you begin to, over the session time of your application, all of those combined together add up to, well, they can add up to some significant time savings for your app and your users.


**Jeff**


Yeah. It’s interesting that you bring up the time factor too, because I feel like that’s an underappreciated part of GraphQL is that notion of savings over time and really utilizing the cache for saving network round trips and all sorts of other resources over the long term, over the life of your application, or the life of a session, or whatever. That’s cool. Anything else to write home about 1.2?


**Calvin**


Every important bug fixes, there’s a number of them. I don’t know the exact specifics, but there’s a handful of those. And then those are also in the subsequent releases, 1.2.1 and 1.2.2, which was just actually just released yesterday. Then there’s a new CodeGen configuration option. This is where you can specify the access control modifier for the generated models. And this is really to better support modular architectures. One of the big changes in the way 1.0 was built and released is to have this better support for modular architectures. At 0.x with the legacy versions, they could do it, but it wasn’t great. So the way you can structure your code in the version one is a lot better for people that build these modular architecture apps. And when you put code into a module, you can annotate its kind of visibility really within the module and outside the module.


And what we were doing before 1.2.0 is all the code that was generated in the models, they would be annotated as being publicly scoped, which means they would be available outside those modules. It’s not always the best thing for, you know, making sure that you don’t leak pieces of the code around or pieces of the data. It’s also just some teams are quite sticky about what you can and cannot access. So this allows you to now set, it’s either a choice of public or internal, and it’s not available on all configurations. In other words, if you are outputting schema types to a Swift package, if we were to allow you to make it internal, then it’s really kind of dead code ’cause there’s no logic embedded in it and you can’t get into it. So it wouldn’t be able to do anything for you.


In other words, those are always public. But as an example, if you are choosing to manually embed your code in a target that you control and operate, then you’re able to define it as internal. so it doesn’t go anywhere beyond that module. But yes, it’s just offering more control over the visibility of the code that you generate.


**Jeff**


Sounds like a win, honestly, for separation of concerns and things like that.


**Calvin**


Well, there is one more big change, and this is the breaking part of the release, is there was a change to the cache key configuration API. And the reason it’s breaking is that the signature of that function changed. And one of the types of the data that we give back to you is a different type now. Cache key resolution, the source of the data that we hand to you, in other words, to give us the cache key can come from one of three places. That’s either a network response, which is JSON.


It’s either user supplied data through the selection set initializers, or it’s from the cache. And they all are broadly similar, but they’re all unique in one way or another. and the way we were exposing that before as a JSON object is not correct. You had to be aware of some, if it was coming from one particular source, pieces of the data would behave differently, and that’s like, it’s not really great at all.


So what this change does now is it created a new abstract type called object data, which will behave the same across all of those sources, just for more consistency, really. But the thing to bear in mind there is that it’s a fairly new data structure, and it’s not as fully featured as something like a dictionary. So depending on your usage of that data in your logic for your cache key resolution, you may run into something that you needed that is not there. If you do, please let us know and we can build it out for you.


**Jeff**


Nice. And the “please let us know” bit, what does that look like? GitHub issue?


**Calvin**


An issue, yeah, or harass us on Twitter or somewhere like that. An issue would be the best because we can then track it and work against it. Yes.


**Jeff**


Fair enough. Yeah. Keeping track of… I always feel like the Twitter replies are harder and harder to keep track of these days. I don’t know. Something about the planning logic.


**Calvin**


I don’t visit there very often, to be honest.


**Jeff**


Oh, yeah? So GitHub issues then. Or maybe @ us on this Discord. We’ll say hey. And I want to point people also to the FrontEnd channel here, too. They’re a great place to ask anything client-related. Awesome. So as I understand it– and you talked about breaking everything– one of the things that I always like that I liked looking at and reading about the 1.0 release was this migration guide we have. And now some folks might not be accustomed to seeing a migration guide when it comes to minor versions, because there’s a 1.2 migration guide now. Can you walk us through what people can expect looking at that migration guide and more about that breaking change I think you hinted at it earlier.


**Calvin**


Yeah, so there is a standard called semantic versioning, SEMVer, it’s often abbreviated as at, and that really aims to kind of create consistency and give meaning to the underlying changes that happen from one version to the next, right? If you look at the version numbers, x.y.z, x is major, you know, could be completely incompatible API changes like expect something very major to break. Minor is, the definition of it is when you add functionality in a backwards compatible way.


And then patch is when you make backwards compatible bug fixes. So we do our best to adhere to that with the caveat that our minor version releases can include minor breaking changes. It’s not strictly to the spec, But our definition of a minor break-in change is one where you are not required to go and do major refactoring of your code. In this instance, the migration guide says, in the overwhelming majority of cases, it’s gonna be a simple function signature change, and then you can carry on. So you will get a bold error when you upgrade, but it’s completely within your control and relatively easy to migrate. That’s really our definition of it.


**Jeff**


Makes sense. And forgetting that those performance wins and everything, as a trade-off, I’d be fine making.


**Calvin**


We do our best to avoid them, but sometimes it’s a fine line of, “Is this really worth a bump to 2.0, simply because it breaks something for you, or can we live with something that’s slightly more painful?”


**Jeff**


Yeah, and that relationship to the SEMVer, the letter of the law, I think I’ve seen different projects take different approaches to that.


**Calvin**


Yeah.


**Jeff**


So, that’s interesting. Thanks for that. I think we get some questions about that sometimes. It’s always nice to hear what the pulse of the team is on.


**Calvin**


Yeah. The… Now that I think about it, we had an issue recently about the access modifier because that… It’s not… Well, I mean, I suppose it is a breaking change. The defaults changed had an inconsistency in the way we were doing it before where everything was marked as public but some of it wasn’t.


And the the new default is for everything to be internal so if you just you’re not required to add the new configuration option to your config but with the default being internal you are going to get a difference in the generated code output so if you just upgraded and did your code generation again you may get build errors where you You access in things that were public before, but they’re not now. But again, easily resolvable through a single statement in your config that says, “Make everything public like it was before.”


**Jeff**


Makes sense.


**Calvin**


Still surprising, though.


**Jeff**


Yeah, yeah. Yeah, makes sense. Thanks for walking us through that. For those of you who are listening right now and are familiar with the Office Hours by now, one of the things that we do is we have a manifest of a few topics we want to hit in advance and they’re in relative order and so Calvin I’m gonna throw you a curveball a little bit put something Ahead of the line if you don’t mind based on what you’re saying Because you One of the other I think things we get a lot of questions about and I’m curious to hear your take on is Initializers, section set initializers and test mocks. For those If folks are taking an interest in this Office Hours, they might be familiar with these abstractions, one or the other, or both. Can you walk us through what they are and then we’ll drill a little deeper? Test mocks and selection sets and initializers.


**Calvin**


Let’s do initializers first because removing them created the need for test mocks. The initializers are on your selection sets. In the legacy version, you had the ability to initialize them to create new ones with the data that you feed into it, right? In the release of 1.0 and the design of that, we removed that ability because we wanted to make it very clear that these models were the result of a response. Like they were created with data that was given to you by a server through the network, not of your own, like the data that in there was not yours, like it’s come back to you, like these are response objects, they’re immutable, they are contained and you cannot change them. It was a very contentious move.


A lot of people didn’t like it. And we stuck by the design and created Test mocks, which gives people the ability to create these selection sets with Test mock data, which I hope everyone is testing their code. That’s the need that it satisfies. Since then, we’ve come to learn that it maybe wasn’t the best decision, And people use these in ways that we, you know, weren’t on our mind at the time that we removed them. They want to throw them into yes tests, obviously. But Xcode and SwiftUI supports these things called previews. Which is a weird place to be because previews is not it’s not test code, but it’s not deployed production code either. So you have this weird middle ground where you’re throwing test data into your actual app but not, shouldn’t really be there in the first place.


So yes, initializers are back. As of version 1.1.0, selection sets now have initializers and you can create them as you wish and do what you will with them. Test mocks are still slightly different in that they operate at the schema object level. So with test mocks you create an object from the schema and then you build a selection set from that mock data. So you’re still not in your indirectly initializing selection sets, but you have to you have to work with a different plane of data to begin with.


**Jeff**


Gotcha. So we have two kind of sounds like they’re conceptually similar, but there are details that folks need to be aware of. If you were to give like kind of a piece of generalized advice to developers who might be listening to this, what would you say? Like how what should folks reach for today if they’re looking for a way to generate non-production data for whatever their purpose, like a unit test or a preview or what have you?


**Calvin**


Test mocks are useful if you want to seed a bunch of data somewhere and then create a bunch of operations from that same seeded data. If you’re having a very narrowly scoped test or you want to initialize a fragment or an operation to throw at one of your screens to present to the user before you get data back from the server, use initializers, it really depends on what it is that you’re trying to do. Now that initializers are back on the scene, it’s thrown up the question of, are test mocks, do they still have value in the future? Which we haven’t answered yet as a team. So it’s to be determined, I suppose.


**Jeff**


All right. Nice. Well, it’s good to know that we have options available to us and that the team’s thinking hard about which ones and how to give developers the tool they need now and in the future. So, exciting. I think there was something along these lines similar asked in the Discord. There was a pretty long exchange here. I’m posting the link in here, but this conversation did did span a few different things. I chimed in a little bit. I needed a couple of corrections there. But ultimately, the person who asked this question was talking about building something usable out of JSON. And Anthony Miller from Apollo iOS as well responded in line there. Curiosity here, your take on that and what developers should be thinking about when they’re working with raw JSON or when they think they need raw JSON in order to feed into their applications. Yeah, so I went and looked through that thread. And this is…


It’s a user that’s familiar with the legacy version and how that operated and what they were able to do with the data that was given back to them. And it doesn’t work the same in 1.0 now. So they, from what I can glean, they have like custom data structures that they were receiving the response, taking the raw JSON, augmenting it with something else, and then deserializing it into this custom data structure of theirs. With 1.0, you don’t get access to that raw JSON anymore, which means you can’t augment and then you cannot deserialize it. The reason we do that is, raw JSON is just, it’s for data. Like every time you want to do something specific with it or more efficiently with it, you have to convert it into a data structure that’s easier to work with. So we do that within the executor and we do it for the executor, for the cache, for cache key resolution, for cache reading and writing and passing it around just better than JSON does. But then we have no need to keep the raw JSON around, so we don’t. We just, we don’t make it available to you.


You can still do it. Like you could still take the response that’s given to you and using the inner functions of the data structures, end up with JSON that is the same as what you would have received from the server. And then you could go ahead and augment it and deserialize it into whatever structure you want. It’s not straightforward, but it’s also not terribly complex either.


**Jeff**


That’s helpful, thanks. It’s always nice to have a way forward with that sort of thing.


**Calvin**


The power of Apollo iOS really is in its generated code. We generate these very type-safe models, and that’s what we give you the data in. The shape is predefined by your operations. Other clients would give you… They act as the graphical conduit, they go to the server, they come back, but then they don’t do anything on there. It’s just like they just… They barf the data up to you and you do what you want with it. If you’re wanting the raw JSON data, what value is there in the models for you? And maybe that’s a need we need to satisfy too. I’m not sure.


**Jeff**


Yeah, it might go back to what is GraphQL? What are its strengths? What does it unlock for your team? And it might be different things to different people. But I think it does imply certain features, even if they’re not in the spec, or certain ways of developing. We’ve spoken on this in office hours before about the power of a normalized cache and things like that. And though that’s not there in the spec, there are ways to think along the lines of GraphQL and what the tools that it implies. And there are ways to kind of, I guess, work around it. And wherever possible, give yourself a tailwind. Work with the semantics of the language.


**Calvin**


Well, I mean, that’s another whole conversation itself, but it’s a good one nonetheless, is like, you don’t need a whole client if you want to actually use GraphQL. Like you can do it all today just with URL session and like send the request, get the response back and do what you want. But it’s as soon as you tread into the territory of like spec compliance, that’s when things get complicated. Because there’s a million different ways that you can do things and get things back and different things that you have to support. That’s the real value in it. And like you said, the tailwind, making your life easier.


Type safety is great. Utilize it.


**Jeff**


Type safety is great. Utilize it.


I know you’re not on Twitter very often, but, you know. There’s a tweet.


Cool. We talked about the dynamic features of the language more, and its extensibility with custom directives, right? But another, I think, feature of GraphQL is custom scalars. What is a custom scalar?


**Calvin**


So a custom scalar is at the heart of it, a way for you to have different data structures other than what the GraphQL spec provides by default. There’s a number of them, what is it? Like int, float, string, boolean. I think that’s about it.


**Jeff**


Id, too.


**Calvin**


Yeah, id too. But you may have different needs. The one that I’ve seen most often cited is like a date, you know, like date in this ISO format that you then parse into an actual date data structure. So a custom scalar is a way for you to declare a new type within your schema, and then to use that type in the different types within the schema and in your operations. And then once the code is generated, Apollo iOS provides you a way to do the transformation from the data that comes in into this new native data type that you want to use.


**Jeff**


And when– I mean, dates seem like a pretty intuitive way of using this. Have you seen any other kind of usages that are particularly interesting or something to copy or consider?


**Calvin**


I’m going to say no.


**Jeff**


Dates do seem like a pretty good flyer, right?


**Calvin**


they are. I think the miss here is like I said earlier that we work to be spec compliant. Spec compliant is great and all you have to do is support the notion of a custom scalar so you need to provide the framework around you’re gonna get back something that’s not a traditional scalar, provide for the user to transform it and then let them use it. The actual like the types of different custom scalars that people use those could be anything. Actually like I have seen one And it was someone using, they had a lot of difficulty with it, and it was, they were getting location data. I think it was coming back. It was like a dictionary of lat, long or something like doubles. Yeah, that’s one example. Coordinates.


**Jeff**


Yeah, and then you get into the particulars of how we, you know, the intersection of GraphQL and whatever platform language you’re using and how to do that. And that’s really interesting.


**Calvin**


It’s really about providing the framework for extensibility, like you said.


**Jeff**


Custom scalars, custom directives, all these nice ways to kind of declare your intent there. So changing gears a bit, we talked– we kind of opened the session a bit with our first Apple devices that we’ve bought. I should have brought out my iPod, actually, now that I think about it. Because I do have it somewhere. And I’m pretty sure– there’s one of three boxes it could be in. So I should have brought it out. And so a miss on my part. But obviously, there’s a new class of device that was announced at WWDC this year recently, the Apple Vision. Now, we’re pretty far away from talking about that in terms of Apollo technology. But I’m curious, there are a lot of features for the Swift language and others and other platform features that were announced at WWDC. And I’m wondering, what’s got the team excited? What are we thinking about?


**Calvin**


Anthony yesterday actually put up a pinned issue that kind of details all this stuff. Let me just pull it up here quickly.


**Jeff**


Yeah, I can’t remember seeing that.


**Calvin**


Right off the bat though, like WWDC is great. It announces all these awesome new things that you can do and people get all excited about it and they want to go start using them immediately. It’s going to take us a while to adopt any of these things. Just the first point, like it’s going to take a while for them to settle and become stable within the ecosystem. But then we also have to support a few different versions of Swift and the OSes. So while your particular use case might be like, “I can use macros today. I’m on the very narrow front of the line stuff that I can benefit from this.” Not everybody can. And we need to find what is beneficial to everyone using Apollo iOS, instead of what would be cool to build, like what is actually meaningful.


So a couple of the things that were highlighted there is, the first one is macros. So there was one user that already created an issue, very excited to use it. And the couple of the ways that we see this could be used is the ability to to kind of make your existing generated models smaller, we would be able to then change the implementation of some of the code behind the scenes through a macro that you don’t necessarily have to see in the generated code. We might be able to generate your models through a macro. In other words, you feed in the macro, your operation string, or even just the location of a file. And then at build time, get expanded out into the generated model. That’s pretty nice.


I think that would be very cool. It does make, you know, some of the hidden difficulties in this is, like, if it’s not expanded, then, you know, is it less easy to read about the code? Is it less easy to see what’s going on under the hood? Some people may be interested in that. And And then we would also have to think about how we share schema metadata and types. So that kind of a change could require a pretty big change under the hood. What else do we have? Oh, here’s a radical departure from the way Codegen is at the moment. The way Codegen works is you give us your operations and we define the models. What if we flip that around and you give us the models and we generate the operations and the query text that we get sent back and forth?


**Jeff**


That’s interesting. It kind of inverts. I think there are two different… There are code-first and schema-first server implementations, I think. I’m used to GraphQL Ruby, where you wrote your modules and then they would create a schema from that. It sounds like that’s coming to the client, maybe.


**Calvin**


There is a term for it. I forget the name, though. I can’t remember it.


**Jeff**


If anybody knows, pop it in the chat.


**Calvin**


Another feature we saw had potential is non-copyable structs and enums. So I think this is a pretty fundamental change to the way Swift does its value types where you can mark something as… I’m going out of the limit. I haven’t completely read this, but my understanding from the brief that I did see is that instead Instead of having a value type that is copied everywhere, it gets some of the behavior of a reference type, but with the specific limitation that it cannot be copied. So the compiler could optimize storage, for instance, and how it’s parsed between. And it’s really just for efficiency, I suppose.


**Jeff**


I can see why that could be impactful or a dead end, I guess. More research, right? I think it says there, more research needed. Yeah. OK. Cool.


**Calvin**


Swift data integrations. Swift data is kind of a Swift UI layer on top of core data. So is this something that we could leverage to you know for our persistence of the cache? I’m not entirely sure and like Anthony’s got a caveat here is exploratory but unlikely to be a viable solution. I think there’s going to be a lot of hidden complexity in there. But also just around the concept of a normalized cache doesn’t necessarily translate one-to-one to the way all these other data persistence layers work.


**Jeff**


Shout out to our last Office Hours on Apollo iOS too. There’s an immediate discussion on that as well, on normalized caching versus other persistence type concerns.


**Calvin**


So that one would probably need quite a bit of investigation. Mergeable libraries, it to be a way to use dynamic libraries but get some of the benefits of static libraries. I think it still uses dynamic libraries, but it increases the load time and increases the performance of the runtime, basically, for you to get some of those statically compiled benefits. I don’t know if there’s actually anything for us to do here, because that kind of stuff is probably controlled by the app, by the project itself that pulls in Apollo iOS as a dependency.


We may have to add some support for saying that our library is able to be built in this way. But the actual way that that would be implemented is probably going to be in users’ own projects themselves. And then similar to the last one, the privacy manifest. Apollo doesn’t choose to store any data unless you tell it to. So the privacy declaration really is around your app and what are you choosing to store. There are some, like I think our default cache policy for a request is that we will store it in the cache if you give it a store, like a cache persistent store. But again, that’s changeable in any request that you want. So debatable whether we have anything to declare there, but we’ll take a look at it anyway.


**Jeff**


Fair enough. It’s great to hear that the team is thinking about this stuff actively. Some of it seems immediately reachable and others wait to see. But always an exciting time when we get that kind of fire hose of information from Apple and we we get to figure out what to do with it all.


**Calvin**


And that’s exactly what it is, right? A firehose of a week, they’re throwing all these different videos at you and examples and some of the stuff works, some of it doesn’t, but that’s really in the subsequent months that everything shakes out as to how viable it really is. The great thing about the iOS and Apple ecosystem is people generally tend to stay, well, adopt the new technology as soon as they can. It’s one of the things I’ve always appreciated about it.


**Jeff**


And I think that, I mean, coming from the web world, I think that there’s a certain level of, at least in the web platform, we can expect massive change and turn almost at any time. It’s almost like we always have to be looking around the corner. Whereas there are a few times a year, I think, as working in Apple’s ecosystem where you can expect kind of the next big idea, the next big things. And there are clearly big exceptions to that on both sides. but still it’s nice to kind of know to expect news at a given time and to be able to know that the ecosystem is largely bought into moving with it.


**Calvin**


You have a hot take on the Apple vision.


**Jeff**


(laughing)


A hot take on it. You know, it’s funny because I look at it and I guess I’m just such a boring person that I just look at it and I see, wow, infinite monitors. And I go back to my time being an on-call engineer and having to monitor network health for a non-trivial amount of systems at the same time. Oh, wow, that would have been great. I don’t need six monitors now. I have infinity monitors here, and I can keep all my data dog charts open.


Or nowadays, all my Apollo Studio observability insights open. So that’s the first thing I did, is go out unlimited virtual screens. And that alone could probably justify the cost for me. my hot take is extra screens, extra screens, wow. But then I heard it’s like a two hour battery life. So I’m not sure how, if you could keep it plugged in or not, whatever. So me, I’m excited about the monitor space, which is probably the most boring take possible.


**Calvin**


I’d love to try it.


**Jeff**


Yeah, yeah, I am.


**Calvin**


Find the target audience for the first release, but we’ll see.


**Jeff**


Like what would you use it for? Like let’s say somebody gave you one tomorrow.


**Calvin**


I’m actually not sure. And I’ve had that feeling with a couple of Apple products. I remember getting the first iPad that came out. I got it, it was delivered. I used it for about an hour and I was like, I don’t know how I’m gonna use this thing. So I actually packaged it up and I wanted to return it. And then I kept it for a little bit longer. I was like, yeah, this is kind of cool. Like I can get a, like I can find a use for it. So maybe this would be the same. I don’t know, I am not a big gamer, so I don’t have an obvious usage for it that way.


**Jeff**


Well, yeah, and that’s the interesting part is they do seem to be putting an effort into making it about more than just like experiences or games and media in that regard, which is, I think, the most interesting thing to me personally.


**Calvin**


I would get the most value out of something like that if it were… If it were more augmented into daily life. Like if it was something that you could wear, like if you walked up to a bus stop and it showed you all the times for the next buses that were gonna come because it detected the stop and the number and knew where you were, that kind of thing. But I’m not gonna, I wouldn’t walk around with big goggles on my face permanently, right? So it’s a little bit of less value to me.


**Jeff**


Well, it’d be interesting too, from our point of view, like, you know, Apollo iOS is, it says Apollo iOS on it, right? But you don’t need to use it in iOS, right? Like you can, it can be used for macOS apps. It can be used for other things.


**Calvin**


WatchOS, macOS, and iOS. I think there are some parts of it you can use in Linux now, but the codegen won’t work for Linux, not for Swift Linux anyway, because there’s part of the codegen runtime that needs like Mac OS. It is a goal to have it completely, completely, what is the word? The ability to be used in all of those platforms, regardless of where you are.


**Jeff**


We have a question from Dylan. Should it be renamed to Apollo Swift following Apollo Kotlin’s example?


**Calvin**


Well, we’ve spoken about that, haven’t we? The same like Apollo Android went to Apollo Kotlin. I think it’s a nice name. Do we have to wait for Linux support first, though? I don’t know.


**Jeff**


Well, and then we could also run, I suppose, eventually it’ll compile to be able to make a usable binary for Apple Vision, too. We’ll see how GraphQL permeates the metaverse, so to speak.


**Calvin**


What did they say was Vision OS? I’m not quite sure what version of OS it was.


**Jeff**


I think they did say that, ’cause I remember I read something about XR OS or something, but now they’re calling it Vision OS, if I’m not mistaken.


**Calvin**


Which I assume will probably be similar to the way Watch OS works. Not as independent as kind of, although I guess in the last few versions it has become more independent, I was going to say iOS is the big one that you build for, and the other ones are ancillary to it.


**Jeff**


But yeah, good question about Apollo. I guess if we extend that logic to Apollo Client Web, maybe Apollo TypeScript, if we’re following that naming convention. But again, naming things is hard. Fair enough. Nice. Well, you know, if folks have any, I think we touched upon this earlier, but if they have any feedback for Apollo iOS, or anything like that, where should they go? Should they go to our GitHub? Go to come here?


**Calvin**


Yeah, we have a few channels. There’s the obviously GitHub repo, which myself and the rest of the team monitors all the time. So as soon as anything comes in there, we’ll pick it up. But the other channels are there’s the Apollo Community Forums. We have Discord here. And I don’t think we have any public slack spaces to me.


**Jeff**


No, no, I think we’re Discord is folks should, I think folks who should go to Discord for that. But yeah, we’re all on it, right? So give us an app.


**Calvin**


Yeah, any one of those, you can reach us.


**Jeff**


All right, well, Calvin, thanks a bunch for a great office hours. It’s great connecting with you and chatting about these old school Apple devices and all the cool stuff that we can find in Apollo iOS and GraphQL. Folks listening live, look out for the recording in the coming weeks and we’ll see you next time. I think next on the run, we have another Apollo Kotlin office hours coming up. So we’ll get that on the calendar as soon as we can.


**Calvin**


I always enjoy these, they’re nice.


**Jeff**


Well, take care everyone. We’ll close the stage and have a great Friday and a great weekend.


**Calvin**


Yeah, you too.


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
