---
schema_version: "1.0.0"
document_id: "77855076ee8154604eb25c660dd14a0a341c7638209a5f6552db3c308f2a094f"
company_key: "yc-greywing"
company: "Greywing"
source_id: "yc-greywing-news-import-fac42ec76768"
canonical_url: "https://www.grey-wing.com/blog/software-still-isnt-eating-maritime"
published_at: null
first_seen_at: "2026-07-21T22:08:18.820655+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:768fc0242f9014873c3b0acd6e0a73a4f039afe3b0547b7f4a4612882c9a14ae"
---

# Software still isn’t eating maritime, here’s why

# Software still isn’t eating maritime, here’s why


Why there aren't many software firms in shipping


Hrishi Olickel


Published on 11th Sep 2023


8 minutes read


Maritime technology has come far as an industry - but I'm often surprised at the lack of companies in this space, given how large and inextricable shipping is to the modern world. When Greywing started, the field had few players, especially in maritime operations. There are a lot more of us today, but far less than you might expect.


## Maritime software still has a talent problem


Maritime has problems with communication overload, planning issues, industry leavers and worst of all, a lack of digitisation. These issues get worse as the climate becomes more unstable and the world continues to globalise - every customer we speak to cites “email overwhelm” and increased loads of decision making as growing problems. So in an industry with clear problems to solve, good reasons for solving them, and a need for technology, are there too few companies trying?


One of the more obvious reasons is that shipping requires domain knowledge that is hard to come by. In an industry where years of sailing experience is practically mandatory for many jobs, it’s a hard ask to take software engineers with decades of experience and expect them to understand the specifics of port operations or the compliance details of oil majors. That said, I believe there will be an explosion of new technology firms in a few quarters, spurred on by the supply chain crisis, an increase in funding, or the recent misfortunes of the industry that point to a need for technology. The biggest problems are systemic, and related to the fact that building software (or hardware) in shipping requires companies to solve multiple zero-to-one problems with no common solution. Here are some I have encountered, and why they persist.


## 1. Maritime is massively decentralised


One of the biggest strengths of shipping - in fact the very thing lending it resilience - is also one of its biggest problems. Maritime trade has been fundamentally decentralised for centuries, long before crypto made decentralisation the mot du jour.


Vessels are floating nations in their own right, with crew and cargo that comes from and goes all over the world. Involved in the daily operations of a single vessel at rest are often six or seven different companies, with just as many countries and regulatory bodies. In motion, that number goes up considerably, and navigating this landscape could very well be a sector of its own.


To form a coherent picture of a single vessel, multiple companies and countries need to share data - something the industry has been traditionally resistant to. This is a big reason most new companies are forced to focus on prediction and estimation, rather than work with real information.


As an example, to provide an accurate suggestion for a port of call, we need to know the route of the vessel - from satellite AIS but also the voyage plans of the charterer. We also need to know the visa restrictions involved in moving crew there and back - from the travel provider, as well as the port agency costs and restrictions specific to the port. Once we have these, it’s a simple matter of calculating the fuel cost of deviation, fetching hundreds of flights to filter for corporate requirements, and maintaining an active database of ports and common areas of slowdown to suggest what the best solution might be.


## 2. Maritime technology is still early


Internal digitization in industries often happens in waves. There is usually the first wave of bespoke software to solve specific issues with expensive machines. This is then followed by a second wave of general purpose tools (think MS Office) that have a broader scope and can improve everyday tasks. The third wave comes from a management-led push of internal integration - custom tools are commissioned by separate departments to digitise their workflows. This wave usually encompasses enterprise level softwares. As these systems reach their end-of-life, an unbundling can take place, with externally built single-purpose software taking the place of these individual systems.


Each wave happens as workflows build up inefficiencies to push companies over the hill of spending differently. With enough dissatisfaction - there comes a point where players need to bridge the loss in productivity that comes with trying something new.


With each change, the pattern often swings from externally-built to internally-commissioned. Unfortunately there is no ideal solution: bespoke workflows will always fit better when internally directed, but external vendors can aggregate learning and innovation across multiple players to deliver a superior, more holistic product.


Maritime is still stuck between the first and second wave, with early adopters looking ahead. The problem is that there are currently very few standards for data interchange, as most existing pieces were built in silos for individual consumption.


Greywing might be a prime case to see how diverse things are. As of this piece, we connect to 20-30 different companies, and at least twice as many data sources. Our integrations cover simple JSON push and pull APIs, SOAP, sockets, and even xlsx files that are periodically downloaded and parsed. Today, AI makes it possible to add providers that are not digitised - through email as an automate-able medium. SeaGPT has enabled us to further increase the number of participants we can bring into the ecosystem by a factor of 10x and we couldn’t be more proud.


## 3. Maritime data works on an honor system


Most information in maritime still originates from manual entry, and is maintained on an honor system that presumes you're being honest. AIS (Automatic Identification System) - the underlying technology for tracking vessels on their journeys - is self-reported, as is the data for any ports or offshore anchorages that aren't big enough to qualify for an UNLOCODE. The same vessel - or port - can have multiple names and attributes, and it's often hard to tell what the right information is.


“Correct information” turns out often to be the wrong way to think about this data, which needs to be cleaned and coalesced before it can be of any use. Some companies like Spire provide the valuable service of cleaning this data as they provide it, but the output can only be so clean, before you hit a barrier.


What this means is that failure-tolerance and some level of probabilistic modelling—where the algorithm selects the most correct data instead of looking for 100% truth—need to be worked into any system that consumes maritime data. Some of our internal systems have already moved in this direction, using multiple data points to look for consensus with some level of confidence.


## 4. Maritime UX has yet to stabilise


Software interfaces in shipping have yet to find an equilibrium, with the fundamental interface primitives for the industry not having been designed yet. Like smartphones in the early 2000s, or ridesharing apps at the very beginning, interfaces are still experimental (or worse, ancient), with no real connection to the underlying information or the users in front of them.


Most interfaces in shipping look exactly like the things they replace - spreadsheets. This makes complete sense when attempting to replace a workflow built on spreadsheets, as it involves less training for users. However, this can also hold back progression, as it takes a fair few cycles of iteration before other primitives can take hold and perform on par or better.


We are taking a big risk by making the map our base primitive, and building a new interface for crew changes that relies on contextual visual representation - show not tell - and cutting down information when unnecessary. I wrote about button syndrome some time ago, which turned out to be quite polarising. Users are often split between power users who hate a loss of control, and others that would like less information overload on their screens.


I expect these to be the formative years of shipping interfaces, and the companies formed this half-decade will be the ones defining it.


## 5. Maritime still needs reinventing the wheel


This is one of the biggest hurdles we've had to solve. Maritime is simply missing a lot of the base tooling that we take for granted in other fields.


A good example - among many - is routing. Most data in shipping is closely tied to a vessel's route, which can be a complex path between two ports. Ships prefer to use highways when possible, to avoid congestion and increase speeds in deeper water, which makes straight-line paths often the least efficient. Yet most platforms today still use straight lines to convert port calls to a route, or skip this process altogether.


Almost every company that does have routing has had to build it in-house, as have we. We might soon see startups that focus on these individual problems - as there already are - and try to deliver a general-purpose solution to each one. Further unbundling might be ahead of us. Atleast, we remain optimistic about that. Despite these problems, shipping can be an exciting place to work, for one big reason.


## 6. Maritime’s problems are exciting


Some of the most interesting problems are zero-to-one problems. It can be hard to get them right, and only time will confirm your hypotheses, but they allow for creative solutions that have the potential to redefine how we do things.


The problems of maritime are sometimes hard, but they are not new under the sun. They are the same problems faced by every industry as we try to find new ways to do things that were better than before. A path lined with just as many eureka moments as there are setbacks, I'm happy to be solving them every day. What has been repeatedly clear, is that there is not a boring day in sight for quite a while.
