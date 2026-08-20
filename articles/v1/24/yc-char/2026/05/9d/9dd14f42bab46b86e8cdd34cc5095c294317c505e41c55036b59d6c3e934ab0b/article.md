---
schema_version: "1.0.0"
document_id: "9dd14f42bab46b86e8cdd34cc5095c294317c505e41c55036b59d6c3e934ab0b"
company_key: "yc-char"
company: "Char"
source_id: "yc-char-news-import-7acae7b8fb95"
canonical_url: "https://char.com/blog/who-manages-the-managers"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-21T13:13:16.479183+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:af6667a34ab5545598e045add30f5f09945fc56122e88f3b1b2b56a1a028cbb1"
---

# Who manages the managers?

I have a friend who runs a squad at his company. When he needs to coordinate something — a workshop, a launch, a campaign — he opens his Slack DM-to-self, types out the task list with` @owner` next to each line, and copy-pastes it into the channel where the work will actually happen.


That's not a productivity hack. That's the workflow.


He does it because the people are in Slack. The replies will be in Slack. The clarifications, the follow-ups, the *"sent it via DM"* , the nudge two days later when nothing's moved — all in Slack. Switching to Notion or Linear would put a wall between his thinking and the place where the work ships. So he uses Slack DM as a notepad, because it's one keystroke away from publish.


I see this everywhere. The ones I love most are the people running their lives out of Apple Notes, Telegram-saved-messages, a single open Obsidian file. They're all hacking the same thing: **a private surface, close to where the work happens, that they trust.** No tool gives them that, so they build it from whatever's nearest.


This is who Char is for.


## Why now


Four things I believe, that won't change for the next ten years:


**Taste is the last moat.** AI flattens execution. The gap between people with craft and people without is widening, not narrowing. A statistical average has no point of view. Great work still requires someone who refuses the average.


**Preference is the forcing function for collaboration.** Orgs are shrinking — AI resolves inefficiency faster than headcount. But the work that matters still needs people, because no single person can hold design, engineering, marketing, sales at the same level of attention simultaneously. Collaboration doesn't disappear. It becomes more deliberate.


**Everyone is becoming a manager.** Whether you call yourself one or not, you're running a fleet of agents to get your work done — Cursor for code, Claude for drafts, ChatGPT for research, Devin for tickets. The traditional org-chart model — decisions flow down, ICs execute — is breaking. There's no more pure IC. Every knowledge worker is dispatching, reviewing, and rerouting work all day.


**Humans are naturally poor at managing complexity.** Entropy always wins. The most senior operators have an EA, a Chief of Staff, sometimes a COO. The rest of us just deal with it — at 11 PM, in our heads, while our brain becomes the API between Slack and Linear and Gmail.


The four compose into one question: **if everyone is becoming a manager, who manages the managers?**


That's the question Char is built to answer.


## What we got wrong


We wrote the launch post as[After the meeting ends](https://char.com/blog/after-the-meeting-ends) . The short version: we launched Char as an AI meeting notetaker, shipped 21 versions, swapped storage layers mid-flight, removed features users relied on, and watched 200 of our earliest adopters churn out instead of upgrading.


Most of it worked. Some of it broke trust.


The part we could not stop seeing was the same one my friend had already solved with Slack DM-to-self. Tools capture slices. People carry the connection.


So we split. The original Char — privacy-first, on-device, BYOK, OSS — lives on as[anarlog](https://github.com/fastrepl/anarlog) . It serves the people who showed up for those constraints, honestly, under its own name. **Char is something different.**


## What Char actually is


Char is a daily note that carries work forward.


The notepad is the form factor — because that's the surface where my friend's Slack-DM workflow already lives, and where every operator I respect already drafts their day. We're not asking anyone to switch surfaces. We're giving them a real one.


Underneath the notepad is the part that matters: a context layer. Tuesday's note has context from Monday's. Thursday's meeting references a decision from Tuesday's. When you ask *"what did we decide about pricing?"* Char doesn't search a folder of markdown — it traverses a graph of context that's been building for weeks.


You type a checkbox. An agent can pick it up — researching, drafting, scheduling. You stay in command of *what matters* . You approve what leaves.


**Meeting capture is the cold-start wedge, not the product.** A new user opens Char on day one. Their first meeting fills the daily note with action items before they've typed anything. The blank-page problem is gone on day one. From there the notepad accumulates — your meetings, your screen, your integrations, your stray thoughts — until tomorrow's note knows yesterday's context.


## Where this goes


I think about Char in three tiers. They map to the roles every growing organization adds, in this exact order, when it can afford them.


**Executive Assistant.** A daily note that evolves throughout your day. Captures meetings. Surfaces what's next. Tells you what you forgot. *"You need to follow up with X, send the thing to Y, prep for Z tomorrow".* This is what Char does today.


**Chief of Staff.** Tasks aren't just things you don't have to execute — they're things you don't have to manage either. Char drafts the email you would've drafted. Routes the spec to your IDE. Closes the loop with the customer. Learns your patterns over time, does more of what you would've done.


**Chief Operating Officer.** When everyone on a team has their own daily note, those notes become a shared operating layer. Context flows between people automatically. Decisions leave a trail. Half of every team's meetings exist to keep humans aligned on what's happening. Char is what makes those meetings shorter.


That's the company we're building. Not "AI for notes". A daily note where work is remembered, delegated, checked, and handed off.


## Who this is for, honestly


The wedge is founders, operators, and small teams. Not because the TAM is founders — it isn't — but because they feel the pain hardest, they'll try unfinished products, and they write checks themselves.


But the real customer is anyone who would benefit from an EA they can't afford. Anyone who's currently being the integration layer between their own tools. Anyone whose Slack DM-to-self has become their operating system because nothing real is close enough.


If you've ever finished a long day of meetings and wondered where it all went — Char is for you.


We're opening this in private beta. I'm onboarding every user personally for now.


[Join the private beta →](https://char.com/)
