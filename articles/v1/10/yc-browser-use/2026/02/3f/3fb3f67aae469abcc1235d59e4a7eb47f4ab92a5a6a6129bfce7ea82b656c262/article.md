---
schema_version: "1.0.0"
document_id: "3fb3f67aae469abcc1235d59e4a7eb47f4ab92a5a6a6129bfce7ea82b656c262"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/how-to-win-hackathons"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:57:38.048658+00:00"
content_hash: "sha256:7bdc7afc405ce1f0e43f9dfe48d660bcc33a998a5286e6e6866c595498f78ffe"
---

# How to Win Hackathons: A Guide

Hey! I'm Reagan, a founding engineer at Browser Use (YC W25) and former avid hackathon-er. I've won hackathon prizes at Y Combinator, MIT, Stanford, UCSD, and others. These are some tips and tricks I've learned along the way.


*Winning first overall at Y Combinator!*


## A Brief Snapshot


Almost all of the following advice is grounded in two things: **being curious** , and **being someone who wants to build things** .


If you have both, you will inevitably be good at hackathons. Your ideas will wow others, your passion will shine through in demos, and your projects will be built from the perspective of someone building a real product, not someone just trying to win a competition.


## How to Get Ideas


Good ideas are hard to come by, and it's daunting to compete against others who have been thinking about a project for weeks, even[months](https://devpost.com/software/foggy) .


However, it's more than possible to come up with great ideas in a short time span. Below are some of my tips.


### 1. Talk to sponsors.


They have good ideas on what projects are boring versus innovative, and occasionally will even tell you what they would like to see made.


Also, sponsor products are often super cool - allowing you to build things you had never imagined before - yet people foolishly skip over them.


One personal example is when I tried[Convex](https://convex.dev/) at Y Combinator - I haven't used any other database in a hackathon since.


### 2. Start with a problem.


The most important aspect of a project is that it actually serves a purpose. Finding a problem to solve is a necessary first step.


### 3. Use personal ideas.


Good ideas are often personal ones, and vice versa. You automatically have a valid pain point, an idea of how to solve it, and a customer: yourself. Having these three solves the issue of how most projects keep judges asking: "Who would use this?" and "Why was this made?"


### 4. Don't force originality.


For one, do not look at past hackathon projects for inspiration. It becomes hard to think originally after doing so. Also, in reference to the former point, none of them will be truly personal.


Also, do not ask yourself, "How can I be different?" This usually results in products that have no good purpose. Instead, ask yourself, **"What do I wish existed that currently doesn't?"** If you cannot find an answer to this question, think harder, because there are definitely many.


### 5. Identify inspiration channels.


Here are mine, which are honestly pretty simple:


- Writing random shower ideas down in my notes app
- Saving cool-looking UI on Twitter/Instagram
- While programming, always asking, "What would save me some time, no matter how small?"
- Saving interesting technical concepts that I can apply to projects (e.g. most recently, I read up about WebMCP and thought it was super cool)
- Keeping up with the latest startups in Y Combinator


## How to Plan


Planning a good hackathon project goes hand in hand with ideation, but these tips are for after you have an initial concept/direction.


### 1. Plan a specific user flow, keeping the demo in mind.


Have a specific set of steps you will demo, and keep it as short as possible so judges can fully understand your product.


### 2. Plan for a visual wow factor.


Unfortunately, it's difficult for judges to understand technical depth in the span of a few minutes, especially if you're using tools they've never heard of. What's universal, though, is appreciation for great-looking UI/UX.


### 3. Aim for first place.


A lot of people optimize for sponsor tracks, but in my opinion, you should always go for first place, especially if you're with a good team. Believe in yourself!


## How to Implement


- **Separate frontend and backend, and link them later.** You can mock API endpoints on the frontend.
- **Keep an "if it works, it works" mentality.** Hackathon projects are proofs of concept. Make them work end-to-end, but no need to make implementations pretty.
- **Use coding agents liberally.** I typically have ~5 running in parallel. You should probably have even more than this, given you're starting a project from scratch. Dedicate a few to planning, a few to research, a few to ideation, and a few to actual implementation - at all times.
- **If you're going to mock part of your project, mock things that aren't core to your product** (e.g. data feeds). For judges, it's simple to tell what has been mocked, and it really hurts your project.
- **Only implement additional features that add to the "wow" factor** without adding much friction to the user flow.
- **Reserve at minimum 10 minutes, ideally 20 minutes, to plan your demo.** This means you need to submit early, and you'd be surprised at how long it takes to make a high-level, two-minute script.
- **Keep your main branch functional.** Implement features that may break production on feature branches.


## How to Demo Your Project


Demos are the final hurdle. Generally, explain with as few words as possible, and let your demo do the talking.


Passion, speaking concisely with confidence, and having good body language are all necessary.


- **Have one short, sweet, and clear sentence for your pitch.** Should be your first sentence.
- **Assume the judges know basic principles of full-stack development, and nothing else** - so do not go into deep technical detail during your demo unless you are confident you can keep it in under two sentences. You risk confusing the judges.
- **Use slides to aid your explanation.** One sentence pitch → problem → solution → demo format. Numbers/stats always help. Here's an example first slide:


[Access the full presentation in Figma](https://www.figma.com/deck/1jqHnJWTV9ZVZZ7V6yJJHj)


## Extra Tips


- **If a hackathon goes overnight, pull the all nighter!** The best ideas often come at 2 am. Also, it's a canonical experience.
- **A team of 2-3 is optimal.** In my experience, four is too many, and going solo makes it hard to be creative and manage workload. One frontend, one backend, and a third person can help out where needed.
- **Be willing to reject people from joining your team.** Especially non-technical people. In my experience, non-technical people are brought in order to help with presentation skills and ideation - neither of which they contribute to because they don't know how to explain technical details in a simple way, and they have no idea what projects are technically possible.
- **Don't let a time constraint stop you from trying something.** You'd be surprised at how much you can get done in just a few hours. (though I will note that anything less than three gets iffy).
- **Finding good teammates is hard.** Here are specific traits I always check off: willing to pull an all-nighter, well-spoken, technical.


## Sanity Checks for Your Idea


Please make sure you can answer all of these questions:


- Is it showstopping? Will any engineer see it and go, "That's cool"?
- What problem does it solve?
- Who is it for?


## My Dev Stack


Here are the tools that I recommend that I still use regularly when creating side projects:


- **Frontend:** Vite + React + Tailwind (Expo for iOS app)
- **Backend:** FastAPI
- **Database:** Convex
- **Deployment:** Vercel
- **Coding environment:** Claude Code in Superset.sh


## My Note to Builders


Hackathons can change your life. In fact, winning the Y Combinator hackathon is what got me introduced to Browser Use, and now I work here full-time!


Losing hackathons sucks, but it's typically clear what you need to work on when you do. This provides a clear path for improvement, and you will inevitably get better and start winning.


Most importantly, each hackathon is like a day at a startup. You are:


- Pushing coding agents to their limits
- Either revamping an existing product or making a completely novel one
- Learning to be a good teammate under high stress
- Working long hours, with the understanding that there is a high probability you will lose in the end


In other words: **if you're good at hackathons, you will be good at a startup.**


So what are you waiting for? Hack away :)
