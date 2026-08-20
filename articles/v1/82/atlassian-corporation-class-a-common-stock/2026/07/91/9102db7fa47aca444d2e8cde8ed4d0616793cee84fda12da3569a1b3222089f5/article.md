---
schema_version: "1.0.0"
document_id: "9102db7fa47aca444d2e8cde8ed4d0616793cee84fda12da3569a1b3222089f5"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/ai-at-work/rovo-github-software-teams"
published_at: "2026-07-16T21:40:28+00:00"
first_seen_at: "2026-08-11T20:27:01.399838+00:00"
fetched_at: "2026-08-11T20:27:01.894968+00:00"
content_hash: "sha256:b5742f670cd114abc7191b507149f9f1647e2bb877f33b4dfa52ae9ec971c351"
---

# Bring GitHub context into Rovo with the Teamwork Graph connector for GitHub

## GitHub in Rovo for software teams


AI is making it faster to write code. The new bottleneck is understanding the work around the code: what changed, why it changed, who reviewed it, and how it connects to the plan.


That context lives across Jira, GitHub, Confluence, and the conversations that happen around them. Teamwork Graph helps bring it together. That connection is powered by the Teamwork Graph connector for GitHub, which helps make eligible GitHub data available in Atlassian experiences through Teamwork Graph.


For software teams, GitHub is where implementation takes shape. Pull requests show what changed. Issues capture technical tasks and decisions. Commits and branches show how work moved. Review comments explain the tradeoffs behind the final code.


For engineering leaders and software team leads, that context matters because delivery is not only about writing code. It’s about understanding progress, unblocking review, reducing handoff cost, and keeping the plan connected to your teams’ work.


## The opportunity: connected software context


The process of building software work creates useful signals everywhere: a work item in Jira, decisions in pull request comments in GitHub, design content in Figma or documentation in Confluence.


When that context is connected, teams can move faster with less manual reconstruction:


- **Planning gets clearer** because teams can see what already happened
- **Reviews move faster** because teams can find the surrounding work context
- **Status reporting gets easier** because the progress from planning and code tools can be understood together
- **AI Agents become more useful** because they can reason with more of the work record around the code


Rovo is built for that context problem. With GitHub connected to Teamwork Graph, Rovo can help teams find and use GitHub context from inside Atlassian.


## Why this is different


GitHub, Jira, and AI coding agents each solve important parts of the software delivery workflow. The gap is that each sees only part of the system.


Approach What it is good at Where teams still get stuck


GitHub alone Code, pull requests, branches, commits, and review workflows. It does not hold the full teamwork record across planning, decisions, docs, and delivery status.


Jira alone Planning, ownership, work tracking, and delivery coordination. It can miss the code-level context and review detail that explains how work is progressing.


Code-only agents Helping developers write, explain, or change code. They often lack the work record, team context, and outcome loop around the code.


Rovo with GitHub context Connecting software work across Teamwork Graph so people and agents can reason over more of the delivery context. Your teams need GitHub connected and available in the right Atlassian experiences.


The goal is not to replace where developers work. The goal is to connect the places where software work happens so teams can understand and act faster.


## How this relates to other GitHub + Atlassian experiences


Atlassian and GitHub work together in several ways. This article focuses on the **Teamwork Graph connector for GitHub** : bringing GitHub context into Teamwork Graph so Rovo can help teams find, understand, and act on software work across Atlassian.


If you want to… Use… What it helps with


See branches, commits, pull requests, and builds on Jira issues **[GitHub for Atlassian](https://marketplace.atlassian.com/apps/1219592/github-for-atlassian)** Tracking code progress directly from Jira work items.


Search and reason over GitHub context in Atlassian **[GitHub in Rovo](https://www.atlassian.com/software/rovo/connectors/github)** Finding repositories, pull requests, issues, commits, branches, and comments alongside Jira, Confluence, and other teamwork context.


Bring Jira and Confluence context into GitHub Copilot Chat **[Rovo for GitHub Copilot](https://www.atlassian.com/blog/development/atlassian-developer-innovation-rovo-for-github-copilot)** Helping developers use Atlassian context from their IDE.


Assign Jira work to a coding agent **[GitHub Copilot coding agent in Jira](https://marketplace.atlassian.com/apps/1582455624/github-copilot-for-jira)** Delegating implementation work from a Jira issue to an agent that can produce a pull request.


## Three jobs Rovo can help with


Once GitHub context is connected to the Teamwork Graph, Rovo can do more than search code. It can help teams understand how implementation work connects to Jira statuses, Confluence plans, support readiness, and customer impact.


These examples use a fictitious financial-services customer, Beaconstone and their launch of a new product called Titan, to show the kind of questions a software team can ask from inside Atlassian.


### 1. Spot work that needs attention


Sprint health is hard to judge from Jira or GitHub alone. A Jira issue can sit in the wrong status while the code has moved, or an implementation can stop moving while the plan still looks active. Rovo helps reconcile those signals so teams can see what needs attention.


In this example, Rovo checks the current Titan sprint against recent GitHub activity. It finds that the sprint items all have recent PR activity, then flags the real issue: Jira still shows the items as *To Do* , even though one item already has merged work.


Query:` What's stuck in our sprint with no recent commits?` Rovo compares current sprint work in Jira with recent GitHub PR activity, then highlights where the real follow-up is Jira status hygiene rather than missing code.


### 2. Catch up on what shipped


Weekly status updates are often spread across merged pull requests, still-open follow-ups, Jira tickets, and team handoff notes. Rovo can summarize that work without forcing the reader to inspect each PR or reconstruct the launch context manually.


Here, Rovo separates what shipped from what is still open. It identifies the ‘signed replay receipt’ and ‘onboarding checklist’ as shipped work, then keeps the ‘remaining replay validation’ and ‘operational follow-ups’ visible.


Query:` Catch me up on what shipped this week for Titan Rovo` pulls together merged PRs, current Jira work, and launch-readiness context so the team can see both shipped work and remaining follow-ups.


### 3. Prioritize reviews using Teamwork Graph context


GitHub can show which pull requests are open. Teamwork Graph helps explain which ones matter most. By connecting PRs to sprint commitments, Jira blockers, Confluence plans, and customer-readiness signals, Rovo can help reviewers decide where to spend attention first.


In this example, Rovo prioritizes review work by impact. It puts ‘replay validation evidence’ first because it unblocks launch readiness, then ‘customer onboarding readiness’, then ‘operational follow-ups’.


Query:` Which PRs are important for Titan this week?` Rovo uses Jira, Confluence, and GitHub context to explain why some open PRs need review before others.


## Available today


Today, eligible GitHub for Atlassian connections can bring GitHub context into Atlassian experiences powered by Rovo. That means teams can use GitHub work alongside Jira, Confluence, and other teamwork context instead of treating implementation detail as a separate island.


For teams, this means GitHub work can become easier to find from Atlassian. For leaders, it means planning and delivery conversations can connect more directly to implementation context.


## Where this is headed


This is also part of a bigger shift in software delivery.


AI-assisted coding is becoming normal. The next frontier is not only generating more code. It is helping teams coordinate the software delivery lifecycle with better context: planning, implementation, review, status, and learning from what shipped.


Atlassian’s advantage is the teamwork system around software delivery. Jira holds the work record. Confluence holds the knowledge. GitHub holds the implementation detail. Teamwork Graph connects the context. Rovo gives people and agents a way to use it.


GitHub in Rovo is an important step toward that future. It brings high-value software delivery context into the flow of teamwork so teams can plan, build, review, and ship with more shared understanding.


## Get started


To use GitHub context in Rovo, please follow the[instructions here](https://support.atlassian.com/organization-administration/docs/connect-github-to-teamwork-graph/) .
