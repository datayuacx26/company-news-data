---
schema_version: "1.0.0"
document_id: "f0f1ee38fd3f3858765be8d6a116727448505a668135e5a60ec5fcb83637e6d7"
company_key: "yc-nextmv"
company: "Nextmv"
source_id: "yc-nextmv-news-import-d2226782a65f"
canonical_url: "https://www.nextmv.io/blog/in-conversation-with-pyomo-and-highs-open-source-in-or"
published_at: "2026-03-24T00:00:00+00:00"
first_seen_at: "2026-08-19T05:42:15.544580+00:00"
fetched_at: "2026-08-19T05:42:17.807879+00:00"
content_hash: "sha256:a9f43fec055455f7aba50943ef906ac4df50a263a6bebeb0b188a239172072c2"
---

# In conversation with Pyomo and HiGHS: Open source in OR

Pyomo and HiGHS are part of the landscape of open source projects in the operations research and decision science space. Tiffany Bogich, Nextmv Head of Product, hosted a conversation with members of[Pyomo](https://www.pyomo.org/) and[HiGHS](https://highs.dev/) to discuss the differences between building research software versus commercial software, how their projects are maintained, and the future of open source in OR.


*The following is a companion interview to*[a longer, related conversation](https://www.nextmv.io/videos/open-source-in-or-q-a-with-pyomo-and-highs) *that has been edited for length and clarity.*


‍


**Tiffany Bogich** : Let’s give an introduction to the origins of Pyomo and HiGHS. Where do your logos come from?


**Julian Hall** : The HiGHS colors are sampled from the Manchester United website, and the name comes from the initials of the original contributors: me and Ivet, plus Qi Huangfu and Lukas Schork, who wrote LP and simplex solvers as PhD students. It all started because we had their solvers and codes, but I could do nothing with them. Ivet could write C++. She’d done LP pre-solve and she melded everything together, and we started turning gradware into software so that there were utilities for LP modification. Then she got into build systems, thankfully, and CMake.


**Tiffany** : John or Miranda, do you want to talk a little bit about the Pyomo logo and origin?


**John Siirola** : It was completely made up. Back in the day, we did a lot of work with mostly algorithm development, and we had a number of other packages. One of them happened to be a package called Acro. It had a bird as a logo. There was another utility library called Utilib, and it had a set of birds as logos, so we had a bird thing going. Bill \[Hart\] liked birds and he had gotten enamored with some other logos that were an origami style. We went to Doug Prout, who was one of the graphic designers at Sandia, and said, "Can you do something with this?" And he came back with the logo. It doesn't symbolize anything. It doesn't mean anything other than we have a thing about birds. Since then, all of the other parts of Pyomo have been using the same set of birds. We keep commandeering the birds from the PySP logo and the main Pyomo bird, and they just keep appearing in different forms for all the various subparts of Pyomo that want their own logo. They tend to adapt and modify those.


**Miranda Mundt** : My personal favorite is the Pyomo Design of Experiments logo because it’s the bird with a little jacket and some lab goggles. It is the cutest version that we have out there, for sure.


**Tiffany** : We also have a bunny in a similar kind of get out, so we have a bunny thing at Nextmv. Let's dive a bit further on the context. Both projects come from academic backgrounds. Can you speak about the difference between building research software versus commercial software and how you've had to adapt and grow from that beginning.


**Miranda** : I’m what’s called a research software engineer, and if you haven’t heard that term, you’re not alone. It’s very new. When it comes to research software engineering, the idea is that you really have to straddle that line in between something that’s more productionized and something that’s still exploratory. That’s something that we do a lot within Pyomo.


John refers to Pyomo as production research software. We have this particular area of Pyomo that we call core, which is what we consider to be the most stable. We expect it to have some amount of backwards compatibility. We’re in the process of formalizing that promise of stability right now and writing it down so people can understand where we’re coming from. We don’t want people to be in a situation like Julian talked about where you have code that you’re trying to run and it just doesn’t work, it’s not easy to use, it doesn’t have documentation, or it may have worked a year ago, but it doesn’t work now. So we have this area that we think of as more productionized and much more stable. Then we have our more Wild West area where people can do whatever. We have some standards on a certain amount of quality and testing, but it’s significantly looser. We’re also in the process of codifying that as well to make sure that people really know what the commitment is in those areas. It’s all about right-sizing and we have a strong focus on that – making sure that we’re doing the right thing for the right different part or the right different intent of that type of code.


**Julian** : Although we’re based in an academic context, HiGHS is not really an academic project. For a long time it had no research relevance at all because it was mainly a case from the original solvers which weren’t research projects. It was mainly a case of implementing established techniques. Some academics will use it, but a lot of academics will use the commercial solvers because they get free licenses. It’s more like a small software company for people who can't afford commercial software, but we now have PhD students funded to work on HiGHS and so some novel stuff will come in. There's[a paper on our new interior point solver](https://arxiv.org/abs/2508.04370) , which is probably still more industrial than academic.


**Tiffany** : Thinking about project relevance, what applications have you seen that have surprised you? Where are you seeing adoption in HiGHS that you didn't expect?


**Julian** : The open source energy modeling ecosystem is one that we just hadn't realized how big it was and that's been very important for us. Otherwise, I’ve been in this game a long time. I’ve seen a lot of applications so I was not surprised by most of the things we've seen.


**Ivet Galabova** : I’ve been surprised by the water management application and how much logistics is going on there. That’s one of my favorite applications.


**Tiffany** : I know that with open source it can be hard to know exactly what people are doing, but at conferences or maybe elsewhere, what have you heard about Pyomo use cases?


**John** : We're always surprised. Quite frankly, the fact that Nextmv was using Pyomo at all was a real surprise to us a number of years ago. Every so often you do a little bit of gratuitous self-Googling to find out who's using Pyomo, and we’re always amazed at what comes up at the top. For instance, a consultant for the California Energy Commission wrote an expansion planning model. It turns out that that’s in an open model and it was written in Pyomo. We have colleagues in the universities who use Pyomo as part of their core curriculum.[Al Holder at Rose-Hulman](https://www.rose-hulman.edu/academics/faculty/holder-allen-holder.html) gets a particular shoutout because he started using it in about 2011. Pyomo was not terribly functional in 2011, so we’re always surprised at where it pops up and who’s using it.


**Miranda** : I think the most interesting one I’ve seen, because I keep track of what GitHub says are downstream repositories and packages, is Facebook has something called Nevergrad, and they list Pyomo as a dependency, and I thought, "Oh, Facebook research is using us? How exciting."


**Tiffany** : How do you find out where your users are? I know you mentioned GitHub downstream repositories, are there particular conferences, events, or journals that people are publishing use cases in?


**Miranda** : All over the place. I once was at a conference for something entirely unrelated and heard somebody at a table next to me talking about how they were trying to learn how to use Pyomo, and I whipped around and said, "Wait a minute. Let’s talk about this."


**Tiffany** : Ivet and Julian, I know you hosted your first HiGHS workshop in 2024, and you just had another one. I would love to hear about what you’re learning from those events, who’s coming, what are the big topics, and how are people using HiGHS and talking about it.


**Julian** : Airports and flight scheduling, all sorts of energy things, people scheduling battery farms, etc. The very nice one is a company in Denmark that produces software for managing small power plants, multi-renewable power plants. They’re using HiGHS, and we found out that among their customers is Edinburgh University. Edinburgh University’s estate is managing its power systems, and the problems are being solved with HiGHS, which is really cool. It’s also interesting to hear about the GitHub approach to finding out who’s using us.


**John** : It’s particularly effective for Python packages. I don’t know how well it works for other things but for Python, because there’s a standard way of declaring dependencies, it seems to be able to scrape that pretty well.


**Miranda** : It can sometimes inflate the numbers a bit though because if anybody who makes any package or repo pulls in Pyomo, whether or not it’s something formalized or they’re just playing around, it’s going to show up as, "This is something/someone that’s using Pyomo."


**Tiffany** : John or Miranda, if you could speak to technology choice, I believe Python was chosen in 2008 when Java and C++ dominated. It seems like that may have paid off. What other technology decisions did you have to make? What are you grappling with today in that space?


**John** : The short version of the long story is that several of us on the team, including Bill, myself, and John Paul Watson, learned Python by writing Pyomo. And by the way, if you ever dig into the code and think, "Oh my word, why is this done this way? They don’t know what they’re doing," well, actually we didn’t. And some of that code is still buried in the core. I distinctly remember when Bill looked around and said, "You know, there’s this Python thing, and it looks like it might be useful. I think I want to learn it. And I’ve got this project that needs an algebraic modeling language that is extendable in a way that the commercial ones aren’t. So I’m going to have to write this anyway. Let’s use this as an excuse to learn Python." We quite literally stumbled into it.


**Miranda** : We actually just wrote[a paper about the history of Pyomo’s development](https://www.osti.gov/biblio/2585602) and a holistic look back on choices that were made – maybe some bad ones, maybe some good ones. I think one of the best things though, at least for the Pyomo team over the years, is that we haven’t been afraid to try something new, to throw away something that is causing us lots of bottlenecks, and to say, "Well, let’s just try it and see what happens." It’s worked out pretty well for us for the most part.


**Tiffany** : I love the title of that paper, "Accidentally outrunning the bear." It very much captures that journey. Ivet or Julian, when making early technology choices, how did you navigate that path? And also consider the present, with adding GPU acceleration and some newer things in the OR space.


**Ivet** : The original simplex solver left to us was C++, and then Python was very popular so we had to add Python. Julian knew Fortran from before so we added Fortran. And C# ( the .NET part) came later on and we're not really .NET experts so we just support that and count on more competent C# programmers to help us with that. Since then, we've had interfaces for many languages, mostly contributed by our users. We have R and Rust and even JavaScript. I think there’s an Excel plugin somewhere as well. We’ve tried to provide as many interfaces as we can so that people can use HiGHS from their system.


**Tiffany:** That’s a great segue into talking about the communities behind your two technologies. I’d love to hear what participating in Pyomo or HiGHS looks like, how the community has evolved, and how the community can best support an open source project.


**John** : At first, we were still surprised we have a community. The Computing Research Center at Sandia has a long history of being encouraged to release software and get it out there to make it useful to others. A lot of the software that’s developed through the research that’s done there is released open source. The corollary is that we didn’t write Pyomo for the community, we actually wrote it for *us* and then released it in the hope that it might be useful to others. Now we’re getting very close to 20 years later, and there is still a very active community. I should have said this earlier, but when you want to find out where your users are, the first thing you should do is break something and then they come out of the woodwork.


In the last several years we’ve had a large number of developers coming from outside of the original core team in Sandia, including some really active developers from academia, but now we’re also getting several developers from commercial solvers. They are starting to actually work with us to maintain and expand the interfaces between Pyomo and their particular solver which is fantastic. We are always happy to have more folks involved, whether it’s writing code, asking about documentation, or coming up with new ideas. It’s grown organically, but it’s gotten much bigger than we ever thought it would be.


**Tiffany** : What does participation look like? Do people just come to you or do you find folks that want to contribute?


**Miranda** : We’ve gotten weirdly lucky with it, honestly. We have a weekly development call on Tuesdays that’s in our readme. So if someone wants an invite, they can just send an email and let us know. We also have a distribution list that goes to all of the main core developers. So if one of us is out, somebody else can reply to it. But we’ve gotten very, very lucky with some of these, especially these commercial solver folks. We got an email this week from somebody that said, "Hey, I’m interested in adding this support to Xpress into Pyomo." And we responded, "Okay, please come to a meeting. Come talk to us about it." We tend to have people who join pretty regularly from these communities. It’s become part of their Tuesdays. We’re also more than willing to do things like reply to long emails with our own long emails or work through lots of different PRs with them. It seems like a lot of people have mostly come to us, for which we are very fortunate.


**Tiffany** : I’d be curious to hear about the HiGHS community and what you have seen in terms of folks not just using the solver but wanting to contribute to it, and what that has looked like?


**Ivet** : It’s a bit more tricky on the solver side. We have had contributions on the interfaces, which we're very happy to accept, but as far as the solver is concerned, we've been more careful about what we accept because we have to ensure we can maintain it in the future. It's not as easy as for a modeling framework, but mostly people have gotten in touch with us, and we have friends in some modeling languages who have helped us a lot with the integration.


**Julian:** One particularly nice contribution is through[our relationship with MathWorks](https://blogs.mathworks.com/matlab/2025/03/14/advance-optimization-software-fully-funded-phd-with-highs-and-mathworks-at-university-of-edinburgh/?s_tid=srchtitle_site_search_2_HiGHS) . They decided to ditch the MIP and LP solvers they were using internally, which meant that the person who was supporting their MIP solver could start working for us. That’s helped to develop our MIP team, which was previously a bit thin because we just had somebody who wrote the original solver leave. Getting people to contribute to the interfaces in languages that we just can’t handle is particularly useful.


**Ivet** : We also like the problems. It’s always good to have more problems to test HiGHS on, and we do miss edge cases sometimes. If a user gets in touch and has a funky model that we can look at, then we can catch why it’s being funky. We've made many improvements to HiGHS with models like this.


**Tiffany** : Miranda, you mentioned you’re a research software engineer. Can you talk about how software quality practices and the thinking behind that shapes Pyomo's development, the community around it, and how they interact with Pyomo?


**Miranda** : Something that I mentioned we're in the process of codifying, something that we try to be really cognizant of within Pyomo, is making sure that we’re applying the right amount of quality to the type of work that’s being done. We have this core part of Pyomo that we consider to be more stable, and there we have a lot more strict guidelines of what quality means. We want to have a certain level of code coverage to make sure we’re actually testing things appropriately. We want to have a certain level of documentation. It’s not always perfect, there’s still some legacy things out there that we’re trying to fix, but we take a very careful, long-term approach to changes. For example, in terms of our solver interfaces, we’re on year three of probably a three to five year effort to completely reinvent the backends of our solver interfaces. This is something we’re doing very slowly, very carefully because we don’t want to mess up backwards compatibility for folks who are using our current interfaces. We’re doing all of our changes in a highly controlled location. We call it "contrib", as in anything that’s contributed. That’s a bit of a misnomer, so we’re in the process of shaking up what those naming schemes are and what we expect from people. But in that Wild West “contrib” area, we tend to be more loose with what these requirements look like. We still want some minimal correctness checks because when we’re looking at the more researchy side of things, the bigger emphasis is, "Does this work the way that it’s actually intended to work?"


Once upon a time I wrote this paper talking about a tiered approach to software quality for research projects/scientific software. Tier one is I’m throwing darts at a dartboard, I have no idea if this experimental thing is even going to work. It’s not like it’s really important for me to have a solid automated testing setup right now because I don’t know if I’m going to actually get that 2 + 2 = 4. At that stage, the bigger focus is going to be on “Am I saving my data? Am I saving my points in this process? I have something that works today, but is it going to work tomorrow?” Then you start building from there. That’s the approach we try to take within Pyomo too. If you're just experimenting or playing, we want a minimum amount of correctness before we’re going to incorporate it in, but we’ll work with you on that. We’ll look through it from a mathematical perspective and from a general software quality perspective. We do have certain things that we apply everywhere, like using PEP 8. We use[a tool called Black](https://black.readthedocs.io/en/stable/) to apply PEP 8 everywhere, which is the Python standard for what they would expect file formatting to look like. It’s how many line breaks, naming schemes, etc. John hated me when I originally made us do that. But now he tells me he can’t read any files that haven’t been blackified because it’s just so built into all of us.


**Tiffany** : I've heard HiGHS described as going from gradware to software as part of your journey. Can you talk about how you've managed knowledge transfer and standardization in a project?


**Ivet** : I was an intern at Google just before we started putting HiGHS together, so I had the great fortune of observing Google's code from the inside. I was really impressed, and so from the start I've insisted on the style guide that Google used. We've had to adapt and make exceptions, but mostly I learned how to format, test, and manage code, and how to keep code modular. It’s not always straightforward. The transformation of gradware to software was non-trivial and took a long time and a lot of work. We spent a lot of time early on designing the C API, which was going to be used across interfaces, and we wanted to make sure it was consistent and had no internal contradictions. We're also very careful when we add things to HiGHS to make sure they don't contradict what’s already there. We try to eliminate the problems before they appear, but that is time consuming and it takes effort as well. I've been really fortunate to be in a position where I have the time to focus on things like that, which end up being more important in the long term.


**John** : Software engineering is a huge aspect of this. The other one we found, which makes a little more sense when you realize that Pyomo has actually never been a funded project, is that we have a lot of graduate students involved at a number of universities. One of the goals of the project is to get the research into the hands of practitioners and so the vehicle we use is Pyomo and derivative projects. One of the things that has been incredibly effective for helping us to bridge the gap of "How do I take gradware and actually make it usable?" is pairing graduate students with somebody from the core package development team – someone from either the IDAES team or the Pyomo team. They start working with the graduate student at the very beginning, long before the code ever sees a public repository or a pull request. We start working with people pretty early on because, as Ivet is hinting, if you start with some good practices at the beginning, that goes a long way.


Another aspect is to let the graduate students do their own exploratory research, but also check in and say, "You know, that’s probably not a good design." None of them are getting degrees in software engineering. They’re operations researchers, they’re engineers, they’re something else. So part of their education turns out to be "How do we think about research software? How do we go through good design, and how can we avoid a situation where a graduate student goes off and throws darts at a wall for five years?” One of the big ways we’ve worked on this is starting way earlier than you’d think reasonable. Some of the new capabilities that you see popping up have been in development for years – lurking on the sidelines, being refined, and going through formal design and review long before the public ever sees it.


**Tiffany** : That experience is incredibly valuable for students that may end up ultimately working in different industries or commercial spaces and need to make these things actually influence real decisions.


Let’s shift gears by talking about sustainability in the future of open source and OR. I would love to hear what you’re excited about, areas of focus, or things that you’re concerned about.


**Ivet** : There are a few exciting things happening, mostly in linear optimization, particularly the GPU hype at the moment. HiGHS has started working on a first-order solver as well, so I am curious to see how far this will go. I’m not entirely convinced it would be beneficial for all of the problems that we have to solve, but it’s certainly something to explore.


**Julian** : Long term, the key focus is to bring in funding to keep our main developers and to keep the project going. In terms of things changing in the ecosystem, people are using optimization solvers in the context of ML and AI. In a sense, we don't really see the effect of that because mathematically they're still solving the same problems. So as long as the software can be integrated, and we focus on the interfaces for doing that, we're still solving the same object underneath mathematically. Linear programming has been around for 80 years, and it’s going to be around for another 80 years.


**John** : I’m going to quote Yogi Berra first: "Predictions are hard, especially about the future." I’m excited about a lot of core infrastructure things that I hope will quietly make everybody’s life faster or better or easier. But for user-facing things, I’m most excited about leaning into this idea of model transformations. Modeling in a framework that makes sense to the domain of the problem and deferring decisions about how we turn it into that standard structure Julian mentioned for the MIP solver, the LP solver, or the CP solver. Deferring those decisions as late as possible, to the extent we can rely on automated model transformations to do that. That’s something we’ve been doing for a long time. It’s been in Pyomo for almost 15 years, but we keep finding new applications. We do a lot more with disjunctive programming than we ever used to before because we've found that to be a very convenient modeling paradigm. The same goes for how we handle piecewise linearizations of nonlinear functions. The idea of how we can express higher level structure in our math programming problems in a way that allows us to explore alternative ways of converting that to the *Ax ≤ b* form that the solver wants.


**Miranda** : I’m most excited about something that we as the Pyomo team try really hard to do. If you don’t know something, we have a textbook! In our textbook, everything that’s written down is what we will keep working on for the extent of the life of that textbook edition. We’re in the process of coming up with a new one, which means I get to do lots of backwards compatibility breaks sometime in the next couple of years. Those are my favorite things to do because it’s an opportunity for us to really improve the experience for our users. I’m excited that our path has a lot in the works that’s going to make Pyomo more performant and a little bit easier for people to understand.


‍


‍[Check out the techtalk recording for the full interview](https://www.nextmv.io/videos/open-source-in-or-q-a-with-pyomo-and-highs) *to learn about open source project adoption in research and industry contexts, plus ways to contribute and support to these communities.*
