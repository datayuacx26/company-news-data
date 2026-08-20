---
schema_version: "1.0.0"
document_id: "1595e7371595d7a088cf90384623f5b9ac4d23fe43d4a919e9a1a9f10af32aad"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/solid-principles-when-they-fail"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T08:44:52.452861+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:f3f66aa4497984464ab3fa21c43dabf153ba03611dd65b787b89e7a3d09815c8"
---

# SOLID Principles Won't Save You From Bad Design

A pull request sits open for three days because of one comment: "This violates the Single Responsibility Principle, can you split it up?" The class in question is forty lines long. It validates a discount code and applies it to a cart. Two things, sure, but two things that only ever happen together, written by one person, tested by one test file. The reviewer wants three classes: a validator, an applier, and a coordinator that calls both. The author caves because arguing against SOLID principles in a code review feels like arguing against brushing your teeth.


Four files and two interfaces later, the discount logic does exactly what it did before, except now you need to open three tabs to trace one behavior. Nobody made the code more correct. Nobody made it more testable in a way that mattered, since the tests were just split into three files that still mock each other. What actually happened is that a rule everyone learned in an interview prep sheet got applied to a piece of code that never asked for it.


This happens constantly, and it's worth asking why a set of five guidelines from a 2000s-era Robert Martin paper has become a moral standard that senior engineers still feel obligated to defend, even when following them makes the software worse.


## **Where the single responsibility principle gets misread**


The Single Responsibility Principle says a class should have one reason to change. Somewhere along the way, "one reason to change" got flattened into "one method" or "one verb," which is not the same claim at all. A reason to change is about what forces edit the code, not about how many things the code happens to do.


Take an OrderProcessor that calculates tax, applies a discount, and writes the final total to the order record. That's three actions. But if the business only ever changes tax rules, discount rules, and total calculation together, as part of the same pricing overhaul, then it has one reason to change: pricing policy. Splitting it into TaxCalculator, DiscountApplier, and TotalWriter doesn't reduce coupling. It just spreads a single coupled concept across three files that are all edited in the same commit anyway. You've traded a class with three methods for three classes with one method and an implicit contract between them that lives only in the head of whoever wrote it.


The tell that SRP is being used as a slogan rather than a judgment is when the justification for a split is "it does more than one thing" rather than "these two things change for different reasons, at different times, driven by different people." If your billing team and your tax team are different people who ship on different schedules, split it. If it's one engineer who owns pricing end-to-end, you just built yourself three files to keep in sync by hand.


## **The interface you built for a class with one implementation**


Open-Closed and Dependency Inversion are often cited together to justify an interface for every service, even when that service has exactly one implementation and will plausibly always have exactly one. PaymentGateway is an interface with a single class, StripePaymentGateway, that implements it. The reasoning is always the same: what if we switch payment providers someday?


Maybe you will. Most teams don't. And the cost of the interface isn't zero: every time someone wants to know what processPayment actually does, they now have to jump from the interface definition to the one implementation, because tooling and habit both push you to read the interface first. You've added a layer of indirection to protect against a future that may never arrive, and in the meantime, you pay for it on every read.


Contrast that with a real case where the same pattern earns its cost. A team building a checkout flow that has to support Stripe today and is already contractually committed to adding a regional processor in Brazil next quarter has a concrete second implementation on the calendar. Defining PaymentGateway there isn't speculative, it's modeling a requirement that already exists. The difference between the two cases isn't the shape of the code; it's whether the second implementation is a guess or a fact.


Kent Beck's rule of three is a decent gut check here: don't abstract until you have at least two real cases that need the abstraction, or a documented near-certain third. One implementation plus a hypothetical isn't two cases. It's one case of wearing a costume.


## **Liskov substitution and the rectangle you don't have**


The Liskov Substitution Principle gets taught through the rectangle-square problem, which is a genuinely useful thought experiment and a genuinely rare situation in application code. Most LSP violations people flag in review aren't about geometry; they're about a subclass that throws NotImplementedError on a method the parent class promises, because someone modeled an "is-a" relationship that was really a "sometimes-a" relationship.


The fix people reach for is always more inheritance discipline: pull the offending method up into a narrower interface, add another layer, respect the contract. Sometimes that's right. But often, the problem is that inheritance was the wrong tool from the start, and no amount of careful interface slicing can fix a modeling mistake. A Penguin that inherits from Bird and can't implement fly() isn't a Liskov violation waiting for a cleverer hierarchy. It's a sign that "bird" was never a useful shared abstraction for the behaviors you actually needed.


Composition solves this faster than any amount of principled inheritance surgery. Give Penguin and Eagle the capabilities they actually have (swims, flies) instead of inheriting from a shared ancestor and then negotiating which methods get to throw. LSP isn't wrong here. It's just answering a question you shouldn't have asked in the first place.


## **When the interface segregation principle actually pays rent**


Of the five, Interface Segregation is the one most often skipped in production code and would genuinely help. Teams build a single-wide Repository interface with fifteen methods, and every consumer, including the one that only ever reads a single record by ID, depends on all fifteen methods. Change any one of them, and everything that imports the interface has to be checked, including callers that never touch the changed method.


This is the case where the abstraction cost is already being paid, badly, and Interface Segregation is a genuine fix rather than an added layer. Splitting that interface into ReadableById, Searchable, and Writable doesn't add ceremony; it removes it, because now a consumer's dependency footprint actually reflects what it uses. Static analysis gets sharper. Mocking in tests gets narrower and more honest. Nobody has to fake fourteen methods to test one.


The distinction that matters: ISP pays off when the interface already exists and already has many callers with different needs. It doesn't justify inventing five granular interfaces on day one for a service with one caller. Same principle, opposite verdict, depending entirely on whether the pain it solves is real yet.


## **What actually predicts whether SOLID principles help here**


Across all five, the pattern is the same. SOLID principles work when they're a response to observed pain: code that's hard to change because two unrelated concerns got fused, or a test suite that requires a live database because nothing was ever separated from its dependencies. They work badly as a checklist applied in advance to code that hasn't hurt anyone yet.


The senior engineers who get this right aren't the ones who can recite the five letters fastest in an interview. They're the ones who can look at forty lines of working code and correctly say "leave it" as often as they say "split it." That's a harder skill to demonstrate than reciting a definition, which is exactly why interview loops default to the definition instead.


A useful question to ask before invoking any of the five: has this code changed for more than one reason, more than once, in its actual history? Not hypothetically. Actually. If a class has been stable for a year and the "violation" is purely structural, the fix is optional at best and a source of churn at worst. If a class has been touched by three unrelated bug fixes in the last two months because two concerns are tangled together, that's not a style complaint; that's a real cost you can point to, and now SOLID principles are the right lens for describing what to do about it.


## **SOLID principles are a diagnostic, not a doctrine**


None of this means the five letters are useless. Dependency Inversion genuinely saves you when you're building something that needs to swap real infrastructure for a fake one in tests. Interface Segregation genuinely helps once an interface has outgrown a single caller. Liskov genuinely catches broken inheritance when the inheritance was already broken.


What none of them do is replace judgment. They describe symptoms of good and bad design after the fact; they were never meant to be a sequence of moves you perform on code regardless of what that code actually needs. Treating them as a checklist to satisfy in a code review, instead of a set of questions to ask about real, observed pain, is how you end up with three files and two interfaces doing the job that forty honest lines did just fine. The code that survives five years isn't the code that satisfied five letters on day one. It's the code someone kept simple until the day it genuinely needed to be something else.
