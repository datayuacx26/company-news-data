---
schema_version: "1.0.0"
document_id: "e271689b542913895a44f5d23f34c4510cce5a5dd2be6d233b1a6a9c65e052b9"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/part-ii-why-good-data-and-feedback-loops-in-criminal-justice-are-hard-to-come-by"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:42480cea6792434251a7e4f2fbcf6c296068fedb6ba73cbad1efa1fe10976b1e"
---

# Part II: Why good data and feedback loops are hard to come by

*This is the second part in a four-part series Recidiviz has written to summarize some of the biggest issues we see with the use of technology in the criminal justice space and to create a “playbook” with specific recommendations that practitioners can utilize in tech procurement, integration, and deployment (see Part I*[here](https://medium.com/recidiviz/part-i-a-mapping-of-tech-in-the-criminal-justice-system-bce3c5f37fd7) *). As always, we welcome thoughts and feedback.*


Imagine you’ve just built and deployed a new software tool for consumers — say, better spell-check for email. Building and launching that tool is only half the battle — the big test is what happens after. How much time are users spending with the tool? How often does it crash, or do they seem to get stuck and walk away? How satisfied are they with the usability of the tool? What do they need that it can’t do yet (for instance, checking grammar)? And most importantly, to what extent is the tool actually impacting their goal: to send messages faster and with fewer mistakes? Based on the answers to these questions and more, your team will spend a lot of time on the next stage: iterating and making incremental changes, all the while ensuring that the quality of the product stays high throughout the process.


Most products are not great when they start out — as the saying goes for any app or website, “The first day is the worst the product will ever be — it only gets better from there.” But in the criminal justice tech space, where getting something wrong[can cause a lot of harm](https://medium.com/recidiviz/part-i-a-mapping-of-tech-in-the-criminal-justice-system-bce3c5f37fd7) , whatever goes out the door on the first day — right or wrong, good or a pain in the neck — is pretty much the same as it will ever be. The feedback loops we expect and benefit from in our private lives are often elusive. Why?


In this post, we’ll walk through a few contributing factors we’ve noticed from our experience working with governments from the tech side. The challenges essentially boils down to three things:


1. An antiquated government procurement process
2. Fragmentation of users and agencies across government and the justice system
3. A lack of standardized data and metrics


# 1. Procurement: Noble goals, unintended consequences


The modern procurement process looks something like this: an organization that wants to procure new technology assesses its needs and develops a comprehensive list of criteria, which is turned into a Request for Proposals (RFP). This document is published, and vendors submit bids for what they can do and how much it will cost the government. The government agency collects the bids, scores them according to their criteria, then selects the highest-scoring, lowest-bidding vendor for the contract.


It’s not surprising that procurement doesn’t seem tailor-fit for information technology tools. The practice in the U.S. can be[traced all the way back to supply purchases for the Continental Army](https://washingtonpapers.org/resources/articles/supply-problems-plagued-the-continental-army-from-the-start/) in the Revolutionary War, which included ammunition, blankets, and “a large number of hogs…to be purchased, slaughtered and salted.” In fact, the oldest procurement law still on the books was created just a few years later, in 1808. The core principles behind these rules have more or less stayed constant:


- Promote competition so government can get the best price
- Keep public trust — minimize risk of project failure and public fiasco
- Make it difficult for corruption and unfair practices to take place


All of these seem reasonable. As citizens, we don’t want our tax dollars going to waste, we don’t want to risk infrastructure projects on unproven providers, and we certainly don’t need a repeat of[Tammany Hall](https://en.wikipedia.org/wiki/Tammany_Hall#Tweed_regime) . These are good ideas, so where do things break down?


## Size feeds complexity, and vice versa


First, take a look back at those core principles — every one seems to push governments toward taking large contracts with well-established vendors. To ensure open and fair competition, the procurement process itself is intentionally cumbersome and long — it can take over a year to go through the bidding process and finalize a contract. Thus, governments prefer to procure in large bundles (i.e. all-in-one solutions including database management, frontend engineering, design, etc) rather than in separate pieces. The resulting contract size, combined with the government’s inherent risk-aversion, gives[large vendors with long histories](https://www.quora.com/What-does-the-phrase-Nobody-ever-got-fired-for-choosing-IBM-mean) an undeniable edge in the process.


The size of these projects, in turn, pushes vendors toward delivering a technical system all at once after years of work, instead of making incremental changes to continually validate functionality, ensure usefulness, and expose early bugs — the exact opposite of agile development. This is like taking a journey of a million steps with a blindfold on — if you’re even a few degrees off, you’ll end up far away from where you wanted to be. And of course the world doesn’t hold still: even as workflows, requirements, security standards, and technology itself inevitably change over time, vendors are often stuck delivering what was initially agreed-upon in the contract five or ten years prior. “The length of the process is really terrible,” said a former official we talked to at a state Department of Corrections. “We can get locked into a 3–5 year contract even when we know something isn’t working.”


## The cost of switching is high


Furthermore, winning vendors have every incentive to make it as hard as possible for the government to switch providers, even once the contract ends. The vendor’s hope is that the software becomes so complex and custom-built that any additions also have to be custom-built, creating an increasingly unmaintainable software system that only one vendor in the world can work with. Tools riddled with custom data formats, intellectual property restrictions, and other types of lock-in are not only bad for the agency and the public; nowadays, they also require more effort to build than tools that are open and standards-compliant. The fact that the vendor’s time and attention is spent on figuring out lock-in, as opposed to essential functions for the tools, is a sadly common occurrence. This helps to explain why there are so many legacy government[IT systems that are 30–50+ years old](https://www.gao.gov/products/gao-19-471) — it’s simply too difficult and costly to switch, even when the system is clearly outdated and struggling to meet modern demands.


## What comes after isn’t top of mind


Lastly, because contractors primarily compete on price, and because the federal grants that are often used for big capital improvements are only meant for new investments, important yet intangible factors like ease of use or maintenance often fall to the wayside. With little budget or incentive specifically attached to staff forward-looking efforts, it becomes exceedingly difficult to do much beyond the initial delivery of the product. While private sector buyers increasingly consider the total cost of ownership (TCO) of adopting new technologies, governments on the whole have yet to adopt this trend.


## There is a tech knowledge gap


Another reason that government (and especially criminal justice) tech seems stagnant is that in some places, the agency is trying to spec their own technology without much experience doing so. If they’re not[asking for the right things](https://www.goodreads.com/quotes/15297-if-i-had-asked-people-what-they-wanted-they-would) , they are unlikely to get them through the hyper-rigid procurement process. In others, it’s because the vendor ecosystem is more predatory, taking advantage of public servants who shouldn’t have to understand the ins and outs of niche technology. In other words: there are a lot of challenges here.¹


# 2. Fragmentation: When the justice system isn’t one system


The criminal justice “system” is actually a collection of institutions from separate branches and levels of government. “There isn’t a single individual, group, or entity responsible or accountable for all stages of the system, or even a line of communication between many of the stakeholders,” one former Secretary of Justice notes. As a result, there’s no global accountability for the system’s performance, or often even a way to know when a change (to policy, practice, or tools) in one part of the system has had a positive or negative effect in the rest. In other words, there’s no feedback loop between the inputs to the justice system and its outcomes.


Say Prison A has implemented a new education program. To decide whether they should keep the program, Prison A wants to find out whether recidivism rates have decreased since its introduction. Unfortunately, this isn’t possible without reliable and timely access to arrest records from the police, and revocation data from the parole system. And arrests would only be a weak, early indicator of recidivism: if Prison A wanted to know the number of convictions that actually resulted from those arrests, they would need access to court records, too. That’s not even in the same branch of government.


Much of the organizational and technical infrastructure that would be needed to bridge these gaps doesn’t exist today, impeding the system’s ability to make informed and feedback-driven decisions. How did this happen? As the Secretary puts it: “Information sharing is simply not a priority, resulting in the system we see now — one riddled with silos that rarely come down.” Without an understanding of what’s working and what’s not, it’s hard to see the system making any great steps forward in the near future.


# 3. Data: Both a problem and a potential solution


Even if it *were* easy to get data from any institution we wanted, we would have a hard time making sense of it. Standard reporting formats, like the[General Transit Feed Specification (GTFS)](https://developers.google.com/transit/gtfs) for the transportation industry, don’t yet exist for the criminal justice system. Any data that’s shared would have to be cleaned, reformatted, validated, and linked across silos before it could be used, requiring an amount of time and expertise that is often not available. This is part of why federal surveys of information like[arrests](https://www.fbi.gov/services/cjis/ucr) and jail populations are voluntary, only sample some jurisdictions, and can take years to publish after being collected.


Not surprisingly, a set of standardized metrics that should be calculated doesn’t exist, either. As a result, the definition of recidivism becomes slippery and confused — if an individual commits a technical violation while on parole, that’s recidivism to some jurisdictions, while re-arrest is in others, and only a return to prison counts in still more. The fact that the answer varies from place to place makes it very hard to make meaningful comparisons or projections — information that could be critical in informing a policy decision.


# Parting thoughts


Consider the spell-check tool example from the start of this post. If logging is set up correctly, feedback is almost instantaneous — with a simple query, we can see the number of users, the average amount of time it takes to compose an email, the average number of misspellings per email, etc. Now imagine that logging is outsourced to 10 individual companies who track different metrics but don’t talk to each other, and that each email takes *years* to go from draft, to sent, to received. That’s how the criminal justice system can look, coming from most other fields. In a space where outcomes are the opposite of instantaneous (e.g. revocation can happen years into parole) and so much is at stake, it becomes even more important for feedback loops to be as tight as possible to drive better decision-making.


Though it isn’t realistic to overhaul the system overnight, there are ways to push for change within the system and build feedback loops despite these structural limitations — our next post will outline some of what we’ve learned operating in this space.


*We would like to thank*[Clementine Jacoby](https://www.linkedin.com/in/clementine-jacoby-50578483/) *,*[Alex Kasavin](https://www.linkedin.com/in/alexkasavin/) *,*[Samantha Harvell](https://www.linkedin.com/in/samantha-harvell-359b9814/) *, and*[Jessica Saunders](https://www.linkedin.com/in/saunders-jess/) *for their helpful comments and contributions.*


*Recidiviz is proud to be a*[GLG Social Impact Fellow](https://glginsights.com/glg-social-impact/) *. We would like to thank GLG for, among other things, connecting us to some of the experts who provided valuable insights showcased in this post.*
