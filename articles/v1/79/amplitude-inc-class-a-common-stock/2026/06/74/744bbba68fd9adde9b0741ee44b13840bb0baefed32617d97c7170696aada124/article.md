---
schema_version: "1.0.0"
document_id: "744bbba68fd9adde9b0741ee44b13840bb0baefed32617d97c7170696aada124"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/ai-impact-awards-2026-winners"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:39de36f4ee9b574ad63878bc2a4e16a5ad9a5909aba2ef2034cea86b1d2690b2"
---

# Meet the Winners of the 2026 Amplitude AI Impact Awards

Many teams are experimenting with AI. Few are realizing its full potential.


We wanted to know what separates the two. Not the teams testing the waters, but the ones going deep. Building new ways of working, not just faster versions of the old ones.


That’s why we created the first annual Amplitude AI Impact Awards.


We invited customers using[Amplitude AI](https://amplitude.com/ai) to share their best workflows. We didn’t know what to expect. What we got blew us away. Teams from around the world are reinventing how they build and market products in the AI era.


Thirty-one submissions came in. We evaluated each one on four criteria: impact, clarity, repeatability, and speed to value. We’re excited to share this year’s winners: three teams that didn’t just experiment with AI, but built something the rest of us can learn from.


## First place: Dhana Lakshmi Kunapareddy, Infosys


“Amplitude AI contributed speed and scale—surfacing the error pattern, isolating it to the mobile platform, catching the version-level rate distinction, and ruling out backend endpoints, all within minutes across millions of events. Our team contributed depth and context—the session replay analysis that revealed the exact trigger, and the internal knowledge that connected it to a known navigation issue. Neither alone would have reached the full conclusion as quickly or as accurately.”


Dhana Lakshmi Kunapareddy, Consultant, Infosys


### The problem


[Dhana Lakshmi Kunapareddy](https://www.linkedin.com/in/dhana-lakshmi-91aa201b8/) is a consultant at[Infosys](https://www.infosys.com/) who helps clients improve their digital products and experiences. One major retail customer was seeing a high volume of mysterious application errors in their mobile platform. The errors didn’t show up in frontend or backend logs and didn’t seem to stop users from completing purchases. Was it a real bug? Or just a data anomaly?


### The AI workflow


Dhana opened the Amplitude User Profiles page and asked the Amplitude Global Agent one question: “What are ATC errors here?”


From that single prompt, the[Global Agent](https://amplitude.com/global-agent) :


- Identified the existing Add to Cart Error Rates chart and decoded its segment logic
- Surfaced the dominant error message: “Cart validation error.” That message accounted for the vast majority of occurrences in the analysis period
- Ran simultaneous queries on error type, error code, and platform. Every single instance was a Gateway Request Error, isolated to a specific mobile platform
- Pulled a historical trend showing the error was previously rare before increasing significantly during a recent period
- Linked the spike to a recent mobile platform release


When Dhana pushed back and asked, “Are you sure this release caused it?” the agent recalculated by error rate rather than raw counts. That changed the picture entirely. Further analysis showed the issue originated during a recent release cycle rather than the initially suspected release.


[Session Replay](https://amplitude.com/session-replay) analysis and an internal investigation confirmed the root cause: a navigation workflow was incorrectly triggering a validation check under specific conditions.


### The outcome


Dhana’s team deployed a fix based on the AI-guided investigation. Validation errors are now significantly reduced, and the team has a model for AI-guided investigation.


## Second place: Brittany Cole, Algolia


“I had a ready-to-launch experiment in a matter of hours instead of what would have been weeks, maybe months. I was able to share preview links of the working experiment with stakeholders just hours after coming up with the idea.”


Brittany Cole, Website Optimization Manager, Algolia


### The problem


[Algolia](https://www.algolia.com/) ’s marketing team wanted to run an A/B test on their pricing page, but the test required custom JavaScript, CSS, and HTML. With a small dev team focused on higher priorities, Website Optimization Manager[Brittany Cole](https://www.linkedin.com/in/colebrittany/) knew the experiment could take months to ship through their normal process.


### The AI workflow


Brittany saw this as the perfect use case for the new Amplitude[Website Conversion Agent](https://amplitude.com/docs/amplitude-ai/website-conversion-agent) . She described what she wanted to build. The agent:


- Spun up multiple working variants for her to preview
- Iterated with her on appearance and functionality adjustments
- Created and configured the full experiment with two live variants that were ready to launch


The code matched Algolia’s brand guidelines and worked exactly as envisioned.


### The outcome


The experiment delivered. The change drove a 15% increase in their target metric and an estimated six-figure impact on their pipeline. By removing their dependency on developers, Brittany’s team saved months of waiting and now has a repeatable way to run experiments on their own terms.


## Team award: LIFULL Co.


“We’ve significantly reduced our dependency on specific individuals, and the team can now run the growth cycle at a quality level approaching that of our experts.”


Kotaro Inoue, LIFULL HOME’S Business Division, Product Planning Department


### The problem


[LIFULL Co.](https://lifull.com/en/) , the company behind one of Japan’s leading real estate platforms, LIFULL HOME’S, was looking to increase the quality and speed of its digital optimization and experimentation work. Two bottlenecks were slowing them down.


First, non-technical team members, such as designers and marketers, couldn’t review product specs or analyze experiment results without asking an analytics expert. Every request added 90 minutes of overhead. Second, teams were manually building funnel charts, pulling insights, and proposing next steps for every growth initiative, a process that consumed several hours each time.


### The AI workflow


The Product Planning Department, including Yuuki Sasaki, Kotaro Inoue, and Maiko Suzuki, knew Amplitude AI could help solve both problems.


Application Engineering Manager Yuuki built an AI knowledge system by connecting[Amplitude MCP](https://amplitude.com/mcp-server) , Figma MCP, Jira, Confluence, and other tools to Kiro (AWS). He structured product specifications, codebase architecture, and team domain knowledge and passed them to the AI agent as a “package.”


This enables product managers and designers to do all of the following in natural language:


- Investigate specs, including screen structure, business logic, API specs, and navigation flows
- Complete data analysis, including funnels, conversion rate (CVR) comparisons, A/B test results, and pre/post-release comparison
- Diagnose issues, such as root-cause analysis of KPI drops and UX issue discovery
- Evaluate results and create proposals for initiative prioritization and ROI estimation
- Auto-generate outputs based on investigation results for Jira tickets, proposals, and Confluence pages
- Support design work, including interaction review, accessibility, dark mode support, and implementation feasibility assessment


The biggest benefit: no coding required. They just ask in plain language. The team can go from Amplitude data to diagnosis to next action, all in one flow.


Meanwhile, Product Growth Specialist Kotaro used Amplitude AI to automate the full analytical workflow for LIFULL HOME’s growth initiatives: funnel chart creation, insight extraction, bottleneck identification, and next-step generation. He built human review and approval checkpoints into each phase, keeping AI in the driver’s seat without taking humans out of the loop. That balance was key to building team trust and driving adoption.


The team also set up weekly Slack notifications for product performance changes, making it easy for anyone to stay on top of what’s shifting week over week.


### The outcome


The team’s AI initiatives had an immediate, measurable impact.


- Per-initiative investigation time dropped from 90 minutes to around 15 minutes
- Analysis that previously took several hours is now largely automated
- Any team member can now run the full growth cycle independently, at a quality level approaching that of an expert


LIFULL Co. used Amplitude AI to show what’s possible when data belongs to everyone, not just the analysts. By connecting Amplitude AI to the tools their team already works in, they didn’t just save time. They made expertise accessible. That’s the kind of shift that compounds: more experiments, faster decisions, and a team that doesn’t have to wait on anyone to move.


## Write your own AI impact story


Three companies, three problems, one common thread.


Teams using Amplitude AI to change how they work. They move faster now. They make smarter decisions. And data that once lived with a handful of experts is now accessible to everyone.


That’s what good AI workflows do. They raise the floor for what your whole team is capable of.


The workflows from this year’s winners are now part of Amplitude’s[Builder Skills Library](https://amplitude.com/builder-skills) , so you can learn from them, build on them, and make them your own.


Now it’s your turn. Hone your own AI workflows, share them with the[Amplitude Community](https://bit.ly/cohort-community) , and you could be our next AI Impact Award winner.
