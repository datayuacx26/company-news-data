---
schema_version: "1.0.0"
document_id: "d7c49f828f30cd7c3821cbb5fb1ac4543d543b010fe10b0e9fa23afa386fa7e6"
company_key: "yc-protocol-labs"
company: "Protocol Labs"
source_id: "yc-protocol-labs-rss-95d08b63fd6d"
canonical_url: "https://pl.xyz/blog/data-sovereignty-in-a-privacy-first-future/"
published_at: "2025-11-07T00:00:00+00:00"
first_seen_at: "2026-07-22T20:57:16.093704+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:44989179b044174d16ae72a7baf8a6d496c75190cb8d4a2408c5e024cafee198"
---

# Data Sovereignty in a Privacy First Future

*In this conversation ahead of[LabWeek Web3 (opens new window)](http://labweek.io/25-w3) and[Devconnect (opens new window)](http://devconnect.org/) in Buenos Aires, Will Scott of Protocol Labs and Andy Guzmán of the Ethereum Foundation’s PSE team, explore how data sovereignty is evolving, from encrypted transactions and private reads to a broader redefinition of digital autonomy. This dialogue is a window into a movement: builders designing systems where privacy is the default, not the exception, and where sovereignty is measurable, enforceable, and real. This conversation first appeared as an X Space and has been edited.*


## # Summary


- Privacy and sovereignty are intertwined concepts — both center on control. Privacy concerns the selective sharing of information, while sovereignty extends that control to ownership, security, and persistence of data.
- Sovereignty is broader than privacy — it involves defining what constitutes “your data” and resolving ambiguous ownership between individuals, platforms, and collective behavior.
- Privacy can be viewed across layers and use cases — from protocol-level encryption and network privacy to application-level confidentiality for DeFi, governance, or AI collaboration.
- Ethereum’s privacy efforts now focus on “private reads” and “private writes” — ensuring that both actions (transactions) and queries (state reads) don’t leak metadata or user information.
- Technological primitives are maturing — tools like ZK rollups, PIR (Private Information Retrieval), and mixnets (e.g., Nym, HOPR) are moving from theory into practice, creating composable building blocks for private systems.
- Network-layer anonymity remains a challenge — as wallet usage grows, it becomes easier to enumerate users; future systems may need larger peer-to-peer populations or browser-level participation to provide stronger anonymity sets.
- Developers need legible privacy frameworks — complexity risks alienating builders and users. Andy references L2BEAT’s “stage 0–2” rollup model as a template for standardizing privacy maturity levels.
- Compliance vs. cypherpunk values is not a binary — Will argues for pluralism: compliant systems can coexist with rebellious ones if they preserve privacy, and progress in one domain benefits the whole ecosystem.
- Privacy as a default is both cultural and economic — as data becomes more valuable, users and builders will demand control over its release, creating capitalist incentives for privacy-first architectures.
- This is a pivotal moment for privacy — with more funding, awareness, and cross-sector collaboration, the ecosystem must move from building perfect systems to ensuring adoption and cultural momentum.


## # Defining Privacy and Sovereignty


**Will Scott:** What do we mean by data sovereignty? And this broader question of sovereignty, generally. I think that can mean a lot of different things, but I think in the context of PSE and in how we're talking about it, we mean a somewhat more specific thing, although it still has a few different axes. Do you want to give your definition of what you're trying to achieve in terms of sovereignty, and then I'll also try to give mine.


**Andy Guzman:** I would say like in general, privacy and sovereignty are very intertwined. I would first define privacy and then sovereignty.


In my view, privacy is your ability to control which information you share with others — with whom, when, for how long, and in what context. It typically relates to the flow of information, whereas data sovereignty relates more to ownership and control, deciding when and to whom data is accessible. Sovereignty also incorporates more of the security dimension: ensuring that your data cannot be tampered with, made unavailable, or disclosed without consent. Privacy, in that sense, is one essential aspect of sovereignty.


**Will Scott:** I think there's definitely like a sense where both of them are about control. When we think about sovereignty it's that I have control over my own data, and privacy is that I can limit that, which is one aspect of having that control. If you own it, but everyone can see it, that's not necessarily feeling like you actually have that sovereignty. Although being able to say things and not have them deleted is also part of sovereignty, this ability to, have speech and have platforming to some level is the other set of systems built into Ethereum and so forth. But I think that's maybe a little less what you're looking at in PSE or what I've been looking at. Although I think that sense of how you keep stuff online is an important part for both.


**Andy Guzman:** Yeah. And I think there are two ideas here. One is definitely like sovereignty is a bigger concept of privacy. The other one is what defines your data as your data, right? Whenever you use a platform and that platform records your activity, is that a platform's data or is that your data because it relates to you, right? And when those lines blur. I think it's messy. I think the blockchain world, when we just think about privacy. And sovereignty. I think it's more like sovereignty of assets, maybe privacy on your actions. It's a bit easier to delineate. I do feel there might be other use cases where it's a bit harder and when things mix or when it's like behavior about a group of people or organization. Is that your data? Is that everyone's?


**Will Scott:** Yeah. And then I think there's this shared ownership where it gets messy because we've got systems and cultural understandings or norms around what we would expect from patterns, but that's not necessarily the same as what a technical system is going to then end up offering by default. Getting it to match people's expectations ends up being very tricky to have the semantics or the behaviors that people are actually expecting about. Does this work the same way that a physical document might work? Or is this working in some other way?


Maybe as we get into the privacy side of things it can be a little bit useful to think about the scope that you see for privacy. And so I guess there's encryption and then there's metadata around it and so forth. What is it you're actually trying to achieve with that sharing and then the weird secondary effects of interactions especially as they apply to these distributed systems where the transactions end up being on chain or very public.


## # The Many Dimensions of Privacy


**Andy Guzman:** So maybe I'll do examples more related to Ethereum, but I think they're applicable to all distributed systems. I think there's just so many ways to slice and dice and analyze privacy or data sovereignty in this space. These are different views or categorizations.


One is by use case, right? Like financial privacy, relational communicational privacy. And then things on voting. Things on gaming data. So it's like privacy depends on the use case, right? The other one is analyzed by tech stack. For example, with Ethereum, is it that we cannot add privacy to the protocol layer? Or the app layer? Or Layer 2? Or even more specifically, is this like networking level privacy and infrastructure privacy that happens offchain, or metadata, as you were referring to. There's just many ways to slice and dice it. Overall, I think the question is, what do people want and expect and what guarantees do we want to give when we build our systems?


So in the blockchain world, I would say in this pareto load, like the 80-20 what people mostly carry is like private transfers and private DeFi, in most cases. But in different systems you'll prioritize different activities, like voting or like corporations or for example, offchain privacy. When two organizations want to transact or cooperate on machine learning models, it definitely expands a lot more.


I just want to start by saying there are many ways to slice and dice the privacy space view and it all depends on within the system we're building and the infrastructure, and what type of guarantees do we wanna give to people? And what type of guarantees do people expect? Or need.


## # From Private Writes to Private Reads


**Will Scott:** Yeah, I think that's a good initial thought. And I agree. There's this core thing of, is the value hidden in a transfer? If you don't have that you don't really have privacy on the transaction.


And in the same way that we've now got a lot of chat systems that are end-encrypted, there's this initial, okay, we need to make sure we fought that battle. For Chat, we now have good systems for how we would do the encryption side. And it feels like we have a set of technical systems in things from Zcash to various zero knowledge roll-ups in Ethereum for how we do relatively scalable private transactions. I think the exciting thing is as we feel more comfortable about that technology, technically we get to then move on to these second order metadata things. Great, I can hide the value, but people can still potentially front run me by knowing when I've made transactions or, get intelligence about who I am or what I'm doing, just from the fact that there are or not transactions. And so now we've got this much harder, metadata problem that I think in some ways chat is again, a little bit ahead of and so that gives us a set of ideas and building blocks to work with, which is a nice place to be.


The flip side is that, as you're dealing with things like financial privacy, you've got a trickier landscape with a lot more regulatory and surrounding hurdles and, not necessarily the same prevailing headwinds that private one-on-one communication has.


**Andy Guzman:** Yeah. Perhaps there is more public support or awareness to this point — and even adoption, right? Like messaging apps or why it has been adopted in a much bigger capacity than blockchain. So perhaps that's why they are a bit ahead. Private transfer is just one of the use cases, right? And perhaps this is part of the 80-20, but at least within PSE, our current mapping and thinking is just a very simple method model to wrap our heads around this distributed blockchain world is private rights.


Whenever I want to do an action onchain or within the system, what type of actions do I want to typically do, which is like transfers, DeFi, governance, just general computation, perhaps key management, perhaps things like cross-chains and now not only within one of these systems. And they're like problems and solutions that many teams are exploring within all these use cases. The other one, which is like private reads, which is like, whenever I wanna query the state of this distributed system or blockchain how do I not leak my metadata? And I think there are many things that are happening right now that are gonna be interesting that we can find at Devconnect, and that already kinda like working in parallel to this, right?


Or it's tangential, it's like complimentary I guess is a better word. So for example, whenever I connect my wallet, do I leak the balance that I have or first I'm leaking basically to my RPC provider, my IP with the address that I'm requesting? Do I leak the icons that I am trying to pull from? And with that I can leak the tokens that I own, the timings, right? Whenever I do an action and broadcasting, am I leaking my location or my geographic timezone? So I agree with you, which is perhaps a lot more complex in a way, or I don't know, a different level of analysis that you have to do versus unchained privacy for anything, right? All from voting, which is non-financial to this other side, which is financial. I don't think we have even scratched the surface of just general private computation across a web2 world without distributed systems. There's all these rabbit holes that you can go into and spend your life, working and pushing the space forward.


## # Building Privacy into the Network Stack


**Will Scott:** As you were talking about the reading side, I kept thinking about things like spiral, PIR, where in 2022, 2023, we had examples of how you can read wallet balances at the scale of all of bitcoin and or Ethereum without leaking which wallet or which account you're accessing.


So we have a bunch of the primitives already — and have had for a couple years now — that work at the scale that we would need for RPC providers. And now we're into the systems side of this, like how do we get the prototypes and show that we can fit these primitives together, into building better, more private systems. Which is exciting. I think that's a good place to be. There's plenty more cryptographics developments that we have coming down the pipeline, but I think we've got that pipeline where we also have the development side to prototype and implement that. And, I'm really excited about a bunch of the projects that PSE has taken that are transitioning things to practice and actually trying to build things, not simply the research papers as an output.


**Andy Guzman:** Yeah. And I think yeah, some other things, for example, is just the organization or mixnets that we can add. I think PIR is great in this server model. And I think that, in itself, is great when it’s just so many use cases, but I guess there will be others. So for example when there's this validators privacy and networking or broadcasting — this is where mixnets or things like Nym or HOPR or I don't know —


**Will Scott:** Especially when you don't have this latency or direct


**Andy Guzman:** Right.


**Will Scott:** And so for things like rights or for other places where you aren't quite as latency sensitive or have more data.


**Andy Guzman:** Exactly. Latency is a huge problem in this, networking part of the stack. But for certain retail users, it's not a problem, right? Even with transfers, it's more institutional or high-trading users. Whenever other use cases where it's very critical definitely wouldn't help. But it does feel that we are in a renaissance with a lot more focus, activity, attention and funding, even. And making progress, like the retreat that we did this year with you guys. I just feel renewed energizing opportunities, technical progress and adoption, as well.


## # Scaling Anonymity and Reducing Exposure


**Will Scott:** One interesting thing, continuing on this thread of mixnets and so forth, is that one of the interesting things in these privacy or anonymity sets is, what is the thing you're trying to blend into? So what is the larger population that you want to hide within?


I can hide it, but by hiding it, I need to have either other traffic or some other set that I can be confused by. And I think that's been one of the places where, we've now got an ecosystem that has grown enough that there start to be not as many large populations where we can hide in.


I've heard it from a couple of the wallets for instance, do we want people to figure out who all the MetaMask users are or all of whatever wallet because the fact that you have a wallet, having the wallet participate in its own peer-to-peer network, even if the transactions are onionized or so forth, they're worried about that participation. Because if someone can enumerate now everyone who's got a wallet, that becomes like a list of people who now become at more risk for other sorts of attacks.


And so that has been one of the interesting problems I've been thinking about over the last, I don't know, few months: what would be that population? Is there something that we need to, either go to browsers or some other, larger super set of users to provide the space for there to be a peer-to-peer network that we can blend into. Because it feels like that ends up being one of the next limits in this space of having the right cover traffic to give ourselves space.


**Andy Guzman:** So I think I need to understand better what is the problem that you're describing, because typically whenever there's more users within a system, the more it proves its privacy strength by expanding the anonymity set. You can have more people to blend in, but what you're saying is that as a group of, for example, MetaMask users, it's harder for them to blend in into other wallets?


**Will Scott:** No, I think it's all wallet users versus the broader population of humans. If your computer is making requests either directly to some Ethereum endpoint or to an RPC provider? If there's two of those computers in the coffee shop that's now a network level issue where there's a set of these sort of physical or local attacks there.


And one of the hesitations to decentralize and not go simply to an RPC provider, but have some sort of peer-to-peer level clients talk directly to the decentralized network, is that now other members of the decentralized network have the potential to enumerate all wallet users, not necessarily their addresses, but their IP addresses and network level positions. And so that potentially has issues where it can get re-identified with other data sets of oh, who was using this IP address at this time?


Is there a way to have a peer-to-peer network where you can get the decentralization and not have these choke points of RPC providers in the way we have them now? But not expose the node locations of clients to the broader network or to an arbitrary observer.


**Andy Guzman:** So would you saythe problem is because there are too few clients, too few exits or choke points, because coming back to data sovereignty, many of the privacy issues that we see right now, not all, but are okay. Maybe some of the privacy issues that we see right now are because people don't run their own infrastructure, their own hardware — and we typically rely on third party services. But if people ran more locally and they could do a bunch more stuff with privacy and perhaps it could, strengthen the amount of exits that exists.


**Will Scott:** I think that's right. You've got like a couple different ways to approach this. One is you leak less if we had a world where all of the balances in my account were hidden and so I'm not really giving anything to an RPC provider, then I'm less worried about what the RPC provider can do with that intelligence. You've got another option, which is you lower the barrier of being an RPC provider, or you have users run at least light nodes or even full nodes, locally, and now they are participating or they're talking to their own infrastructure that's outside of that local network threat model, and give them control to do that easily.


Or you find, distributed systems type things where you can say, okay, here's a broader peer-to-peer network that you can send your messages in without differentiating or without them being a way for people to know that you're part of Ethereum even or so forth. So I think it's fun when we've got this broad space with several different approaches. And we can figure out which ones work.


**Andy Guzman:** Yeah. Probably all work in different scenarios or in other words, we should push forward on all of those fronts. But there are ones that perhaps they have for now, lower hanging fruit that are more impactful. And others where, I guess, it also depends on your threat model, technical expertise, and the resources that each user has within the system. So when deciding a system, perhaps some users prioritize convenience and speed, but they might be more comfortable leaking information. Whereas some others because of the threat model, they're willing to put all the resources in the highest levels of operation and self-sufficiency, and on hardware.


## # Making Privacy Legible


**Will Scott:** One of the things that this then also brings up is we've got these pretty nuanced trade-offs that we're evaluating as we build systems. And do you worry that it ends up being tough for many developers to make the right choices? And how do you think about it in the systems and products that you guys are prototyping? How do you approach that in terms of having interfaces or having things that are approachable or that people can pick up and make use of, in useful ways?


**Andy Guzman:** I think in general, the more complexity, the less legible systems are, and the more technical specialized again, the less legible systems are. And developers or users for your infrastructure, for your research for the tooling and end users or retail are also users, right, of the end the final parts of your chain of value? So I think we need to come up with abstractions that summarize the key element that we care about in order to protect the use cases that we want. So for example, in the Layer2 context, if we start evaluating and sharing all the trade offs it's almost impossible to capture all of the different approaches, technically, and all the different tradeoffs, but at least in this case, this team, L2BEATS, did a good job doing this framework, which for some people is arbitrary. For some people it's not enough. But it's good enough for the ecosystem to rally around. Sets stage zero, stage one, stage two. I guess basically also Vitalik did some work before. So can we do something similar with privacy? For example, I remember reading from Bela about bits of anonymity, which is basically within a privacy anonymity set, like how many bits does it equate? Is it one, is it 10, is it 30? Is it a hundred bits of anonymity? And that's your private anonymity set. It is just interesting mental models.


And if we, as an organization and as an ecosystem, can agree on certain abstractions that capture the most important things people care about — even if those are contentious or hard to define — it becomes much easier to communicate them. Even without that, we have done experiments, just listening — What are the trust assumptions? What are the privacy guarantees that we expect or effort, like privacy and variance that we are trying to promise to the users? — and creating threat models against those security, privacy and variance, and see if they break.


From your perspective, what do you think would be a good approach to make things more legible for developers but also legible for end users without losing key information that might be relevant?


**Will Scott:** I do agree with you that getting concrete artifacts as things that people can make use of at higher levels ends up being a useful way to help frame and provide understanding. Here is a thing that you can pick up and here's how it works. And that's an easier explanation than talking about the nuance of the design space or making people configure or parameterize systems. And choosing good defaults is a really important thing for us to be thinking about at each step up that product tree.


And as you get usage, you get a useful feedback loop of how well the usage and people's understanding of the model match with what was intended. You can see if people are actually making use of it in the right way or if there's emergent behaviors that end up underlining what was expected.


And so there's the ability to course correct and the counterpoint or the mitigation is that as much as you want to get it right and provide something that is safe you've got the ability to course correct in some sense and release updates and improve things where people are using it in ways that end up not achieving what you wanted.
The other thing we can talk about is are there things that you're excited about in Buenos Aires? Are there specific products or technologies that you think are making progress that you're excited about?


## # Compliance, Rebellion, and the Future of Privacy


**Andy Guzman:** I would love for wallets in general to broadcast transactions more with maintenance or using things like Snowflake. I would love to see the crypto space get more used to using onion services and domains. We announced a few weeks ago that EF is trying to lower the cost of adopting privacy with a reference implementation called Kohaku — basically a wallet implementation, a wallet reference implementation.


And in general, I'll say anything that is helping lower the cost for developers and for users to use privacy, is a way to go. I think things that have people recently are how to build real private digital identities and within that space, for example, within Ethereum Foundation, just supporting for example the country of Bhutan and a few other jurisdictions that are exploring using privacy preservative digital identities.


It's been very helpful, very eye-opening — basically building from a solid foundation within the identity space, you can add a lot more different use cases: voting, payroll, donations — everything built on top of that. So those things excite me. Another thing that has been an interesting realization this year is that cypherpunk and the institutions have been converging into this need of privacy, and at least within this front, have somehow become allies in the sense that both care about them.


Maybe one cares a lot more about compliance and different elements. It's been an interesting evolution. As the world adopts more blockchain crypto systems and distributed systems, it cares a lot more about these topics.


I'll ask you two questions. What things excite you and where would you draw the line about compliance and how much as a space we should quote-unquote bend our knee and do it, or quote-unquote rebel or the other way around, be practical and seek, good actors versus, just being constant rebels?


**Will Scott:** Okay. So things I'm excited about. I'm very excited about having a better privacy reference wallet. I think the other thing I'm excited about at the network level is that Tor has already gotten to a place where there's this pretty easy library that you can just drop in and get onion services and so forth.


So I think that will help and already is spurring a whole set of additional things with good networking-level privacy underneath them.


It’s exciting to see what people build, given that it’s an easier-to-pick-up building block. In terms of compliance stuff, I think my take here is that there's not one answer, and that's something to embrace. That there's going to be projects that are willing to take a more cooperative approach, where they go in and try and figure out something that is compliant, and there's other things that are going to exist that are more rebellious. And that the point of having privacy is that if you can make progress and get approval for something that's private, but still somehow be compliant, that space that you've made then ends up almost inevitably interoperating over the ones that are less compliant. If you're actually doing privacy then I'm going to be very happy to champion you even if you're taking some steps towards compliance.


Because if you've actually done privacy, there's going to be some way to have that interoperability over any other privacy system because that's the sort of freedom that privacy is giving us. And I think, if something that's privacy preserving but has a coherent compliance story that is then able to make inroads and get approved and into more hands happens. That's a great thing because that is opening a door to normalize the broader set of privacy technologies. So I take that sort of broader or more open view where I think having those things is a good thing to exist.


I think the caveat for me is less the oh, is this like weakening something and more is this taking our dev resources? Are we just spending too much time thinking about this and not spending time building the good systems? Okay. So is it too much of a distraction? Is it something that we like, actively don't want?


**Andy Guzman:** I agree and I like that approach. I guess a counter or counterview is: Are we normalizing compliance everywhere? And eventually is there a future where people will just operate on regulated, compliant, and, I don't know, captured protocols for systems versus this more permissionless? That would be a counterargument.


**Will Scott:** So I guess it's good — the enemy of great — that we're selling ourselves short in some sense by making these compromises, right?


**Andy Guzman:** Yeah. That could be a good framework.


**Will Scott:** My take is that we will continue to have needs that go beyond what a regulatory compliance framework is going to give. In a world with multiple countries and multiple nation states, figuring out how you're actually going to handle things cross border, or through whatever other interaction between those policies, is going to lead to this jurisdictional arbitrage that then ends up having that space where you want something that is fully private but can't necessarily comply with every country's sanctions list at the same time. That's almost infeasible.


And so that opens the space that you're not necessarily going to have everything end up in a fully compliant and compromised place. But if you've got things that give you some amount of privacy, that gives you the ability for whatever that unregulated market is to coexist and solve, those places between the edges of what coherent regulatory frameworks could offer.


**Andy Guzman:** Yeah. Which many of them don't even exist or are not developed, right?


**Will Scott:** I think that's a very optimistic view, that they are coherent regulatory frameworks. I think right now we have probably more issues, like internal markets. But even in that utopian world you still end up with the development and value provided by something that goes more towards the cypherpunk side.


## # Privacy as Default: The Next Frontier


**Andy Guzman:** Yeah. Another question that I would like to get your take is where do you see the default falling into? Is it private by default and you have to opt out of privacy or privacy as an add-on and you have to opt in? Inferring that defaults basically run the world in most use cases, but at the same time that privacy has a cost, in terms of: if you don't have it, you weaken the set. If you do have it perhaps performance or cost or gas or whatever load on the system is higher. What do you think about this?


**Will Scott:** My sense is we are moving and we'll continue to have pressures that move more towards privacy as a default. I think we're already seeing some of the effects of increasing data analysis capabilities, and that's just gonna get more widespread. And as that value of data becomes easier to harvest, there's going to be an increasing desire by system builders and users to have control over what is released because it becomes more and more clear how immediately valuable that is. And you don't wanna just give that away without getting the value yourself. And so just from that extreme capitalism perspective, you've got a desire to have the control limited to you. I think that's a nice alignment that's reasonably plausibly going to keep things private by default. In the web2 world, what you got was this limited number of entities that are able to capitalize on leaked behavior patterns, the service that you're directly talking to. And then you've seen these secondary markets of the network operators, right? Like your ISP is probably spying on your traffic to some extent and is able to then target advertisements or sell some data analysis feeds.


The DNS servers have a set of intelligence feeds that they're selling off of watching what domains you're accessing. So there's a whole set of these little pocket industries that have flown under the radar because they're a small enough percentage. But I think as you both get increasing capabilities to turn that set of patterns into marketable value and also you've moved to something that's more decentralized, both of those lead to much stronger impetus to have things private and have control over how they're released.


**Andy Guzman:** That's an interesting take. I think for some systems that are transparent first and then not in privacy, it'll be an interesting battle, both culturally, socially, technically. It's very interesting, right? Because I feel, at least for Ethereum, it pursued decent centralization first and scaling second. And I feel like only now it's seriously adding privacy third, whereas things like Zcash, it looked for privacy first and now, kind of, scaling, right? And other chains have been like scaling first and now thinking of decentralization. So it's just like all these different strategies and approaches. All have trade-offs.


**Will Scott:** Yeah, for sure. And there's these pragmatic things. Are you gonna take this really hard line on privacy? We've seen that ends up both being tough to move as fast and also tough to have the narrative battles because privacy alone is a tough thing to sell as a future.


**Andy Guzman:** Yeah. One topic I've been reflecting on or want to get your thoughts on is how can we uncover more incentives to build and use privacy? So if you think about why selling privacy is hard or why privacy is not the default, it's because there's huge surveillance capitalism. Basically, economic incentive funds buying people to sell ads to sell more. So there's a very strong cycle that fuels these incentives into this different approach. Question is are there ways in which privacy can counteract the cycle? You can argue that you can do it ideologically, but perhaps you have to pay more. You have to pay for add-on services. And I have only discovered one, which is when you have privacy, perhaps if you're a trader, you get MEV protection, so that is better for you, economically. It makes sense. But are there other use cases or incentives, which makes people — besides ideological and this MEV use case — which helps people choose to build and basically contrast this surveillance capitalism incentive?


**Will Scott:** It's a good question. I think there's a few answers to that. One is that privacy, in its stronger form, gives you more freedom in how you release or describe that. Also you're not restricted in which accounts or which other currencies you can swap over to and so forth. You've unlinked it from the various shackles or limitations on assets that you would where you're operating in public. And so the same things that cause people to want to save cash end up being motivations that move things towards privacy over a transparent banking system.


You can look at that distinction between cash or someone storing something in a private wealth thing versus in a traditional banking system, and the parallels of what those advantages are, that end up happening, at least in the financial privacy side.


For data stuff, I think the value of data is something that has been harder to understand because it's not something that we're as used to. But again, traditionally people would keep their data, their photo album is something that you would keep in your house, you would keep private. It's not something you would have publicly. And so you've got this interesting balance of cultural norms, where even as they are shifting and more people are putting more of their lives online, there is a desire to have good access control and have different spheres of sharing and having control over that.


Currently in web2, we have a fiefdom model where you're entrusting the corporate platform to do that on your behalf. But it’s much harder to have that model, but you still have the underlying desire to have that control in a decentralized world. And so that ends up being a thing that you need to be able to solve for with appropriate privacy systems if you don't have that central authority. So the question there is not do people want it? Are there strategies for getting people? But can you offer a system that gives those same or equivalent properties that people can understand and feel comfortable with, that they get from a centralized authority? I think if you do that, there's absolutely people who will want that over not having it. It's really a question of what is the thing that causes people to move to something decentralized or away from the central authorities. And there, you've got a couple things, which is increased regulatory things that you already see — a bunch of headwinds in Europe, not feeling comfortable with many of the large US based tech companies.


That worry of what are the other things associated with that stewardship or authority leads to a desire to have, again, direct control. I'm a little less worried about we need to sell privacy in that way. And it's more that I think we have a headwind where a lot of what has been this fiefdom model is going to break apart. Do we have systems that are able to pick up that set of models, but based on cryptography and privacy systems rather than a centralized authority?


**Andy Guzman:** Yeah. That approach and for sure, one thing is solving the problem, technically, and the other one is getting that solution adopted.


**Will Scott:** Exactly. Easier said than done. But I think having the systems ready and technically working is certainly a thing that needs to happen. And then it's a question of the other parts: the marketing, the partnerships, and the “being in the right place at the right time.” And those are harder to predict.


**Andy Guzman:** Yeah.


## # A Call to Build and Adopt


**Will Scott:** We've got maybe a few more minutes. Any last things you wanna say?


**Andy Guzman:** I would just say it's an exciting time. It's an opportunity window that I feel we have in terms of regulation clarity tailwinds on this topic. A lot more need and adoption and just general excitement from the builder space. I think it's a good moment to be building privacy and sovereignty systems. But the perfect ecosystem won't get adopted if we don't push it. So we also need to do that.


**Will Scott:** Totally. Yeah, I'm excited for what's getting built. I'm also excited to just see a bunch of people in person over the next couple weeks. Yeah, there's a bunch of privacy stuff that'll be happening at Devconnect and around Devconnect.


I know PSE is doing a bunch of stuff, especially on the 21st and 22nd you've got an Ethereum privacy stack. And then a bunch of more focused things around FHE and obfuscation and so forth that should be exciting to get builders together, on some of these more specific, cryptographic sprints or systems that are getting built. And there's a bunch of surrounding projects as well, working on various things from the Cypherpunk Congress, which Web3Privacy Now is working on, which is gonna be a great sort of shelling point gathering. There's some FHE stuff, there's a bunch of zero knowledge stuff as always. So it should be exciting just to see the energy and see what people have been hacking on over the last months.


**Andy Guzman:** Yeah, agreed. We're gonna stumble into many of those.


**Will Scott:** Alright. Great talking with you, Andy. It's been a pleasure and I'm looking forward to seeing you in person for a couple weeks.


*Will Scott is a systems researcher and engineer at Protocol Labs focused on building the foundations of user sovereignty across the web3 stack. With roots in networking and privacy infrastructure, from libp2p and NAT traversal to IPFS and Filecoin, Scott’s work explores how cryptography and systems design can enable direct user ownership of data, identity, and computation. Before joining Protocol Labs, he studied and developed private information retrieval (PIR) systems at the University of Washington, designing architectures that partition data per user to reduce compromise and strengthen individual control.*


*Andy Guzmán leads the PSE team at the Ethereum Foundation, where he drives efforts to advance privacy infrastructure across the Ethereum ecosystem. With a background spanning cloud engineering, hardware-software automation, and applied cryptography, he has helped shape foundational projects like Semaphore and MACI (privacy-preserving identity and voting), as well as early community development for zkEVMs before they became mainstream. Andy also contributed to TLSNotary, which catalyzed the Web3 zkTLS movement, and continues to guide PSE’s evolution from an exploratory R&D lab to a problem-driven team accelerating practical privacy solutions across Ethereum and beyond.*


***[LabWeek Web3 (opens new window)](http://labweek.io/25-w3) takes place in Buenos Aires from Nov. 13-19.[Devconnect (opens new window)](http://devconnect.org/) takes place in Buenos Aires from Nov. 17-22.***
