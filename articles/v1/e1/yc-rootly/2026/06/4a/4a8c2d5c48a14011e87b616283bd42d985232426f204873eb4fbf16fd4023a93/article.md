---
schema_version: "1.0.0"
document_id: "4a8c2d5c48a14011e87b616283bd42d985232426f204873eb4fbf16fd4023a93"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/every-incident-is-an-invitation-behind-the-mind-of-a-reliability-expert"
published_at: "2026-06-19T14:54:00+00:00"
first_seen_at: "2026-07-24T00:13:28.618726+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:d181ed307a2451b6c7c7fee2dd855d01310b88a7fadcd9c5606b51b8d1b8958d"
---

# Every incident is an invitation: Behind the mind of a reliability expert.

Every time an alert fires, Ankita Gandhi asks two questions. How do we fix this. And what do I do so this doesn't happen again. She's been asking that second question for eight years. It's the through line across every company she's worked at, every on-call rotation, every incident that got resolved and then quietly repeated itself two months later because nobody went back to look at why.


A photo of Ankita Gandhi, SRE at Glean.


## The internship she almost didn't take


Ankita grew up watching her sister work in site reliability. She assumed it was mostly systems and networking, not her thing. When a college internship opened up at her sister's company, she applied mostly because that's what you do when you're in college and need an internship.


What she found surprised her.


The role sat in between debugging and coding, close enough to real customer problems to feel meaningful, technical enough to actually fix them. She wasn't just identifying what broke. She was fixing it. That was eight years ago. She hasn't left the field since.


## The idea she's never let go of


Early in that first internship, at Nutanix, a manager told her something she's carried ever since: “Reliability isn't about solving the problem in front of you. It's about making a better product.”


In practice that means resisting a strong pull in the opposite direction, because the fastest path through an incident queue is the patch that closes today's ticket. Asking the second question takes longer. It doesn't always show up in your metrics.


> “I can be the quickest ticket resolver. I can be like the quickest patch deployer. But no customer likes to file tickets every day. What did I do to make the product better? That's the measure I like to follow.” said Ankita.


## The Slack channel that stopped scaling


When Ankita joined enterprise AI company Glean two years ago as the third SRE on a team that had just tripled in size overnight, the incident process was a single shared Slack channel. Everyone piled in when something went wrong. It worked because the team was small and context was shared. Then the customer base grew. More features, more surface area, more things that could go wrong simultaneously.


The shared channel that used to feel like signal started to feel like noise. Moving to Rootly let the team restructure around the reality they were actually operating in. Dedicated channels per incident. Defined roles. A clear separation between where incidents were coordinated and where they were announced.


> “Creating dedicated channels, but still having that central channel for publishing hey, there's an incident going on, that's helped us manage noise a lot. It's helped with context switching. It keeps knowledge in one place.”


Something else happened too. With incident data now contained in structured, searchable channels, Ankita could start building on top of it. She's been experimenting with agents inside Glean that read through incident channel history and surface patterns or run custom workflows.


## The thing she'd tell her earlier self


Ankita spent years before Glean working at larger companies. Guard rails everywhere. Decisions that required multiple sign-offs. Carefully bounded scope.


At a startup, none of that exists. You own the problem end to end. You find it, scope it, fix it, ship it. It took her longer than she'd like to admit to fully trust that.


> “I wish I could have figured this out sooner. You own what you do. You can find your problems, you can fix your problems. There was no 'you can do this or you cannot do this.' But it took me a while to understand that.”


## Getting reliability on the agenda before the feature ships


The change Ankita is most proud of at Glean is also the one she's most reluctant to take credit for: reliability now gets discussed before features ship rather than after. Two years ago, the SRE team was mostly reactive. A feature would go out and they'd figure out the observability story afterward. Now they're in the room before the feature goes out, asking what could fail and what they'd need to see if it did.


> “Now my team stays involved before something's going out. What can we miss? What will not work in terms of observability? Those talks are happening before.”


Building a status page was part of that shift too. Rootly's status page gave customers a consistent place to check during incidents, which reduced the pressure on engineers to write updates while simultaneously trying to fix things.


## The hard part of building AI agents


Ankita is spending a lot of her project time right now on AI agents. She's careful about how she describes the challenge. The tools aren't the hard part. The hard part is the iteration required to build something genuinely useful rather than just technically working.


> “Your output can only be as good as the prompt you feed it. There's a learning curve and I really hope in the next few months I'm working more towards creating agents that are helpful to the whole company, not just a team.”


That's the bar she's set for herself. Not functional. Useful enough that other people want to use them without being told to.


## What she'd want other SREs to steal


Ask the second question. Every time. Not just how do we fix this, but what can we improve because of this. Ankita frames every incident as an invitation, not a failure. That reframe changes what you do with the hour after the incident is resolved.


> “Focus on what can I improve today. It can be just one thing. But that's okay, as opposed to trying to figure out how do we solve everything at once.”


That habit, more than any tool or process, is the difference between a ticket closed and a product improved.


—


Ankita Gandhi is a site reliability engineer at Glean. This conversation is part of Rootly's Humans of Reliability series, spotlighting the people shaping how engineering teams think about failure, response, and learning.*
