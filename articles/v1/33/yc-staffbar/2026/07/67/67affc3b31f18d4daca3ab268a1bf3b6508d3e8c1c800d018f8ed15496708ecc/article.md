---
schema_version: "1.0.0"
document_id: "67affc3b31f18d4daca3ab268a1bf3b6508d3e8c1c800d018f8ed15496708ecc"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/how-we-test-paywalls-at-superwall-and-how-you-can-too"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:5ec4395a1e438afff7b7ca849f988337dccdfe7d8ecb95a4703ec913912831bb"
---

# How We Test Paywalls at Superwall (And How You Can Too)

Testing paywalls is in our DNA at Superwall. Showing the right paywall, with the right content, to the right audience, and at the right time can be the difference between a few bucks and thousands of dollars. But, it's hard to hit that mark without knowing what to test. At Superwall — we use a tried and true framework we've developed. Informed by data, battle-tested and backed up by real results, here's how we tend to structure our experiments:


These are the critical components to paywall testing, and using them to inform your decisions will lead to more revenue. Using this framework — on average, we see a 20% lift in revenue using these core components to guide our tests (and that's the *floor* , usually the results are even better than that).


Internally, we like to call each of the categories a broad lever. And, the items which fall under each of them are sub-levers. So, let's break each lever down, and use this paywall as an example:


Our basis for paywall testing.


## Where *not* to typically start


Before we dive into each lever, we need to reinforce why we use this framework. With the Superwall platform being so powerful, and testing so easy — many developers will do something like this:


Testing two completely, and wildly, different paywalls.


Usually, we don't tend to start tests that way. There's simply too much that's changed — making it hard to isolate *why* something is working (or isn't!). By introducing a variable into each of our core testing tenets (personalization, price and packaging, design, messaging, and placement or frequency), we take on too much. The signal versus noise ratio is too off-balance to make good product decisions.


In short, you don't want to (typically) test each lever at once. We try to stay within one broad lever, with the understanding that there will be some natural crossover between them from time to time. By isolating a lever within a test as best we can, we're able to figure out what's truly working.


With that said, let's look at how to test each of these on their own.


## Price and packaging


Price and packaging is likely what comes to mind first when testing paywalls. Here, you're primarily trying to figure out ideal price points which strike a good balance between conversion, trial start percentages and ultimately, revenue. In addition, the way that you present these options can have major implications across all of those metrics, too. In this lever, you could also try things like exit offers, change which plans are shown, and generally take a few bets on anything related to prices and the way they are ultimately displayed.


For example, beyond simply pricing two different packages — you could change the way they are presented:


The same prices, presented differently.


On the left hand side, the control's user experience remains. On the right-hand, we've moved to a horizontal row of the two products. Altering the pricing UX is a test we internally use quite often — and for good reason, they way users perceive pricing, value and trial eligibility are usually direct results of how they are offered (regardless of the actual prices themselves).


Similarly, you can run a simple price test. Here, you've got the same paywall — the only difference would be the products on offer:


Control Variant Change


Annual: $19.99 Annual: $24.99 + $5.00


Monthly: $2.99 Monthly: $4.99 + $2.00


In the image below, that's exactly the type of test we're running:


A price test experiment puts two or more packages against each other.


Price tests are *extremely* powerful. I find this doubly true for indie developers, who tend to underprice their offerings. If you're an indie reading this, this is the first type of test I would recommend you try. By their nature, you stand to boost your revenue using these types of experiments more-so than others.


## Paywall design


When we run paywall design tests, we are running one *close* to what I mentioned at the very beginning — a bold, different direction for your paywall's design. The paywall might evolve holistically, with changes spanning across the user experience, its content, the design, or overall format. Here, you're using what you know about your audience, users and their behaviors to take a bigger swing with a new approach to your paywall.


In our case, maybe we'd consider that social proof is important to our users, so we might try placing reviews prominently on a paywall, along with any accolades from Apple:


A new design, using social proof.


While that is a drastic change (an entirely new design), opting for smaller refinements on your existing paywalls is a viable experiment to run, too. For example, a classic Superwall tweak we've seen work *over and over* is simply adding a "✅ No Payment Due Now" text label above the payment button:


This label reinforces the free trial that users can claim.


It's such a small change, isn't it? But we've seen it work in many cases, and we believe it's simply because consumers are reassured that no charge will occur if they continue.


While anyone reading this post may think that's quite obvious, remember — iPhones are a *global* product. Billions use them, and outside of the world of tech — the way these things work (i.e. free trials) may not be common knowledge. It's a poignant reminder to often question your assumptions, and reinforces why design experiments can be so valuable.


Within the design lever, we also tend to try different user experiences, too. Perhaps a multi-page paywall, changing up the imagery that's shown (i.e. lifestyle shots versus product shots), making a paywall visually driven or text driven, and other similar changes all would fall within this lever.


## Messaging


Next, let's cover messaging experiments. Social proof could be a form of messaging tests (something we briefly touched on above), the way your free trial is presented, or more generally — the way you present value proposition. In essence, we're primarily talking about copy here. How you sell a subscription matters, and if you have a pretty design with terrible copy, your conversion rates will suggest as much.


Since we covered an example of social proof, let's try tweaking our value proposition with stronger copy. When I run these types of tests, I always come back to the classic "iPod" example. Which line do you think sold the original iPad:


1. Over 10GB of storage.
2. 1,000s of songs in your pocket.


Of course, the second one is much stronger. It tells a story and pushes the benefits, the other one simply states a fact. These are the kinds of improvements you can attempt to make on your own paywalls, too. Let's look at some of the copy we're working with right now:


- Headline: Understand the math of caffeine
- Graphs and charts
- Caffeine half life
- Sleep impact score
- Quick caffeine logging


To me, a lot of those read as facts. What if we tried something like this?


- Headline: Your caffeine, visualized. Your energy, optimized.
- Watch caffeine fade with smart visual graphs
- Know your exact half-life — personalized to you
- See your sleep impact score at a glance
- Log caffeine instantly, right when you drink it


Updated value-driven copy.


While you may think that's an improvement, the challenge here is that value-driven copy can tend to run longer. That's doubly true in a more constrained interface context, like these bullet points. That's why (again!) you should test any change like this.


## Placement and frequency


This type of test is all about timing and pace. *When* do you show a paywall? And, how much? For example, many apps I run across forgo showing a paywall during onboarding. While there is no one-size-fits-all advice to paywall testing, in my opinion — encouraging developers to show one after onboarding is about as close as it gets. Many apps see more than 50% of their revenue being captured during onboarding.


In that example, it's a *when* situation. If you were testing how many times you present a paywall, that's a *frequency* change. Thankfully, running these types of tests are trivial in Superwall. We've built these capabilities right in. Let's check out some examples.


When it comes to showing paywalls at certain times, we recommend you register several[placements](https://superwall.com/docs/campaigns-placements) (what Superwall looks at to determine if a paywall should show) even if you don't intend to show a paywall for now. Why? It gives you tremendous flexibility, because you can toggle these placements on or off anytime in Superwall (no app update required):


Click the pause icon to halt any active placement.


So, if we had code like this...


```text
Superwall.  shared  .  register  (  placement  :   "  logCaffeine  "  ) {
store.  logEspresso  ()
}
```


...now we can remotely decide if logging caffeine show should free users a paywall or not. If we pause it, the caffeine is logged. If it's active, it'll show based on your campaign filters. This opens up a whole world of flexibility, where you can decide what's paywalled on the fly. So, all of that to say — register your placements *anywhere* you may want to show a paywall. Then, you'll be in the best position to test whether or not showing paywalls at critical junctures helps or hinders.


In a similar vein, you can also easily tweak *frequency* . For example, you can do things like:


- Only show this paywall once. Or twice, three times — whatever.
- Show it every third time.
- etc.


Setting limits on an audience.


You can learn more about setting up limits[here](https://superwall.com/docs/campaigns-audience#setting-a-limit) , but again — Superwall gives you the tools to perform tests like "What happens if we show a paywall up to three times a day when someone attempts to log espresso, but we want to test three different paywall variants when that happens" easily.


## Personalization


Finally, we arrive at *personalization* , which is typically the most broad of all the levers in our framework. When you test *personalization* on a paywall, you change one or more of these components:


- **Audience** : Who sees the paywall?
- **Content** : What's on the paywall?
- **Pricing** : How much do the products cost?


For our paywall, we could try slightly tweaking content. In our control, we're using the "Free Trial" toggle pattern, which can sometimes lead to more trial starts because users aren't always consciously aware that there *is* a free trial available. However, we can test that assumption by putting it up against a paywall that removes it, and changes the call-to-action button to read "Start Trial":


Testing content changes on a paywall.


Setting these two paywalls up against each other will lead to some interesting insights, which opens you up to experiment with other aspects in our testing framework.


Another example? Consider an onboarding flow for a sports betting app, where a new user selected which sports they might be interested in. Using that, you can turn around and personalize the paywall to fit their needs (you can do this in Superwall with[placement parameters, too](https://www.youtube.com/watch?v=7KT9FfKLByA) ). This form of personalization makes users feel like they've stumbled upon an app that's just for them — and the better you solve their problems, a higher percentage of conversions will follow.


Also, a quick tip — to run these tests easily, just duplicate the control paywall in Superwall and make the changes you need:


Duplicate a paywall to quickly make changes.


Then, you can assign a percentage of who should see which one (i.e. 50%/50%, 60%/40%, etc.):


You can control what percent of users should see any paywall.


## Wrapping up


There is no magic bullet to paywall testing, the only way to know what works is to...try it! And, there's no easier way to do it than Superwall. Running these kinds of experiments can quickly boost your revenue — but success depends on two things: your ability to test, and knowing what to test. Combine what your gut is telling you might work, along with your product knowledge, with our testing framework. You're likely to start making more money, or learn what's not working along the way.


And remember, don't get caught up in the "perfect" system for tests. While our system acts as a guardrail, sometimes you just need to go for it, too. If you have a big idea that breaks our own framework, don't be afraid to try it! We certainly do.


So, take a look at all of the levers, and pick one to try first. If you're not sure where to begin, starting with slight copy changes or smaller design tweaks is a solid start. If you have a strong paywall already, then maybe price testing would make more sense.


The point is to experiment! Find a winner, and then try and beat that one next. Rinse and repeat. Get started with paywall experiments today by signing up for a[free account](https://superwall.com/sign-up) .
