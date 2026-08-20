---
schema_version: "1.0.0"
document_id: "cf3664a7d9246b8806caa1eac7946e47f1b4eaf12b0acd138f412d4a6f57ce42"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/how-photoroom-runs-daily-standups-for-40-engineers-and-what-s-a-good-standup-message"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:efbdafe0a82414d37570bfd9f5c051f6ecc62e3f410c7fa0e7144f966577a2cf"
---

# How Photoroom runs daily standups for 40 engineers and what’s a good standup message

> The following article is an internal page of our internal guidebook that was made public


[Photoroom](https://www.photoroom.com/company) has reached the 40 engineers mark, prompting us to evolve the way we run daily standups. This posts summarizes how we run them today and best practices we recommend photoroomers to apply.


## Purpose of standups


Organized well, a standup will spark the following interactions:


-


“Oh you’re refactoring that part of the app, I was the one who touched it last and here’s what you should know” \[context spreading\]


-


“It seems this is taking longer than we expected, would you like us to have a look at it together / can I help?” \[estimate refinement, blocker removal\]


-


“Hey I didn’t know about that library / that part of the codebase / that technique, today I’ve learned something!” = \[knowledge spreading\]


-


“John from team2 has already made a proof of concept PR for that last year, here’s the link: …” = \[not coding something twice\]


-


“This seems overly complex, I have a better idea” = \[avoid exploring wrong directions\]


**To the writer** , it will bring clarity on their current priorities and blockers. **To the team,** it helps spread knowledge and improves alignment. **To managers and other teams** , it communicates on progress, prioritization and blockers.


As the CTO, it also allows me to peek asynchronously at the in-flight topics of each team and ensure everyone has the proper context. If a key project is taking much longer than anticipated, in 2 clicks I can get an idea of why.


## How Photoroom runs standups


While in the early days of Photoroom we were running standups in live video calls every morning at 10am, we now use per-team channels on Slack. This way people can subscribe to each channel to follow progress of other teams (we evolved from one single channel which was too noisy). As of early 2025, we have: #standup-backend-api, #standup-ml, #standup-web and #standup-engine (for our cross-platform team). Each member is prompted by Geekbot to fill their standup message at 9 am. Filling the standup is usually mandatory, but this decision is up to the team’s manager.


## How to write a great standup message


*Comparing a great message vs a not so great message*


Your target audience is your teammates. Think of it as meeting one of them at the coffee machine in the morning and taking 5 minutes to update them on what you did the previous day and what you’re planning on doing today.


**You should:**


-


**Be specific** : link to Linear tickets, Pull Requests, Notion documents, Github issues


-


**Share work in progress** : your strategy facing a complex problem, the options you’ve considered, what remains to be done to finish your task


-


**Be technical** : since you’re not communicating to the whole company, you can talk about libraries, share code snippets / methods, even that weird error message that randomly pops up once a day


-


**Vent out…** : yes, refactoring with Xcode is like playing the lottery. Yes, we can’t do any work today since Github is down.


-


**…or celebrate successes** : that complex feature finally shipped to production or that subtle bug was finally squashed, time to bring out the party parrot emojis 🎉🍾🦜


**You shouldn’t:**


-


**Be vague** . The most common pattern I see is a single-sentence “Worked on upgrading our authentication library / feature X / Y refactor ” for 3 days straight. It provides very little value to share that information. Your hypothetical coffee machine colleague would ask a lot of follow-up questions to understand what you’re up to those days


-


**Give too much details** : it must not turn into a design document or a pull request description. Your colleague would zone out of your explanation


(yes, those 2 conflict, it’s a balance to strike)


## Tips and tricks


-


It’s okay to miss filling it from time to time, simply don’t forget to update on the missed days in the next message


-


In Geekbot (the tool we use for standups) you can customize the time the bot sends you prompts you to fill it


-


See the “What’s blocking your progress” as “what’s slowing you down”, a way to reflect on things that were in the way to achieve your goal: flaky tests, cryptic build errors, unresponsive external provider.


## Bonus


The change to async standup was led by Jeremy. Thanks to him, the transition was very smooth. Here are his very clear Slack messages explaining the switch:
