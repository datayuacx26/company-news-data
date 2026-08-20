---
schema_version: "1.0.0"
document_id: "6d9ec4c6a6eb420cffac2934d6c669b4450f89caa053629d5a8c1b6e81ac13a4"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/good-and-bad-agentic-behaviors/"
published_at: "2026-08-07T13:01:00+00:00"
first_seen_at: "2026-08-07T15:08:08.883286+00:00"
fetched_at: "2026-08-07T15:08:09.986593+00:00"
content_hash: "sha256:9d7eb7d0ef0542a41d5a1d122fedbf61f59ca67ab694331635e2cbecf86969d4"
---

# Unveiling good and bad behaviors on the Agentic Internet

The Internet isn’t a single lane of traffic. For a long time, the rule of thumb in web security was that bots are bad, while humans are good. Of course, we’re far past this generalization. Humans can be fraudulent, and bots can be helpful at different levels. Site owners actively want some automated traffic to interact with our sites to make the Internet functional and discoverable.


To complicate things further, the line between "human" and "bot" is blurring more and more. Now, we have a type of “hybrid” traffic where a single session shifts from human to agentic and back again. (Think of a user browsing a store, and then handing off the checkout process to an automated shopping assistant.)


So, how do website owners manage this kind of complexity? What matters here is assessing **behaviors** . Is this behavior abusive? Malicious? What’s the risk presented here, and can I trust this visitor based on their actions? Solving this requires moving beyond static, point-in-time checks. It requires analyzing continuous behaviors to evaluate Trust.


In this post, we’ll share an inside look into the strategy of the Web Integrity & Trust team (covering the bots and fraud problem spaces) around detecting and analyzing good and bad behaviors, providing tools to help site owners tackle emerging challenges in the shifting Agentic Internet. We’ll also share findings around agentic traffic since the launch of[Precursor](https://blog.cloudflare.com/introducing-precursor/) , and a simulation where you can see how your own cursor movements would be assessed as human or bot — plus some exciting launch updates to expect in the near future.


## Defining Risk and Trust


Let’s talk about the distinction between **Risk** and **Trust** , the way we discuss it within the teams at Cloudflare who work in bot detection. These are often viewed as polar opposites of a continuum. At Cloudflare, we look at them as independent, but reciprocal, values. Trust is the essential ingredient in making informed decisions on what to do about your traffic.


Risk is how likely something like a request or action is to be harmful, and it’s often ephemeral. Trust, however, is built up over time, and it’s based on reputation.


We can illustrate this with an example from real life: say that you’re enjoying some evening television at home, when suddenly, you hear the doorbell being rung repeatedly. Besides being annoying, this behavior is strange. Frantic doorbell rings late at night are alarming.


You check through your door camera and see that the person ringing your doorbell is your best friend who lives next door. Of course, you trust your best friend, and we’d bet you would let them in.


In this example, it wouldn’t be enough for you to say, “Reject anyone who rings my doorbell at night” or “Reject anyone who rings my doorbell more than 10 times.” Again, Trust is the essential ingredient.


Going back to traffic on the Internet, the strategy as we build products in the bots and fraud space focuses on building an entire ecosystem based on Trust. And our goal is to provide the incentives and primitives for site owners to use to incentivize behavior that makes the Internet safer for everyone: starting with blocking malicious activity at the bottom, to encouraging participation in a safer Internet at the top.


A spectrum of trust from the highest trust behaviors at the top, to untrusted malicious behaviors at the bottom. An additional column indicates how that behavior may typically be handled by a site owner.


## Good behaviors, rooted in transparency


Starting at the top: what counts as good behavior? We can draw clear examples from the Verified bots and agents within BotBase. Last month, we announced an[updated pragmatic taxonomy for the good bots we track in our system](https://blog.cloudflare.com/content-independence-day-ai-options/) , boiling down the definition of “[Verified](https://developers.cloudflare.com/bots/concepts/bot/verified-bots/) ” to two things: 1) you declare yourself honestly, and 2) you don’t abuse the trust you’ve earned.


Transparency between a site owner and a bot operator allows for a symbiotic relationship: site owners can note what behaviors and data uses they want to allow on their websites, and bot operators can be granted access more easily. The transparency allows for Trust in the relationship; if you have nothing to hide, declaring who you are should reduce friction from the sites who want to allow your behaviors.


[BotBase](https://developers.cloudflare.com/bots/botbase/) is not meant to only make declarations of “who is good”. It is intended to be a directory of all known bots and agents, and provide the facts. Compared to our previous Bots Directory, which only included known good bots, BotBase is also capable of tracking *less-than-good* bots and agents. Why? Because our systems track and validate behavior for known good actors, meaning we have the tools to identify when these expectations aren’t met. If you abuse trust on the Cloudflare network, you should **not** be easily allowed, so you will be unverified.


## Bad behaviors: blatant, stealthy, and everything in between


A few weeks ago, we announced[Precursor](https://blog.cloudflare.com/introducing-precursor/) , a continuous client-side system to detect even *subtly inhuman* bot traffic that can fly under the radar when assessing network signals alone. When a customer enables Precursor, the JavaScript detection is CDN-injected, so it doesn’t require sitting at the computer and figuring out where or how to rerun these detections. What's more, Precursor evaluates user behavior[continuously throughout the session](https://developers.cloudflare.com/cloudflare-challenges/precursor/) , so no more free hall passes for abusive traffic that found a way to pass client and browser-side checks just once.


Applying our Risk and Trust framework to these client-side detections, we can point out that CAPTCHAs or one-time hurdles are Risk-based, meaning they lack *context* . On the other hand, verification using behavioral tells is Trust-based, since it can capture more context clues from the full user session. Precursor is the tool for us to analyze this behavior. To sum it up, Precursor is so powerful because it:


1. Provides Trust-based detection over the entire user session.
2. **Drives up the cost for bot developers** to replicate human behavior over a multipage timeline.


By making it *economically disadvantageous* for bot developers to outrun these detections, we win the adversarial game.


Now, what have we learned since we’ve launched? Looking at just a 24-hour period at the time of writing this blog, we can see **206 million Precursor evaluation events** , across **73,438 zones** on the Cloudflare network.


A screenshot of usage statistics of Precursor in a 24-hour period.


We can see patterns in the data that reveal things that we had suspected when launching the detection, but can now validate across tens of thousands of domains:


- Suspicious behavior often happens mid-session, which point-in-time detection wouldn’t catch.
- **Behavior often shifts from human to agentic and back over a session** . In these cases, it’s important to understand the *intent* so that site owners don’t block user flows that they actually want.


- This highlights the importance of a bot classification system that allows website owners to handle traffic by use case, purpose, and data use. This is precisely why we prioritized taxonomy updates for BotBase.


For those curious to learn more on how Precursor actually works, we shared a sneak peek — how the signals we analyze showed us that to err is human — in our announcement[blog post](https://blog.cloudflare.com/introducing-precursor/) . **Today, we’re going a step further: giving anyone on the Internet an interactive demo simulating how Precursor would trace your cursor movements.**


Screenshot of custom cursor movement being assessed by Precursor.


**[Precursor Trace](https://precursor-trace.cloudflare.app/)** is live now, sharing how we’d assess *your* cursor movements using (part of) Precursor’s detection mechanism. Here, you can see whether you’re accelerating or correcting yourself, the rhythm and texture of your cursor movement, and more — all things you’ve probably never thought about as a real human being interacting with a computer. Try it out!


## Adaptive Intelligence is coming soon


Cloudflare’s[bot detection engines](https://developers.cloudflare.com/bots/concepts/bot-detection-engines/) can produce[different outcomes](https://developers.cloudflare.com/bots/concepts/bot-score/#bot-groupings) when assessing if a given request is automated or not. For requests that are deemed to be automated, the assessment can be 1) definitely automated, based on proven, deterministic methods or fingerprints of bots, or 2) likely automated, based on predictive scoring from Cloudflare’s Bots ML.


Historically,[Bots ML](https://developers.cloudflare.com/bots/concepts/bot-score/#machine-learning) has been updated in versions, meaning we announced each new model version as a product launch. This pacing doesn’t work when bots adapt on the scale of hours or even minutes.


Adaptive Intelligence, a completely new detection engine, is different from anything we’ve built before in the Bots ML space. **The model itself is adaptive** . It has learned from everything we’ve seen in the past, but more importantly, it will *continue* to learn and self-adjust based on what it sees. Adaptive Intelligence will upgrade itself based on a wide range of traffic patterns we identify, from good to bad behaviors, and customers will no longer need to upgrade to a formal new model version to have the latest predictive bot detections working for them.


All Bot Management customers will have access to Adaptive Intelligence in the near future — stay tuned for the launch announcement coming soon.


## Moving beyond determinism to influence bot behavior


So far, we’ve focused on Cloudflare’s side of things: strategy, detection, and taxonomy. All of this allows Cloudflare to equip website owners with the tools they need to set the traffic policies they want on their sites. Zooming in on the website owner side, we want to take this chance to discuss some **advanced mitigations** that allow website owners themselves to influence bot behavior.


With more blatant mitigation techniques, we face something that we’ve nicknamed the “Bot Antibiotic Problem.” Always sending bots a deterministic response (like a 403 block) makes it easy for a malicious developer bot to probe, observe, and reverse-engineer your defenses.


We know this, so we’re designing mitigations *specifically made for throttling bots* — with different approaches for malicious bots vs. benign bots. We can break them down into three approaches:


Approach 1: Unpredictability and Random Actions. Applying random responses (between block, challenge, or allow) to suspected automated traffic breaks a bot's automated retry logic and fingerprinting.


Approach 2:[AI Labyrinth](https://developers.cloudflare.com/bots/additional-configurations/ai-labyrinth/) , a defensive response that traps unauthorized bots in an endless maze of AI-generated web pages. You can waste malicious bots' compute and crawl budgets by using *misdirection* . Site owners will be given three options within AI Labyrinth, depending on their preference:


- **Maze** : Generates an endless web of linked pages for bots to follow.
- **Summary** : Feeds crawlers an LLM-generated summary of a page that looks real but is entirely useless as AI training data.
- **Poison** : Serves deliberately fake content (like fake prices or inventory) to a bot, polluting the data it collects for AI training.


Approach 3: Queuing for Good Bots. Not all agentic traffic is bad; queuing manages throughput for legitimate automated traffic (like user-directed shopping agents) without denying them service entirely.


These advanced, bot-specific mitigations are set to roll out closer towards the end of the year, and will be available for the website owner to choose how strict they want their mitigations to be.


We also know that a great defense is a predictive one — one that self-learns and course-corrects without needing multiple security experts on a call to reactively set a fix that accounts for the latest stealth attack. This might look like having a system of “disposable” rules, in which the ruleset is dynamic in nature. This is by design: if attacks constantly evolve, the defenses should, too. That’s why we’re working to keep both detections and mitigations a step ahead.


## Establish the Trust ecosystem that works for you


Anyone and everyone can take steps to define how automated agents interact with their infrastructure.


A few things to try:


- Turn on[Precursor](https://developers.cloudflare.com/cloudflare-challenges/precursor/#get-started)
- Play around with[Precursor Trace](https://integrityand.trust.cfdata.org/precursor-trace/)
- Explore[BotBase](https://developers.cloudflare.com/bots/botbase/)


By moving away from static, point-in-time checks and embracing continuous trust evaluation, we reduce the game of whack-a-mole with bot operators. If you’re not already using[Cloudflare’s bot detection](https://www.cloudflare.com/products/bot-mitigation/) , check it out and establish the Trust ecosystem that works for you.
