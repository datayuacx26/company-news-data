---
schema_version: "1.0.0"
document_id: "0e58fb65333d33c8371479b3173f1e2b5bcdd524d219fcc0686cd4b32efcdd5a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/office-hours-apollo-client"
published_at: "2023-06-30T10:04:05+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:64c87cc0bfbd289dd2fe0757af8a93ea6e030460bcb086dc4116fb40c964be8c"
---

# Office Hours: Apollo Client

Here we are with another episode of our office hours!


We have a recording of the stream below, where Jeff Auriemma and Lenz Weber-Tronic discuss all about Apollo Client![J](https://discord.gg/NEPmWUdKn7?event=1103357411680600124)[oin the Discord server](https://discord.gg/NEPmWUdKn7) , now so you don’t miss future office hours!


Recording of the office hours, originally streamed live in the Apollo Discord on 2023-05-22


## Transcript


**Jeff**


Hey everybody, welcome back. I’m Jeff, Jeff Auriemma. My pronouns are he and him. I’m the engineering manager for the Apollo client maintainers for web, iOS and Kotlin here at Apollo.


I live in Connecticut, near New York City in the United States. And that’s a little about me, but I would like to introduce to all of you or reintroduce to those of you who are familiar with Lenz Weber-Tronic. Lenz, why don’t you tell us a little about yourself?


**Lenz**


Yeah, hi there. I’m Lenz Weber-Tronic. I do a lot of open source. So I’m on the Apollo client team for the web client. I’ve also done a lot with[Redux](https://redux.js.org/) ,[Redux Toolkit](https://redux-toolkit.js.org/) and[RTK Query](https://redux-toolkit.js.org/rtk-query/overview) .


And generally, like, if I need a pull request, I open a pull request and suddenly I’m a maintainer. Stuff happens. I live in Germany, in Würzburg, which is in the middle between Frankfurt and Nuremberg. I think those names are a little bit better known, at least from the airports.


Yeah, that much about me.


**Jeff**


All right. Lenz, what programming language have you always wanted to learn but haven’t tried yet?


**Lenz**


Honestly, I always wanted to learn C++, but I never had a real reason to. And I would change the question. I would try to learn TypeScript better, like to the level where I really understand what’s going on in the compiler. Because I’m pretty good at TypeScript, but it’s a lot of trial and error. And then I see some people on Twitter that can just debug the compiler to find out why their type is doing the wrong thing.


And that’s amazing. And I want to be able to do that at one point.


**Jeff**


That’s really interesting. So when you say getting to know TypeScript better, and especially at the compiler level, is that something that you want to learn the actual language of TypeScript better? Or is that more in the compiler code?


**Lenz**


The thing is, there’s not a real specification for TypeScript. So you learn the language beyond a certain level by experimenting and seeing what it does when you change a little thing here or there, like in the types.


I mean, TypeScript is JavaScript is JavaScript. That’s nothing special. But the types are not perfectly documented beyond a certain point.


So either you spend two weekends experimenting and trying get a feel for what the programming language, like the type language, actually does. Or you just read the source code and see what the compiler does and where the optimization lies and why this type is compatible with that type, why something is inferred in that way and stuff like that.


That’s not very useful for like normal programming jobs, but if you do what I do as a library maintainer, you want to give people the best user experience in the editor. So you want to write types that give them the best auto-completion and all of that stuff. And for that, you essentially have to know how the compiler works.


**Jeff**


I didn’t know that I wanted to learn that. But now I might change my answer. But my original answer I had, I always wanted to learn[Elixir](https://elixir-lang.org/) . I never did.


I just– I’m a big fan of the Kitchen Sink web frameworks. I spent a lot of time doing Ruby and[Ruby on Rails](https://rubyonrails.org/) . And I always wanted to learn[Phoenix](https://www.phoenixframework.org/) , because I just thought it was probably a faster and more interesting, more cutting edge version of that.


But yeah, maybe one day. That’s my shorter answer there. But actually learning the compiler level things and some of the other stuff you were talking about sounds really interesting. So maybe that’s tied for first.


**Lenz**


It’s also moving target. So you can’t just always keep up with what they are doing in there. So it never gets boring, I guess.


**Jef**


True.


I might also be in denial, too. I think I still do want to learn Rust. But I don’t know. I just feel like that’s the answer that you probably hear a lot, especially from the JavaScript world.


All right. But we’re here to talk[Apollo Client](https://www.apollographql.com/docs/react/) . So let’s talk Apollo Client. And my first question for you on behalf of the community is really about caching.


I want to talk about caching in Apollo Client for a bit. And can you give me a basic overview of how caching in Apollo Client works?


**Lenz**


OK. Yeah, so we maybe take a step back.


Caching, there are two different strategies that are usually used in web applications. You have document caches. that’s usually like your REST cache, something like[React Query, TanStack query](https://tanstack.com/query/v3/) ,[SWR](https://swr.vercel.app/) or[RTK query](https://redux-toolkit.js.org/rtk-query/overview) would do that. So you have a cache that says, if I send this request, I get this answer.


And you only cache on this document level, so per request.


And then there’s the other thing where people do a request and then they get the answer and they split the answer up into separate parts, like they normalize it.


So they pick out all the entities. So we might have a user one and a user two and a user three. And each of these users might have connections to comments and stuff like that.


So we pick all of that apart, store each part of it separately. And that means you normalize your cache and you have a normalized cache.


And then you get to the point where you have an overlap between an old request and an old answer you might have and a new request and a new answer that updates your old request for you because maybe both of these answers contained the same username and maybe that was updated in between.


So that’s a normalized cache. And Apollo Client at its core is a normalized cache where it’s possible.


The thing is, GraphQL generally allows you to do that because you have like that fixed schema for your answers and you can do a lot on that information.


Everything has a type name and almost everything has an ID. So it’s easy to normalize that.


But then you have a lot of edge cases where you might be familiar with like having very similar queries that apply different filters so you get different result sets.


So at that point, you have to denormalize a little bit and go more in the direction of a document cache that at some point goes over into a normalized cache. And that’s where it becomes really confusing, but also powerful.


So I’d say Apollo Client is a 90% normalized cache with a little bit of fallback sprinkled in here and there where it’s necessary to still work, even if you do stuff like having arguments in there or– yeah, I think that’s the main thing.


**Jeff**


Tell me more about that argument. But I guess we notice a lot of questions coming in about cache functionality in Apollo Client. And where do arguments come into this idea of a normalized cache and maybe that 90% normalized cache?


**Lenz**


Well, you see oftentimes fields can take arguments. And then it looks like in your GraphQL like a method call.


And you get to the point where if you call the same field with three different arguments, you essentially do three method calls. And each of them has a different result, even if it’s the same field, like the comments for a user and you say page one and page two, or the first 30 and skip 10 or something like that, or just the user and you give it a user ID. So obviously, user four and user five are different users. And what we do is we store those fields with arguments within their parent entity.


So either in your big query and root type, and then we have cache entries for user four, user five, user six. And as soon as it’s possible, we go over and point it over to a reference to a normalized cache, or you are already a lot deeper in something like you are nested three levels deep, and then you pass arguments into a field. And then we have to do the same on that type. So also again, we store the fields and the arguments together with the response.


And as soon as it’s possible, we go back to a normalized cache, but it might take a little bit until we encounter the first type name and ID. You can make that a little bit easier by creating[cache policies](https://www.apollographql.com/docs/react/caching/cache-field-behavior/) to tell us what to look for. if you don’t have type names and IDs in there, but something else we can use as a normalizing ID. But that’s a general gist.


**Jeff**


Yeah, it’s really interesting. Thanks for taking us through that.


I know it’s a really commonly asked about feature of Apollo Client. And I guess maybe building on that too, the idea of type policies. Do you feel like folks who come to us looking for help and things like that, what should they be looking out for with type policies? Is there something underutilized there that folks should know about? Are there specific kinds of strategies that you see that folks could benefit from in general? Or do you feel like they’re pretty well understood?


**Lenz**


Honestly, I would try to avoid type policies where possible by doing a better schema design. So oftentimes, the type policy is necessary because your API delivers something that we don’t expect. And going back to having every field have a type name, every entity have a type name, every entity having an ID, and also requesting them when you do GraphQL queries.


That already helps a lot, and then you don’t need type policies for that. Of course, if you do stuff like[infinite scrolling,](https://www.apollographql.com/blog/apollo-client/pagination/pagination-and-infinite-scrolling/) or use[fetch more](https://www.apollographql.com/docs/react/pagination/core-api/#the-fetchmore-function) , then you might need type policies that specify how to merge these results. But usually the more you can avoid them by just designing your server more to the standards or best practices, the better you go. Because also you don’t have to ship those type policies to the client, you just save and code.


**Jeff**


Yeah, so schema design, that’s really interesting. And I think that’s one of the interesting parts too about GraphQL is a lot of folks, they come at it from a user interface developer point of view. They might not feel like they can alter their back end code or their BFF, back end for front end code, in their GraphQL service. But build those relationships with the folks who are your graph stewards on the back end. And yeah, don’t accept everything for granted, it sounds like. OK.


There was an interesting question that popped up in the Front End Channel kind of recently that I think is pretty emblematic of this. Maybe not, but it was about an unexpected type coming back after a specific operation or things like that. And so folks wanted to refer to that a little bit.


It was– I think that that schema design might benefit from a second look. But it’s hard to say because I think that some of the details were abstracted over and given placeholder names for the purpose of asking the question. But yeah.


But anyway, kind of adjacent to this, you recently put together– a user was asking about capturing cache metrics. And I know this is something that folks have talked to us about for a while on and off. And you put together a sketch for them. And I’ll link to it in[chat as well](https://github.com/apollographql/apollo-client/pull/10826) .


Can you tell us just a bit about what you’ve been experimenting with there? I think it has a lot to do with the way the cache works. And curious to pick your brain about that.


**Lenz**


Yeah, so their big request was to know when they have a cache miss or when a query goes out and how long that query takes.


And the big question is for us, how do we implement that? in the way that it takes not too much bytes up, because we want a safe and bundle size. If we add a new functionality and only one percent of our users use it, we still ship it to 100 percent of our users, unless we find a way to hook it into the core in a way that can be tree-shaken or that is generally pretty much like space-saving. So, for something like this, that’s a very big consideration to make.


So, what I was doing was I just went into the cache at the point where we decide if we have to make a request or if we could answer it from the cache that I added an event emitter in there and created essentially like a metrics event emitter and you could just add or like lazily load or later load additional code in there that would hook into that and do any additional logic.


In my case, I wrote an example link that could be used in the link queue, and that link could do additional metrics like when a request was started, how long does it take to respond to it, and record all this additional information without it having to be part of the core, which was very important to me. So far we haven’t settled on this design.


My colleague Jerel Miller wants to do a lot more exploration because he’s been doing a lot of that at his old job. But we will probably try to find something that has as little code as possible in the core so we don’t ship it to too many many people, but still exposes everything we can so you can do powerful metrics in the end. But it’s going to be a lot of exploration to get there.


**Jeff**


Makes total sense. Yeah, it’s really interesting kind of seeing this and what you’ve posted in the[pull request](https://github.com/apollographql/apollo-client/pull/10826) and everything. I know at my last job, we were doing a GraphQL experiment, one of the things that I wish that I had had was to better understand how much data we could save through the cache and everything like that. Is that sort of one aspect of this? What else can people expect to learn when this feature is released? With the asterisk, of course, the final design isn’t there. But what would you recommend people use this for, I guess, is what I’m asking.


**Lenz**


I would say it gives you a pretty good overview on which request fires often. So you can maybe look at something like that and see, yeah, I have five requests that have very overlapping data, all of them fire often.


Why not pull that out into one request? Or we can look at places where you see a lot of requests happening in succession and maybe identify waterfalls that allow you to move fragments out and do fragment composition, do one query instead of five queries, stuff like that. And of course, also, it helps you find places where you think it would be a cache hit, and it isn’t. And that might help you find stuff like missing IDs where stuff just can’t be written back into the cache in the right way because you forgot an ID in a query or something like that.


So pretty much stuff into that direction, but it could also just be like a learning tool to understand more what’s going on in the cache, what’s happening in there. But that’s really like the more metrics we collect, the more we’ll see how they can be used. Right now, we have this one use case we were asked for, and everything else is pretty much speculation, I guess.


**Jeff**


That’s awesome. And I think down here, if I’m not mistaken, do we have an NPM, like a PR package on this? Yeah.


So for folks who, if you scroll down in the comments, folks, if you see in the middle there, you see a GitHub Actions bot right beneath Lenz’ comment on it says,[“/release:pr”](https://github.com/apollographql/apollo-client/pull/10826#issuecomment-1531635584) And that’s something that I believe Alessia set up where we can do one-off releases where you can NPM install something that’s based on– that’s in a branch. So it’s not released in an alpha or a beta or even in a patch release.


It’s just there for you to try out in a way that you don’t have to do a bunch of coordination or local package stuff or anything like that. You could just NPM i and then paste that in. So folks are interested in playing around with this.


**Lenz**


Yeah, honestly, it’s so useful that she added that. I’m using it all of the time.


It’s just like you start fixing a bug for someone, and you just ask them, can you quickly install this and try it out? It gives such a good feedback loop. It’s amazing.


**Jeff**


Yeah, it definitely saved me, I think, at least– and I’m not even in there all that much. It saved me definitely many– almost an hour at this point, I would say, mostly because I have to relearn how to do all these local package stuff every time I do it because I do it infrequently. So hats off to you, Alessia.


Thanks a bunch for that. And folks who are interested in seeing what they can make of this, especially if you’re doing something internal, you want to check out any of these metrics for your team or see what you can do or how it might hook up to your existing observability platforms, we’d be eager to hear from you.


**Lenz**


Yeah. Right now is the best point in time to give feedback what you need so we can take that into account.


**Jeff**


Kind of continuing on the topic of the cache and some of the– Lint, I think it would be fair to say, is I think you said it before. I’m paraphrasing here, but you talked about it being kind of complex, the idea of a normalized cache. And that complexity is also powerful. So you have that power and that complexity.


And given all of that power and complexity on this client-side cache that is in the box with Apollo Client, where do you see that all fitting into the big picture of[React server components](https://legacy.reactjs.org/blog/2020/12/21/data-fetching-with-react-server-components.html) , everybody’s favorite topic?


**Lenz**


I think it will still be very relevant. Right now, we get React server components in Next.js only, what they call the[app directory](https://nextjs.org/docs/app) . And I personally think it’s the worst name they could choose because apps are the last type of application that will benefit from this.


The thing is React Server Components and React Client Components, I’ll have to go into that for a second, are also a bit of a misnomer. So let’s talk about what they do and what they are a little bit. React Server Components are a type of component that is executed first from your root component down. And those components can be executed on the server, hence the name, but they could also be executed during build.


They don’t have a specific time that they run. I think someone also brought them to execute in the browser, but that’s probably not a good use case. The thing is, server components have different rules and they have that nice benefit that they don’t need to ship JavaScript to the client to execute.


Essentially, of course, React needs always to be on the client, but after that, you pretty much stream HTML or JSON representation of HTML that can very easily be converted to HTML over to the browser, and there it just exists. It’s not really interactive. So it’s more your layout. And if you want to do data fetching in there, it’s a very good place to do data fetching.


But it not being interactive means it’s pretty much static and non-changing. So it renders out once. And if you want anything in there to change, you need to do the round trip to the server again, who will completely re-render that part and send that JavaScript, I don’t know that, that HTML JSON thingy back where your JavaScript will try to merge it in.


Further down the tree, you have client components. And again, misnomer, those are just like the old React components that we’ve always been using. They can also execute on the server.


In fact, they do execute on the server because when you load your page for the first time in Next, and if you’re on Dynamic, your React server components will execute, and then your React client components will also execute on the server doing a server-side rendering run, and the result of that is streamed over to the client where it’s rehydrated and also gets interactive.


The thing is, those are the components that you are used to, and they are interactive. So you can do stuff like a quick cache update that should immediately be reflected. And you can send a little bit of data to the server or get a little bit of data from the server. And it can just update in the browser without the big round trip to the server, re-rendering everything, building a ton of HTML for no good reason.


So you essentially get two different worlds. you have server components which are for more static data, and you have client components which are for more interactive data. And Apollo Client being the normalized client that it is, is in my eyes at this point, probably most useful in client components because you do stuff like sending off a mutation, getting a very small response and your UI immediately updates everywhere that is used due to the normalized nature. With server components, as you don’t really know where all the data could be used, you essentially have to re-render everything.


You have to get all the data over. It’s probably not very good. So if you think of like an online shop, you would have stuff like the product description which would be rendered in a server component because that probably rarely changes. But when you go down into the comment section it’s very possible that those comments would better live on the client. I mean they can load a little bit later because they are not in view when you scroll there for the first time. Then you want to scroll through pages or have some kind of infinite scrolling, you might want to remove some DOM objects of stuff that’s out of view, like doing something like virtual scrolling. All that stuff is not possible with server components at the moment.


And also, you want to write your own comment and you want to see that update immediately. And you can do that with an optimistic update if you’re in a client component, but you cannot do that with a server component because that will just wait until the server answers.


So that’s my point where I say like the app folder is the worst possible name because in an app something that’s highly dynamic, you will use a lot of client components while on a web page something that’s pretty much static with a little bit of interactivity, you will probably use a lot of server components and a few client components. So it depends on your application. All that said, of course, you can use Apollo Client on the server.


For one, in SSR of client components, we[launched a package](https://github.com/apollographql/apollo-client-nextjs) for that. And on the other hand, also in React server components, where you can essentially decide on your own if you want to have that client shared between all requests to do caching, what I would recommend to create one client per request. But even that can deduplicate some requests for you that would otherwise run twice or something.


I’m not getting too much into the way[Next.js hacks fetch](https://nextjs.org/docs/app/building-your-application/data-fetching/caching#per-request-caching) because I’m not a big fan of that. It doesn’t give a lot of control. It’s essentially like either that everything is invalidated or nothing is invalidated. So you either have to redo all your fetches or you are where you were at your last render. So I’m not too much of a fan of that because it’s just missing granularity.


**Jeff**


Makes sense. You had said in that– this is really helpful, by the way– it is fascinating how the words server and client have been stretched very far, I think, in this abstraction where we have to be specific about client components running on a server. And that seems– I mean, it’s intuitive once you grasp it. But the language certainly is a challenge to get over, I think. So thanks for that.


And one of the things I heard you mention was– and I think it’s a question that the community asks a lot, which is, when you’re structuring your Next.js app, when you instantiate Apollo Client, is it one client instance for your application? Or is it one client or for your server, or one client instance per request? And you had just recommended requests, right?


**Lenz**


I would recommend requests. And that’s what the package that we created that kind of wraps things for you, that’s also what we do by default. For the one server all over, you wouldn’t need our package.


You just create a global variable, and that will be shared between all of the requests coming in. But I would be pretty much careful with that, because as soon as you start using cookies to authenticate a user, you just suddenly start throwing private data from multiple users through each other.


And they will wonderfully merge into each other because it’s a normalized cache. I would be very careful here.


So personally, I think we have been doing like five database requests from when we do server-side rendering. Since the days of PHP, we don’t need to cache everything all of the time. And we’ve always been very, very careful when we share data between multiple requests.


And I think we should stay that way. And I wouldn’t try to over-optimize everything to save a millisecond here and there with a risk of exposing user data.


**Jeff**


Makes sense. Yeah, thanks for walking us through that. And I think last week, you were streaming with Patrick on the library that you were referring to,[Apollo Client Next.js](https://github.com/apollographql/apollo-client-nextjs) . So if folks get a chance, I think we’re going to be releasing a recap of that pretty soon with the video.


**Lenz**


I hope that worked out. unfortunately, my recording cut off. So I hope Patrick has a good recording of that. Otherwise, we might just have to do another live stream. Could also happen.


**Jeff**


Well, I’m always a fan of the live streams. And yeah, I guess for the folks who catch these live, I guess there you go. You’re guaranteed to see it there. Awesome.


Well, looking forward to figuring out what the next steps are there. But there’s also an[RFC available](https://github.com/apollographql/apollo-client-nextjs/pull/9) . I’ll drop the link to the library that we were referring to in the chat here in case folks are interested in following along.


**Lenz**


I’ll just say that again. React server components at this point are undocumented and extremely confusing. Essentially, I’ve been discussing on Twitter with[Dan Abramov](https://twitter.com/dan_abramov) for like two hours, at which point he was annoyed that I still don’t get it, offered me a video call and walked me through it.


And then I still stumbled around for another few weeks until I kind of got everything, wrote that RFC and then Dan reviewed it and still corrected the heck out of me.


This stuff is very unintuitive if you already know React because all of your old instincts are wrong. And it really helps to get that outside help. And I hope that this RFC is that outside help for other people. That’s why I’ve written it down.


I want other people to maybe just struggle for a week and not for two or something like that. I think at this point, people that just start learning React and learn this from the start will have a lot easier time than people that already know React and have to learn what React server components are.


**Jeff**


Thanks for that. When I think of the server components, I do struggle with, I think, a lot of the concepts with them, client and server, and what those mean.


And one of the things that I kind of– I thought that occurred to me the other day was, is it that server components are more about application state and client components are more about UI state? Or is that an oversimplification?


**Lenz**


It could be a little bit of an oversimplification because also application state is something that can be very dynamic depending on the application.


I would say server components are not state at all because essentially all they have is a little bit of a cache for requests, but that’s it.


So I would say they are not about state, they are maybe about caching. And everything that’s state lives in the client because you have no way of really having a long living state in there.


So if you set settings, the only way of having that state in a server component would be that from there you go and write it into like a persistent database and then in the future requested from the persistent database, but your server component itself has no concept of state. It could maybe cache the last request from that persistent database, but it can’t save anything by itself because every request that it gets is completely independent.


And everything you have there is maybe a little bit of data that’s in the cookies from the session, and the rest you have to get together from external data sources.So yeah, user settings could maybe be persisted in a database, but a lot of the time, a lot of the things will be a client-side only.


**Jeff**


Gotcha. Yeah, OK. I think I’m following all of that. And yeah, this notion of having these network transactions that are highly stateless coming through the server component and then having the more rich interactivity and longer term tracking.


I mean, real time, there’s no other way to do it, right? You have to– real time, it’s going to be a client component concern, right? have to monitor network state over time and everything like that.


**Lenz**


I mean, if we don’t want to go back to the first days of PHP where we didn’t have Ajax and Co, yeah, all of that should live in the client. So yeah.


But building on top of that, from my perspective as a Redux maintainer, a lot of apps also don’t have any state. Like, that shouldn’t be forgotten. Not every app has global state. Many apps also just have a little bit of local state in their components, like the form only exists while you’re in the form and only has the data you need to fill the form right now. That’s not global application state.


That’s just local component state, and it can be thrown away after you’re done with the form because you will probably persist it somewhere. You could save the result of that form into some kind of global application state if you needed to come back to it later or something.


But usually, many apps do not have a lot of global application state, maybe apart from the application configuration. But that really depends on the application itself. It’s just with years and years of global application state, we’ve been overusing that and abusing that for things that would never have needed it.


**Jeff**


As I understand it, right now, one of the nice parts about server components is that we can use promises within a component and await a promise resolution and then render the component in that way. But no such capability, as I understand it, exists on client components right now.


But there is this use function. And I think that this is important, I think, for the future of data fetching libraries, including Apollo Client.


And I’m curious to hear if you could give us a little more information about the use function and where we see it kind of fitting into the future of Apollo Client.


**Lenz**


So generally, you’re right. Async functions are officially only supported on the server. Right now, we are all running with a[canary version of React](https://react.dev/blog/2023/05/03/react-canaries) , if you use Next.js, that actually allows them to exist on the client too.


It’s just not documented and definitely not recommended. But you could use async/await and client component at the moment, I just wouldn’t do it. The thing of use is that it integrates with React suspense and use is actually a very simple hook. And right now that it has only two meanings. you can do use context and it will give you the value of the context.


You can do use promise and will suspend the current component, wait until the promise is resolved and then resume the current component. At that point, it will return to you the resolved value of the promise. So you could use a use pretty similar to the await keyword in your components. The interesting thing about use is that the rules of hooks don’t apply.


So where all the other hooks cannot be used in loops, can only be used in very certain situations and only in the same order, and you can never skip one of them and stuff like that, you can just use use wherever and that makes it a very powerful API and makes it a lot more interesting than a lot of the others. For us, use alone is not enough yet because we don’t care about a promise.


We do care a little bit about a promise. We want our first value to be returned, of course, after a while but we also want all the other and later values to be returned and to update our application. Use wouldn’t do that. Use would only take that first promise and suspense once and give you that value back and that’s all use does.


So we still need additional hooks on top of that to create an internal subscription and wait for cache updates and get all of those cache updates. So right now the question is actually how useful it is for data fetching libraries because the vision of the React team here is to essentially be feature symmetrical with React server components.


You have async/await on the server, you need something similar on the client. in my eyes that symmetry doesn’t hold because server components are executed once and will not update because that means you just re-render the whole thing while client components need constant updates to their values. So what we are hoping for and are pushing the React team a little bit on is that use will in the future also support observables and at that point it would be very very useful for us because then you could just get a value from us and use it and you would get all the updates in the future too.


So that’s the missing part of use there that we just don’t have yet. We can at the moment work around that and essentially give you a promise at first that isn’t resolved yet with the first value and then we refresh the component from the side of our hook and give you resolve promises with new values after that. And that would allow us to give you a way to use use like that. But given that at the point in time where we started our suspense journey, use was only an RFC and not available at all.


We didn’t create an API, especially with all of those caveats and things that I just said, we didn’t create an API that gives you something that you would use. Instead we essentially created our own version of that.


We have a[useBackgroundQuery](https://www.apollographql.com/docs/react/api/react/hooks-experimental/#usebackgroundquery-and-usereadquery) which would start something asynchronously and gives you a handle and that handle is something you would like give around like you would that promise in the case if you would be using use and you put that into use read query and that allows you to get the values out of that and that does a little bit more and does all the subscription handling for you and all of those things for it to really make sense on the client.


It’s very possible that use will support all of those things in the future and we are just learning in this journey and it might be very well the case that we at some point have a use usable query hook that gives you everything you need for that but right now use for us as an implementation detail you are not going to call it Instead, you are going to call either useSuspenseQuery, which will suspend immediately, or you will use useBackgroundQuery, which will not suspend, in combination with useReadQuery, which will then suspend for you.


**Jeff**


A lot of new hooks there. And one of the things– I’ve just posted a link in chat here about our current internal implementation of use. I think Jerel created this, and I think you recently added to this as well, Lenz.


So in case folks are curious about how in the library space we work around these sorts of cutting edge React features that may not be available in everybody’s React installation, everybody’s production React version. There that is for you.


**Lenz**


Yeah. Essentially, what Jerel did there was write a naive implementation of what we believe how use would be implemented.


Essentially doing what they say they do, like throwing a promise to change application flow, to get the application to suspend and resolve the promise at some point for it to be picked up again. But this implementation of use is something that is a fallback.


We prefer to use the official React use. But unfortunately, that just doesn’t ship with every version of React. It doesn’t ship with the current stable version of React. It just ships with the current Canary release of React.


The official communication from the Facebook team is that Canary releases are meant to be used by frameworks. So only if you have a framework available that allows you to use this, you have React use.


Otherwise, you will have to use a polyfill or we will automatically fall back to that polyfill.


It’s not perfect because these old versions of React actually don’t really behave very nicely in all cases. Stuff like useId might be a little bit unstable behind that polyfill. But that’s React.


We can only work around it at this point. the official React use, which we will automatically be using if you use the latest Next release, that’s not a problem. That doesn’t have those problems, and useId is perfectly stable.


**Jeff**


The future of use, I think, is something that we all have our eyes on.


And it is interesting to hear the React team shipping more and more abstractions that are meant to be used not necessarily in user space, but in the library/framework space. And that’s an adjustment, I think, at least to the community, at least from what I’m used to. And that’s no pun intended there.


**Lenz**


I’d say, let’s see where it leads. Because when the users–[useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore) ship the first time, that was a feature that was meant only for library developers that no user should ever use in their application.


And right now, we are in the era of don’t use use effect,` useSyncExternalStore` instead. So that narrative sometimes changes. And let’s just see where it goes.


Right now, we are working with what we have and what we get from the React team. And we are trying everything to make it work as best as possible.


**Jeff**


Fair enough. Fair enough.


So moving away from React, Apollo Client is not just a React library. It is for your JavaScript application, your TypeScript application in general. I understand there are some changes coming up. Well,[3.8](https://github.com/apollographql/apollo-client/pull/10340) , we just got a beta out on Friday, I believe.


So congrats to the team on that.


And we’re really eager to hear from all of you, all users of Apollo Client, on how 3.8 is working for them. But one of the features of 3.8 that I understand is some changes to how we report error messages to users in development environments.


Can you tell me a little bit more about that and what folks can expect?


**Lenz**


Actually, it’s a change in development and production environments. Before today, if you were in a development environment, you would always get a full error message string logged out, throw depending on where you are right now. The problem with that was, of course, that all those error messages had to be shipped.


If you are in a production environment, you wouldn’t get an error message, you would get an error number and a little bit of a cryptic error message pointing you towards looking into your Node modules folder into a file that contains, for your version of Apollo Client, the error numbers. And you could look them up in there and then see what the error message was and why it was thrown.


But what was lost in there were parametrizations. So if you had an error message that was like could not do or could not read field foo because of error bar, then if you went into the production folder, you would only get “could not read field … Because of …” and that was all you got there. Because yeah, you look at a static file and that’s not very satisfying. So on the one hand, we had a little bit too much bundle size.


On the other hand, we had not very nice error messages. And if you had a bundler that was not really handling that correctly, you had the worst of both worlds, because maybe you got the bad error messages, but you were still shipping all of the error messages. So not really what you would want.


So what we do instead now by default is that we replaced all the error messages with error numbers, but you will not have to look them up somewhere, but the error throws a link at you. You just click that link, it goes to the Apollo documentation, and our documentation page actually downloads your version of Apollo Client and looks the error up, and also everything that would be passed as a dynamic parameter into those errors will be passed as a hash to that URL. So it’s not sent to our servers, the data is perfectly private, but that page can access that data and just put it nicely into the error message.


So now if you’re in production or in development, you will get a super nice error message with all the values filled in. Of course, you can still opt into the error message bundling, which just means you import a function and execute that function.


And from that point on, during development, you will still get all the nice error messages directly without having to visit an external URL. But it’s not an opt-out if your bundler supports a thing anymore. It’s an opt-in because you want it thing. And that’s a very important distinction, especially when we go into stuff like ESM modules in the browser, where we don’t even have a bundler and would always have shipped both versions and not only one.


**Jeff**


Nice. Well, you mentioned ESM, which I think adds another hour to the stream. But I’ll just ignore that part.


But it is really exciting.


If folks want to give feedback on how the new error messaging is working for them, would you recommend they open an issue in GitHub or talk in the Discord? How would you prefer folks to talk about this?


**Lenz**


If there’s something we need to fix, we need to track that we need to fix it, and an issue is the best way always. If you want to give us a high five, please also open an issue because then we share it around and see it all the time. And if you want to talk to us, yep, can also open an issue honestly, or you just come into Discord. And if we don’t notice, then Patrick will ping us because Patrick’s in there all the time anyways. And of course, that means that we come in there and we can have a little bit more of a chat about So it might be more informal, and it might be faster. It always depends on the individual case, I guess.


**Jeff**


We do talk about these things, whether it’s something that needs to be fixed, some feedback, or just a high five. We love to hear it.


So thanks in advance, everybody. Real quick, I know we’re coming up on an hour here, but I did want to make sure that we covered– since you joined the team, one of the things that we get questions about pretty often is React Native. And React Native is something that we definitely– the people use the Apollo client in their React Native apps all the time.


But I’m curious as to what’s new for React Native users, especially if they have an issue. I understand that we have a[new reproduction environment](https://github.com/apollographql/rn-apollo-client-testbed) for them.


Can you just do a quick voiceover on that one?


**Lenz**


We have an error template now for React Native. And that’s just a super small application that you can use as a starter to build a reproduction of some problem that you have. And the nice thing is that we also added some documentation on how to do things like recording a profile.


And recording profile is something that’s only possible in React Native, at least to my knowledge, since the Hermes engine was introduced. And it’s also something that is only possible for Android. And it’s not really perfectly usable yet, honestly.


So the thing is, usually you record a profile, and it tries to download the source maps after that. and those source maps will then be integrated into that profile so you have readable method names. For one, the Metro Bundler did not really do that correctly, at least not for the Apollo client, so you would still have minified stuff, even we ship our source maps and everything.


So there is another console command you can execute that will apply the source maps on top of that again and then give you a readable profile. That’s one change that we have in there that’s not default. And the other thing is a few useful warnings because if you start the profiler you also run into the problem where it oftentimes starts to execute the last process that was executed in your Hermes engine, which oftentimes is not your application but the Hermes dev tools.


So you profile the dev tools of your development environment and not your application. So we have a warning when we recognize that your profile contains the wrong stuff. You get hints like “reload your application twice before you record a profile, and then you don’t look at a profile for two hours and are completely confused, which definitely happened to me. And then there’s also a fix. We have a pull request for that, but it isn’t through yet.


So we patch a package in that reproduction that allows you to also download that stuff for production builds, because per default, the Hermes profile downloader only downloads the source maps for the development build. And if you do a profile and production build, it will still try to download those, and it obviously doesn’t fit. So I checked like two weeks ago.


That pull request is unfortunately still open, but I hope at some point someone will merge it. Until then, at least in our reproduction, it will work either way. Yeah, so much for that reproduction. That helps you profile things. It helps us get reproductions from you.


It also helps us get profiles from you that we can look at. There’s one more thing regarding React Native. And I already mentioned that React Native ships with the Hermes engine. And that’s the enabled default, I think, for two or three versions at this point. And that allows us to use weak maps, at least from what I’ve seen in the feature.


In the past, React Native apparently didn’t support weak maps, so we have a fallback in Apollo Client that will use normal maps instead, which might over time clobber your memory. And right now, I have an[open pull request](https://github.com/apollographql/apollo-client/pull/10804) . I still need to experiment around with that.


But once that’s merged and released, in an Hermes environment, we will use WeakMaps instead, which will save you tons of memory if you have very long-running applications and fetch new data every two seconds.


**Jeff**


A lot of people have talked to us about memory management and things like that.


**Lenz**


It’s hard when you’re in an environment that’s that restricted and just doesn’t support things like a WeakMap. Because when do you garbage collect things then? It adds a lot of complication on top of us. And it’s also just outdated.


So I’m very happy to see that they are now supported in Hermes. And we can close the chapter there, I guess.


**Jeff**


Well, that’s exciting for React Native developers. So curious to see where that leads us and everything like that.


But it looks like we’re coming up on the hour. Thanks so much for sharing your time with us and with the community. Really excited about all the things that are coming up.


Just a reminder to folks to check out[Apollo Client 3.8.0 beta](https://github.com/apollographql/apollo-client/pull/10340) . Very excited about that. And as well, if you’re a Next user, the[Apollo Client Next.js](https://github.com/apollographql/apollo-client-nextjs) library. and read Lenz’[RFC](https://github.com/apollographql/apollo-client-nextjs/pull/9) if you haven’t already.


I just got a direct message from Jerel not too long ago. Also, Jerel has been the force behind useSuspenseQuery and our suspense implementation, which has been the foundation upon which all of this has been built. And he just said he’s happy to teach me Elixir because it’s his favorite language. So that’s exciting.


So maybe we’ll do that today. But anyhow, thanks, everybody. This has been awesome. And we’ll– yeah, Jerel, all right. All right, Jerel, we’ll talk in our meeting later.


Have a great day, everybody. Have a great week. And we’ll see you next to Office Hours.


**Lenz**


Thanks, everyone.


Written by


Patrick Arminio


[Read more by Patrick Arminio](https://www.apollographql.com/blog/author/patrick)
