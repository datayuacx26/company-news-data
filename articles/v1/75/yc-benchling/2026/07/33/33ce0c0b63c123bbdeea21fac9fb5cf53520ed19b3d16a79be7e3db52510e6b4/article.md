---
schema_version: "1.0.0"
document_id: "33ce0c0b63c123bbdeea21fac9fb5cf53520ed19b3d16a79be7e3db52510e6b4"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/polish-bash-2026"
published_at: "2026-07-08T07:00:00+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:7a31488aa42c01e880703c88e54ad9c85e41d7a1c9fe07b0aacc8b3e5c035a18"
---

# Polish Bash 2026: Turning customer feedback into shipped fixes

**You asked, and we built it 114 times. Here’s how we polish our product.**


Every year, Benchling Engineering sets aside its product roadmap work for a week to focus on something different: the fixes that are too small to make a quarterly roadmap, but too important to keep ignoring. We call it Polish Bash.


This year was the biggest yet with more than 541 ideas coming in. We triaged them, voted on the highest-impact ones, and shipped 114 improvements in five days.


**How Polish Bash works**


Polish Bash starts with a submission period. Throughout the year, Solutions Engineers, Customer Success Managers, and Support collect feedback from the field. Community posts, support tickets, and direct conversations are all considered. The focus is on friction points that don't normally fit into engineering sprints.


This year's submission period generated 541 ideas (including duplicates, out-of-scope requests, and ideas better suited for a full release). The Build team leads triaged every one, and together Benchlings voted to surface the highest-impact items in a ranked list that engineering tackled throughout the week.


**This year, AI changed the mechanics of Polish Bash**


Polish Bash has been an annual tradition at Benchling for nearly a decade. This year, for the first time, AI agents were active collaborators in the sprint.


The workflow looked like this: engineers opened a Slack thread for each ticket, linked a Cursor cloud agent session, and shared an initial prompt explaining the problem and pointing to relevant code. Teammates followed in real time, and pull requests started landing hours after the sprint began.


One engineer picked up a ticket about bulleted list formatting getting lost when copying from Benchling notebooks into Google Docs. Five hours later, the pull request was merged. Another ran the same workflow for audit log improvements. Months before the sprint, Benchling's Developer Productivity team built the infrastructure that made this work possible, creating shared plugins for Cursor and Claude Code and preconfigured integrations with Jira, Confluence, Sentry, and Slack.


Shipping faster doesn't mean shipping with less scrutiny. Every pull request from this sprint went through Benchling's standard review process, including the human sign-off, testing requirements, and automated assurance checks that govern every release. The AI agents handled scoping and implementation, and our engineers stayed accountable for what merged.


We build AI products for scientists, but we also use them ourselves. Polish Bash this year was partly a test of that bet, and we’re happy to say that it worked.


As our Engineering Lead, Adam Van Lente, put it:


"Last year, AI wasn't even in the conversation during Polish Bash. This year, it was the entire premise. In the last twelve months, we've adopted tooling that allowed us to take huge strides: from auto-filing hundreds of Jira tickets, assessing which fixes an agent can one-shot, and shipping pull requests faster than we ever could manually. We called it the Polish Bash to end all Polish Bashes, because the goal wasn't just to fix more things. Our goal was to make the event obsolete by ensuring Polish is automated and happening all the time."


**What we shipped**


Some of this year's improvements were suggested by our very own Benchling Community members.


-


Key columns can be locked in place while scrolling through wide tables.


-


Entity chip fill-down now copies correctly instead of auto-incrementing IDs.


-


Analyses can now be starred and pinned.


-


Insights charts now support custom color assignment.


We also made improvements to Benchling AI, including a new data import skill that recreates uploaded scientific records as structured notebook entries, and reliability fixes to Deep Research. Read up on more of my favorite fixes in my[Community Article: 2026 Polish Bash Community Favorites](https://community.benchling.com/product-updates/2026-polish-bash-community-favorites-2184) . ****


**What we celebrated**


This year's awards included the usual categories, plus one addition: Best use of AI.


-


**Highest Customer Impact:** Sticky Horizontal Scrollbar


-


**Biggest Performance Win:** Bulk Unarchive


-


**Best Use of AI:** Process Execution Team, for five features shipped to improve workflows, recipes, and lookups


-


**Exec Choice:** Add "Save as Template" action


Going into this sprint, our teams asked the bigger questions: what if Polish isn’t an annual event, but something that happens continuously? Can the feedback loop get fast enough that a dedicated week is no longer needed?


That's not the reality yet, but this year got closer with 541 ideas in and 114 shipped in five days. We’re happy to see the distance between "customers said this" and "it's fixed" getting shorter.


Do you have an idea for next year? Share it in the[Benchling Community](https://community.benchling.com/) .
