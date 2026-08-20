---
schema_version: "1.0.0"
document_id: "c2842392ebb069f0d3c3d065dbd7366399d139423f2b58ac570984fbf92b2342"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/borrowed-gravity-words-worth-changing"
published_at: "2026-07-02T14:00:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:acb5ee497fc55c8e2310289c17313a75376df3540f1e06d0514201c155e1895c"
---

# Borrowed gravity: words worth changing

"[AI SRE](https://rootly.com/ai-sre-guide) " is a category now. Analysts use it, buyers search for it, more than a dozen vendors put it on their homepage, and Rootly helped create it. But we don't love the name, and we've never fully made peace with it. That's an awkward place to write from, so this post starts there.


One of us builds tooling at Rootly; the other writes about and practices SRE. However, we share an irritation: most of the words our industry uses are borrowed, and most of them are wrong. A borrowed word fails in one of two ways. It takes on weight it hasn't earned, or it shrinks the job down to its smallest, most visible piece. They don’t all cost the same; some are not to our taste, and a few actively mislead the people who buy reliability tools and staff reliability teams.


Here are four worth changing, and what each one gets wrong.


## Firefighter: borrowed danger


Firefighters run into burning buildings. They carry strangers down stairwells full of smoke. Some go to work and don't come home. When an alert pages us and we open a laptop in a quiet room, we are not doing that. It borrows from people who face real danger every shift. What's actually happening is that a service degraded and someone is restoring it. Less thrilling, still true, and still important. Adam has talked to SREs keeping life-saving software running. Sebastian works for a company whose systems get the right meals to critically ill patients at the right time. The stakes there are real, but that's the rare exception, and clearing that bar still doesn't make the rest of us firefighters.


## War room: borrowed discipline


A real war room runs on discipline; one commander, clear comms, every decision logged, calm held by training. When a Slack channel borrows the name, it borrows that gravity and almost never the discipline that earns it. The word implies a chain of command and a controlled process; the channel it's pinned to is usually neither. Good incident response does want what a real war room has: a calm channel, one clear lead, decisions logged as they happen. What it doesn't want is the rest of the metaphor. A real war room carries stakes a Slack channel never will, and nobody in the channel is fighting for their life. The teams reaching for the word are usually reaching past the thing that would help them, which is the discipline, not the drama. Name the channel for the discipline you want in it, not the war you're not in.


## Postmortem: borrowed death


A postmortem is what gets performed on a body. Someone died; cut it open, find the cause. Finding cause is the part a retrospective shares, but an autopsy ends there. The patient is gone, and naming what killed them changes nothing for them. A retrospective is after something the autopsy can't reach. The system is still alive, and the point is to learn from the failure and change what happens next. The death frame gets that backward: it treats a living system you mean to improve like a corpse you mean to explain. There's a tell, too, that the word works against you. The field had to bolt "blameless" onto the front of it. Only "postmortem" needs the disclaimer, because the root word drags in the morgue. We run retrospectives, and we shouldn't have to call them blameless.


## AI SRE: the shrink


The first three take a word that's bigger than the work, a danger, a discipline, and a death the keyboard has no claim on. AI SRE does the opposite: it shrinks the job to a single task.


"AI SRE" tells the market that being an SRE is mostly root-cause analysis and triaging production during an incident. That's a real slice of the work but a small one. SRE is reliability design, service level objectives and error budgets, observability, capacity planning, risk mitigation, cutting toil, and the slow human work of making systems and teams more dependable.


A tool that correlates alerts; analyzes telemetry, changes, and code, and proposes a probable cause along with a suggested fix is useful. Rootly builds one.


It still isn't an SRE. Call it one and you tell every SRE in the room you think the work is that small, and the cost doesn't stay in the room. When a leader reads "an AI SRE that handles on-call," they don't hear a triage aid. They hear permission to under-invest in the practice: the headcount, the reliability design, the slow work that doesn't fit on a product page. The name does the cutting before anyone's decided to cut on purpose.


## Where we disagree


We're leaving this seam showing on purpose because Sebastian's position looks like the clean one, and it comes straight from his own framework: a product name should never double as a job title, so "AI SRE" fails the test and should be retired. Rootly's position is messier. We helped make "AI SRE" a phrase people search for, and that search traffic is part of how a company our size gets found at all. We don't get to pretend otherwise.


So we keep using it, and we constrain it. The product augments the people doing the work, it doesn't replace them. We say in plain language what it does and does not do. Naming things honestly has to include pointing the test at your own label, including the one you helped popularize.


But Sebastian's position isn't disinterested either, and the same honesty cuts toward him too. He isn't a neutral critic of this word. He's the buyer who chooses these tools, the user who lives with them, and the one who sits in budget reviews defending why the practice still needs people. Retiring "AI SRE" would help him too, so his clean line is principle and self-interest at once, the same as Rootly's messy one. Adam and Rootly need the word to be found. Sebastian needs it to stop being a lever. Two seats, two pressures, and neither of us is neutral.


## When should a startup change the standard?


There's a broader question sitting under all this word-swapping, and it reaches past naming. Every young company inherits a vocabulary it didn't choose, and trying to change the wrong part of it at the wrong moment is just expensive invisibility. So we sort the words by what they cost.


Some are only a matter of taste. "Battle-tested" is one. It borrows the same military uniform as "war room," and "proven in production" says it better, but no buyer is fooled by it. When the only thing wrong with a term is taste and you still need it to be found, comply, and swap it quietly when you can.


Some are useful enough to keep arguing about. "Blast radius" borrows from bombs, but it also carries a precise meaning we rely on, the spread of damage when one component fails. Sebastian's own SABRO framework reaches for it; Rootly does too. We're trying "scope of impact" and "impact radius," and we'll admit reasonable people land in different places.


And some actively mislead the buyer in a way that costs them later. "AI SRE" sits right there. The category helps people find a real solution, and the autonomy it implies sets an expectation that breaks the first time on-call pages a human at 3 a.m. Those are the ones worth the work, so we keep the half that helps discovery and push on the half that misleads.


There's also timing. A name you can't afford to change today becomes one you can afford to change once enough people already know you. Distribution earns you the right to lead the language instead of chasing it through a search box. Rootly isn't all the way there yet, but this post spends a little of the distribution it does have on the part of the work we think is worth it.


## A test for naming your own tools


Everything above is about the words we all inherit. If you're the one doing the naming, Sebastian built a framework for it called SABRO, published at[SREInsights.io](http://sreinsights.io/) .


Five quick tests:


1. **S** pecialization (a metaphor whose action matches the tool, Guide or Sentinel, not Engineer).
2. **A** ugmentation (support, never replacement).
3. **B** oundaries (say what it doesn't do).
4. **R** espect (use SRE terms the way the field does).
5. **O** utcomes (name the measurable result, not the vibe).


The fast version is his Naming Compass: if the name could double as a job title, you're pointed the wrong way.


## A start, not a victory lap


Nobody turns a market overnight. This vocabulary has decades of momentum and a search box reinforcing it daily. We'll keep moving the words toward what's actually happening, and we'll keep using a few we don't love until we've earned the room to drop them. Name things for what they do. The work of keeping systems reliable is hard and worth doing on its own terms. It doesn't need to borrow a war, a fire, or a body on a table.


We'd rather not do this alone. So, one question back — the one we keep asking ourselves: which word in your stack makes you wince, and when will it finally be worth the cost to stop saying it?
