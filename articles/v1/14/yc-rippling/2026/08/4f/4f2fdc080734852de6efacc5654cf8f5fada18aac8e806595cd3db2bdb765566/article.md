---
schema_version: "1.0.0"
document_id: "4f2fdc080734852de6efacc5654cf8f5fada18aac8e806595cd3db2bdb765566"
company_key: "yc-rippling"
company: "Rippling"
source_id: "yc-rippling-news-import-ac8763083bf2"
canonical_url: "https://www.rippling.com/blog/introducing-ai-spend-console"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-13T20:22:06.226493+00:00"
fetched_at: "2026-08-13T20:22:08.176383+00:00"
content_hash: "sha256:fcb68e2f07b99d9972ad61610eafc75a24a799fd469a6dc8ae0c1519b21271ca"
---

# Introducing Rippling AI Spend Console | Rippling

Today, we’re launching[AI Spend Console](https://www.rippling.com/platform/ai/ai-spend-console?utm_source=blog&utm_medium=hosted-asset&utm_program=evergreen-conversion&utm_campaign=Aug2026_AISpendConsole&utm_product=AI) . It gives CFOs and CTOs a clear view of AI spend, connects it to business outcomes, and governs the use of approved LLMs.


Unlike simple usage dashboards or point solutions, AI Spend Console ties AI spend to employee attributes, so you can understand which departments, teams, or roles are driving your AI bill. It also connects spend to key business metrics like performance ratings or pull request volume, so you can understand where token usage drives improved business outcomes.


Here’s the journey of why we built this product and how we got here.


## AI token spend was growing 80% MoM


Like many tech companies, we were early to AI adoption. We turned on AI tools for all employees, and there were no limits. We encouraged experimentation. We hosted hackweeks, lunch and learns, and “ship shows”.


Very quickly, we saw a freight train of new expenses rolling our way. Our AI token spend was growing 80% MoM, and we were on a path to spend 40% of our R&D headcount budget on tokens.


At first, we didn't have the infrastructure to track, understand, and manage usage patterns and this new expense category. Our Finance team was manually collating data from multiple vendor dashboards and then running ad-hoc analyses. They could see total spend but not which teams or roles drove increases. More importantly, there was no way of connecting AI-related expenses to any measure of business impact.


We couldn’t answer questions like:


-


Which models are used most frequently and by which teams?


-


Which roles and levels are driving up our AI bill?


-


How does AI spend per pull request differ between our top and bottom performers?


-


Which engineers have high AI spend, whose peers frequently ask them to redo work in code reviews?


I'd seen a similar pattern before. In a previous role, I managed AWS infrastructure spend for one of the largest websites in the world. I knew exactly what unchecked cloud costs look like, and I knew this was going to get worse, so I started to tackle this problem in partnership with Catalina Zhao from our BizOps team. We quickly spun up a SWAT team of leaders from every department, including BizOps, HR, Product Sales, Engineering, Marketing, and more, to build our AI programme at Rippling.


## First, we did a “go and see”


One of our core values at Rippling is[Go and See](https://www.rippling.com/careers/life) , so we grabbed time with our engineers, while diving into usage patterns (e.g. daily active users) and data on model costs, vendor spend, and usage patterns.


We worked with a few engineers to scrape this data directly into a centralized data lake, which was entirely manual and extremely time-intensive.


Here’s what we found:


-


**Concentration was extreme.** Roughly 10–15% of our employees were driving about 60% of total AI spend. One engineer was spending $50,000 a month.


-


**Expensive defaults were the norm.** The newest models were set to fast mode. That wasn’t because we made that decision internally but rather because nobody had ever looked into it and set best practices.


-


**The productivity signal was real, but not linear.** Engineers heavily using AI tools like Cursor were shipping more PRs, but the biggest productivity improvements came from initial adoption, not from spending more on frontier models.


-


**Our engineering org needed to evolve.** We saw which engineers were pushing the frontier of innovation with creative AI use cases, where those use cases stayed siloed instead of spreading org-wide, while identifying blockers to broader adoption.


-


**The trajectory was unsustainable.** We built a model to forecast costs and discovered that we were on a path to spend 40% of our R&D headcount budget on AI tokens. The following year, we’d approach 90%.


To help control costs, we went into every AI provider and set a standard dollar amount per tool per month cap. It wasn't a permanent solution, but it gave us time to get our strategy in place.


## Building our AI infrastructure


We needed clear visibility into token consumption, by whom and on what models, and then connect those costs to business metrics like code quality and number of PRs merged.


### Step 1: Getting all the data in one place


We used our Data Cloud infrastructure to pull AI spend directly from our vendors (such as Cursor, OpenAI, and Anthropic) directly into Rippling.


When external data like token consumption gets ingested into Rippling, our software identifies fields that reference users: email addresses, employee IDs, usernames, and display names. It joins them to the corresponding Rippling identity profile, so that business users don’t have to configure those joins themselves. For example, a GitHub user name (coder3) gets mapped to the employee’s work email (adam@examplecompany.com).


With Rippling’s data ingestion and mapping capabilities, we centralized our AI spend and usage data and mapped it to our built-in org context. A GitHub pull request is not treated as a flow row. It’s a record connected to an employee and attributes such as their team, role, or department. That means business data becomes easier to analyze, easier to govern, and easier to act on.


### Step 2: Building dashboards


Now, with our AI spend and usage data in Rippling, we used Rippling AI’s ability to autonomously design and render dashboards based on any question, without any SQL work.


We started building dashboards to view weekly AI spend over time, model usage by teams, spend vs. GitHub PRs, spend vs. code rework, and more across our engineering team.


We created a dashboard for engineering known as the AI Scorecard. It connects data across Cursor, OpenAI suite (Codex), and Claude suite (Code) to highlight four key metrics:


-


**Adoption score (10 points):** An employee’s rate of agentically prompting with a tool every day.


-


**Usage score (10 points):** An employee's depth of engagement with AI tools.


-


**Productivity score (10 points):** An employee’s output (PRs, lines of code).


-


**Cycle time (10 points):** How long do the PRs take to merge?


-


**Efficiency score (7.5 points):** Spend on AI vs. productivity.


All Rippling dashboards automatically obey the permissions inside of Rippling, so users can share dashboards scoped to each viewer. For example, an Engineering manager can see the dashboard as another manager can, but only scoped to their team.


### Step 3: AI Gateway


Observing how much one person spends is only half the battle. Our team also needed a way to control it.


We built Rippling AI Gateway*: an internal routing layer that sits between our employees and approved models. All LLM traffic goes through it. This gives us one place to set and enforce spend limits and model access policies, route AI requests to the right models for the right job, and log every transaction.


It also means that we can implement new models without requiring the organization to retool their entire setup, implement routing logic, and keep security logs without vendor negotiations. Additionally, we can use our AI Gateway in our developer ecosystem to run agentic jobs and orchestrated workflows (such as Slackbots or JIRA ticket triage). Our tooling is no longer locked to any one vendor.


### Step 4: AI captains


Then, we needed to fundamentally shift how we approached AI across the company, not just in R&D.


We stood up an AI Captains program. This is an internal team of Ripplers whose goal is to expand our use of AI intentionally in every Non-R&D org. They are the connective tissue between our company-wide AI direction and the day-to-day reality of each team. They help select the right tools for the right job, automate the highest value workflows, enable employees, and ensure that we link AI use cases to real ROI.


Each captain owns three things for their org:


-


**Impact:** Building and validating AI solutions for their org tied to real ROI.


-


**Adoption:** Enabling their org on the right AI tools.


-


**Governance:** Maintaining platform hygiene, monitoring usage, reviewing and approving skills, and serving as Tier 1 support for AI issues.


20 AI Captains were nominated from across Rippling, and the results were significant. For R&D, we went from a forecast of spending 40% of our headcount budget on tokens to 10 to 15%. That’s tens of millions of dollars a year. And productivity continued to climb because constraints made engineers smarter, not less capable. We saw the same for other teams.


When employees had unlimited spend, they defaulted to the most expensive models to avoid any cognitive load. When they had a budget, they actually learned the tools. They figured out which models were good at what, which configurations were most efficient, which harnesses gave them the best results. The constraint created the innovation.


## AI Spend Console: One place to track, understand, and control your AI spend


This work led us to productize our approach so other companies can see, understand, and control their AI spend too. With[AI Spend Console](https://www.rippling.com/platform/ai/ai-spend-console?utm_source=blog&utm_medium=hosted-asset&utm_program=evergreen-conversion&utm_campaign=Aug2026_AISpendConsole&utm_product=AI) , CFOs and CTOs can get a clear view of AI spend, connect it to business outcomes, and, soon, govern the use of approved LLMs.


Rippling AI generates permissioned dashboards across your connected data. You can even ask follow-up questions in natural language to drill down into spend and usage patterns or customize charts, without any SQL work.


AI Spend Console allows you to:


-


**Identify what’s driving AI spend.** Break down costs by teams, roles, or departments.


-


**Understand the value of spend.** Map AI spend to business metrics like performance ratings or pull request volume, so you can flag inefficient use.


-


**Control AI access and spend*.** Enforce model access policies based on employee attributes, then automatically route AI requests to approved LLMs.


Starting today, you can try[AI Spend Console](https://www.rippling.com/platform/ai/ai-spend-console?utm_source=blog&utm_medium=hosted-asset&utm_program=evergreen-conversion&utm_campaign=Aug2026_AISpendConsole&utm_product=AI) for free with a 30 day trial. No Rippling subscription required.


## A new era for AI token spend


AI isn’t just a line item. It's embedded in how every company works and how every product is built today. Just spending money on AI doesn’t mean anything. What matters is spending it intentionally.


Context is everything. Intentional spend starts with visibility. You need to see all your AI spend in one place, tied to the teams, roles, and departments driving it, and connected to the business outcomes it's producing. Without that org context, spend is just a number. Rippling AI Spend Console gives you all three.


We used it to go from zero visibility and a trajectory toward spending nearly half our R&D headcount budget on tokens, to a governed and measured AI program that’s making Rippling more productive than ever before.


If you’d like to try it out, you can get started[here](https://www.rippling.com/platform/ai/ai-spend-console?utm_source=blog&utm_medium=hosted-asset&utm_program=evergreen-conversion&utm_campaign=Aug2026_AISpendConsole&utm_product=AI) .


Disclaimer


Rippling and its affiliates do not provide tax, accounting, or legal advice. This material has been prepared for informational purposes only, and is not intended to provide or be relied on for tax, accounting, or legal advice. You should consult your own tax, accounting, and legal advisors before engaging in any related activities or transactions.


Join the Rippling AI Gateway waitlist.
