---
schema_version: "1.0.0"
document_id: "1802a7a32f5fd51414109696fc903af0ec871e921ce096d1358b3ae63210f5f7"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2025-12-03-mob-programming-smells-like-team-spirit/"
published_at: "2025-12-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:debdd8c6f9be6af8b0e21f79cd0430778daaaec9156787cf9af8f8ee23991e88"
---

# Mob Programming: Smells Like Team Spirit

Mob programming often sparks hot takes. “It’s counter‑productive.” “One person could do it faster.” “Why waste five people on one keyboard?” If that’s what it looks like in your head—one senior typing while others half‑listen—that wasn’t mob programming. Real mob programming is structured collaboration that turns a team’s combined attention into forward motion.


## What is mob programming?


Before I get into the use cases, here’s a clear definition and a few basic ground rules—consider it a TL;DR of sorts .


> “All the brilliant minds working on the same thing, at the same time, in the same space.” — **Woody Zuill**


That space can be physical or virtual—the key is combined focus.


A healthy mob is simple:


- **One computer** and **one keyboard** .
- **Exactly one person typing** (the *driver* ).
- **Rotate every 10–15 minutes.**
- **Driver types; navigators decide.** The driver translates the team’s direction into code, not their own individual plan.
- **Make kindness, consideration, and respect explicit.** They’re not “nice to haves”; they’re the operating system for disagreement.


> “For an idea to go from your head into the computer, it must go through someone else’s hands.” — **Llewellyn Falco** , *Strong‑Style Pairing*


This forces clarity and spreads knowledge.


Let me be honest: I was skeptical. The questions at the top? I had asked them myself. Still, the pain was real, and it was clear our way of working had to change—so we decided to experiment.


## The pain we wanted to fix


We’re a mixed back‑end/front‑end team working on **trivago Intelligence** —a tool for partners for bidding and data analysis. Like any team, we’re not perfect; we have our bumps. But we enjoy the fight: overcoming organizational puzzles can be as satisfying as cracking technical ones.


We didn’t start with a methodology; we reacted to a slowdown that hurt delivery. Our trigger will sound familiar: **PRs piling up and stalling** . Reviews turned into ping‑pong—async comments, context‑switching back from the next task, and drawn‑out re‑reviews. So we tried the smallest possible changes:


1. **Review together.** The author hops on a short call and walks reviewers through the changes. Questions get answered immediately; misunderstandings drop. Helpful—but when larger refactors were needed, delays persisted. So we pushed further.
2. **Change code during the review.** Pair on tweaks right there instead of leaving comments and hoping they land. This cut the demotivating pile of comments and the “you’re on your own” feeling. It also reminded us that working together can be fun—and faster.


Both steps helped, but the real shift came when we **brought review into the flow of building** —mobbing from the start rather than after the fact. As a quick proxy, our open PRs dropped from **20** to **5** .


## Where mobbing helped most


Not every task benefits equally; here’s where the practice earned its keep.


**Architecture & migrations.** During a quarter‑long **GCP migration to BigQuery** with a tiny backend crew, we tackled the hardest slices together first. Decisions about data structures, error handling, and rollout order happened with the full context present. We even finished the last query **three days after the deadline** —not perfect, but far better than the “we won’t make it” vibes at kickoff.


**Onboarding.** A newcomer with backend experience but zero Kotlin or product context joined a month before the end of the cycle. We put them **in the driver’s seat** with the team navigating. Their ramp‑up was dramatically faster than any doc or ticket queue could deliver.


I also joined a new team recently; pairing across different parts of the system quickly boosted my understanding of the codebase. It also set clear expectations with each teammate—what I can learn from them and what I can offer in return.


**And for the contrast.** Earlier, on a front‑end **Chart Builder** initiative, work stretched to ~1.5 quarters. The final PR lingered so long that the original author left before it merged. Untangling conflicts and bugs cost another month. It was a perfect example of how long‑running branches and async reviews amplify drag—exactly what mobbing helped us avoid later.


## How we actually mobbed


We ran a few low‑friction experiments and kept what actually helped.


Here’s what we tried:


- **Single workstation + big screen** in the office — the lowest‑friction setup for watching and discussing.
- **Shared IDE session** (e.g., Code With Me) so everyone can follow the same file without constant commits. Handy for code, but real work also means opening tasks, running the app, and querying the DB; IDE sharing alone wasn’t enough sometimes.
- **Hybrid/remote via screenshare.** Our go‑to when anyone was remote. Using your own setup works well, but when you’re juggling many services while screen‑sharing… expect the occasional frozen face and colorful language. It became part of the charm.
- **Use a timer—**when it goes off, stop typing and switch. We never found the perfect tool. Phone/watch/browser timers were easy to forget. Next to try: a mechanical timer (think chess clock—or the clicky Lovevery one my daughter has).


This is what worked for us; you might land somewhere else. Passing on “wise words” from a notorious board‑game rules lawyer—me: enforce a couple of basics (like a timekeeper), but don’t turn it into policing. I’m exactly the person who annoys friends with rulebooks, so take this as self‑aware advice: keep it light and fun, and keep only what actually makes you faster.


## Bye-bye Solo


Working as a “space crew” (all eyes on one problem) helped us in simple, practical ways:


- **Fewer blockers.** Questions get answered on the spot.
- **Better code sooner.** Design debates happen before the PR, not after it hardens.
- **Continuous knowledge transfer.** Domain context, tools, and techniques spread naturally.
- **Smaller PRs, faster merges.** Most of the review already happened together.
- **Stronger relationships.** Solving puzzles side‑by‑side builds trust.


Solo is faster for small, well‑understood tasks—use the right tool. But on complex or ambiguous work, the crew avoids rework and ships sooner overall. In the end, even Han Solo needs Chewie.


## Final thoughts


A practice is only as good as the outcomes and relationships it produces; keep adapting the form to fit your team.


Don’t sell mob programming as dogma— **invite** the team to try it. If what you’ve seen is one person typing while others scroll their phones, that’s not a team; that’s an audience. Done right, mob programming feels less like a meeting and more like **solving a puzzle together** —which is why many of us got into software in the first place.
