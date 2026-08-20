---
schema_version: "1.0.0"
document_id: "103d59ffc779ceaf5e3a4a8ee48e517e27f080fd8c9a730be19bec83ce38345a"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/how-ai-tools-accelerated-six-month-framework-migration-to-nine-days/"
published_at: "2026-04-13T16:22:10+00:00"
first_seen_at: "2026-07-20T23:19:31.841537+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:967815e0c9bc5f6b9e4fd2b2378ffbbc43fa76505760addfcafc3baf65598bbb"
---

# How AI Tools Accelerated a PM’s 6-Month Framework Migration to 9 days by Forrest Evans

If you feel like your technical debt is piling up while your engineering team gets more stretched by the week, you’re not alone. Framework upgrades keep getting postponed, migrations live on roadmaps for months, and engineering bandwidth is maxed out. That’s exactly why I decided to test what AI tools could actually do—by tackling a Grails 6-to-7 migration that consultants quoted at $200K-$300K and 6 months.


I’m a reasonably technical Product Manager, but I am definitely not a software engineer. When I decided to migrate Rundeck from Grails 6 to 7—a project involving 5,100 tests, multiple framework upgrades, and something called “the javax to jakarta namespace migration”—I had reasonable questions. They quickly evolved into “Why am I doing this?” and “Shouldn’t I be grooming a backlog?”


Nine days later, we had a 99.7% test pass rate, 44 documented migration patterns, and proof that AI-assisted development can unlock work that’s been stuck in the backlog. Here’s what happened when a PM decided to see what AI tools could actually do.


## The Inspiration


What drives a product manager to start writing code? Resource constraints, at least in this example. Our engineering team was swamped and stretched thin, and upgrading to Grails 7 had been on the roadmap for months. Spring Boot 3 was out, Spring Security 6 was calling, and we were starting to accumulate the kind of technical debt that makes engineers nervous.


Cursor and Claude Sonnet 4.5 were continuously mentioned as the “AI pair programming” that promised “10x developer productivity”. Naturally, I was skeptical, and as a PM who has written some code, I decided to see what all the hype was about. I quickly learned that a “javax to jakarta” migration isn’t like bumping a version number. It’s every servlet API, every validation annotation, every persistence layer import getting renamed. It’s 15 years of our Java ecosystem history getting refactored.


The idea was to give it a try for a day or two and give up if things get confusing. *Spoiler alert:* I kept going. Things were starting to work, and a lot faster than I expected.


## Reality starts to set in


This wasn’t just bumping a version number. We were simultaneously upgrading:


- Grails 6 → 7 (the headline act)
- Spring Boot 2.7 → 3.2 (the platform beneath)
- Spring Security 5 → 6 (authentication layer)
- Hibernate 5 → 6 (query syntax changes)
- Groovy 3 → 4 (language semantics)
- Java 11 → 17 (runtime environment)
- javax → jakarta namespace (400+ files touched)


Plus, certain functions like LDAP auth and Metrics handling had to be completely refactored.


### Here’s how AI tools accelerated and improved the migration lifecycle:


- **Detect and triage** : With migration guides and release notes loaded into Cursor’s knowledge base, AI identified patterns across the ecosystem and separated signal from noise.
- **Diagnose** : Analyzed compilation errors, identified dependency conflicts, and consulted migration guides to present likely causes with supporting evidence.
- **Remediate** : With my approval, AI suggested fixes. I validated that tests passed and documented what worked.
- **Learn** : The AI tool improved its recommendations over time and generated 44 new migration patterns to prevent future issues.


## Day 1: Pattern recognition at scale


The first challenge in any framework migration is the compilation process. Hundreds of errors across the codebase, many stemming from dependency conflicts where some libraries still used` javax` while others had moved to` jakarta` .


This is where AI tools like Claude Sonnet 4.5 and Cursor excel. Not because they magically fix everything, but because they’re exceptionally good at pattern recognition. Paste an error message, and the AI tool immediately identifies the root cause: *“That’s a Spring Security artifact conflict. You need spring-security-core, not spring-security-crypto.”*


Could an engineer figure that out manually? Absolutely. But it would take reading documentation, searching the web, and piecing together context from multiple sources. AI gets there in seconds, with an explanation of why the conflict exists.


By the end of day one, the code compiled. The real work was just beginning.


## Days 2-5: Learning patterns while fixing tests


5,100 tests stood between “it compiles” and “it works.” *Day 2 started with 221 failures* .


Test failures in framework migrations cluster into patterns. Fix one, fix twenty. Or break thirty. The key is recognizing those patterns quickly.


Here’s where AI collaboration became a force multiplier. A test failure like:


```text
MissingMethodException: No such property: principals for class: Subject
```


would trigger an immediate explanation: “That’s Groovy 4. The` principals` property is now read-only. You need to use the` getPrincipals()` method instead. Also, you’re probably using the wrong test trait. Try` DataTest` instead of` HibernateSpec` .”


Every fix came with context. Every solution was documented as a pattern so by day 3, the agent was spotting Groovy 4 field access issues and fixing items even faster.


This is the real multiplier with AI tools: you’re learning as you build. Every solution can include the why, not just the how. That knowledge compounds.


## When authentication stopped working


Day 5 brought a critical issue. We’d gotten down to 17 test failures, deployed to our test environment, and discovered the admin user couldn’t log in.


Six hours of debugging revealed the problem: Groovy 4 changed how` equals()` works on Java objects. Our` RundeckRole` class had an` equals` method that compared by name. When the admin user had a principal named “admin” and a role named “admin,” Java’s Set implementation treated them as duplicates and removed one of them.


Result: admin user with no admin role. Access denied.


Ultimately, with some iteration and collaboration between local testing and context, the problem was solved. AI helped trace through the authentication flow and spot the duplicate removal, but fixing it required understanding our authorization model. AI can’t rewrite your security layer without domain knowledge.


## The documentation dividend


By day 7, we had accidentally created 44 documented migration patterns. Each one included:


- What broke
- Why it broke
- How we fixed it
- How to detect it elsewhere


AI made this easy. Every solution came with context that could be pasted into our migration notes, cleaned up, and augmented with code examples. Commit messages stayed descriptive. Pull request details were updated after each session.


Most migration projects leave knowledge in one engineer’s head. We shipped with 44 reusable patterns, complete with root cause analysis. That documentation value alone is estimated at $50,000 to $75,000 in saved time for future framework upgrades and handoff to engineering during review and finalization for this project.


## The results


**What it would have cost:**


- Consultant: $200,000-$300,000, 6 months
- Senior engineer: ~$72,000 in loaded cost, 6 months full-time
- Our approach: $200 in AI tools + 9 days


**What we delivered:**


- 99.7% test pass rate (5,083/5,100 tests passing)
- 44+ documented patterns
- Modern platform foundation for the next 3-5 years
- Time acceleration: 40-60x faster than manual migration
- Cost savings: 900-1500x cheaper than consultants


These aren’t hypothetical numbers. We tracked commits, test fixes, and pattern documentation throughout the project.


## The playbook for AI-assisted technical work


If you’re considering using AI tools for complex technical projects, here’s what worked:


**Build your knowledge base first**


Before writing code, gather context: migration guides, common issues from forums, codebase quirks, and historical decisions. Load it all into your AI tool’s knowledge base. AI without context wanders aimlessly.


**Use the right tools**


Cursor with Claude Sonnet 4.5 provided the best results for complex reasoning. The cost is negligible compared to consultant fees or engineering time.


**Document everything**


Documentation is key to long-term success. Write patterns as you find them. Include the problem, root cause, solution, and detection method. If you clarify or have the agent research architecture through code, have it write a summary for use in other sessions or tools. Future you and colleagues will thank you.


**Validate constantly**


Rely on your tests and don’t let AI symptom solve. AI tools love to help, but try to get things done quickly on the shortest path. Keep it focused on the right path. Use CI/CD for real validation. AI makes plausible mistakes—catch them early.


**Know your limits**


AI excels at patterns but struggles with architecture. Continue to rely on best practices, such as 2-party code reviews, to address security and performance concerns. When you hit a wall, document it and move on. Keep parallel investigations going so you never get stuck for long, but if you are stuck, have your agent write it out and move on.


**Embrace iteration**


The first solution often isn’t the best solution, and AI will improve as you go. It won’t have a magic bullet, but with time and repetition, the solution comes to life. Refine patterns based on what works. Speed comes from rapid feedback loops.


## What this means for operations teams


I’m not suggesting PMs should do engineering work. That’s not the point.


The point is: If someone with a limited engineering background can achieve 40-60x acceleration, imagine what experienced engineers with these tools can do. The constraint isn’t capability anymore. It’s willingness to try.


Framework upgrades. Well-scoped refactoring. All those projects that are labeled “important but not urgent” things that never happen because “we don’t have bandwidth”, those are perfect targets for AI-assisted work.


The hybrid model that works: technical team member + AI + occasional engineer review (2-3 hours per day). It is 10x cheaper than consultants, 40x faster than manual work. But don’t leave out the discipline to write “why it broke,” not just “how I fixed it.”


## What’s next


We’re applying these patterns to Runbook Automation now. Issues that took hours in the first migration take minutes because we’ve documented what to look for.


The line between “technical” and “non-technical” work is blurring. Tools that were senior engineer territory six months ago are now accessible to anyone willing to learn. That’s both democratization and disruption.


The question for operations teams: What’s stuck in your backlog because “we don’t have time”? What would you tackle if the time and cost constraints changed by an order of magnitude?


Here’s what I encourage internally, and what I’ll encourage here:


Get started with your AI-enabled IDE of choice and take a bigger swing. You’ll surprise yourself. You’ll surprise your business.
