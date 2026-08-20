---
schema_version: "1.0.0"
document_id: "e103f9c68d6691385db9ed4600c4d443040a15f7019e3239d3b49328984dc25d"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/your-ideas-are-terrible-find-out"
published_at: "2026-07-17T20:18:21+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:60d21577ebf55c732c3b622eeb6d9a22e9623f7c2febccec6b942e73d42fa0df"
---

# Your Ideas Are Terrible---Find Out Cheaply, in Private

# Your Ideas Are Terrible---Find Out Cheaply, in Private


### Using Expected Parrot for "pre-mortem" simulations


[John Horton](https://substack.com/@johnjhorton)


Jul 17, 2026


**tl;dr - A “pre-mortem”—where you have people imagine your idea has already failed and the goal is to discover why—is a powerful tool, but takes a lot of people and time. With Expected Parrot you can use simulated AI people to get most of the benefit.**


Despite the true value of “learning from failure," all else equal, it’s better to just succeed right away. Of course, “do things likely to succeed” is the buy-low-sell-high of planning advice. What is to be done to actually make success more likely or choose better paths? One approach is to just think real hard about what might go wrong and then either scrap the idea or plan various mitigations. This is a good idea, but sometimes it’s hard to be truly critical of an idea if you’re excited about it, and you often lack the full context to understand why failure might arise.


Thanks for reading! Subscribe for free to receive new posts and support my work.


## Getting true critics is challenging


What you really need is


*other* people to help crap on your idea, especially if those people bring a different, relevant perspective. But in any actual team setting, this creates some other problems because “person who craps on other ideas” is almost the opposite of being a team player. As a case in point, I once worked on a team where one of the engineers had a reputation for finding reasons why something was a bad idea or impossible. A teammate said one of the most damning putdowns I’ve ever heard: “Joel is the world’s expert in what cannot be done.” Ideally, you can present an idea to the team that is already thought out, with most objections planned for, that getting consensus isn’t a slog (or you’ve nipped the bad idea in the bud).


## The “pre-mortem” management technique


As a business school professor, I have an encyclopedic knowledge of various management techniques and by that I mean “I looked up on Wikipedia” the idea of a


[pre-mortem](https://en.wikipedia.org/wiki/Pre-mortem) . The idea is quite simple: you get a group of people together—relevant stakeholders—and


**assume** the initiative has already failed. The only thing left is to figure out why. One of things that is nice about this approach is that people have permission to play the role of critic. After you collect the reasons for failure, you can do some fancier stuff to trace out the causal pathways. But people are busy and meetings are expensive.


## Enter simulated AI naysayers


This “have people play the role of critics” doesn’t necessarily require real people. In fact, you can have AI play those roles, get their (very detailed) feedback and then do the same kind of analysis you might do with real people. The big advantages, however, are:


-


The time of the AI fake people isn’t worth very much—you can get their views for pennies.


-


You can get their views with guaranteed, assured privacy.


-


You can try lots of iterations without the AI people wanting to strangle you for boring them or wasting their time.


## Why Expected Parrot for this?


You could try this in a normal chat interface, but there are limitations. Let’s ask Claude for a full explanation of why you should use us instead:


> <🤖>
>
>
> **Why use Expected Parrot rather than one-shotting with Claude**
>
>
> **Independent agents vs. one voice.** In a single chat, one model generates all “personas” in a single pass, so they contaminate each other — the critiques converge and echo. Expected Parrot issues a separate API call per persona, so each reasons independently. You get genuine variance instead of one model’s averaged impression of what a crowd would say.
>
>
> **Multi-model.** A chat is locked to one model. A survey framework lets you run the same questions across different models, which surfaces disagreement and reduces any single model’s idiosyncratic blind spots — closer to real panel diversity.
>
>
> **Structure and reproducibility.** It’s a framework built for this: defined personas, consistent question batteries, structured (parseable) outputs, and the ability to re-run with tweaks. One-shot chat output is prose you’d have to manually wrangle; here you get data you can aggregate, compare, and version.
>
>
> **Scale and iteration.** You can run dozens or hundreds of personas and many iterations cheaply and systematically, rather than repeatedly re-prompting and losing track of what changed.
>
>
> **Reusable digital twins.** As the post notes, personas can be grounded in real survey or observational data to build persistent “digital twins” you reuse across studies — something a fresh chat can’t retain.
>
>
> **Tooling and skills.** The agent chains web search (the Firecrawl plugin) and packaged skills like the pre-mortem workflow into the run automatically, so the critique is grounded in current external info rather than the model’s static knowledge.
>
>
> The short version: a chat gives you one model’s impression of what critics might say; Expected Parrot gives you a structured, multi-agent, reproducible simulation of critics — which is what makes the output feel like data rather than a monologue.
>
>
> </🤖>


## Using the Research Agent to kill our sailing kayak dream


Robin and I were thinking about getting a sailing pedal kayak. They look really fun: you pull right up on the beach (no keel or centerboard); you can paddle, and your legs are stronger than your arms; and if there is a breeze, you can take advantage of it. But, they are not cheap! We were ready to buy, but I thought, “eh - maybe I should see what our Research Agent thinks.”


From


[expectedparrot.com](http://expectedparrot.com/) we launched a session and asked for a pre-mortem on this potential purchase. Note that I included a bunch of context that will help the Research Agent do something smarter and less generic. I explained what other boats I already have, our personal preferences, where we live and so on.


The Research Agent starts immediately doing some web search using a plugin we have with


[Firecrawl](https://www.firecrawl.dev/) (this makes scraping more efficient than doing direct web searches):


Another thing you can see in the tool calls that is leans on a skill / package we’ve created for doing these kinds of pre-mortem exercises.


It sets up personas for our AI critics:


Where are these personas coming from??


The research agent just makes up personas that are relevant to the decision problem at hand. Though, critically, you could create your own digital twins from past surveys (either your own or others), observational data you have, etc.


## The report


The report is


[here](https://www.expectedparrot.com/research-agent/share/akw1RJN4BouvzTmI9klxnw) . It’s quite interesting and raises some considerations, some of which I agree with, some of which are oversold. For example, it says the tides are a big issue—they aren’t. It drew a diagram showing how everything failed:


The root causes are worth thinking about. 1 isn’t a big deal—it overstates the tides. 2 actually is worth thinking about because it doesn’t have a dolly. But 3 stopped me dead in my tracks!


You need to rinse these things with fresh water after every use! Or as Derek puts it:


We don’t have freshwater where we could store them! This is a total deal-breaker. Purchase canceled!


We, as a company, do not aspire to be the #1 tool for hybrid watercraft purchase decisions. The point of this example is to show-off the technique and the kind insight this can generate. But what might be commercial applications? Let’s go back to Claude:


> <🤖> The most valuable commercial use isn’t personas reacting to things in general—it’s aiming that simulated-critic machinery specifically at initiatives a company has already decided to pursue, precisely when enthusiasm and sunk commitment make honest internal criticism scarce.
>
>
> -
>
>
> A retailer about to sign a lease for a new location can run a pre-mortem in which simulated store managers, local competitors, and target shoppers assume the store failed within a year and explain why—surfacing the freshwater-rinse-level dealbreaker (no parking, a competing anchor tenant leaving the mall, foot traffic that evaporates after 6pm) before the lease is signed rather than after.
>
>
> -
>
>
> A SaaS team convinced a new feature will drive retention can assume it launched and nobody adopted it, then let simulated power users, skeptical admins, and budget-conscious buyers reconstruct the failure path: it collided with an existing workflow, IT wouldn’t approve it, the value wasn’t obvious in the first session.
>
>
> -
>
>
> A pharma or hardware company can pre-mortem a launch, a marketing team a campaign that “already flopped,” a PE firm an acquisition that “already went sideways,” an HR team a return-to-office policy that “already triggered attrition.”
>
>
> The through-line is that each of these is a decision the organization is emotionally and financially committed to, where a real pre-mortem is exactly the right tool but expensive to convene and awkward to run honestly—and where simulated critics let you trace the causal pathways to failure cheaply, privately, and as many times as it takes to either kill the idea or harden it.
>
>
> </🤖>


If this sounds like you, sign-up for an account and try it out. It comes with $25 in free credits!


Thanks for reading! Subscribe for free to receive new posts and support my work.
