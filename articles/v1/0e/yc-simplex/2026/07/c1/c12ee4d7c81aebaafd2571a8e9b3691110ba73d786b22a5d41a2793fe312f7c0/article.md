---
schema_version: "1.0.0"
document_id: "c12ee4d7c81aebaafd2571a8e9b3691110ba73d786b22a5d41a2793fe312f7c0"
company_key: "yc-simplex"
company: "Simplex"
source_id: "yc-simplex-news-import-3cbaa83d2dc6"
canonical_url: "https://www.simplex.sh/blog/open-sourcing-launch-video-tool"
published_at: null
first_seen_at: "2026-07-22T13:35:17.258369+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:1d57e5e97aada00f3df6bd353c6418da93260e01dd0e1d849b4ae0c8859163d4"
---

# Open-sourcing our tool to quickly build launch videos

**TL;DR:** Repo here:


[https://github.com/simplexlabs/montage](https://github.com/simplexlabs/montage)


In May 2025 we commissioned a team of motion designers to create a launch video. It cost us $2000 and 4-5 days of our full attention.


I noticed the video we commissioned was likely built using a set of animations and transitions that this motion design team used for all their customers. My theory was that if we could recreate that set of animation primitives and put them in a format coding agents could understand, we'd be able to create launch videos end-to-end way faster and have full control over the final product.


For some context, we're a browser automation tool for developers, specifically targeting complex form filling like prior authorizations in healthcare or instant quoting in insurance. Having potential customers visually see the product is really important: we help developers build long, 100+ question browser automations.


We've seen a big boost in reply rates/meetings booked when we post motion videos layered with targeted outbound. Our posts with animated videos have already outperformed the Screen Studio videos we posted previously that weren't grabbing people's attention.


I explain in the repo's


[README.md](https://github.com/simplexlabs/montage) how to use it (and you should really just go try it yourself) but this project also taught me a lot about the strengths/weaknesses of different models and where some of the gaps in SOTA intelligence are.


Here's what I learned along the way.


## My entire job was to build all the scaffolding so the agent could go work


I spent 3 days setting up my repo and spent 4 hours building the motion video you see above.


Those 3 days consisted of 1) building out the set of primitives and 2) building the iterative agent rendering loop so the agent could work autonomously.


### Building the primitives


The primitives I built were:


- **Text** (word drag-in, flip-up, gradient fill, typing animation)


- **Motion** (object pop in, fade right, fade left, swoop, float, etc.)


- **React components** (Simplex logo, cursor, animated browser, animated terminal)


- **Background designs** (radial gradient, corner gradient, color fade, etc.)


The way I built these primitives was surprisingly simple. I leveraged Gemini's ability to analyze videos (Claude and GPT don't accept videos as a modality yet) by attaching our previous motion-designed video and giving it this simple prompt:


> "Is it possible for you to thoroughly go throughout the video and, in detail, note all the different text transitions and what they look like, describing them in natural language so I can put them in my animation tool?"


It then gave me a text file I dumped into Claude Code to build animation primitives with. I repeated that for motion transitions and background designs, and modified React components from our website to work with Remotion.


I bet you could do the same thing with popular, well-animated videos you see online for launches and get a similar result instead of needing to commission your own.


### Building the iterative agent rendering loop


We've found empirically that agents do best when you give them all the context they need then let them make decisions for themselves, so I gave the agent the ability to test its sections end to end.


Remotion has the ability to render individual frames via a bash command (


` npx remotion still {flags}


` ) so I told Claude Code to heavily use that when designing sections. I also tried having the agent computer-use/browser-use the Remotion editor that can be spun up as a local dev server, but I found this was too confusing to the agent since it had to know how to scrub through frames in the UI, select the right sequence to check, etc.


I'd always seen this testing end-to-end concept described on Twitter (unfortunately our product isn't well suited for tight feedback loops like this, but we're working on it!) so it was cool to see a simple version of it in action.


Claude Code rendering individual frames to test animation sections


## You can't outsource your quality bar — 80% of your time is fixing the last 20%


I tried doing a lazy, slop version for one of our features halfway through development of this repo, and this is what the result was:


Your browser does not support the video tag.


I think it looks pretty good visually! But the content is nonsensical since I didn't storyboard it enough.


I couldn't get Claude to adequately do the human work of deciding what about the feature was important, what copy will resonate with your ICP for this particular launch, etc. I'll admit I didn't push very hard in this direction. Since we're also building an internal GTM engine, I suspect I'll be able to leverage Claude a lot more if we pull enough info from Circleback, Slack, and other data sources that have our customer conversations.


## Local agents are better than cloud agents for your baby render farm


To quickly churn out sections of a motion video in parallel, you need a code agent orchestrator with two features:


- The ability to quickly spin up instances of a repo that has your project's dependencies pre-installed


- A GUI that shows you the images/videos it renders so you can check sections for animation quality and text/object placement


Cursor cloud agents are explicitly built with both these features in mind, so I started with those (I tried CC cloud/Codex cloud/Devin cloud as well but they all felt underbaked). The issue is that Cursor cloud agents run on puny containers that take forever to render a single frame. The GUI is useful though:


Cursor cloud agents rendering video sections


Additionally, spinning up multiple cloud instances that don't share a filesystem mean you can't leverage new transitions or components that you created in any of the other sections. As a result, I ended up mainly using Claude Code, but I bet Codex local would do great too.


## What I learned that we can apply to our main product


We rebuilt our entire product over the course of 2.5 months starting back in December, but so much has happened in the last month that we knew we had to keep up with what's currently SOTA.


We're in the business of building agents, so it's clear as an engineering team we need to lean harder into what we're already doing, which is 1) letting our agent have all the context it needs and the tools to correct itself and make good decisions while loosening constraints and 2) optimize our CI/CD for the same thing.


## Try it out and talk to us if our product would be relevant to you


I'd love for you to try this tooling out at


[https://github.com/simplexlabs/montage](https://github.com/simplexlabs/montage) .


If you're a vertical AI company that needs browser automation, we're a browser automation developer tool that's especially good at complex form filling where you might need to:


- Interface with an AI context engine to answer questions


- Write back questions from web forms into your database.


[Talk to us here](https://cal.com/team/simplex/simplex-intro-meeting) or


[sign up for free here](https://simplex.sh/dashboard) if you think we might help solve your browser automation problems.
