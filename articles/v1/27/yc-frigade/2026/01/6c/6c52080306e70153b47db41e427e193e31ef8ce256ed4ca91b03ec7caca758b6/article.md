---
schema_version: "1.0.0"
document_id: "6c52080306e70153b47db41e427e193e31ef8ce256ed4ca91b03ec7caca758b6"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/onboarding-tours-break"
published_at: "2026-01-02T00:00:00+00:00"
first_seen_at: "2026-07-24T09:12:25.171648+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:9750d01ed296ba176972452e75e0af6ad6e0953d0b5f67aa5e075b4e06f98aa7"
---

# Static onboarding tours break. The real cost of maintaining them in 2026.

Onboarding tours are the most over-promised tool in the SaaS stack. Every vendor in the category sells the same dream: a marketer drags a tooltip onto your product, and a guided experience appears for every new user, forever.


The tooltip part works. The "forever" part doesn't.


We've shipped tours for years. We sell a tool that builds them. So this isn't a takedown of the category. It's an honest accounting of what they cost, because most teams underestimate the second half of the bill.


## The numbers nobody runs


When a team evaluates a[tours product](https://productonboarding.com/articles/how-to-build-product-tours) , they look at the headline price (typically a per-MAU rate, with seats as a secondary line) and call it a day. That's the wrong number. The right number is the all-in cost of keeping the tours alive.


A typical mid-market SaaS team builds somewhere between four and twelve tours in the first quarter after install. Half of them break inside six months. The breakage isn't always visible. A tooltip pointing to a button that's been moved still renders, but it points at empty space. The user sees a hint about the wrong thing.


Now do the math. Twelve tours, six broken, each one needing fifteen minutes to investigate and rebuild. That's an hour and a half per quarter from someone's calendar. Cheap, until you multiply it by every tour you've ever shipped, every release that touched a UI element, and every team member who has to remember the toolset.


We've seen support teams spend a full afternoon a week running this maintenance loop. That's $40K-$60K of fully loaded labor a year, before counting the hidden cost of users who hit a broken tour and walk away without saying anything.


## Why this happens


The product is built on selectors. The tour says "highlight the button matching` .btn-primary` ." When engineering refactors the component, the selector goes stale. The tour points at nothing.


You can mitigate this with discipline. Tag every element you'll ever target. Add data attributes nobody allows you to remove. Run a regression suite of every tour after every release.


In practice, most teams don't. The team that built the tours moves on. The new PM doesn't know which selectors are wired into a tour. The frontend team refactors a component because that's their job, and a tour breaks somewhere in the long tail. Nobody finds out for two weeks.


The mismatch is structural. The product is built continuously, by people who don't know the tour exists. The tour is built once, as a snapshot of a moment in the product's life.


## What changed in 2026


Two things made this worse and one thing made it better.


The release cadence got faster. Most growing SaaS companies ship to production multiple times a day. Every ship is a chance for a tour to break. Five years ago you got a release every two weeks. Now you get one every two hours.


The frontends got more dynamic. Component libraries, design systems, A/B testing tools, and feature flags all introduce conditional rendering that selectors can't keep up with. The button is sometimes there, sometimes not, depending on the bucket the user landed in.


What got better is the alternative. Agents that learn the product by using it can re-anchor against behavior, not selectors. When the button moves, the recipe still works because what's recorded is "click the primary CTA in the billing card," not "click the element matching` \[data-testid='upgrade'\]` ." The agent re-resolves on every run.


Selector-based tours from WalkMe and Pendo carry this maintenance burden by design. Someone on your side has to repair every break, every release, forever. Users hit the broken steps in the meantime and learn to ignore them.


The Frigade Assistant takes a different approach. The agent re-resolves against the live product on every run, so there's no selector cached anywhere to break in the first place. The team gets the time back, and users get the right walkthrough.


## The honest take on traditional product tours


Tours break, but that's the symptom. Products have outgrown the static-tour model entirely.


Products today aren't static. They ship continuously, they branch by user role and plan, they surface different UI depending on what a user has already done. The experience a power user sees on a Tuesday afternoon looks nothing like what a new free-tier signup sees on Monday morning. The number of meaningful paths through your product has exploded.


A static tour assumes one path, one layout, one audience. That assumption was strained five years ago. Today it's untenable. You can't hand-author a walkthrough for every combination of persona, feature flag, and onboarding state. You'd need a small team just to keep up, and they'd still always be behind.


As products get more personalized and more dynamic, the maintenance curve for static tours goes vertical. The only exit is guidance that's generated and adapted at runtime, anchored to what the product actually looks like for this user, right now, not what it looked like when someone recorded a walkthrough six months ago.


Tours are hard to maintain, and for modern products they were never going to keep up in the first place.
