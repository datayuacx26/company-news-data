---
schema_version: "1.0.0"
document_id: "84352907d480e93dced5e63f0f8995098eecf327da155e2ba2bbc66605eacfd5"
company_key: "yc-mocha"
company: "Mocha"
source_id: "yc-mocha-rss-d0ffed2c2227"
canonical_url: "https://getmocha.com/new-pricing-credits"
published_at: "2025-06-20T00:00:00+00:00"
first_seen_at: "2026-07-24T11:28:42.148680+00:00"
fetched_at: "2026-08-20T02:22:16.718062+00:00"
content_hash: "sha256:048f1ea6f1c17734ccb981dcac01e472c6fd43c3905868d5be7c563c1b81626b"
---

# Pricing update: messages -> credits

Last week, we pushed a big change to how Mocha works. We killed our message-based pricing and replaced it with credits.


This change came after a lot of feedback from our alpha testers (thank you!) as well as some research.


The reasoning is simple: this pricing structure is a lot more efficient and adapts to different usage patterns of Mocha.


## The Problem with Messages


When we first built Mocha, we liked the idea of keeping things very simple. We also prefer to have our customers reason in terms of outcomes rather than in terms of inputs. For these reasons, we chose a message-first pricing approach. This worked fairly well for the first few months but started to show some cracks after a while. These issues became even more apparent once we introduced the new[Mocha 1.0 agent](https://getmocha.com/mocha-1-0-announcement) .


One user spent a message fixing a typo. Another user spent a message building an entire authentication system with login, signup, password reset, and email verification. Same cost. Totally different value.


That's... broken.


It got worse. Users would batch up tons of changes into mega-prompts to save messages. Instead of naturally iterating ("make the button blue... actually, make it green"), they'd try to predict every change upfront. This led to Mocha trying to do too much, and failing, which in turn led to a frustrating product experience.


We were trying to get people not to worry about it, but ended up forcing people to think about conservation instead of creation.


## Enter Credits: Pricing That Actually Makes Sense


Credits fix this. Small changes cost small amounts. Big features cost what they should. It's like paying for what you eat at a restaurant instead of a flat fee whether you order a salad or a five-course meal.


Here's what changed:


**Bronze Tier Example:**


- Before: 250 messages/month
- Now: 1,500 credits/month
- Result: Way more building power


**Real-world usage:**


- Fix a typo: 3-10 credits
- Change a color: 10-20 credits
- Add a big feature: 20-100 credits
- Discuss mode chat: Just 3 credits


## What This Means for Building


Remember the last time you were designing something and kept tweaking it? "Move that left... no, too far... perfect!" That's how creation actually works. Iteration. Experimentation. Happy accidents.


Messages hurt that flow. Credits bring it back.


Now you can:


- Try five different button colors without wincing
- Iterate on copy until it's perfect
- Experiment with layouts freely
- Use Discuss Mode to brainstorm (at just 3 credits a pop, it's a bargain!)


The cognitive overhead disappears. You're back to just... creating.


## The Technical Side (For the Curious)


Under the hood, we're now tracking the actual computational cost of each request. A typo fix touches one small part of your codebase. Building a payment system generates hundreds of lines across multiple files.


It gets quite hairy: things depend on the size of your codebase, how hard it is for our agent to gather the right context, how much coding is required to implement the change, and then debug and fix any lingering issues. These are highly variable but we normalize things to ensure that customers don't eat edge case behaviors.


Our AI agent got smarter too. It knows when you're making small tweaks versus architectural changes and adjusts accordingly. This wasn't possible with the flat message model.


We also built in some nice touches:


- You can see exactly what each change cost
- Bulk operations are optimized to use fewer credits
- Failed attempts don't count (because that would be cruel)


## What Early Users Are Saying


Since launching credits in alpha last week:


- Average user iterations: Up 175%
- Discuss Mode usage: Up 4x
- User satisfaction: "Through the roof" (very scientifically measured, trust us)


## Looking Forward: Mocha 1.0


This credit system is just one piece of what's coming in Mocha 1.0 next week. We've rebuilt everything based on what we learned from our alpha users.


The vision remains the same: everyone should be able to build software. Not just developers. Not just designers. Everyone with an idea.


Credits remove one more barrier. Mocha 1.0 removes the rest.


## Your Next Steps


Nothing.


Your credits have been migrated, and if you're a subscriber you'll receive credits every month now instead of messages. That's it!


Have thoughts on the credit system? Let us know!


Now stop counting and[start creating](https://getmocha.com/) . ☕
