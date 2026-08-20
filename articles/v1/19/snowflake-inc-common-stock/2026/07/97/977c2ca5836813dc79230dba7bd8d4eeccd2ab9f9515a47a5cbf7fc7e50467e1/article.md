---
schema_version: "1.0.0"
document_id: "977c2ca5836813dc79230dba7bd8d4eeccd2ab9f9515a47a5cbf7fc7e50467e1"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/snowflake-coco-built-to-scale-enterprise-ai"
published_at: "2026-07-21T16:00:00+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:0790eba07ac446fc203cc75152adc1edee97450a2286536ef032a5a0f4440a77"
---

# Snowflake CoCo: Built to Scale Enterprise AI with Trust

AI is evolving from an individual productivity tool into an enterprise platform, and the opportunity ahead is enormous. But scaling AI across an organization raises challenges that individual AI tools were never designed to solve.


Finance teams need visibility into AI spending before it becomes a budget surprise. Platform teams need confidence that AI operates within existing governance policies. Developers need AI agents that work with their existing tools and workflows. Together, these raise questions of trust, governance and cost.


Today's Snowflake CoCo announcements are built around that reality. For builders, CoCo Desktop is now generally available on macOS and Windows, and Cloud Agents brings the full power of CoCo to Snowsight without a local install. For platform teams and admins, new AI cost governance capabilities give real-time visibility into AI spending with per-user controls to manage it and Agent Settings lets admins configure the CoCo experience once and roll it out across their teams. And for wherever work happens, CoCo Mobile (iOS and Android) is now in private preview, extending trusted AI into devices your teams already use.


Together, these announcements make it easier to govern AI costs, ground AI in your enterprise context and bring trusted AI into the places where work already happens.


## Available today with Snowflake CoCo:


- AI cost governance (per-user quotas and quota enforcements) : Now in public preview
- Agent Settings, including default model picker : Now in public preview
- CoCo Desktop : Generally available today for macOS and Windows
- Cloud Agents : Generally available today in Snowsight, public preview soon via the Cortex Agents REST API
- CoCo Mobile app for Android and iOS : Now in private preview
- Skills and Plugins Sharing : Public preview soon


## Governing AI begins with knowing what it costs


Every major technology platform reaches a point where governance becomes as important as innovation, and AI is now at that stage. As organizations scale AI adoption, they need visibility into AI usage, predictable costs, and the right guardrails to enable innovation without financial surprises. That's why AI cost governance has become foundational to enterprise AI.


That's why AI cost governance is foundational to CoCo rather than an administrative feature added after deployment. The same role-based access controls that already govern your Snowflake environment govern CoCo, no new security model to configure, no parallel identity layer to manage. From there, our approach extends that foundation with three principles that mirror how organizations already manage every other strategic technology investment.


- **Visibility:** Administrators need to understand how AI is being used across users, teams and workloads while adoption is happening, not weeks later when invoices arrive.
- **Optimization:** By using Snowflake's[native tagging framework](https://docs.snowflake.com/en/user-guide/budgets/per-user-quotas#user-selection) , organizations can attribute AI consumption to teams, cost centers, or business units, making chargeback and showback significantly more straightforward, while new default model settings help balance capability and cost without requiring every individual developer to make those decisions manually.
- **Control:** Per-user quotas and automated notifications, both new capabilities launching with this release, allow administrators to establish guardrails before costs become surprises, so developers continue building freely while platform and finance teams gain confidence that adoption remains predictable as it scales.


*Fig 1: Preventing budget surprises by setting up CoCo per-user quotas in Snowsight*


**More done per dollar, same high quality bar.** On Snowflake's own heavy internal CoCo CLI usage, we recently **cut cost per prompt by ~28%** for the same model on the same tasks, with no quality degradation. These gains come from a series of efficiency improvements: Read-Eval-Print Loop (REPL) based tool calling, more compact (or even offloaded) bash and SQL tool output to keep context lean, loading skills and less-common tools only when they're needed, lighter-weight subagents, and smarter model routing in general. Across customer CoCo usage we're seeing the same directional shift - average cost per prompt is down ~20% in recent weeks, thanks to these efficiency improvements being rolled out. We're always investing in token efficiency and faster task completion with a high quality bar so enterprises can scale CoCo without linearly scaling costs. (Details of this set of cost efficiency engineering will be shared in a followup engineering post.)


AI usage for organizations now can be on the same foundation of trust and governance that they have built with Snowflake.


**Agent Settings (now in Public Preview)** extends that governance framework directly to the developer experience. Admins can create a single configuration and roll it out across their users. For this release, we are supporting default models, which is a top request to help admins choose the best model that they want to use across their users. Going forward, this will be the way to support preinstalled skills and other tools their teams use, and apply it across every user without requiring any individual setup. Because Agent Settings is backed by the same role-based access controls that govern the rest of your Snowflake environment, different teams can get different configurations based on their roles.


## Enterprise AI should work wherever your teams work


Building on the strong reception of CoCo in CLI and Snowsight, today we're delivering on one of the most requested customer asks: a native desktop experience. **CoCo Desktop,** now generally available for macOS and Windows, is Snowflake's most complete AI development environment. As a native desktop application, it gives developers a dedicated workspace with integrated access to local files, repositories, terminals, and Snowflake, all grounded in deep, native awareness of their Snowflake environment. With built-in agentic browser and notebook experiences, CoCo Desktop is purpose-built for modern data development. Unlike general-purpose AI coding tools, it comes with your Snowflake environment already loaded, no cold starts, no re-explaining your data model, no governance gaps.


Consider a data engineer onboarding to a new project. With a general-purpose AI tool, the first hour is setup: explaining schemas, re-establishing table relationships, and re-loading governance policies. With CoCo Desktop, now in GA, that context is already there. They open the application, their Snowflake catalog is loaded, their RBAC roles are active, and they're writing a pipeline within minutes of first launch. That's not a configuration step. It's the default experience.


*Fig 2: Accelerating intelligent, governed app development with Snowflake-native context in CoCo Desktop*


**Cloud Agents (generally available)** are now generally available and are the standard CoCo experience in Snowsight. Every CoCo session in Snowsight now runs inside an isolated, Snowflake-managed container, giving every user web search, shell command execution, and full Python script execution by default. No local installation required. Same security model, same governance, no new setup.


In practice, this means you can now do in your browser what previously required a local install. Search the web as part of a task, run a shell script, execute a Python file, install a dependency, all without leaving Snowsight. If you have been using CoCo in the browser and felt limited compared to the CLI experience, that gap is now closed. For teams building their own applications, CoCo's coding capabilities will also be available via the[Cortex Agents REST API in public preview soon](https://www.snowflake.com/en/blog/snowflake-cortex-agents-enterprise-ai-scale/) .


**CoCo Mobile, in private preview** , extends this to wherever work takes place. The interface changes to match where people work. The governance model remains exactly the same.


CoCo meets you where you work.


## From individual workflows to shared team knowledge


Data teams face two versions of the context tax. The first is the per-session overhead every builder knows well: explaining schemas, re-establishing table relationships, and reloading governance policies before getting to the actual work. CoCo reduces that friction significantly, because it's built for Snowflake, your catalog, governance policies, and semantic models are already loaded from the first prompt.


The second version is harder to see but just as costly: institutional knowledge that never leaves the workspace where it was created. A developer figures out the optimal way to structure a recurring pipeline. Another builds a skill that automates a tedious but important task. That knowledge exists, but it stays siloed, and the next person who needs it starts from scratch.


*Fig 3: Skills and plugins are now shareable. Browse your organization's catalog, certify trusted workflows, and invoke them directly in Cortex Code.*


Teams can also package institutional knowledge into shareable skills and plugins (PUPR Soon) through a governed catalog. Instead of valuable workflows remaining isolated in individual workspaces, organizations can publish, discover, and securely share them across teams. Built-in access controls and optional certification help ensure that reusable knowledge remains governed as it scales.


## The next chapter of enterprise AI


With CoCo Desktop and Cloud Agents generally available, AI cost governance and Agent Settings in public preview, and Skills and Plugins sharing in public preview soon, these capabilities are built to help enterprises scale AI from individual use to organization-wide adoption.


Developers gain an AI platform that starts with the context they already work in. Platform teams gain the governance and configuration controls to deploy responsibly across every user. Finance leaders gain the visibility to manage AI as a strategic investment rather than an unpredictable expense. And as teams build and share proven workflows through a catalog of skills, the value of CoCo compounds across the organization over time.


Enterprise AI shouldn't force organizations to choose between innovation and governance. CoCo exists so they never have to make that choice.


## Get started


- [Download CoCo Desktop for macOS or Windows](https://www.snowflake.com/en/product/snowflake-coco/downloads/) .
- [Watch this video](http://youtube.com/watch?v=hDYIq5hbU-g&feature=youtu.be) to get started and[connect to your Snowflake environment in under 5 minutes](https://www.snowflake.com/en/developers/guides/getting-started-with-coco-desktop/) .
- Set up your first per-user AI cost quota using our[documentation page.](https://docs.snowflake.cn/en/user-guide/budgets/per-user-quotas)
