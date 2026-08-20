---
schema_version: "1.0.0"
document_id: "2c3ed970c5e9b0b15ebc2a16fc65e29eb443b1dcabdab55f653b68e5eee4e800"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/ai-native-engineering-interview"
published_at: "2026-06-09T22:58:20.214+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:904db2d500485e604fad91d96e6fcfce113947b6ffef99a5bbb7201b238c50d5"
---

# What We Learned Hiring 33 Engineers in Two Weeks

Earlier this year, we needed to hire a cohort of engineers in Seattle, fast. We had a product launching at our marquee conference,[Deploy](https://www.digitalocean.com/deploy) , a hard deadline, and a clear picture of what the work would actually require. What we didn’t want was an interview process designed for a world that no longer exists.


So we rebuilt it from scratch and opened a brand-new office in Bellevue for everyone we hired. Here’s what we did, why we did it, and what we heard from the engineers who went through it.


## The problem with the standard loop


The traditional engineering interview loop (recruiter screen, hiring manager screen, technical phone screen, take-home, onsite) was designed for a different era of software development. It tests for pattern recognition and syntax recall. It stages information rather than creating genuine signal. And it takes weeks.


More importantly, it doesn’t reflect how engineers actually work today. Production environments are collaborative. Most engineers entering the field right now have been working with AI tools since they were in school, not reluctantly adopting them, but building with them naturally. A hand-implemented sorting algorithm on a whiteboard tells you almost nothing about how someone thinks through a real system.


We wanted an interview that did.


## What we changed


### We made the work the interview.


The centerpiece of our on-site was a three-hour build session. Candidates chose from a short list of assigned prompts and were asked to design, build, and deploy a working prototype on DigitalOcean by the end of the session. They could use whatever AI tools they wanted: Claude Code, Codex, ours, whatever they were fastest with.


Three hours is the minimum window in which you can actually watch someone make real engineering decisions: what to scaffold versus what to write by hand, how they prompt, what they verify versus what they trust, how they handle the moment when the AI confidently produces something that doesn’t work. That moment always comes.


We shifted from hiring niche specialists to sourcing strong, well-rounded software development engineers. We weren’t evaluating whether candidates could produce code. We assumed that. We were evaluating whether they had the judgment to productionize an idea.


Shivani Shirolkar, one of the engineers we hired for our model catalog team, had never been in an AI-assisted interview. She prepared by using Cursor to work through sample problems herself. “I ended up creating a spec for myself that I could feed into Cursor during the interview,” she said. “For the first three hours, it was just heads-down building. They gave us the space to fully focus, which made it so much more engaging than a typical technical interview.”


### We followed the build with a real conversation.


After the coding session, candidates walked interviewers through what they’d built: design choices, trade-offs, and what they’d do differently with more time. Then the conversation went further, into hypotheticals about scaling, business constraints, what it would look like if traffic spiked, and there were windows of downtime to work with.


Eric Daniel, now on our serverless inference team, found that the discussion after the build was where he could really show his thinking. “I was able to explain the architectural decisions I’d made and why they’d make future improvements easier to build on,” he said. “Then the conversation shifted into territory I’d never covered in an interview before: product trade-offs, business constraints, how the system would behave under real-world pressure. That’s the kind of problem-solving I actually do every day.”


That’s exactly the kind of thinking we were looking for.


### We cut the steps that weren’t adding signals.


For candidates with directly relevant backgrounds, we removed the recruiter and hiring manager screens from the loop. For candidates with strong but adjacent experience, the technical assessment was enough to qualify them for the on-site without a separate call.


Every step we removed was either duplicating something we already knew or generating information we weren’t going to use. Redundant steps don’t just slow things down; they signal to candidates that the process wasn’t designed thoughtfully.


### We made decisions the same day.


At the end of each interview day, the full panel (recruiters, hiring managers, technical program managers, and engineering leadership) debriefed all of the candidates together. Because we had aligned on decision criteria and pre-approved compensation bands ahead of time, we could move the moment we found the right fit.


Our fastest offers went out the next morning. Both Eric and Shivani had offers within 24 hours of their interviews, and both started within two weeks. Eric was shipping PRs on his second day. Shivani had a database-related PR in review before her first week was out.


## Who we hired, and why that was a deliberate choice


Most of the engineers in this cohort are early in their careers. That was intentional.


There’s been a quiet assumption in parts of the industry that early-career hiring is less strategic in an AI era, that AI tools reduce the need for foundational engineering roles.[We see it the other way around](https://www.digitalocean.com/blog/zero-to-deploy) . Engineers entering the field today don’t think of AI as a tool they’ve had to adopt. It’s simply how they build. That fluency isn’t something you retrofit into someone; it’s something you hire for directly.


We also know that senior engineering judgment—the ability to own a system, lead a review, make the call on architecture—is something that develops in seat, with the right environment and the right challenges. What we can’t manufacture is the disposition to build with modern tools naturally. That’s what we were looking for, and that’s what we found.


## Where they work


The timing of the blitz also gave us a reason to do something we’d been working toward for a while. The Greater Seattle area is already home to more than 90 DigitalOcean employees, including our[new CPTO](https://investors.digitalocean.com/news/news-details/2026/DigitalOcean-Appoints-Vinay-Kumar-as-Chief-Product-and-Technology-Officer/default.aspx) . With a cohort of new engineers joining at once, we opened a new office at 10700 Northup Way in Bellevue to give the team a home base.


The space is designed for exactly the kind of work this cohort was hired to do: co-working, team sprints, and the in-person collaboration that accelerates ramp-up. For a group that hit the ground running, shipping in their first days together, having a dedicated place to build together contributed to their success.


## What the cohort has shipped


The timing of this hire wasn’t incidental. Both Shivani, Eric, and Andre Hernandez joined in mid-March, with Deploy on April 28. There was no gentle ramp; all 3 and the rest of the team in their cohort jumped right in. Vinay Kumar, Chief Product and Technology Officer, set expectations that this next phase for DigitalOcean means we needed to operate differently—make different choices, work in different ways. And we did.


“In a matter of weeks, we came up with an execution plan, built and shipped as many services as we possibly could as a small team, and delivered a product vision our customers could start testing immediately. The feedback from our customers, developers, and investors was incredible. That’s how powerful what we’re building really is.”


Shivani has been predominantly on[Model Catalog](https://docs.digitalocean.com/products/inference/how-to/browse-model-catalog/) , working on the SI and DI model launches that were part of Deploy. “From day one, I was building,” she said. “It wasn’t about owning a particular stack; it was about jumping into whatever was the highest priority and making an impact. That kind of dexterity was actually really energizing.” Since Deploy, she’s carried that same mindset into more focused work on custom metrics for model evaluation.


Andre shipped the[Spaces MCP](https://docs.digitalocean.com/reference/mcp/spaces-mcp-tools/) ,[Block Volumes MCP](https://docs.digitalocean.com/reference/mcp/volumes-mcp-tools/) , and[NFS MCP](https://docs.digitalocean.com/reference/mcp/nfs-mcp-tools/) servers, in a short amount of time, which was a great way for him to hit the ground running. “Working on MCP servers really connected the dots for me early on. It forced me to think deeply about how DigitalOcean’s customers interact with Block Volumes in new, agentic ways. I was learning our internal storage operations while simultaneously understanding our shift to an AI-Native cloud. Moving from task breakdown to production deployment so quickly has made all the difference to my onboarding.”


Eric joined a two-person team on serverless inference, helping migrate models from internal hosting to the dedicated inference platform. The work involved automating configuration steps across roughly 30 models while ensuring zero customer downtime on existing hosted models. Post-Deploy, he’s been working across the team, adding autoscaling to the multimodal cluster, improving error logging, and bringing the infrastructure back into compliance with code standards after the sprint.


## What we’d tell other teams


Two things have made this work.


First, **evaluate the work as it actually exists today** . The most important change we made was philosophical. When you design an interview that mirrors how engineers actually build right now, you start asking better questions, attracting better fits, and making decisions you’re more confident in.


Second, **treat hiring as a design problem first** . Fast teams move fast because the upstream design work is done. When every interaction has a clear purpose, the process gets faster, and the experience improves, for candidates and for the people doing the hiring.


We’re planning to run this again. Eric, Shivani, and Andre are part of the first cohort; there will be more. And if you’re a builder who wants to work on infrastructure that’s genuinely at the edge of what AI-native cloud looks like, we’d love to talk.
