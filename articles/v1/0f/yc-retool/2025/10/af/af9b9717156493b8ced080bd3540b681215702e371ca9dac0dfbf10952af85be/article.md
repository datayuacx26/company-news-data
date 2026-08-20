---
schema_version: "1.0.0"
document_id: "af9b9717156493b8ced080bd3540b681215702e371ca9dac0dfbf10952af85be"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/how-we-built-ai-assisted-development"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:98267a31609547fd9d738e28bc0633d157a93e703d456a9dca7a5417ddaa5a61"
---

# How we built AI-assisted development into Retool

01 The three principles of AI-assisted development in Retool02 1. Collaborative builder, not just more code, faster03 2. Production quality by default04 3. Design for mixed modes05 Making apps LLM-readable06 Managing context efficiently07 Building safe editing controls08 Handling data connections securely09 Protecting production data10 Lessons from the build


Earlier this week, we introduced a[new mode of building in Retool](https://retool.com/blog/introducing-enterprise-appgen) . Our enterprise AppGen platform changes[who gets to build software in the era of AI](https://retool.com/blog/2025-builder-report) . Domain experts join engineers as builders, capable of turning any idea into a solution, built on your data, deployed in your environment, and governed by your enterprise standards.


Today, we want to go under the hood and share how we've built AI capabilities that truly understand your existing apps: how your apps become LLM-readable; how the platform manages context to understand exactly what you're building; how you maintain control over every edit; and how your data connections remain secure throughout the entire process.


Our goal is clear: allow customers to securely expand their bench of builders, build solutions as unique as their problems, and operate at the speed of change.


## The three principles of AI-assisted development in Retool


Before writing any code, we locked in three principles. These would guide every decision:


## 1. Collaborative builder, not just more code, faster


Our AI builds *with* users, not for them. Why? Because our users aren't building casual prototypes, they're creating mission-critical apps. The kind that[run businesses](https://retool.com/customers) . Getting to production quality requires understanding real requirements, workflows, and edge cases. That only happens through collaboration.


We keep users in the loop at every step: explaining reasoning, surfacing decisions, and creating teaching moments.


## 2. Production quality by default


Every generated app automatically inherits[enterprise security](https://retool.com/enterprise) , SSO, RBAC, and data-level permissions. The code follows best practices. It handles errors correctly. It's maintainable.


## 3. Design for mixed modes


Modern builders recognize that flowing between chat, canvas, and code is the most efficient way to work. So, every piece of the generated app must be human-readable, human-editable, and human-maintainable. This builds sustainable patterns that work, no matter how developers choose to work.


## Making apps LLM-readable


How does an LLM understand an app?


Using[ToolScript](https://docs.retool.com/source-control/concepts/toolscript) , the format we use for serializing apps and source control. It represents apps like a file system, with a JSX-style markup structure that maps cleanly to app elements. This was perfect for LLMs because LLMs understand code. And ToolScript looks enough like code that models can parse it naturally. ToolScript represents components, queries, and layout in a nested, file-like structure that models can traverse.


ToolScript's structure made it ideal for AI comprehension. Components nest naturally—containers have Header, Footer, and Body elements with child components inside. Tables have nested Columns. Select dropdowns have nested Options. The ordering follows visual logic: top to bottom, left to right on the canvas. When you move a button above another, its position in the ToolScript file changes too.


The file structure separates logic (e.g.,` main.rsx` ,` functions.rsx` , and` lib/` ) from positioning (JSON dotfiles), which clarifies what’s being edited. This separation means the LLM can focus on logic in` main.rsx` and` functions.rsx` without getting bogged down in positioning coordinates.


Instead of inventing some new representation, we gave the LLM the same format we already trusted for version control. It could read the entire structure of an app, components, queries, transformers, everything, in a format it inherently understood.


But reading is only half the story. ToolScript also lets builders target specific components and properties, and add or update queries in place. A query can reference` lib/` files via` {include("./lib/query1.sql", "string")}` , so logic can live alongside, but separate from, component structure.


## Managing context efficiently


An LLM without context is just guessing (and LLMs love guessing). With the proper context, it can make intelligent edits. The challenge is giving it exactly what it needs without overwhelming it with irrelevant information. We have a team of engineers on precisely this problem, so we can scale to arbitrarily large workspaces (with many connected resources) and arbitrarily large resources (with hundreds or thousands of schemas for the LLM to parse through).


Before generating anything, we use agentic tools to understand what exists in your workspace, such as the[components](https://docs.retool.com/apps/concepts/components/) ,[queries](https://docs.retool.com/queries/) , and[database schema](https://docs.retool.com/queries/guides/sql/database-schema) . This inspection happens on demand, pulling only what's relevant. If you're building a form, we fetch form-related components and patterns. If you're working with a specific table, we pull just that table's schema. This keeps the context focused and token usage efficient.


Users can control the scope through several mechanisms:


- Click on a component to select it
- @mention a query or resource to bring it into context
- Highlight a section to focus changes there


Context stays current. We re-inspect before each action to always work with the latest state of your app, to immediately pick up changes like adding a query or moving a component.


If your organization has shared components, queries, or common patterns, we detect and use them. When a data table is needed, your organization's standard table component is used. When user data needs to be fetched, your existing queries are referenced. This keeps the generated app consistent with what your team has already built.


## Building safe editing controls


Letting an AI modify production apps requires serious safeguards.


Before we make any change, we create a checkpoint of your entire app state: components, queries, positions, *everything* . This happens automatically. Changes are visually indicated and you can review each change individually:


- Approve a query save
- Accept a query modification
- Adjust this component's position


Each AI-generated change is tagged in your app's version history, separate from human edits. This helps with compliance auditing, but it also lets you understand how AI approaches problems and learn from its solutions. You're driving the development, with AI assisting along the way.


## Handling data connections securely


Real apps must connect to real business data. So AI in Retool has to handle data connections from day one.


First, we detect what resources are needed from context. When you ask to "build a customer dashboard," we look for customer-related tables in your connected databases. When you say "add order history," we search for existing order queries—always starting with what's already connected before trying to create something new.


If a needed resource isn't found, there is a decision tree for what happens next:


- For existing databases: guidance to connect the specific table or write the query
- For missing connections: a prompt to add the data source first
- For prototyping: safe mock data is created that matches the expected schema


Permissions work by inheriting your access. If you can't access production customer data, you can’t through AI, either. SSO, role-based access control, and row-level security are automatically respected. No special configuration needed. This means different users get different experiences based on their permissions. A support rep building a dashboard sees only their assigned customers. An admin sees everything. The new app is generated with the same structure but connected to different data scopes.


The LLM never sees sensitive data. It works with schemas, column names, and data types, not the records themselves. When it needs to understand a table structure, it gets metadata like "customers table with columns: id, name, email, created_at" rather than actual customer records. This keeps your data secure while still allowing intelligent app generation.


## Protecting production data


Production apps touch production data. One bad query could corrupt records or expose sensitive information. We built multiple safeguards to prevent these scenarios.


As with all aspects of Retool, queries get parameterized and sanitized automatically. When a query like` SELECT * FROM users WHERE id = {{table1.selectedRow.id}}` is generated, the platform handles SQL injection prevention. And nothing executes without human review. Even if you ask to "delete all inactive users," the query is generated, but you trigger the execution. The builder is always in control, and this human-in-the-loop approach catches both AI mistakes and unclear requirements.


These safety measures work together to make Retool trustworthy for business-critical applications. The cost of a mistake is too high to leave things to chance.


## Lessons from the build


- **Good defaults matter more than customization options** . Most users just wanted something that worked and looked professional, not control over every detail. So we built in sensible defaults—proper spacing, automatic component connections, familiar patterns. Users can still customize everything, but they rarely need to.
- **ToolScript is ideal** . Its JSX-like structure gave the LLM something familiar while maintaining fidelity to Retool's app structure. Using our existing format accelerated development significantly.
- **Human-in-the-loop is essential** . We worried that requiring approval would slow things down, but the opposite happened. Users felt confident experimenting because they could reject bad suggestions, and the approval step became a teaching moment.
- **Context window management is an ongoing battle** . Even with smart fetching and token optimization, complex apps push limits. The difference between a confused response and a perfect edit often comes down to having the proper context at the right time.
- **Security can't be bolted on later** . Building in SSO, RBAC, and audit logging from the start shaped every architectural decision. For any AI tool targeting business users, security and governance need to be foundational, not additions.


Retool allows you to generate apps that connect to your real data sources from day one. Your deployment stays in your preferred cloud, under your control. No migration, no compromise. Build where you already work.


If you're ready to turn your ideas into apps on your data, in your cloud, and secure by default,[try it for free today](https://login.retool.com/auth/signup) and let us know what you create.


light Reader
