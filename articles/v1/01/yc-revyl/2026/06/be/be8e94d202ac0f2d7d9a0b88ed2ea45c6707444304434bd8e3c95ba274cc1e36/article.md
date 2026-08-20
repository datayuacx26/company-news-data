---
schema_version: "1.0.0"
document_id: "be8e94d202ac0f2d7d9a0b88ed2ea45c6707444304434bd8e3c95ba274cc1e36"
company_key: "yc-revyl"
company: "Revyl"
source_id: "yc-revyl-news-import-bdee0e34c3b5"
canonical_url: "https://revyl.com/blog/parrot-software-factory/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T21:41:44.227724+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:7b7c78b9fa7bc1a5e3067620a10ee457715925faac45700b1b255e8a032dc764"
---

# Parrot × Revyl: A Mobile Software Factory in the Cloud

# How Parrot lets agents ship code around the clock


Parrot runs a mobile software factory where agents write and ship production code without a human babysitting every change, and Revyl’s dev loop is what lets them do it in an increasingly asynchronous manner.


Revyl CLI with Coding Agent


---


## Quote: Erik Dahl - Cofounder @ Parrot


---


## The problem


**What Parrot is building.**


Parrot runs what they call a software factory. Instead of engineers writing every feature by hand, AI agents do most of the building. They pick up work, write the code, and push changes into the product continuously. Humans set direction and review the hard calls, but the volume of day-to-day code is coming from agents.


**The bottleneck.**


Getting an agent to write code turned out to be the easy part. The hard part is knowing whether the change actually works once it’s running on a device. Before Revyl, that check was a person opening the app and looking. Regressions could sit unnoticed until someone happened to tap the wrong (or in this case right) screen, and agents didn’t have eyes to see what their code changes did on screen.


This verification is what lets the coding agents take on much longer tasks and do them in a more token-efficient manner.


Revyl Agent Testing


---


## How it works


**1. Agents write code against the live app.**


Parrot’s agents pick up work and make real changes to the app, feature work, fixes, and refactors, the same things a human engineer would do.


**2.` revyl dev` puts the change on a cloud device, instantly.**


The moment the agent saves,` revyl dev` hot-reloads the change onto a live cloud device. No rebuild, no waiting on a pipeline, the new code is just running on a device in seconds. This is the piece that doesn’t exist anywhere else.


Under the hood we run a relay between the user’s metro server and our cloud devices, which is what makes the change land instantly.


**3. Revyl validates the change and gives the result.**


Revyl then checks what actually happened on the device, the change either holds up or it doesn’t. When it fails, the agent gets that back with the context it needs to fix it and try again, instead of the failure sitting in a queue for a human to find. That’s what closes the loop: the thing that wrote the code is also the thing that finds out whether it worked.


**A few things Parrot’s agents can do because of this:**


- After an agent builds a feature, it hands back a recording of that feature running on a real device, so Erik sees proof it works instead of taking the agent’s word for it.
- Run the whole loop on the cloud. Nobody needs a Mac on their desk to run the coding agents, the devices and the build all live in the cloud.
- Revyl’s CLI can bypass auth and set app state directly, so an agent can drop straight into the screen it’s working on instead of logging in and clicking through every time it wants to test something.
- The Revyl CLI has a live stream of the performance, network, state of the device and the app. A use case the Parrot team has started using is doing` /goal` and then hillclimbing on performance metrics with very fast feedback loops.


---


## What’s next


With the loop closed, Parrot can let agents take on more of the SDLC while still having the confidence that everything is working and the visibility into the agent’s work. The next step is widening what the agents own and turning our new product Atlas into the one place where teams like Parrot do mobile development.


---


## Want this loop for your team?


Try it out! We are giving out free credit to teams who want to try, just shoot us a DM!


[@tryrevyl](https://x.com/tryrevyl)


---


## Parrot is hiring


Parrot is building one of the most agent-native engineering teams out there, and they’re looking for engineers who think that way too. If you’re an AI-native engineer who wants to build where agents do the heavy lifting and you get to design the system around them, reach out to Parrot.


[https://www.ycombinator.com/companies/parrot/jobs](https://www.ycombinator.com/companies/parrot/jobs)
