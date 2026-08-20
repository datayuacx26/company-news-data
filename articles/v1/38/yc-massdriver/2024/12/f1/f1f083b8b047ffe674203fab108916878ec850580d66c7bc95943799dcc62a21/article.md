---
schema_version: "1.0.0"
document_id: "f1f083b8b047ffe674203fab108916878ec850580d66c7bc95943799dcc62a21"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/gitops-is-not-the-right-tool-for-the-job"
published_at: "2024-12-20T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:de95665810ac45ddcda4126250374b530e9b87f5c7ac1ff6d6410933039c4f89"
---

# GitOps is not the right tool for the job

Before we critique GitOps, let’s define the job of Ops, DevOps, and Platform Engineers. There are so many definitions, titles, and misnomers. “DevOps” itself often elicits a cynical roll of the eyes from seasoned engineers who’ve seen buzzwords rise and fall—cue the sourpuss look of hard-earned skepticism.


At its core, the job is about enabling developers; truly enabling them, not just throwing them an obtuse “programming language” and a CLI. It’s about being a steward of the cloud, not a gatekeeper.


As an ops, DevOps, or platform engineer, you’re responsible for translating a tangled web of hyperscaler APIs, configuration files, and compliance mandates into something developers—your customers—can actually use. And not just once, but every day, at scale, and under pressure. It’s a role that demands *continuous improvement* of modern infrastructure, despite the ever-growing complexity of the cloud.


Yet, somewhere along the way, our tooling stagnated. We clung to outdated workflows in a world that had already outpaced them.


The complexity is staggering. It’s not just about hooking up a database anymore; it’s dozens of microservices, data stores, queues, and pipelines, all evolving independently, across globally distributed teams. Your job isn’t spinning up VMs or Kubernetes clusters—it’s understanding the relationships between services and the business context behind them. You’re tasked with keeping it all secure, compliant, and cost-effective, while ensuring developers have frictionless access.


In short, you’re expected to tame chaos while keeping developers productive and happy. Need anything else?


## Why GitOps Fails the Job


GitOps promised to be the answer. But Git isn’t the source of truth for your infrastructure; it’s merely a snapshot of intentions. Production is the real source of truth—unless you’ve implemented real-time drift detection and reconciliation—but wow, what a wrist slap for the end-user.


Even when you implement GitOps, developers still need read access to the cloud console. Why? Because Git can’t provide the real-time visibility and context they need to do their jobs. Despite its flaws, the cloud console reflects the real-time truth of what’s running now—something Git can’t replicate.


Some might argue that GitOps works well for simpler systems or smaller teams, and that’s fair. But as complexity grows, so does the gulf between what Git offers and what your infrastructure requires. For dynamic, large-scale environments, Git simply can’t keep up.


The real question is: why are we still forcing infrastructure into Git when better solutions exist? AWS, for example, doesn’t store **your** configurations in Git—they store it in a database. Why? Because a database allows them to understand and serve their customers better. It provides real-time insights, models complex relationships, and evolves with their systems—just as your infrastructure demands.


When you are using infrastructure as code and you’ve built modules that are shared amongst teams, whether they are highly editable or not, the user of those modules is a user. Why do they have to write code to use something? We've already solved this problem in our industry with building web apps.


Think about it: if someone wants to order a shirt from me, they go to my website and fill out a form. Why? Because it's more convenient for them as users. On my end, that data goes straight into a database, allowing me to understand their buying patterns and preferences. This is the value of putting things in a database—it enables insight, efficiency, and adaptability.


Now imagine asking customers to visit my GitHub repository and fill out their order in a document. You might laugh and say, “That’s absurd.” But it's not as absurd as you think—people used to fill out paper forms manually. We evolved because building user-friendly systems isn’t just convenient for users; it provides us with valuable insights about their behaviors.


So why don’t we, as operations engineers, demand the same value from our systems? And more importantly, why don’t we extend that same level of convenience to our users—the developers? It’s time we held our tooling to the same standards we expect from the rest of the tech stack.


We trust databases to manage complexity in modern applications. Why not trust them for operations? Infrastructure isn’t static—it’s a living, breathing system. Instead of forcing it into tools designed for simpler, static use cases, we need systems that reflect its dynamic nature.


## Patchwork Solutions Make It Worse


When GitOps doesn’t deliver, teams resort to patching together a mess of vendor tools and open-source solutions to make up for its shortcomings. You’re adding monitoring systems here, compliance scanners there, and yet another dashboard for querying state—all to “fill the gaps.” But in doing so, you’ve created the very complexity GitOps was supposed to simplify.


Instead of one source of truth, you’ve multiplied them. Your Git repository reflects intentions, your monitoring tools reflect real-time state, and your dashboards try to tie it all together. The result? A disjointed system where no tool provides a complete picture. You’ve defeated the purpose of GitOps by embracing it.


## The Right Tool for the Job


Your job is to manage a living, breathing system—not a static codebase. The tools you use should reflect that reality. The right tool must let you:


- Track relationships across services.
- Query the current state of your infrastructure instantly.
- Differentiate operational concerns from developer concerns.


With the right tooling, developers could move fast and confidently, trusting that complexity is under control. You, as the steward, would have the visibility to make informed decisions in real time.


GitOps promised to simplify our workflows, but it forced us to adapt to its limitations instead of addressing the root challenges of modern infrastructure.


The developers we enable deserve better tools.
