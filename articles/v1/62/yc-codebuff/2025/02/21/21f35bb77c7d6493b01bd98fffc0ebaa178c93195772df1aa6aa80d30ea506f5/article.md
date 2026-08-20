---
schema_version: "1.0.0"
document_id: "21f35bb77c7d6493b01bd98fffc0ebaa178c93195772df1aa6aa80d30ea506f5"
company_key: "yc-codebuff"
company: "Codebuff"
source_id: "yc-codebuff-rss-bef55ad83d13"
canonical_url: "https://news.codebuff.com/p/codebuff-is-the-best-agent-on-large"
published_at: "2025-02-24T15:10:58+00:00"
first_seen_at: "2026-07-27T08:33:23.704221+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:b146e4cdc9bab4cb503465397c7a32ad9792dd602fbc25e47a0ac8463bee5fb1"
---

# Codebuff is the best agent on large codebases.

# Codebuff is the best agent on large codebases.


### We’ll pay you $100 if that’s not the case for you! (until March 14th 2025)


[Codebuff](https://substack.com/@codebuff)


Feb 24, 2025


Hey everyone,


Here to share a few fresh updates from our corner of the terminal!


# Big Performance on Big Codebases


Codebuff now runs like a dream on large projects (yes, even the VSCode codebase!), and you’re even less likely to hit the context window limit during marathon sessions.


We’re so confident about this that we issued an


[open challenge](https://x.com/jahooma/status/1888754030048129140) a couple of weeks ago to give $100 to anyone who prefers another coding agent to Codebuff on large codebases. Many challengers knocked on our door, but only one person has able to claim it so far. Wanna try your luck? Email us at hi@codebuff.com if you found a situation where other coding agents did a better job on your codebase, and include screenshots to explain! Submissions are valid until March 14th 2025.


Be warned, you may just fall in love with the product, like this user:


# Upcoming change: 500 credits per month for new accounts


New users will soon receive only 500 free credits (instead of the current 1,000) per month. We’ll be


[updating the system this week](https://x.com/jahooma/status/1893086556036214885) , so please sign up asap to lock your 1,000 credits in!


Install codebuff:


```text
npm i -g codebuff
```


And start it up in a project directory of your choosing:


```text
codebuff
```


We’ll start the login process and set you up with 1,000 credits! And of course, you can


[refer your friends](http://codebuff.com/referrals) to receive an extra 500 credits each.


# New Features


## Upgraded File Editing


Our file editing just got a huge boost! We’ve reworked it with a shiny new


[speculative decoding](https://research.google/blog/looking-back-at-speculative-decoding/) model (huge thanks to


[Relace AI](https://relace.ai/) ), which means lightning-fast, more accurate rewrites. We hope you’ve been noticing the extra reliability recently. And of course, let us know if issues crop up when Codebuff edits your files.


## Planning


This is the a big one: you can now tell Codebuff to “plan” your next feature or refactor and it’ll whip up a detailed markdown blueprint (e.g.


` \`user-stats-page-plan.md\`` ) with a thoughtful plan and a few follow-up questions for you to help create alignment. You can tweak it manually (it’s just a markdown file after all) or tell Codebuff how to further refine it. When that looks good to you, just tell Codebuff to build it out. It will continue to reference the plan as it works, so it doesn’t lose focus and wander. It makes a world of difference and can help you code multi-hundred line changes with


[confidence](https://x.com/brandonkachen/status/1893114736964718681) .


## Browser Actions


Introducing a secret alpha:


**browser actions.** When you ask it to, Codebuff can now launch Chrome to actually


*see* your frontend in action! It automatically navigates, snaps screenshots, and pulls browser logs from the website. Here’s a quick demo:


For more complex actions (like clicking, dragging), you can perform the actions manually and then ask Codebuff to capture and analyze a screenshot. This feature is still in alpha, so it's a bit rough. Please let us know what bugs you find!


# Feedback pls


We want to know how Codebuff is working for you! We’re particularly excited about planning, we think you’ll really like it :)


If something seems off or an edit goes sideways, ping us on


[Discord](http://codebuff.com/discord) (or over email at


support@codebuff.com ). Codebuff also recently


[started storing your conversations](https://www.codebuff.com/docs/advanced#troubleshooting:~:text=Accessing%20Your%20Chat%20History) on the client-side, so you can also share it with us to help troubleshoot.


We put together a


[little survey](https://forms.gle/J2ZGFtyhywdeF44L6) , could you take a minute to fill it out? Thanks for being part of the journey!


Happy coding,


James and Brandon
