---
schema_version: "1.0.0"
document_id: "3fbd48e61cea2f2b4033f8e831b7efb2034bde44c276df5548eab71b5b47a177"
company_key: "rent-the-runway-inc-class-a-common-stock"
company: "Rent the Runway Inc."
source_id: "rent-the-runway-inc-class-a-common-stock-rss-571d2b3d11b7"
canonical_url: "https://dresscode.renttherunway.com/blog/fix-it-week"
published_at: "2022-02-10T22:50:59+00:00"
first_seen_at: "2026-07-20T23:19:27.389009+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:9520f286928c605836c9da717d70b81fd899f1ba04326c87e3d130ca1a64af52"
---

# Rent the Runway’s first Fix-It Week

***A story of growth spurts, tech debt, and teamwork***


*co-written by Helen Chavey*


As a child, hitting a growth spurt can be both exciting and frustrating. You may now be the star of the volleyball team, but you’ve outgrown every piece of clothing you own and need to replace those garments with items that fit and flatter your longer limbs*. The same thing happens to organizations: growth spurts often result from a business taking big, successful steps forward, but they can lead to a “closet” full of processes, documentation & code that need some tailoring in order to fit the organization in its new state. In this post we’ll share the impact such growth spurts have had at Rent the Runway, how the Data org has tackled these challenges, and the impact our efforts have had.


### Growth at Rent the Runway


Rent the Runway[has been a disruptive company from day 1](https://www.stitcher.com/show/how-i-built-this/episode/rent-the-runway-jenn-hyman-51037589) , first by normalizing apparel rentals for special events at scale and quickly following up with fashion as a subscription service. This willingness to take risks and experiment with new ways of serving our customers has paid off — Rent the Runway has helped over 2.5 million lifetime customers rent dresses, accessories, everyday outfits, coats, athleisure, and more. This has accelerated a smarter, more environmentally conscious way to get dressed compared to buying new; we estimate that our model has displaced the need for production of[over 1.3 million new garments](https://www.linkedin.com/pulse/rtr-commissions-first-comprehensive-study-clothing-rental-/?trackingId=07iFTrvBn5oSrq3JKJMU4g%3D%3D) since 2010.


Rent the Runway doesn’t just pivot our business model; we are also constantly testing new technologies in pursuit of a high-performing tech stack that is fun to work on and serves our customers well. In the data org, recent examples of this have included enabling[Great Expectations](https://greatexpectations.io/) to validate data,[Prefect](https://www.prefect.io/) for code-controlled orchestration, and migrating from on-prem servers to cloud.


All of these changes make our work faster, easier, and more fun, but pivots don’t come without costs. In particular, as anyone who has worked at a growth-stage company knows, pivoting as you find product-market fit requires moving fast and making tradeoffs.


### Introducing Fix-It Week


While the engineering org chose to approach the growth spurt challenge by bumping up the total number of story points dedicated to scale & resiliency over the course of a quarter, the Data team looked to another concept tested by companies like[Spotify](https://www.linkedin.com/pulse/why-we-turned-our-hack-week-fix-sofie-lindblom/) , Twitter, and Uber: **Fix-It Week** . To continue the analogy above, if setting aside more time in every sprint to tackle tech debt is like slowly replacing your old clothes over a series of small shopping trips, Fix-It Week is a one-time shopping spree after which — hopefully — you love everything in your closet.


Before committing to the idea, we asked ourselves a few questions:


**Is carving out specific time to fix things antithetical to our culture where we aim to get it right the first time?**


A unique time often calls for a unique solution. We ensured the entire org understood this rationale for a one-time Fix-It Week, and not to expect it again any time in the future. We also worked in parallel to reestablish our standards for quality and the processes that support those standards day in and day out, so that coming out of Fix-It Week we could maintain the clean, shiny code and docs we worked so hard to polish.


**Will setting aside this specific time lead to more tech debt creation between the day we announce Fix-It Week and the day it starts?**


To avoid this risk, we very intentionally leveraged the time in between announcing and beginning Fix-It Week to get people excited about what they could really get out of this week if we approached it properly (details in the next section).


**Is a few days of focus enough time to make a real difference?**


Rent the Runway already runs[successful hack weeks](https://dresscode.renttherunway.com/blog/tag/hackweek) every year! In fact, we considered that a full week could be too much for anyone working on a project at a critical stage, so ultimately carved out just 3 days for the team to focus on fixes.


### Process


Rather than simply say “it’s Fix-It Week!” and hope for the best, we added structure to the week by doing the following:


-


We created & maintained an internal wiki page documenting why we’re doing this, what we hope to get out of it, and all the logistical details. Importantly, we left the definition of a fix very broad, but we specified several prize categories (yes, with real prizes) to encourage fixes in the areas we felt needed the most love.


-


We sent out a Google jamboard to collect ideas several weeks in advance. No one knows your pain points better than you, so team members quickly took ownership of this opportunity to write down all the things that would make their day-to-day easier and more fun.


-


We created a Slack channel for people to self-organize to work on the fixes they cared most about, to ask & answer questions, and to share any logistics updates.


-


The day before Fix-It Week officially kicked off we came together over Zoom and split into small groups mixed across data teams to debate the most exciting ideas on the jamboard, then asked each group to pitch their top Fix-It Week priorities, focusing especially on ideas that would require multiple collaborators to successfully execute. We paired this with a virtual team cooking activity to amp up the fun.


-


The day Fix-It Week began, we shared one final doc to track every fix being worked on. This ensured that there would be no duplicate effort, facilitated collaboration, and ensured we could give credit toward the established prize categories. We also brought in pastries to keep the energy high.


-


On the final day of Fix-It Week we closed with another team event, this time bespoke (and highly competitive) trivia, providing an opportunity to celebrate the effort we had all put in. We also awarded cookies to the winner of each prize category**.


The managers on the data team also adjusted project timelines and gave an early heads-up to stakeholders that this would be a priority for the entire team. While a few projects still required some work during that time, all of our stakeholders were extremely supportive of the idea, as it was clear this work would ultimately result in their lives being easier, too.


### Impact & follow-up actions


Maybe it was the promise of faster runtimes or maybe it was the vision of no longer having pesky “TODO”s punctuating code, but whatever the cause, **100% of data team members contributed at least 1 idea** to a jamboard and **92% of team members participated in fix-it-week** ! Of 128 unique proposals, nearly half (63) were tackled, and of those 54% (34) were completed during the 3-day period. Every sub-team within the data org participated, and every category of fix with an associated prize saw a substantial amount of effort (at least 7 fixes tackled and at least 4 completed per category).


We combed through the list of fixes, looking for patterns in that data. We found some interesting themes and assigned specific follow-ups actions based on those findings:


As a data-driven company, we also wanted to understand the impact Fix-It Week had on the organization. We had hypothesized that it would increase team happiness and productivity; was it successful? The day Fix-It Week ended every team member was prompted to respond to a short survey asking for data & feedback. Here’s what we learned:


-


100% of respondents said their job would be somewhat or much easier as a result of the work done during Fix-It Week.


-


Nearly every respondent said they had learned something new during Fix-It Week, and in the free-form comments several people noted they had also had the chance to collaborate with someone new.


-


100% of respondents found at least one of our prep structures useful. The idea-generation jamboards and pitch session were especially popular, highlighting the importance of the blameless, bottoms-up approach we took.


-


3 days was probably the right duration. While some people said they felt they could’ve used longer, some people reported that they had pushed back on project work and expected the following week to be busier as a result, meaning any longer would’ve been a challenge. Another option here may be to broaden the participant pool — with more people across the company engaged, fewer external pressures can exist by definition.


-


Bookending the week with bonding events ensured people retained positive feelings even after putting in a ton of hard work.


TL; DR: Carving out time to focus on a specific area can really work. Not only did the team accomplish an enormous amount, but we found value in bonding over tackling the challenge of Fix-It Week all together as a team. As a data team, we agreed that we should use this model in the future when working through major migrations or onboarding new tooling within the data org. We also felt it was important to share all we had learned with the tech community (hi!) as well as with other teams around Rent the Runway. This model would likely even work outside of a technical space — much of the work we did, after all, was oriented around cleaning up processes and documentation. So far one other tech team has followed this template and run their own Fix-It Week, and the entire company ran a successful Hack Week laser-focused on resiliency. A cleaner stack and a leadership team who support technical excellence have us feeling ready and excited to take on our next stage of growth.


** Should you be in this position today, or know someone who is, may we suggest a*[fantastic subscription service to an enormous shared closet](https://www.renttherunway.com/memberships) *to solve your wardrobe woes?*


*** Congratulations to our prize winners!*


-


***Niall O’Hara*** *(Machine Learning Engineer)* ***The Exterminator*** *— squashed the most bugs*


-


***Sean Talia*** *(Data Engineer)* ***The Undertaker*** *— decommissioned the most dead code, unused dashboards & abandoned jobs*


-


***Viv Chan*** *(Data Science Manager) &* ***Rob Sokolowski*** *(Machine Learning Engineer) are the* ***Zen Masters*** *on the team, so proved by their work simplifying unnecessarily complex projects and codebases*


-


***Danielle Dalton*** *(Data Engineer) is the* ***Tidiest Housekeeper*** *in the data org, who closed old tickets and commandeered old PRs to get our workflows squeaky clean*


-


*And* ***Helen Chavey*** *(Business Intelligence Analyst) showed off her* ***Shakespeare*** *chops, first with amazing documentation and next here on this blog post*


*Special prizes were also awarded to* ***Ada Lin*** *(Machine Learning Engineer),* ***Cathy Zhang*** *(Business Intelligence Analyst), and* ***Luis Honsel*** *(Data Scientist) for amazing work that we couldn’t have even predicted well enough to create a prize for in advance. Applause all around.*
