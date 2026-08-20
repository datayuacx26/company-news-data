---
schema_version: "1.0.0"
document_id: "91bb8d8acd4a405be513a13f5f669b40074be3c46f5001874eb6d369c62119f9"
company_key: "yc-happl"
company: "Happl"
source_id: "yc-happl-news-import-53ae215aa1b0"
canonical_url: "https://happl.com/blog/happl-app-store"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T22:26:08.216138+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:4b6830894657acfad2f27afe2fde30b8b0109fc640664611d67fdc848b78c326"
---

# Happl's internal app store: How I built it

Apollo is Happl's internal app store. It looks a lot like the App Store on your phone, but the apps are tools our team has built to solve our own problems. Anyone at Happl can find a tool or build one.


‍


I built it because I kept watching the same thing happen. And now it saves the team 100+ hours every month.


‍


Someone in Finance would create a dashboard to flag unusual spend before month end. Someone in Sales would write a script to automate an hour of work. Someone in Marketing would build a quick tool to import leads from event sign-ups.


‍


But it all lived in different places and half the team had no idea what existed.


‍


That cost showed up everywhere. Engineering kept getting asked to build things that already existed. Processes that could have been sped up for whole teams stayed stuck. And every hour someone spent rebuilding or rediscovering was an hour that could've gone back into the product.


‍


The problem wasn't that people weren't building. It was that nothing they built was easy to find or easy for someone else to pick up and use. And the less technical people on the team, often the ones with the clearest sense of what to fix, didn't know where to start.


‍


## **The first tool was for me**


The tool that ended up being the first one on Apollo wasn't built for the team. It was built for me.


‍


Every quarter we run reconciliation between insurer broker packs and our member exports. Complicated enough that it needed significant developer time for automation, and frequent enough that everyone hates doing it. Over a week, four times a year.


‍


I opened Claude Code and planned the logic flow first, using regex rather than AI because we can't risk mistakes in this data. Then I described what the reconciliation needed to do and let Claude write the code. Two days later, the thing that had been over a week of work was a tool I could open in a browser, click, and walk away with a download of every issue found and the fixes already applied.


‍


That was the moment that mattered. Not the speed of the tool. The speed of building it. If I could solve a real recurring problem in two days, then anyone at Happl with a problem they'd been tolerating should be able to do the same.


‍


## **A secure system for every app**


For that to work, two things had to be true at the same time.


‍


The first was that anyone, including someone who'd never written a line of code, had to be able to push a tool to Apollo and have it work. If the path involved a Git tutorial or a meeting with engineering, it was never going to happen.


‍


The second was that letting anyone push code couldn't break anything. We work with HR teams handling sensitive employee data. If someone accidentally pushed a tool that exposed a credential, or stored customer data somewhere it shouldn't, that would be a much bigger problem than the friction the tool was solving.


‍


The solution was to make the platform easy to use but strict on checks.


‍


## **How it works**


The most common thing you do with Apollo is open it and find a tool someone has already built. You search by name or by department, hit launch, and the tool runs in your browser.


‍


When the tool you need doesn't exist, you build one. You describe what you want to Claude Code in plain English. Apollo gives Claude a detailed brief, written and maintained by the platform team, that walks it through how to build for Apollo. How to handle login. How to ask for any data or keys the tool needs. What's allowed and what isn't. Because the rules are already in the prompt, Claude usually writes something that works the first time.


‍


Before you ship your tool to the rest of the team, you can run it in build mode. You iterate, test on real workflows, and submit when you're happy. A small platform team reviews and ships it.


‍


The whole thing stays safe without the person building the tool having to think about it. Every Apollo app is a static bundle of HTML, CSS and JavaScript. There are no servers or databases unique to each app. The platform handles those centrally.


Every submission then runs through two scans: a fast first pass for obvious credential leaks in seventeen formats (Stripe keys, AWS tokens, private keys, JWTs and so on), and a second AI scan that reads the actual code and decides whether to ship automatically or send for human review.


‍


If a tool needs to call Claude, Slack or anything else outside Apollo, it asks Apollo to make the call on its behalf. The credential never reaches the tool directly. Apollo also enforces a daily spend cap for each app and if a tool starts behaving unusually, it’s quarantined.


‍


Every action is logged. Audit trails only ever get added to and access can be revoked instantly. None of it has to be remembered by anyone building a tool, because Apollo enforces it by default.


‍


The result: someone on the team can ship a tool that uses Claude, with verified users, persistent state and a spend ceiling, in an afternoon. They don't have to provision infrastructure, rotate credentials, or write any backend code.


‍


## **What the team has built since**


The tools that have shown up on Apollo so far have mostly come from the teams that felt the pain first.


‍


Customer Success built a bulk policy renewal letter generator. They used to stitch these together by hand, one customer at a time. The tool produces them in minutes.


‍


Marketing built campaign trackers tailored to the way we plan campaigns at Happl. CS built a client health dashboard pulling signals from across our systems. There are calculators for insurance broker rates across multiple providers. And upload tools that quickly translate messy data into the structure our systems work with.


‍


None of these are particularly groundbreaking in isolation. Each one saves someone a few hours a week. But add them up across the company and we've cut hundreds of hours of manual work out of our month, all of it work that shouldn't have been manual in the first place.


‍


## **What it means for us**


The obvious effect is speed. Anyone with a problem can solve it without waiting for an engineer to free up. But the team is also starting to think differently about what's possible. We're seeing members who wouldn't call themselves technical saying 'wouldn't it be great if...' and then going off and building it themselves.


‍


And everyone knows every tool that exists. They’re all there with a fun name and description. So nobody rebuilds something that's already there or uses an outdated workflow.


‍


The most important effect is what happens to the engineering roadmap. We're not pulling engineers off product work to fix a small operational pain. The product team gets to focus on what actually moves Happl forward. And engineering uses Apollo as a sandbox: a place to prototype a new feature internally, get a few teams using it, see how it actually performs, then decide whether it's worth investing real effort in.


‍


All of this lets us work more efficiently. We can scale fast and still stay lean.


‍


## **What it means for our customers**


In short, we ship better products, faster.


‍


The team isn't blocked on the work that shouldn't have been manual in the first place. That capacity goes back into the product. Sharper thinking about what to build next. Faster releases. Better support when something goes wrong.


‍


There's a second thing customers get, and it matters more than you'd think. We sell software to HR teams that handle deeply sensitive employee data. The way we handle the security of every internal tool we use to serve them is part of what they're buying.


‍


"How do you secure the tools your team uses to handle our data?" is a question we get asked often. The longer your answer takes, the worse it sounds.


‍


With Apollo, the answer is a single page. Sign in with your Happl account. Every action logged. Every secret encrypted. A scan at the point of submission. Browser only storage to maintain compliance. And all of it is the way the system works by default.


‍


## **Why it matters**


There's a pattern in scaling companies that everyone recognises. Hundreds of small frictions, each one too small to justify fixing, collectively eating large amounts of the team's time. The default state of a growing company is to live with most of them.


‍


Apollo doesn't fix the frictions. The team does. What Apollo does is make the cost of fixing them low enough that someone will. And we work better because of it.


‍


If you're working on something like this at another scaleup, or just want to talk about how this works, I'm happy to chatryan.wilson@happl.com


‍


*Ryan Wilson, Operations Manager*


# Heading 1


## Heading 2


### Heading 3


#### Heading 4


##### Heading 5


###### Heading 6


Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.


> Block quote


Ordered list


1. Item 1
2. Item 2
3. Item 3


Unordered list


- Item A
- Item B
- Item C


[Text link](https://university.webflow.com/lesson/add-and-nest-text-links-in-webflow)


**Bold text**


*Emphasis*


Superscript


Subscript
