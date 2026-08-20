---
schema_version: "1.0.0"
document_id: "88b81a4aa59a93f05798ece37bb45bf7277a9131d0f62ac54d8a7fb604c4eee7"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/golden-paths-werent-built-for-agents-part-1"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:98710ca4b48e0fcb63dbe6c1048396e1c14e686aa6b9301ed276f373f3d815b9"
---

# Your Golden Paths Weren't Built for Agents - Part 1: How Agentic Development Breaks Control

Everyone's talking about the blast radius of agents. Put them in a container. Put them in a sandbox. Scope their credentials. Give them their own cloud account so they can't reach anything that matters.


It's good advice. We give a version of it[ourselves](https://www.massdriver.cloud/blogs/development-environments-for-agentic-infrastructure) . "Put them in a sandbox" … it's a good *start.*


But containing where an agent can act and governing what it's allowed to do are two different problems, and the second one is where you see agentic development start to break developer self-service and infrastructure orchestration. You can put an agent in the most isolated cloud account in the world. The moment it needs to do real work inside that account, it runs into a permission model that was broken years before anyone said the word "agentic."


This is the first in a short series on what agentic development breaks in DevOps. I want to start with control, because it's the one most teams are *sure* they've already handled.


## Self-service was never self-service


To manage a resource, you have to be granted access to that resource. For that to happen, the resource has to exist. For it to exist, someone with more privileges than you had to create it first. So either you hand developers the ability to create resources freely and you've lost any real control, or you gate creation behind an operator and call whatever's left "self-service."


It was never self-service. Permission models built around the identity of a resource undermine self-service by definition. It's resource-provenance hell or ticket ops.


Take a common day-two task that's inaccessible to most "self-service" pipelines. A developer owns a Postgres database in production. Postgres 14 is going EOL. The upgrade for a major version isn't a setting in a dropdown; you stand up a new instance on the new version, cut over, migrate the data, and decommission the old one. You build it, you run it. That's the promise.


Except to stand up that new instance, the developer needs permission to create a database. Not manage the one they already own, the new one. The one that doesn't exist yet. Of the big three, only AWS can really gate this by attribute, and only if every resource is tagged perfectly the moment it's born and you've locked down who can change tags; GCP and Azure can't condition the create on a tag the resource doesn't have yet, so you fall back to a project- or subscription-level grant. The grant model assumes the resource comes before the permission, and a day-two task like a database migration inverts that order every time.


So the developer files a ticket. An operator with elevated credentials creates the instance. The operator grants access. The developer finishes the migration they were perfectly capable of running themselves. You built a self-service platform and the most routine day-two operation in existence still detours through a human.


Kubernetes has the same problem. A RoleBinding ties a subject to a Role inside a namespace, which is fine until the workflow needs to create the namespace. The RoleBinding can't reference a namespace that doesn't exist yet, same as the IAM policy can't reference the database that doesn't exist yet. So you either hand out cluster-scoped create permissions, pre-provision every namespace anyone might need, or file a ticket. Same root cause: the permission is pinned to something that has to exist before the permission can mean anything.


## We've just never felt how broken it is


Every platform engineer reading this has lived some version of this and shrugged, because it's always been survivable.


It's survivable because humans are slow. The ticket-to-ops detour for a Postgres major version stings, but you do it maybe twice a year. The orphaned databases nobody can account for pile up over years, slowly enough that you keep telling yourself you'll clean them up next quarter. The permission model leaks the whole time. The leak is just slow enough to mop up the tedious churn on IAM definitions.


Agents are not slow.


When developers are shipping 10x faster, the cloud underneath them changes 10x faster too. Every migration, every version bump, every "I finished the roadmap so I'm finally going to upgrade Postgres" task that used to sit in a backlog for two years starts coming down the pipeline. And a lot of that deferred work is exactly the create-migrate-destroy churn that the identity-based model handles worst. "We're breaking up the monolith, we need to create 30 new queues and mysql instances!"


A permission model pinned to specific resource IDs doesn't slow this down. It breaks, because the resources it's pinned to are being created, migrated, and recreated constantly, and every recreation orphans the grant that referenced the old ID.


## The unit was wrong the whole time


The fix is not a bigger pile of grants. It's changing what a permission is about.


The identity of a resource is the wrong unit. It's been the wrong unit for the entire cloud era, and the change rate of agents elevates it from an infrequent paper cut to a shiatsu from Edward Scissorhands. What matters is the class of the resource.


"A developer can create and manage non-production databases." "A developer can read and tune config on a production database, but never drop one." "An agent can manage the full lifecycle of a development environment, but only read production." Those are rules about kinds of things. None of them name a resource ID. Every one keeps holding when the underlying resource is destroyed and recreated, because they were never attached to the resource. They're attached to the resource's classification.


That expressiveness is what any SaaS platform will need to call itself AI-native. Without it, governance falls back on a human in the loop, approving the creates, granting access to the resources that didn't exist a moment ago, and that human is exactly the bottleneck self-service was supposed to remove. Whether you get there with attribute-based access control, a relationship-based model, or something you grow yourself is a detail. The principle is the point: express what can act on what kind of thing, not which specific IDs someone got handed in a ticket six months ago.


## How we're approaching it


Massdriver is the control plane every infrastructure change runs through, so its permission model sits above the tools and clouds being governed instead of inside each one. That distinction matters. AWS has ABAC via IAM conditions, but Snowflake doesn't. The next vendor in your stack has some third dialect, or nothing. As agents start acting across every cloud and every SaaS service you run, you can't wait for each of those vendors to ship a permission model good enough to govern an agent. Most never will. So the gate can't live in any one of them. It lives in one place, above all of it.


The model itself is[attribute-based](https://docs.massdriver.cloud/platform-operations/security/access-control) , so you write rules against the kind of a resource rather than its ID. It's hierarchical, so those rules cascade down through projects, environments, and IaC components instead of being restated at every level. And it's extendable with your own domain language, so you can define entire classes of resources that people or agents are allowed to manage.


The postgres migration from earlier — stand up a new instance, cut over, kill the old one — is something a developer or their agent runs end to end inside a lane you defined once, because the permission is expressed against the kind of resource, not a list of IDs anyone maintains by hand. The compliant path stops being the gated path and becomes the easy one.


## Control is the first problem


Identity-based permissions break self-service. We've tolerated it for twenty years because the breakage was slow enough to cover with a ticket queue. Agents remove that. A platform that still routes resource creation through a human, while developers and their agents move an order of magnitude faster, becomes the thing everyone learns to route around.


We've spent the better part of two decades letting DevOps flounder. Meatgates at every stage of the DevOps infinity loop, humans in the loop where there should have been automation. In an AI-native world, it risks calcifying into the silos we set out to tear down.


Get the permission model right and the agent has a real lane to work in. Get it wrong and the sandboxing doesn't save you, because the agent still has to come back and tap a human on the shoulder for the one thing it needed to do.


Control is the first of the things agentic development breaks. Comprehension, context, and the human cost of reviewing everything these agents produce are next.


---


*The next three parts cover comprehension, context, and the human cost of agentic review. Subscribe below to get them as they're published, or if you'd rather see how we're solving control today,[let's talk](https://www.massdriver.cloud/demo#schedule-a-consultation) .*
