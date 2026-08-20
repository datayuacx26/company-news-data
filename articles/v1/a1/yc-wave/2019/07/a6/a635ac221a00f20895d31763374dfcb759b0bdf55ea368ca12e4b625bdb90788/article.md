---
schema_version: "1.0.0"
document_id: "a635ac221a00f20895d31763374dfcb759b0bdf55ea368ca12e4b625bdb90788"
company_key: "yc-wave"
company: "Wave"
source_id: "yc-wave-rss-404ae0f13af9"
canonical_url: "https://www.wave.com/en/blog/split/"
published_at: "2019-07-26T20:58:47+00:00"
first_seen_at: "2026-07-24T06:52:44.556575+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:dc83858e5685c2c33a08f1fcc0b1a13413ecfa67b1279a95ed4f2ea4dc457a86"
---

# How we split our codebase down the middle

# How we split our codebase down the middle


Ben Kuhn


on


July 26, 2019


engineering


---


By far the biggest code improvement we made to Wave was to split our codebase in half.


Our first product was building faster and cheaper money transfer to Africa, by delivering funds directly to M-Pesa and similar systems. That business grew incredibly quickly, but eventually hit a wall: most countries in Africa didn’t have a system like M-Pesa. We realized that this roadblock was actually an opportunity. Instead of just international money transfer, why not build our own mobile money systems in the countries that didn’t have them yet?


To build a mobile money system, we’d need a network of agents throughout each country that users could deposit or withdraw funds at. We decided to bootstrap this agent network using international money transfers, then build the rest of the mobile money system on top of it once it was working. So we made a few tweaks to our existing international money transfer app, adding a “ledger” (keeping track of how much each recipient could withdraw) and “agents” (special Wave user accounts that could process withdrawals for transfer recipients). When it was time to add domestic money transfer, we repurposed our US-to-Africa smartphone app to support Africa-to-Africa as well.


Over the next year we learned a few different things:


-


We didn’t get any traction with international transfers in the market we’d chosen for our domestic pilot. It had currency controls, which meant black-market exchange rates were much more favorable to the dollar than the official rates. We had to offer our users the official rate, but that meant we couldn’t compete with black-market money transfer.


-


Our repurposed smartphone app didn’t succeed. The internet within Africa was too bad and not enough people had smartphones. We ended up building an app that worked over[USSD](https://en.wikipedia.org/wiki/Unstructured_Supplementary_Service_Data) instead; it was much more reliable and anyone with any kind of mobile phone could use it.


-


We still had a smartphone app for our agents, but HTTPS was too slow for it—it required three packet roundtrips before data could be exchanged—so we rebuilt its network layer to use a custom UDP-based (and still encrypted!) transport instead. (We would have used QUIC, but it was too immature at the time.)


-


Entering text over USSD is hard, so our new African user base had to have PINs, not text passwords. And they didn’t have email addresses or mailing addresses either.


Before long, we’d rebuilt our entire user interface, business logic, and infrastructure for domestic transfers, all the way down to the transport layer. But it was still unhappily shackled to the international code base, through our shared abstractions for things like a “user” and “money transfer.” Over time, these started to cause more and more pain:


-


International money transfer was still far bigger than domestic, but engineers working on international money transfer would have to remember that not all users were international users, and not all transfers were international transfers. As our engineering team grew, this became easy for new engineers to forget, and we ended up with many incidents where, for example, someone shipped new anti-fraud code that ended up falsely blocking a large number of domestic users. We ended up having code littered with checks to` is_domestic_user` and` is_domestic_transfer` —I was always paranoid about forgetting one.


-


The core “money transfer” abstraction became bogged down in needless polymorphism, especially once we added things like the ability to use Wave to buy phone airtime or pay for things at shops. It ended up supporting not only US → M-Pesa and US → Wave wallet, but also wallet → wallet, wallet → airtime, and wallet → merchant transfers.


-


Similarly, a “user” of international transfers ended up looking completely different from a “user” for domestic mobile money. International users logged in using their email and a password, whereas domestic users logged in with a phone number, PIN and SMS code. International users used a debit card to pay for things, while domestic users used a mobile wallet. Ordinarily, this was “just” confusing and bug-prone, but I was always worried that in the password reset flows, interference between the two account types would cause some sort of security vulnerability.


A couple years after starting the domestic mobile money project, we reorganized the company to split international and domestic transfers into completely different business units, with separate leadership, separate engineering teams, and so on. At this point, it became obvious that they should be separate code bases so that the two teams could work on them with complete independence.


What surprised me about this transition was actually just how separate they already were. I removed huge chunks of our code that were only needed for international transfers, and one of my colleagues on the other team removed all of our domestic code from their codebase. At the end, we compared notes and` sloccount` output and discovered that the combined total lines of code between the two codebases was *lower than the original total!* In other words, on net the two codebases were not just not sharing any code, but were *actively interfering with each other.*


The size reduction was a huge benefit, but the second-order benefit was even bigger: it became way easier to make[codebase-wide improvements](https://lethain.com/migrations/) . Not only did we have less than half as much code to migrate, but we also had less than half as many developers to train on whatever migration was taking place. For the domestic mobile money team, splitting up the codebase unlocked a huge number of other code improvements that ultimately made a huge difference to our tech quality and velocity.


In retrospect, we clearly should have split up our code earlier. The more interesting question is, when exactly should we have done it? What lesson should we actually learn from this? I have a few different ideas.


The biggest lesson for me is actually one of business strategy. In retrospect, we probably could have predicted that international transfers weren’t the best entry point into the domestic money transfer market. If we’d dug into the exchange rate issue more, we would have discovered how hard it would be to convert users from black market remittance to Wave. Not only would this have saved us from viewing our domestic mobile money tech as “international with a few tweaks,” it also would have let us skip months of iterating on a product that wasn’t what we cared most about anyway.


(Why didn’t we see this at the time? I think we weren’t confident enough in our vision. It seemed much less crazy to work on mobile money if it was a small offshoot of our existing business, rather than a completely different product that had a small integration point with our existing business. But fundamentally, it *was* completely different, and we shouldn’t have convinced ourselves otherwise.)


Given the business strategy we followed, I think starting out with a single codebase was the right move, but we could have made the transition to a split codebase easier in two ways.


First, we could have been **more worried about polymorphism and less worried about duplication** . Even if we had started with shared User and Transfer abstractions, it became clear almost right away that they had nothing in common. At that point, we should have been way more willing to split apart the concepts into international and domestic versions. In architecture reviews, I’ve learned to look out for this, and bias against reusing existing code or tables for something that seems similar today but likely to diverge in the future.


Second, we could have drawn **sharper interface boundaries between modules that would eventually split up** . If we had started duplicating instead of polymorphizing, we would have noticed fairly quickly that the international and domestic codebases had a very small interface of shared behavior (in particular, a single endpoint for delivering an international money transfer to a domestic user). At that point, we could have drawn a hard interface boundary and forbidden dependencies between international and domestic code, so that when we did want to split them apart, “all” we had to do was turn some function calls into API endpoints.


Of course, keeping interface boundaries as sharp as possible is mostly just a subset of the general good practice of having cohesive, decoupled modules. But I think it’s useful to have a few particular interface boundaries where we take extra care not to add coupling across them, because we know that they’re especially likely to become a service boundary in the future. “Extra care” can mean things like configuring our ORM to refuse to join across the relevant tables by default (so the database schemas aren’t coupled), or adding a lint rule to forbid the main monolith from importing the service to be split off.


---


We work on Wave because we think it’s[an extremely effective way to improve the world](https://www.wave.com/en/blog/world/) . If that’s how you want to spend your career too,[come work with us](https://www.wave.com/en/careers/) !


---


If you liked this post, you can subscribe to[our RSS](https://www.wave.com/en/blog/index.xml) or our mailing list:
