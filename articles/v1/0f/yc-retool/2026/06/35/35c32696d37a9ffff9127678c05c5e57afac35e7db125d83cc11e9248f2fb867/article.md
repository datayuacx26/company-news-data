---
schema_version: "1.0.0"
document_id: "35c32696d37a9ffff9127678c05c5e57afac35e7db125d83cc11e9248f2fb867"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/retool-launches-react-ai-app-builder"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-25T01:05:38.302154+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:3e1588b767765973888da0661b28c6e66cef750ae3eb5e248bb5b5985d180144"
---

# Retool launches a full-stack React AI app builder | Retool Blog

01 From low-code to AI explosion02 A new app editor for how people actually work today03 Code tab04 Data tab05 Build anywhere. Ship in Retool.06 One governed runtime for everything07 Deploy with our enabled service partners08 What comes next09 Getting started in Retool’s new app builder


There’s more software being built inside companies today than at any point in history—and the average quality is lower than ever before. LLMs have made it possible for anyone to generate a working app on localhost in minutes. But localhost isn’t production. Most of these apps have no authentication, no data access controls, no audit trail, and no path to deployment that IT has blessed.[The result is shadow IT](https://retool.com/blog/ai-build-vs-buy-report-2026) , but faster, and at 100x the scale.


How do you let people keep building with these tools—without creating ungoverned software at enterprise scale?


We have the answer. Today, we’re launching the new Retool: a platform where you can build software wherever you want: Claude Code, Codex, Replit, Lovable, or Retool’s own editor—and ship it through a single, governed runtime that enforces authentication, permissions, and data governance automatically.


**The new Retool platform:**


- **Run anything.** Deploy any React app on Retool—whether it started as[a prototype in Replit](https://retool.com/competitor/replit) , a Lovable project, or a Claude Code session. On import, Retool maps the app’s data connections to resources your team has already configured and secured. No rewrite required.
- **Governed by default.**[Every app on Retool inherits centralized authentication](https://retool.com/govern-enterprise-apps) , role-based access controls, and data access policies—none of it lives in the app code. Every interaction with your data is auditable. Deploy on Retool Cloud, in your VPC, or fully on-prem. The governance layer is identical everywhere.
- **Built to operate.** Most AI-generated apps get built and forgotten. Retool handles what comes after: versioning, release management, environment promotion, access changes, and monitoring. One place to manage every app, for its entire lifecycle.


**[Try it today and get free hosting through July 1](https://login.retool.com/auth/signup?source=navbarcta) , plus bonus AI credits on every paid plan.**


## From low-code to AI explosion


In the early 2020s,[low-code and no-code platforms](https://retool.com/blog/what-is-low-code) bet that the secret to building software faster was moving away from code. We made that bet, too. And for a long time, it was the right one. Customers built and shipped real internal tools faster than they could have written everything from scratch.


But[AI changed the economics of building](https://retool.com/blog/code-is-free-now-what) . Generating code—written by models that get better every month—is now the fastest way to build custom software, and it’s accessible to a far wider group of people than low-code tooling ever was.


When we started Retool, our proprietary abstractions made building fast. But they also meant LLMs couldn't work in our platform fluently, engineers couldn’t bring their own tools and workflows, and apps built elsewhere couldn’t run on Retool’s platform or inherit its centralized governance.


When the best way to build changed, we changed, too.


## A new app editor for how people actually work today


Retool’s new app builder is the fastest,[most natural way to build internal software](https://retool.com/build-enterprise-apps) —without giving up the control, visibility, and trust teams need once an app connects to real business data.


The new builder is optimized for prompt-first workflows, but it’s also built on a new architecture: apps in Retool are now written in React on the frontend and TypeScript on the backend.


That means the agent is building with the same languages and patterns modern software teams already use. The result is higher-quality app generation, more expressive and customizable UIs, real backend logic, and code your team can inspect, review, and improve. Nothing is trapped in a proprietary format that only exists inside Retool.


We’ve also opened up far more input styles than before. Drag in screenshots, PDFs, design files, or spreadsheets, and the agent works from the same references your team already uses. Builders can pull in every place where work has already happened and hand it to Retool to pick up the torch and carry the project forward.


You can prompt against the whole app, or click into any individual part of the preview to scope a prompt to exactly that component. And when you want to get closer to what’s actually been built, two modes sit alongside the prompt interface.


## Code tab


The Code tab lets you inspect or directly edit the underlying React. Click on any element in the app preview and you're taken to the exact line that needs to change in the code tree, without hunting through hundreds of lines of AI-generated code.


## Data tab


As AI-generated apps have become more common, it’s gotten harder to know what an app is actually doing with your data—and this is where most builders leave you with the least visibility. Retool isn’t just generating your frontend; we’re generating the backend, too. So instead of asking you to read query logic buried in a large codebase, we kept this layer legible by design.


In the new Data tab, every piece of logic interacting with your data sources is created as a function you can open and understand. For more complex functions with conditional logic, there’s also a visual data graph: a diagram that maps the relationships between inputs and data sources, showing you how user actions and app logic translate into operations against your data.


## Build anywhere. Ship in Retool.


The surface area for how apps come into Retool has expanded significantly.


Now, teams can deploy existing React apps directly on Retool’s platform—everything from[prototypes vibe-coded](https://retool.com/blog/vibe-coding-risks) in Replit, Lovable, and v0 to a project from a local repo.


On import, Retool maps the app’s data connections back to resources your team has already defined and secured. Those resources already carry the right data access rules. The app also gets Retool’s app-level permissions, so teams can control who can view, use, and manage it through the same role-based access controls they use everywhere else in Retool.


Instead of asking—or expecting—every team to figure out hosting, authentication, permissions, secrets, auditability, and data access from scratch, Retool becomes a center of excellence you can apply programmatically to every app you deploy.


A few weeks ago, we launched[Retool’s MCP server](https://retool.com/blog/retool-mcp-server) so admins could manage their Retool environments programmatically—querying resources, inviting users, auditing access—from Claude Code, Cursor, or Codex.


Now, we’re adding app building to that set of tools. You can build and edit Retool apps directly from the agent you're already working in, and the project context you have there comes with you instead of getting rebuilt on the other side. And, because Retool can host any existing React code, you can keep building in your favorite CodeGen tools and point them towards Retool when you’re ready to ship.


## One governed runtime for everything


If you’re going to let people build from anywhere (which we think you should!), governance can’t be something each app implements on its own. It has to be a single layer that sits below all of them.


Every interaction with enterprise data goes through Retool’s governance layer. Data access, permissions, approved resources, environments, and deployment rules are configured by the customer and enforced consistently, regardless of how the app was built. The security doesn’t live in the AI-generated layer. It lives underneath it.


Having permissions and security live inside an individual app works when there are a handful of apps and a handful of builders. But it breaks down when software is being created across teams, tools, and skill levels. When every team has its own path to production, technical leaders lose visibility. Retool creates a single governance checkpoint.


This matters for the long tail of software maintenance, too. Most of a piece of software’s life isn’t the initial build—it’s updates, access changes, debugging, monitoring, deprecating, and keeping the app aligned with how the business changes. Retool handles all of that in one place.


## Deploy with our enabled service partners


The more people building software, the more critical it becomes to ensure every app is secure before it reaches production. With Retool, your team has ultimate flexibility in how you build—but you don’t have to figure out deployment alone.


We’ve partnered with leading consultancies, system integrators, and agencies who had early access to the new app builder and completed hands-on enablement to help you ship with confidence:[Artefact](https://www.artefact.com/company/about-us/) ,[BitHippie](https://www.bithippie.com/) ,[BoldTech](https://boldtech.dev/) ,[GxP](https://gxpretool.gxp.jp/) ,[Lingaro](https://lingarogroup.com/) ,[Nymbl](https://www.nymbl.app/) ,[Sabai System](https://www.sabaisystem.com/) ,[SingleWave](https://www.singlewave.tech/) ,[Sixth Generation](https://www.sixthgeneration.io/) ,[StackDrop](https://stackdrop.co/) ,[Stradia Partners](https://stradiapartners.com/) ,[TechRebels](https://techrebels.io/) , and[Paramint](https://paramint.ai/) .


## What comes next


Today’s launch is about the best way to build and ship vibe-coded apps. But this is the beginning of what’s next. As we’ve come to expect, AI-assisted development is already quickly moving forward. We’re seeing agents that act on data, trigger workflows, and call systems without a human in the loop at every step.[The governance problem only gets more urgent](https://retool.com/blog/ai-automation-governance) as AI gets more autonomous. The resources, permissions, and approved pathways you define in Retool now become the harness that makes more autonomous software safe to run.


We’re excited to share more soon.


## Getting started in Retool’s new app builder


**[The new app builder is available now](https://login.retool.com/auth/login?source=navbarcta) for Retool Cloud customers.** Self-hosted customers can access it in the Retool 4.x stable release.


**[Try it today](https://login.retool.com/auth/signup?source=navbarcta) .** New Enterprise plan customers who sign by September 30, 2026 get AI credits **worth up to $10,000 annually.**


[Join the launch webinar](https://events.retool.com/meet-the-new-retool) to see the new builder in action.


**[Check out our latest report, The State of AI Governance in 2026](https://retool.com/blog/ai-governance-report-2026)** for insights from 300+ technical executives facing today’s AI governance challenges.


light Reader
