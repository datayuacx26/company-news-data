---
schema_version: "1.0.0"
document_id: "13ea20526c6e2a242c3108f304fff2a1da10f5bcb6fb30406a984ab20251c3b2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/office-hours-apollo-kotlin"
published_at: "2023-05-19T13:13:15+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:9db703f6e767ec42b19b41f37d5c2156f607edab83dbf5d92579619761fa4f46"
---

# Office Hours: Apollo Kotlin

Here we are with another episode of our office hours!


We have a recording of the stream below, where Jeff Auriemma and Martin Bonnin discuss all about Apollo Kotlin![J](https://discord.gg/NEPmWUdKn7?event=1103357411680600124)[oin the Discord server](https://discord.gg/NEPmWUdKn7) , now so you don’t miss future office hours!


Office Hours: Apollo Kotlin, streamed live in Discord May 10, 2023


## Transcript


**Martin Bonnin**


And hello!


**Jeff Auriemma**


Alright, we’re here!


**Martin Bonnin**


Sorry, I made the mistake we said I shouldn’t do and I was in the wrong stage.


**Jeff Auriemma**


Oh, that happens.


**Martin Bonnin**


But I found it. It had some nice background music. I enjoyed it for a few seconds before I realized it was the wrong one.


**Jeff Auriemma**


Which channel did you go to?


**Martin Bonnin**


The Supergraph one. Supergraph stage. Like we said.


**Jeff Auriemma**


When we met before.


**Martin Bonnin**


Sorry about this!


**Jeff Auriemma**


But we’re here, maintainer office hours, the second one that we’ve ever done. Welcome back to those of you who have returned. We are really excited to be here.


I’m Jeff, Jeff Auriemma. I’m the engineering manager for the Apollo client teams here at Apollo GraphQL.


And I’m joined here by Martin. Martin, do you want to introduce yourself?


**Martin Bonnin**


Hello, everyone. Hello, Jeff.


So my name is Martin. I work at Apollo on[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , help maintain the Kotlin and Android client.


I live in Paris right now, where the weather is really bad. Really, really bad. And what else?


Yeah, I’m also an organizer at the Paris Android User Group. So if you want to discuss Android or GraphQL, and you happen to be in Paris, please come say hi.


**Jeff Auriemma**


And I forgot to mention my location. I’m located in Connecticut near New York City. So yeah, there we are.


So Martin, I have a question for you here. Answer truthfully, please.


If you could go back in time and witness the creation of any technological invention or breakthrough, which one would you choose and why?


**Martin Bonnin**


So I have one, which is, I would go back to the beginning of time if I have the choice.


But I don’t know if the Big Bang counts as a technological breakthrough or not.


**Jeff Auriemma**


I wasn’t there, so.


**Martin Bonnin**


Yeah, exactly. Like if this is an option, I think this would be an interesting time to witness.


But speaking more about like more recent stuff, like more classical answer would be, I guess, going into Steve Wozniak’s garage, see how the Apple 1 and Apple 2 were built.


Just because, I don’t know, there’s so much that has been told about this story that I wish I could see from my own eyes what is truth and what really happened.


**Jeff Auriemma**


That’s pretty cool. Did you ever have one of those computers or use one of them?


**Martin Bonnin**


No.


I started with, what was the name of my first computer? Something called Amstrad. I don’t know how popular this was in the US. I guess mostly a French thing. It was a CPC 6128. It had small cassettes like this, you had to rewind them.


I remember it very well because one day, there was no internet, yeah I’m a bit old, and I wanted to learn programming so you had listings in magazines. So you would go to the store and buy magazines that contained listings and I was something like 10 years old or something like this.


So I manually typed a full game, it was small cars going into some ASCII circuit.


So it was ASCII art circuit and you had a car, you could control it. So you had to enter the coordinates of the circuit and everything. Took me one day, maybe a bit more.


I didn’t know I could save my work because I was 10. So at the end of the day I just turned off the computer. So this is my earliest memories of programming and computers.


**Jeff Auriemma**


I was fortunate enough to have a school with a computer lab when I was young and it was stocked with Apple IIs, the first one I remember.


So I remember those giant floppy disks and that was definitely special for me.


If I could go back in time and witness the creation of any technological invention or breakthrough, I think I might choose, and I’m not super familiar with the history behind it, but the invention of recorded sound, like the Wax Cylinder was the first one, or something like that, I just think what a thrill that might be to be able to witness that moment when music could be preserved through time in such a manner, just because it’s such a gift to humanity.


And until that point, it was kind of every instance of music was lost to lost to time. And then that’s the moment where it couldn’t be anymore.


**Martin Bonnin**


Which one came first, sound or photography?


**Jeff Auriemma**


I believe photography. I think photography was invented before I think, I don’t know, livestream. So there are fact-checkers in the room. Feel free to hit up the chat. But anyway, that was cool.


I feel like I learned something about you, Martin.


But we’re here to talk[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) . All things GraphQL,[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , and we’re really excited to have you here.


And the first question I have for you is about programmers’ favorite activity, which is naming things.


[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) used to be known as Apollo Android. For those in the community that missed that name change or didn’t quite understand the context, could you elaborate on why we changed that name?


**Martin Bonnin**


Yes, so for a bit of background,[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , Apollo Android started in 2016, I think.


It was at the time, it was all Java, because mainly all the Android development was made in Java.


So it made sense to use the same language as Android. And then Kotlin came, I think, Google I/O 2017, I think like six years ago, actually, by day because Google I/O 2023 is today. So 60 years ago, exactly right now, Google announced that Kotlin was the main language for Android development.


And so more and more people started adopting Kotlin and the developers, the maintainers of[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) at that time, was mainly Ivan Savytskyi. I hope I’m pronouncing it right. I decided to introduce Kotlin.


And yeah Kotlin kind of took off from there, like the adoption really grew really really fast. Fast forward to 2020 Kotlin multiplatform began to be a thing and like everybody on Android uses Kotlin and the nice thing with Kotlin is that you can target not only Android but you can also target iOS, the web, desktop, native, a lot of different targets. So you’re not limited anymore to the JVM.


And so we decided to start porting Apollo Android to Kotlin and I think in 2022[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , no, Apollo Android, sorry, was rewritten 100% in Kotlin. So if I do the timeline it’s 2016 initial release 100% Java. 2020, there’s a 2.0 release which introduced Apollo API as Kotlin.


So you were able to use Kotlin, but only for the API part, which is everything but not the network or normalized cache.


And 2023 is 100% Kotlin.


And since it was 100% Kotlin, it made sense to name it[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , just to convey the the idea that you’re not limited to Android.


Of course, a lot of the user base is still on Android. But you can also use it on other platforms, like iOS and the web.


**Jeff Auriemma**


So we’re not moving away from Android, right? Like, people see that, we’re absolutely not moving away from Android.


**Martin Bonnin**


Definitely not, no. It’s still, I don’t have usage number, but I expect it to be the vast majority of users.


**Jeff Auriemma**


You had mentioned other platforms, like iOS and other platforms there too. Are there any others that come to mind?


Like are folks using[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) in like server-side development at all?


**Martin Bonnin**


Yeah, I think so. Like server-side, on the server-side, obviously the JVM is a big one. So you can use Kotlin on the JVM. Guess you can use it in native servers.


That would be interesting, just because you don’t have to pay the price of starting a JVM. There is this whole, but maybe I’m going too fast here, but there is this whole GraalVM thing right now, where you take your programs, your JVM programs, JVM bytecode, and in order to save the JVM and not pay the cost of having to start a JVM in your cloud functions, you basically burn the JVM inside your program so that it all happens at compile time and not at runtime.


So a lot of, more and more people do that, so that’s an option you can do. But if you are really, really wary about startup time and you want to optimize things even more, you can directly compile to a native x86 or ARM binary using Kotlin Native.


**Jeff Auriemma**


That’s really cool. Such a flexible language. Yeah.


Yeah. Lots of options, I think, is how I could summarize Kotlin for sure.


**Martin Bonnin**


And I actually think we do use Kotlin on the back end, even[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) at Apollo.


Like, we use Kotlin a lot on the back end, and we use[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) for, like, back-end-to-back-end communication on the JVM. So yeah, definitely some users, including Apollo.


**Jeff Auriemma**


One of the most beloved features of[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) is its normalized cache.


What is a normalized cache?


**Martin Bonnin**


I’m glad you asked. What is a normalized cache?


It’s a way to represent your data without duplication. I think that’s the easiest way to reason about it.


It takes all your objects and flattens them so that you have only one place in persistent memory where it’s stored.


And this unlocks a lot of nice use cases.


The typical one is when you have a list, detail screens where you have a list of items with a title and description for an example. and you can go into one item and maybe change the description or maybe change the title and more data.


Then when you go back to the list, this data is automatically updated. So this is what the normalized cache unlocks.


It’s a bit… you have to… for this to work correctly because… and I don’t want to go too much into the in-depth of the GraphQL theory, but you can reach a certain piece of data through different GraphQL paths.


Obviously, if you have a to-do list application, you can access a to-do list from the root query, but you might also be able to go to a user and to be able to access all the to-dos from that particular user.


So what the normalized cache does is that it takes all the to-dos and usually identifies them with an ID.


So it requires you to have a way to identify your object and store them in a single location. It’s also pretty handy because in GraphQL you can do overlapping queries. You don’t have to request all the data all the time.


So you can merge two different queries in one single location in your normalized cache. I don’t know if that makes sense.


**jeff auriemma**


I think it does. Yeah.


And to follow up on that, I think a lot of folks come at the normalized cache and are thinking about, especially if they’re new to GraphQL, and they’re thinking about maybe other ways of persisting data that they’re used to and, you know, kind of RESTful architectures and things like that.


I wonder if you might be able to help folks who are still new to this concept kind of draw a line. What’s the best use case for the normalized cache? And where might you be better served by using more traditional persistence methods? Or is it even a persistence method? I don’t know. I’m just curious to pick your brain on that.


**Martin Bonnin**


Yeah so something I didn’t touch on yet is that with a normalized cache you have two versions of it. You have an in-memory one which is lost as soon as you exit your app and another one which is using SQLite which is persistent that you can use for offline mode and stuff like this.


So back to the initial question of what do you get by using a normalized cache, I think The first and obvious benefit is that you don’t have to write any code for it. It’s relatively easy to set up, like it’s a couple of lines and you have in-memory caching or persistent caching basically for free. So this is really cool.


If we take a sample app like[Confetti](https://github.com/joreilly/Confetti) for an example, it’s really just a couple of lines and you can make your app faster or persistent offline which is really cool. And the second really interesting fact about it is what I was mentioning earlier is that your whole app becomes reactive.


You can use your cache as a single source of truth for your UI and then you can have your UI react to your cache and you don’t have to have any logic about remembering to update some components or stuff like this, this is all very easy to reason about. You have all your data in your cache and you just have a UI that is a function of your data.


**Jeff Auriemma**


Now, I understand that not just[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) uses this idea of a normalized cache, right? Apollo iOS, Apollo Client, other JavaScript GraphQL libraries, it’s a pretty typical feature to have a normalized cache. But a lot of times I kind of think of that as being really typical in the GraphQL community.


And I wonder, why do you think this is such a commonly used feature in the GraphQL ecosystem?


What is it about GraphQL and normalized caches that seem to fit together?


**Martin Bonnin**


Good question. I think the schema is what unlocks this, because you have a schema.


It’s easy to… Well, actually, I don’t know. What unlocks it?


**Jeff Auriemma**


I mean, as I understand it, the type name field, at least on Apollo Client in particular, has been a big difference maker there for determining how to normalize a field. But I don’t know. I’m not sure if that is relevant here.


**Martin Bonnin**


If we keep the comparison with REST, in REST you have a list of to-do items and a list of users with to-do items.


There’s no way you can know that users’ to-do is the same as to-do until you add some custom logic yourself.


So you can do all this work manually, but GraphQL, having a schema, allows you to do that work automatically. And this is where tools like[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) comes in handy. It’s doing all of that for you.


So I think this is why it’s become such a thing on GraphQL.


**Jeff Auriemma**


Yeah, it makes perfect sense, that notion of, yeah, you’re getting GraphQL, the networking abstraction, that language and everything like that, and with it comes all of these kind of implicit capabilities that seem to spring up from that. It’s awesome. As I understand it, one more question on caching, because it’s such a huge topic.


[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , as I understand it, enables declarative caching and programmatic caching.


What’s the difference?


**Martin Bonnin**


As I really related, programmatic caching came first. As you said, we need a type name for each object to identify the type of object and an ID for each object.


Both of them combined declare what we call a cache key, which is going to identify a unique object in your graph. And sometimes the ID is not the ID field.


Like you can think of storing books, for example, in your graph. And in that case, what is going to identify a book is maybe the ISBN or something like this.


Or maybe you don’t have an ID because you have only one singleton object in your graph, and this kind of stuff. So in order to define what is identifying your object, early version of Apollo Android and[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) had a programmatic way to do this.


They would basically take a map, a dictionary of key values, and the users would be in charge of taking that map and returning something that uniquely identifies the object. And this is all working well.


It’s very flexible. It’s all programmatic, so you can do all kinds of crazy logic in there.


But it’s also a bit error-prone because if you forget to query one of these fields, one of these key fields, then you won’t have it in your map, and you won’t be able to return an identifier, and you end up with very hard to diagnose cache misses that only happen at runtime.


So[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) 3, what it does is it unlocks declarative caching, which is instead of defining a function where you programmatically return an ID, you extend your schema by writing a small` schema.graphqls` file, or we call it` extra.graphqls` file.


And you can extend your server schema with some client-side directives.


So you can write something like extend type book, key field ISBN. And this would say that every time the normalized cache sees a book object, it can use the ISBN as an identifier. And the nice thing about this is that because the schema is extended at compile time, the[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) compiler can generate queries and and make sure that every ISBN field is queried every time a book is requested.


So this means there is no way to forget a key field and you have a lot less cache misses at runtime.


**Jeff Auriemma**


Nice.


So is there like a specific recommendation you made?


Which approach do you think is kind of the one that people should reach for first?


**Martin Bonnin**


If you have a schema that defines IDs and that is working well, I would say that it’s designed for cachability in mind.


Declarative caching works really well because, as I said, you have real-time guarantees that no key fields are never going to be omitted from your queries. But in some cases, this is not enough.


We had some users that have a really complex way of identifying objects that depends maybe the user or maybe, I don’t know, maybe it depends the time of the day or like the current, like some state that is outside the graph and that is inside the application for an example. and in those cases we cannot do everything at build time, we have to do things at runtime. And so if you have any custom logic to identify your object, you still have the possibility to define your ideas programmatically.


So yeah, declarative first and if that’s not enough you can always go programmatic. I think we will keep both options forever.


**Jeff Auriemma**


Yeah, I know a lot of folks kind of ask about the different caching methods, so it’s nice to have that kind of guidance straight from the maintainer. Thank you.


[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) 3.8 was just released. And to me, the big story with that release was the introduction of experimental extensions for Compose, or Jetpack Compose.


Tell us more about those.


**Martin Bonnin**


Yeah.


So as I say, the nice thing with the normalized cache is it’s allowing you to take your data and have your UI be a function of your data, which is exactly what Compose is about.


Compose is unidirectional data flow. It’s always the same idea that you should have a single source of truth for your graph of UI components. And GraphQL is a way to represent your graph of data.


So it’s really like similar problems or like connected problems. And we feel like until now, we’ve always been very agnostic about architecture or UI, being a networking library.


But because the fields are so close and the problems that Compose and GraphQL are trying to solve are related, we feel like we should be able to do more and make it even easier to consume GraphQL from your Compose widgets.


So we started working with Benoit about defining extensions and APIs that are on top of the runtime, so it’s not going to change any of the runtime APIs. It’s new stuff that is Android-specific, or should I say Compose-specific, because you You can use Compose on other platforms as well.


And that would be a layer above the runtime and make it easier to work with your data as a state. So Compose has this notion of state that your UI reacts on. And this is what we made for these versions. So it’s pretty much still work in progress.


We’ve been working a lot on a small– which is not so small anymore– app, Android app called[Confetti](https://github.com/joreilly/Confetti) , that allows us to dogfood a lot of this.


And we found out that there’s a lot of stuff we can do better, especially thinking of things like retry, for an example.


If we’re talking about Android only, Android knows about the network state. So we could take it even one step further and maybe have the Compose extension be aware of the network state and do some kind of retry when the network goes up again.


Or maybe if we know the network is down, skip trying to go to the network.


So all this cache versus API versus UI component interactions that we are trying to streamline to make it even easier to consume your GraphQL API.


**Jeff Auriemma**


It’s really exciting.


I knew about that feature, but it’s just great to hear you talk about it more because it’s just such a superpower, in my opinion.


**Martin Bonnin**


Yeah, and as always, and I’m going to say that a lot during these office hours, but it’s driven by the community.


So if you feel you have a use case or something that could be done more easily, feel free to reach out because we are designing the APIs, but we are not the one writing the large scale applications.


So we don’t always have all the use cases in mind. So it’s very crucial at this stage that you give us feedback and try it out.


**Jeff Auriemma**


How do you want folks to give you feedback? Should we open a GitHub issue? Should we post here in Discord? Tap you or Benoit on the shoulder on Twitter? What’s your preferred mode there?


I love GitHub. I try to keep stuff more about issues on GitHub than discussions. For discussions, We have the discourse forum, which is community.apollographql.com, which is a nice place. There’s also a channel on the Kotlin lang Slack.


Whichever is easier, I think with these three entry points should be a good start.


**Jeff Auriemma**


And for folks asking questions, yeah, those communities are super welcoming. And you can also use the Frontend channel here in the GraphOS Discord, and we’ll be happy to help you out and to listen.


When will Compose extensions be considered stable?


**Martin Bonnin**


Ha, excellent question. When they’re ready.


Jokes aside, backward compatibility is really hard, especially when for a library that is deployed in the wild on a lot of apps.


I would personally rather wait a bit more and be confident about what we ship and make sure that we don’t break people apps compared to shipping something and break it a couple of months later. I think it’s not a really good experience.


So I don’t know, somewhere in 2023 I guess, that would be a good goal but I wouldn’t make any hard promises there. And most importantly, it depends on feedback. I know this is a very consensual question, but the more feedback we have, the more confident we are about the release and the faster we can go stable.


If we know big applications are using it and they’re happy with it, we’re definitely going to do alphas. So actually, it’s already possible to try them in versions 3.8.


They are experimental, which means you need to opt in the Apollo experimental annotation in your Kotlin compiler settings.


But you can try it out. But we still keep open the possibility to change the API. And so as soon as we have enough feedback and we’re happy about the API and[Confetti](https://github.com/joreilly/Confetti) is working well with it, then it will become stable. And we will maintain it for years and years to come.


**Jeff Auriemma**


Nice.


**Martin Bonnin**


until the big crunch, I guess.


**Jeff Auriemma**


Is that the other end of the big bang? Is that what you’re hearing here, folks?[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) , as eternal as possible.


Cool, and I noticed that you snuck in an “it depends” there, which is always my favorite quote from software engineers, is “it depends.”


So yeah, everybody, try out Jetpack Compose extensions. Let us know what you think. And critical feedback, nicely wrapped. hopefully, is always welcome. Cool.


You mentioned[Confetti](https://github.com/joreilly/Confetti) . You have a sizable Twitter following. I’m a follower, too, and also on Mastodon. And I see you posting about[Confetti](https://github.com/joreilly/Confetti) pretty often. I know that you had even a[recent talk at Kotlin Conf](https://www.youtube.com/watch?v=uATlWUBSx8Q) and Android Makers about it.


For folks who aren’t aware, what is[Confetti](https://github.com/joreilly/Confetti) ?


**Martin Bonnin**


So[Confetti](https://github.com/joreilly/Confetti) is your conference app, ( **Con** fetti Conference). So it gives you all the conference data for mostly Android/Kotlin conferences. So KotlinConf, AndroidMakers, DroidCon, these kind of conferences. And it’s a community project that is led by John O’Reilly and the a community at large. I think there are something like 12 contributors now and it’s growing really fast.


It’s a conference app that’s available on Android, iOS, Wear OS, iOS obviously. It has a backend written in Kotlin and it’s even in Android Auto so you can go to your favorite Kotlin conference and listen to the talks while you are driving. Actually maybe not listen yet but this is a feature request but you can tap on the screen and have it open Google Maps, this kind of stuff. It’s really a showcase of everything you can do with Kotlin on all the platforms.


And for me personally, it’s a good opportunity to try new technology and also to dogfood[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) .


I’ve been working on[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) for a bit less than three years now. And at the beginning, I still had this experience of writing GraphQL. I mean, I was doing a lot of GraphQL in my previous job. And when I started working on Apollo Kotlin, I knew what I wanted to improve because I knew what were the pain points.


And I had a very clear vision of what I wanted. But the more you work in the library world, the more you forget a little bit about what app development is like. And it’s also evolving constantly.


We have now Jetpack Compose, Compose for iOS. It’s always changing. and I’m pretty sure Google I/O tonight will change stuff even more. So,[Confetti](https://github.com/joreilly/Confetti) is a good way to… What was the phrasing already?


Like, “ingest our users’ pain”?


Infuse our users’ pain?


Like, really go in the shoes of what a typical[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) user would feel feel and experience firsthand what’s working well and what could be improved.


**Jeff Auriemma**


There you go. That’s a shout out to Smruti there. I think it was “internalize our users’s pain” and I just looked it up.


**Martin Bonnin**


Internalize!


**Jeff Auriemma**


I’ve written it down.


**Martin Bonnin**


Yeah, that’s the one.


(laughing)


**Jeff Auriemma**


Yeah, that’s really great. So like, can folks kind of, I know it’s open source, a lot of maintainers, can folks kind of who are maybe at the beginning of their GraphQL journey or at a crossroads on app design, is[Confetti](https://github.com/joreilly/Confetti) a useful place to get sample code?


Like if folks are like, well, how did[Confetti](https://github.com/joreilly/Confetti) do it? Is that something that people do or that you would recommend?


**Martin Bonnin**


So if you’re in the beginning of your journey, I would recommend going to the[Rick and Morty example](https://github.com/joreilly/MortyComposeKMM) by[John O’Reilly](https://twitter.com/joreilly) . So same GitHub user, but different repo.


It’s a lot more simple because it’s only a few targets and a few screens because[Confetti](https://github.com/joreilly/Confetti) has got so much traction recently, like there are a lot of new features, which is really cool. But it’s also become a bit more complex. Like we always joke with John how we, like the title of our talk is building a conference app in 40 minutes. And I think it was an initial goal. But if we sum up all the work from all the maintainers over the last few months, we are way past minutes. So if you’re looking for a very simple example, I think the Rick and Morty example is a good one.


But if you want more guidance about how to structure stuff, how to share code between different platforms and have a more real-life sample, then it’s definitely a good one. And I think it’s a good example of everything you can do with Kotlin today. It’s only missing a web frontend, but hopefully we’ll get there soon.


**Jeff Auriemma**


All right, we’ll put it on the roadmap.


**Martin Bonnin**


Yes.


**Jeff Auriemma**


It’s a full-stack app, right? Like you have the server code in that repo.


**Martin Bonnin**


Yeah, actually, this is the origin of this project. I wanted to experiment with server code. So I gave[GraphQL Kotlin](https://opensource.expediagroup.com/graphql-kotlin/docs) a spin, Hi Derek 👋, if you’re listening, which is initially developed by Expedia and maintained by Derek. And yeah, this was really a small playground initially to see what it’s like working with GraphQL on the backend.


Because I knew GraphQL was great on the frontend. I had only good stories about GraphQL on the frontend and I wanted to witness what it was like on the backend. So yeah, I’m sorry, I kind of lost track of the question.


**Jeff Auriemma**


I think that was basically it. Backend, curious to hear more about it.


**Martin Bonnin**


It’s really nice and I really encourage you to try it out. If you are new to backend development, it’s really easy to get started because you don’t really have to know much about backend.


The way it works is it takes your Kotlin classes and at runtime it uses reflection to look at all the classes, all the fields in your classes, and expose them as GraphQL types and fields.


So you don’t have to write any resolver, you don’t have to write any serialization logic, you don’t even have to write HTTP handler, it’s all automatic. So it’s very easy to get started.


And yeah, then we discussed with John how to do something, I think it was, we were hoping for KotlinConf to resume in 2022.


We were preparing a talk and we were like, this could be a good showcase of Kotlin, full stack Kotlin. He started writing the Android and iOS frontend, and we renamed the project based on a suggestion from Baud, Benoit. It got accepted. Ultimately, there was no KotlinConf in 2022. But it got accepted as a topic for KotlinConf 2023, a few weeks ago.


**Jeff Auriemma**


There you have it.


**Martin Bonnin**


We hope we can keep adding features. And maybe someday, I don’t know, maybe make it a white label solution that people can go pick it up, customize it. The sky is the limit.


**Jeff Auriemma**


So yeah, I was curious what is next? That was my next question. But it sounds like you know, it’s pretty wide open right now. I didn’t know you supported Android Auto, so that’s still sticking in my brain.


Can you support, I’m just curious, is CarPlay a target? Like are you able to do a CarPlay thing for Apple? I’m curious, using multi-platform.


**Martin Bonnin**


I guess so. We need to ask Anthony and Calvin and Zach if they have any idea how to do this.


**Jeff Auriemma**


Zach’s on the stream right now. He’s in the audience, it looks like.


**Martin Bonnin**


Yeah, Android Auto was contributed by[Carlos Mota](https://twitter.com/cafonsomota) , who is a GDE, and he made a presentation at Android Makers about it. It’s pretty cool to see all these places where Kotlin can be used.


We got a lot of requests about Apple Watch, so I think this would be maybe the next one.


I don’t know much about CarKit, but it sounds like an interesting one.


**Jeff Auriemma**


Nice. All right. Optimistic updates!


Changing the subject dramatically here. Optimistic updates, what are they?


**Martin Bonnin**


So optimistic updates, I would define them as a way to make your UI react faster to network changes. So typical example, and I’m going to bounce on[Confetti](https://github.com/joreilly/Confetti) . In[Confetti](https://github.com/joreilly/Confetti) , we have bookmarks.


So the way it works is you would typically click a small icon and it goes to the server and the server does some processing and returns a response.


But sometimes you want to give a feedback to the user faster than that if you are on a slow network or like even in general you want the feedback to your user to be as fast as possible.


So what we are doing there is we are using optimistic updates so that as soon as a user click the icon, the icon becomes selected, and the user knows it has– they have selected and bookmarked the session.


And if by any chance the network request does not go through or there is an error, then the normalized cache knows about it and rollbacks all the optimistic updates. This is why we’re saying it optimistic. It’s– yeah, it’s all based on the promise that the network call is going to be successful. And if it’s not, it’s going to roll back all the changes. So it’s working well.


If I go back to the[confetti](https://github.com/joreilly/Confetti) example, I think there are ways to make it even better by making the read-writes, again, more silent. If you want to bookmark something, you want to bookmark it. It’s weird if you press something, it becomes bookmarked and then five seconds later or ten seconds later, if there’s a timeout, it’s reset.


Maybe you switch screen and you will not notice that the bookmark request did not go through.


So the more I think about it, the more I think optimistic updates are a quick way to give instant feedback, but maybe not the best way. Like we can think of different ways to represent data and maybe in that special occurrence, just retrying, like saving it locally to a persistent cache and retry it later when the network is better. It would be a nicer user experience for the user.


**Jeff Auriemma**


Developers can use them, right? There are a few caveats, it sounds like, you know, that folks should be aware of, right?


**Martin Bonnin**


Yeah, it’s there. At the end of the day, whether people should use them it depends.


I use them in[Confetti](https://github.com/joreilly/Confetti) because it was a very quick way. It’s very easy to set up. Like this is the main benefit.


But the more and more I think about it, the more I think there are better ways to do it.


So maybe we should write Compose extension deal with if I don’t know, feels like we have some work to do there.


**Jeff Auriemma**


That’s really interesting actually. All right, cool. We’re going to catch up with chat a


little bit. Pierre has given us the dates. Photography was invented in 1822 and the phonograph in 1877. Thank you.


And Patrick’s saying that they like the idea of using optimistic updates with retries. So, first feedback on that notion coming in, thank you. This is what Discord is for. Speaking of the roadmap, I noticed that better Java support is slated for version 4.


Especially given our embrace of the Kotlin language, can you help the community understand why Java support is still relevant?


**Martin Bonnin**


Well, I think the Stack Overflow survey, or is it the JetBrains survey was out a few days ago, and Java is still in the top three of programming languages. Like, Java is never going away. Like, it will be there after the big crunch, I think. Java has been there for more than 25 years, and there are people, there are codebases, very large Java codebases that are really hard to convert to Kotlin and maybe some users don’t want to do it for various reasons.


So there is still a very large user base for Java outside the Android world. I think Android is mostly Kotlin. But outside of Kotlin we still have users that were a bit disappointed at[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) 3 because we made a few choices that are very good for Kotlin users but not great when you’re coming from a Java background. The two main ones are we now have coroutines in the Apollo Runtime API which means it’s very easy to handle concurrency and do your


requests without doing any job on the main thread or do your requests in parallel. It’s a few lines of coroutine’s code but it’s very hard to consume that from Java. And the other one is using


extension functions for the normalized cache. With Kotlin it allows us to really separate the runtime, so networking and the caching, which means if you don’t want the cache you can just leave it aside and not even pull the symbols or the dependencies in your dependency graph.


And this is all working because thanks to extension functions, we can make the API look very good in Kotlin. But in Java, it’s a bit weird because calling extension functions, you have to do all this ceremony of calling static class, and it’s looking really, really verbose.


So yeah, we’d love to keep it working well for the Java user, especially because, what I did not mention is the code generation, which is really a big part of the value proposition of[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) . And it’s a lot of the work, all the compiler, parsers, and validation. This is all still working 100% with Java.


So what’s missing is a few glue APIs to make it easier to call into these codegen models using Java. So we hope we can make the Java story a bit better there.


**Jeff Auriemma**


Speaking of languages that aren’t Kotlin right now, or maybe that, but WebAssembly, WASM.


It’s gotten a fair bit of hype in the Kotlin community recently, and we’ve gotten some questions about it from the community in the repo.


Where does WASM fit into the[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) roadmap? Or does it fit the roadmap?


**Martin Bonnin**


To be honest, I’m not really sure.


There’s a lot of hype right now because it’s a new target in Kotlin, I think 1.8 something.


I did a WASM target and my very superficial, very high level view of it is that it’s working pretty well with two huge caveats which are one it requires the WASM GC so it requires garbage collection to be enabled in the WASM runtime and this is not something every browser supports. Maybe I should have started with WASM in general. But WASM, it’s a bytecode


I think that you can run in your browser and it’s a new target. So instead of compiling your application for x86 or ARM or whatever or Java bytecode, you compile it for WASM bytecode. So it’s a few instructions and the very core of WASM doesn’t have a garbage


garbage collector and of course Kotlin is mainly a garbage collected language so it works way better with a garbage collection and actually the WASM target in Kotlin requires a garbage collector.


And this is something you can enable today in Google Chrome. I’m not sure about Firefox but you have to go into chrome://flags and then go to the experimental section and then opt in enable WASM GC so it’s not 100% there yet.


So that’s the first big caveat and the second one is from my very high level understanding of WASM is WASM works well for everything that is computations that doesn’t do a lot of I/O like if you’re doing a pure mathematical library it’s a perfect target because you You can ship it to basically everywhere, all platforms.


But if you’re communicating to the outside world, like drawing pixels, getting input, writing files, reading sockets, well, you need to have APIs for that. And I’m not sure how ready that is. And my understanding is that it’s not 100% there yet.


There is an issue open in OKio, which is the library we use for input/output, made by Square asking about WASM support. And I think until this is shipped, it won’t be possible to do much on the[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) side. So maybe one day, but if anyone’s curious, yeah, it’s all open source. There’s an issue, so feel free to comment. We can assign the issue to anyone interested looking into it.


**Jeff Auriemma**


It’s nice to know that we have our eyes on it and, you know, see if the situation now it develops. I mean, that’s an exciting technology for me. I think it’s really…


I think more and more, it will become more and more relevant and more and more widely used as time goes on. But I’m still excited about it.


**Martin Bonnin**


We were talking about JVM start times earlier, and it looks like WASM could also solve this problem.


Yeah, really looking forward to see everything that gets built and maybe we can make a WASM target for[Confetti](https://github.com/joreilly/Confetti) .


We’ll see.


**Jeff Auriemma**


There you go, yeah, widened it out. I mean, that’s the point, right?


We get to experiment with all of these progressive technologies as time goes on. That’s awesome.


Well, that’s all my prepared questions, Martin. I really appreciate you taking the time to talk to the community about[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) and let us pick your brain about all the things that, all these questions that have popped up throughout the past months and things like that.


Is there anything else you want the community to know about[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) ? Where can folks find you? Tell me more.


**Martin Bonnin**


Well, Twitter, Mastodon, GitHub, Discord, yeah, it’s all working.


We have this question right now about the package name of[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) . At some point we will make a v4 release and right now the package name is com.apollographql.apollo3.


So the canonical way to do stuff would be make it Apollo 4, but it’s going to break everyone’s code base.


So we were thinking about keeping it the same just because to reduce the noise around migration and stuff like this.


So if you have any preference there, we have a couple of issues. We have this one.


We also have one about error handling, a couple of RFCs in the repo. So please go check them out and let us know what you think.


It really helps us make informed decisions for the next version. Yeah, that’s it. Keep the feedback coming.


I think we’re happy to have a very open community and very engaged community. So I love it.


So yeah, keep it on.


**Jeff Auriemma**


All right, same here.


Thanks folks who tuned in live and thanks to all of you who are viewing the recording in the future.


And I think we can tie it up here.


**Martin Bonnin**


Thank you, Jeff.


**Jeff Auriemma**


Thank you.


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
