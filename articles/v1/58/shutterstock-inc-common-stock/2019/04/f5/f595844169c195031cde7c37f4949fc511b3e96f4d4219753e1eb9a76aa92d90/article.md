---
schema_version: "1.0.0"
document_id: "f595844169c195031cde7c37f4949fc511b3e96f4d4219753e1eb9a76aa92d90"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/04/02/demystify-story-points"
published_at: "2019-04-02T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:e199aeb543d6275b99c0ffedcd8227f833dba3bb2083031cb89d4624b82a973a"
---

# Demystifying Story Points

Estimating work accurately is probably one of the most challenging skills for software engineers to master and perform through their careers. As such, how agile engineering teams assign points to their sprint backlogs is a[subject](https://www.scaledagileframework.com/story/) of[ongoing](https://www.scrum.org/resources/blog/why-do-we-use-story-points-estimating)[debate](https://www.scrumalliance.org/community/articles/2017/january/story-point-estimations-in-sprints) that traces its roots all the way back to the beginnings of Scrum.


Regardless of what methodologies you use to determine point estimates, the point scale that you choose, or the length of your sprints, the tips in this post will hopefully aid in demystifying how to approach assigning points to backlog tickets.


## Can’t We Just Use Time-Based Estimates?


At some level, “points” are an abstract representation of the quantity of work required to complete a given task.


Though time-based estimates are easy to understand (e.g. 2 days, 1 week, etc.), there are a few general flaws in viewing or determining estimates from that perspective:


1.


The estimate becomes invalid if the person who estimated the work is different than the person executing the work. The reason for this is that these estimates are dependent on an individual’s experience and knowledge level, which are difficult to account for.


2.


[Parkinson’s Law](https://en.wikipedia.org/wiki/Parkinson%27s_law) : work expands to fill the time allotted for its completion.


3.


[Planning fallacy](https://en.wikipedia.org/wiki/Planning_fallacy) and optimism bias: wishful thinking leads us to override the voice of our experience, allowing overly-optimistic views to color our estimates.


Note that if the work being estimated is repeatable and more or less identical each time you perform it, our estimation should get more accurate. However, software development projects or features are rarely repeatable or identical. In the cases where they are, that work can probably be automated.


## How Can Teams Go Wrong With Points?


Story point estimates intend to address these flaws by removing time from the equation; yet this is easier said than done—eventually we need to produce a release date. We normally think about our work in terms of how long it’ll take us, so it’s logical to map that directly to a point-based scale. However, when we do this we are susceptible to the pitfalls articulated above, now masked with the notion of “points” which makes these pitfalls much harder to spot.


## How Can We Get Better at Point Estimates?


Regardless of what type of point scale you use and the length of your sprint, here are some tips to improve your grooming sessions and make assigning points feel less like pulling rabbits from a hat!


### Complexity


It’s common to hear that points are a measure of the relative complexity of the work. When grooming tickets, discuss the relative difficulty of the work itself. You can do this by comparing the work to other items that you have completed recently. Avoid the trap of thinking about how long it will take to do the work, as even some simple tasks may take a lot of time.


### Risk


What’s the potential impact of the work we’re doing? Is it localized to a very specific area? Is there potential for it to have an impact across multiple areas? Not all code is created equal, and, generally speaking, a change that involves 100 lines of code in one module is probably less risky than changing 100 lines of code across 10 different modules.


### Unknowns


Finally, we don’t know what we don’t know, so it’s important that we assess the level of unknowns specific to a change. This estimation should draw from the team’s collective experience, knowledge of the platform, and understanding of the work required, rather than any one individual’s.


### Putting it All Together


“Complexity” and “risk” are assessments that are based on your level of knowledge, and to balance our natural tendency to be optimistic we need some hard-nosed realism. That’s where the “unknowns” measure comes into play.


Regardless of what story point scale you use, you can combine those three scores into one to determine a story point estimate. Every team member present in grooming produces a scorecard like the above, which are combined to produce the point estimate, like so:


1.


Get the average of each individual’s scores to produce their average individual point estimate, \\(p_i = {w_1C +w_2R + w_3U \\over 3}\\) where \\(C\\) is “complexity”, \\(R\\) is “risk” and \\(U\\) is “unknowns”


2.


Discard the highest & lowest scores as outliers in the list of participant’s points: \\(\[p_1 .. p_{n-2}\]\\)


3.


Average the remaining points to produce your final estimate: \\(P = {\\sum_{k=p_1}^{p_{n-2}}k \\over {n-2}}\\)


You can experiment with the weights \\(w_1, w_2, w_3\\) for your team, but I would recommend you have a higher weight for “unknowns”. On our team we assign twice as much weight to “unknowns” as we do to each of the other dimensions.


Once you have produced this average, round it up to the next point that makes sense in your point scale. For example, if the average was 10.72 and you are using a Fibonacci point scale, you would round this up to 13 points.


We also do a quick variance computation across all the scores. If there’s a wide variance between the individual scorecards it means the team is not aligned on some of the three criteria and it’s crucial that the team discuss this ticket more. This prevents us from falling into yet another trap of estimation: often what’s common knowledge to you may not be common knowledge to the whole team.


### Don’t Forget, They’re Relative Estimates


Last, but not least, we often forget that story points are a relative measure of work. The final pitfall teams will often fall into is failing to compare one story’s points to the points that they’re giving other tickets, or to stories that they’ve completed in the past. A good exercise to help here is to review all similarly pointed tickets to ensure that all of these items are actually similar to each other in scale. You’re not looking for a perfect match here, but often a couple of tickets may stand out as mispointed after the fact. In this case, flag those tickets, review the breakdown and re-groom the tickets.


### What if We Can’t Agree on the Reason for Variance?


Sometimes you will not achieve alignment across the team on the variance in your estimates. This is a great opportunity to evaluate the breakdown of the work itself. You are always in control of the breakdown, and everything can be further broken down to the level where your team will be more or less aligned on these three dimensions.


To summarize, if you are able to get alignment on “complexity”, “risk” and “unknowns” across your team, you will arrive at better estimates and breakdowns of the work and have beautiful burndown charts sprint after sprint!
