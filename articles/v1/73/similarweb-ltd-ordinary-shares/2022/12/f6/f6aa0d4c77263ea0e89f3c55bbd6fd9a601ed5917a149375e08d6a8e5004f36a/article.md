---
schema_version: "1.0.0"
document_id: "f6aa0d4c77263ea0e89f3c55bbd6fd9a601ed5917a149375e08d6a8e5004f36a"
company_key: "similarweb-ltd-ordinary-shares"
company: "Similarweb Ltd."
source_id: "similarweb-ltd-ordinary-shares-rss-2bd7dd1f88a6"
canonical_url: "https://medium.com/similarweb-engineering/taming-the-legacy-beast-a-refactoring-algorithm-in-5-steps-f5de831c37a1"
published_at: "2022-12-14T13:06:50+00:00"
first_seen_at: "2026-07-20T23:19:16.581763+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:f7ef9bdef21e03fb8344bd577318cc4dc3e2e27ddfb44059028be59c30f7cfa0"
---

# Taming the Legacy Beast — A Refactoring Algorithm In 5 Steps

# Taming the Legacy Beast In 5 Steps — A Refactoring Algorithm


[Dor Amram](https://medium.com/@doramram210?source=post_page---byline--f5de831c37a1---------------------------------------)


6 min read


·


Dec 14, 2022


--


You’ve been there. At some point in your career you were probably tasked with changing something in a feature when you could not understand what exactly it was doing.


The documentation was pretty basic, raising even more questions.
After reading the code itself, you still weren’t sure.


When you got to the tests, they were naive and had insufficient coverage.


As you debugged, you didn’t understand the nature of the side effects you were causing.


In a state of frustration, you turned to *git blame*, hoping to find someone who could shed some light on this code. Only to realize that the is author is YOU.


Press enter or click to view image in full size


A while ago I was tasked with working on one of our team’s core projects. We wanted to add support to a new use case, but the entanglement of the code made it a very difficult task.


The codebase I’m working on started as a clean project. Thorough research was conducted, the design was simple, concerns were separated and things were looking quite good.


Unfortunately, as often happens, somewhere along the line things changed. Feature requests started to pile up and “minor” compromises were made.


The implementation details became part of the business logic, and the separation between the abstraction layers became a bit fuzzy. Not to mention the code itself, which now had very specific business logic conditions in very unexpected places.


Clearly some refactoring had to be done, but this was a bit like opening a Pandora’s box…
How do you change one of the team’s main projects while still running in production? How do you modify it without affecting existing performance? How do you approach code when you’re not familiar with all its bits and bytes?


In this post I will do my best to answer these questions using the *Legacy Code Algorithm.*


## The Process


As mentioned, for this task I used the *Legacy Code Algorithm,* described in the book *‘*[Working Effectively with Legacy Code](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052) *’.* This ** algorithm provides a few simple steps you can take to handle legacy code as smoothly and cleanly as possible.


But before jumping into it, let’s keep in mind what it is that we’re aiming for.
The main goal of refactoring is to make adding or altering features easier. It’s kind of hard to do so without understanding how exactly that domain behaves. In order to do that, we’ll simply need to see how it handles itself in different scenarios, or, to put it simply, have a bunch of tests around it.


## The Legacy Code Algorithm


Back to the algorithm you can follow in order to achieve that goal:


1. **Identify Change Points**


The first thing you need to do is understand exactly what your new feature requires and how it interacts with the existing code base.


2. **Find Test Points**


Once you understand what parts of the code you need to alter, you’ll want to add tests around those parts. You need to do this in order to make sure your changes are only doing what they’re meant to do.


You’ll want to do so in the smallest granularity possible. This will help you understand the existing flows of your code and where the road to writing those tests is easy. One of the techniques we use, when trying to prioritize testing areas in the code, is thinking about it as a network.


Say that you’re interested in understanding how the data flows in your code.


Imagine each function could point where it’s getting its data from. Now imagine giving each function a score based on the pointing of other functions.
The functions with the highest scores would be considered good candidates for data sources.


Luckily —[Google’s PageRank algorithm](https://en.wikipedia.org/wiki/PageRank) can provide us with this exact knowledge. Without going too deep into its implementation, lets look at the following example:


Press enter or click to view image in full size


Here we can see that we have a function that our data is coming from (\` *query_db* \`) and the data flows are affected by it. In this small example, it’s quite easy to figure it out, but in real-life this might not be the case.


Today, most languages have tracing mechanisms built into them.
In this case I’m using Python’s *\`trace\`* package. When running it on the following code and formalizing it into a graph (code snippet available[here](https://tinyurl.com/tracecodegraph) ) we get the following image:


By looking at the graph attributes, we see that *query_db* received a high pagerank score — meaning we’ll want to start testing the types of data there.


Press enter or click to view image in full size


3. **Breaking (Bad) Dependencies**


After identifying all the areas in your code that you wish to test. You may discover that testing them is not as simple as you imagined. Some of the functions might be way too long and/or perform multiple operations, and as a result they’re too difficult to simulate.


You’ll first need to divide the functions into smaller chunks of code, based on the parts of the procedure it is trying to do. There’s a lot to be said on what the guidelines are for this kind of modification, but that’s beyond the scope of this post.


4. **Tests, Tests and Some More Tests**


Once you’ve separated your functions into smaller parts, you start with writing your tests. You will find that writing an isolated test has become an easier task.


You still have your integration tests that check the entire flow from E2E, but now you can introduce the relevant unit tests that cover all possible use cases.


5. **Make Changes and Refactor**


Once you’ve done all of that, you’re finally ready to start the work you wanted to do all along.


Your code looks a bit different, it’s changed from its initial state. It is separated in a better way, less coupled and perhaps even more readable. Not to mention that you now have your tests to alert you if anything unexpected happens.


## The Insights


Once you start refactoring and making your changes, you might notice that modifications to the code don’t feel as risky as they felt in the beginning.


But you should keep the following in mind:


- This process should be done in baby steps. You’ll have multiple iterations in which you’ll change and test a bit of code each time, but it’s a necessary phase
- The “healthier” your codebase is — the quicker this process will be
- Some of these steps may take more or less time on different projects


Refactoring production code is a complex task. Although this scaffolding process may add some additional work to an already long process, sticking with these principles will significantly increase your chances of doing it right.


## What’s next? Expecting the Unexpected


So you’ve reached a point where you can now add your new desired functionality, but where do you go from here? How do you make sure that the same process that brought you here won’t repeat itself? The urgency of delivery is a feature, not a bug, and as such it will follow developers through every step of the development process.


Our code must be written in a way that enables adding changes to it at a minimal cost, without having to rewrite parts that are not relevant to the change. We must maintain a clear separation of concerns, so that when changes come, and they will, they will be isolated to the specific domain they are related to.


Once you’ve located the players on the field, and the types of interactions they have, formalizing it into an API becomes an easy task. Fortunately, we have tools to help us face these exact types of challenges.


I found that, when tackling these kinds of challenges, sticking with Domain Driven Development and[SOLID](https://en.wikipedia.org/wiki/SOLID) principles, among other things, can be extremely useful.
In my next post, I’ll elaborate on how we use these methods in practice to minimize development efforts and deliver *quick & clean* code.
