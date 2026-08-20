---
schema_version: "1.0.0"
document_id: "70820c11694f808b272838a017b2840cba3f08a081fe4394c493e417f6318e38"
company_key: "yc-secoda"
company: "Secoda"
source_id: "yc-secoda-news-import-8239d8ce1f4c"
canonical_url: "https://www.secoda.co/blog/secoda-product-news-july-2025"
published_at: "2025-10-28T00:00:00+00:00"
first_seen_at: "2026-07-24T00:59:20.364488+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:c224013e15bd80264770d08a5bb09c36d7bcaf1c13c756f1ba08e490a143a77a"
---

# Letter from the CEO - July 2025

I feel lucky to work with such forward-thinking, AI-first organizations that challenge us to build better, faster, and smarter tools. These teams aren’t looking for another assistant or standalone tool. They want intelligence that fits into the way they already operate. They want something that understands the problem, sees the bigger picture, and helps them move faster without adding complexity.


Secoda AI is built to support that way of working. It offers proactive, high-quality suggestions based on how your team actually works. It’s fast, focused, and constantly improving with the context of your workspace. Data workflows today are collaborative, dynamic, and full of nuance. AI should work inside that reality and help teams take action, not get in the way.


## What we’re hearing from customers


We’re seeing it play out in the numbers. Secoda AI adoption is growing more than 40% week over week. That kind of adoption doesn’t happen unless it’s solving real problems and delivering accurate answers.


What’s most encouraging is how naturally it’s being adopted. We’ve heard from teams that rolled it out with minimal setup and quickly made it part of their daily routine. There’s no steep learning curve and no need to overhaul existing processes. Secoda fits into the workflows teams already have and starts contributing right away.


I love getting messages from customers about how they’re using Secoda AI to work smarter and faster. One organization ran a company-wide hackathon around Secoda AI and walked away with entirely new workflows. Another is using it to support their entire sales motion. Reps receive deal-specific recommendations, including opportunity risk and ways to improve deal health. Sales leaders use it for targeted coaching, identifying which reps need support and where to focus to hit quota. They’ve even integrated Secoda with an external prediction model to generate consistent pipeline forecasts across teams using both qualitative and quantitative signals.


We’re seeing Secoda AI support teams across the full data lifecycle, from onboarding and documentation to pipeline reviews and incident follow-ups. It’s not a one-off tool. It’s becoming part of how teams collaborate, troubleshoot, and make decisions. That level of adoption is rare, and it underscores where other AI tools fall short.


## Why AI needs to work like a team member


Most AI tools today still operate like black-box assistants. You feed them a prompt, they return an output. But that model breaks down in real workflows, especially when context, speed, and cross-functional coordination matter.


Secoda AI takes a different approach. Instead of acting as a standalone assistant, it integrates seamlessly into your team’s workflow. Acting as an observant, proactive data team member, it steps in with the right contribution at the right time, adapting to the needs of the workspace. Rather than relying on a single AI model to handle every request, Secoda AI coordinates its responses based on the specific context of your data, ensuring the most relevant action is taken. That orchestration is already powering one of the most valuable features we’ve built so far:[AI suggestions](http://www.secoda.co/blog/secoda-ai-suggestions) .


AI suggestions proactively offer recommendations in your workspace like adding glossary terms or data quality monitors on your core assets. These recommendations are based on how your data is actively used, whether it's identifying frequently queried data, addressing emerging issues, or highlighting areas with limited context. This ensures that the suggestions are perfectly aligned with your team’s needs, streamlining workflows and saving valuable time.


## AI suggestions in Secoda


Let’s take a closer look at two ways Secoda AI is providing actionable suggestions today with observability and cataloging.


### AI observability suggestions


Secoda looks at query patterns, column volatility, and historical incidents to suggest exactly where you should be monitoring and what to watch for.


Imagine your team depends on a table like monthly_revenue_summary. It feeds critical dashboards across finance, growth, and leadership. You ask Secoda AI, 'How is the health of the **monthly_revenue_summary** table?' Behind the scenes, our AI architecture detects that this table sees high usage and has recently undergone structural changes.


Rather than waiting for something to break, Secoda AI proactively steps in with three recommendations based on this insight:


- Set up a row count monitor to flag sudden drops
- Add a freshness monitor to catch delays in data updates
- Deploy a SQL-based monitor to track monthly customer engagement with a single value


Each monitor comes pre-configured with thresholds and schedules based on your data’s behavior. You can apply them instantly, right from the conversation.


By automating this process and providing actionable recommendations in real-time, Secoda AI saves your team valuable hours they would otherwise spend manually identifying and setting up these monitors, ensuring issues are addressed before they impact critical operations.


Secoda AI suggests to add a row count monitor to track unexpected changes on a core Snowflake table.


### AI cataloging suggestions


One of the most common gaps we see in growing data teams is inconsistent terminology. A term like 'Active Plan' or 'Trial Conversion' might be used across multiple dashboards, but its meaning can vary depending on who you ask. This slows down onboarding, creates conflicting reports, and chips away at trust in the data.


That’s where Secoda’s Cataloging suggestions come in. Secoda AI reviews how your team actually uses data and identifies the terms that matter most. It flags what’s missing, suggests definitions, and connects each term to the data it touches.


You might start by asking, “What glossary terms are we missing?” In seconds, Secoda returns a list of high-impact suggestions based on real usage.


For example, you might see terms like:


- “Active Plan” used across product and billing reports, but calculated differently
- “Trial Conversion” mentioned in lifecycle dashboards with no shared logic
- “Premium User” referenced in experiments, but never defined in plain language
- “Time to Value” used in onboarding metrics, with inconsistent time windows


Each suggestion includes a proposed definition, owner, Team, and links to relevant tables or dashboards. Teams can review and publish directly or tweak the content before sharing.


Organizations using cataloging suggestions have reduced onboarding time by weeks, improved reporting consistency, and scaled documentation without manual cleanup.


Example glossary term suggestions from the Secoda team workspace


## How it works under the hood


Behind every suggestion and interaction in Secoda AI is a modular architecture built for task-specific performance.


Instead of relying on a single model for every request, the system orchestrates specialized components in parallel, each optimized for a specific area. Some components focus on search across your data stack, others handle data quality issues, and some analyze metadata and ownership patterns. These components continuously improve based on usage over time.


When a request is made, the system evaluates it and routes it to the most suitable model for the task. For quick, structured queries, the system directs the request to specialized components designed to handle fast search and retrieval. For more complex business logic and reasoning, it routes the request to components optimized for deeper analysis. For conversational queries, the system uses components that excel in natural language processing.


By running these components in parallel, Secoda AI reduces processing time by 60 to 70 percent compared to traditional sequential workflows. This mirrors how high-performing teams delegate tasks to specialists, ensuring that each request is handled by the most suitable model for maximum efficiency.


For example, if you ask, “What tables contain customer data?” the system routes directly to the appropriate search logic and skips unnecessary steps. This kind of architecture is what enables AI to act as a true team member. It integrates into the work, adapts to your systems, and helps your team move faster without sacrificing quality.


## Built for collaboration, not automation alone


Every recommendation and output in Secoda is transparent and reviewable. Secoda AI can suggest and assist, but it won’t override your judgment. You stay in control, with the flexibility to accept, edit, or ignore. That’s how we believe AI should work: context-aware, specialized, and aligned with your team’s intent, not a replacement for it.


This approach helps organizations move beyond static catalogs and disconnected dashboards. Secoda becomes a dynamic layer across your stack that guides decisions, reduces manual effort, and helps everyone from analysts to executives get more value from data.


## Getting real about AI-first teams


There’s a lot of talk about what it means to be an “AI-first” organization. In our view, it’s not about adding a chatbot or surfacing a few insights in the UI. It means designing your systems, workflows, and expectations with AI in mind from the start.


When your documentation is solid, your lineage is reliable, and your processes are intentional, Secoda AI adds real leverage. Without that foundation, AI tends to multiply confusion instead of reducing it. That’s why we don’t just build AI features. We help teams build the systems and habits that make those features actually work.


## Why this matters now


We’re seeing a clear shift in how organizations want to work with data. Instead of just adding another interface or assistant, they’re looking for smarter, more integrated solutions that blend seamlessly into their existing workflows. The focus is on driving efficiency and speed without introducing unnecessary complexity or silos, enabling teams to get more done with less friction.


That’s why we focus on building systems that support complete workflows, not just isolated tasks. Our goal is to help teams solve real problems, end to end.


Data work is becoming more collaborative and more contextual, and AI should support that evolution by turning shared knowledge into action. Based on how Secoda AI is being used today, we’re already seeing that transformation in motion.


The kind of adoption we’re seeing doesn’t happen with reactive tools. It comes from systems that understand your environment and contribute meaningfully at the right time.


If your team is exploring how to apply AI across your workflows, we’d love to connect and share what we’ve learned.


Thanks for being part of the journey.
