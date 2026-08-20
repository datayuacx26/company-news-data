---
schema_version: "1.0.0"
document_id: "ee67105ad458c377e421870221abc77cae3d75e5a522e0333a680a39dde54fe0"
company_key: "yc-autumn"
company: "Autumn"
source_id: "yc-autumn-news-import-b2979559dee3"
canonical_url: "https://useautumn.com/blog/coding-agents-and-abstractions"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T08:43:21.090032+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:b85f5a15e17b1d83a36f640fd619d230f04e64d1576a0adb0f31c6d0936d50c7"
---

# Making internal agents useful

At Autumn, we experiment a ton with AI to try and speed up internal workflows. A lot of things we build don’t stick, but the ones that do sometimes feel like magic. One interesting pattern I’ve noticed is that these work because they give agents the right abstractions to operate on.


We're sharing what works for us so that others can do similar internally, and also help people build better agent products.


## Automating migration scripts


A good example of this is an internal scripting framework we’ve made a lot of progress on recently. We write a lot of scripts to help our customers perform batch actions and unblock them from any limitations on our API. To give a simple example, we might help with a promotional event by granting a free month to all of our customer’s users. In the beginning, following the mantra of “doing things that don’t scale”, this was done manually — hand written and locally run.


While this worked for a while, as our customers grew and requests got more complex, things started to break. Soon, we were writing scripts that operated over millions of customers, with added complexity around checkpointing, dry runs, retries, and recovery. You can imagine this doesn’t work very well on a local machine. Tools like[Trigger.dev](https://trigger.dev/) helped, and coding agents got really good too, but we still found ourselves spending weeks on these requests instead of building our core product.


Our first attempt at solving this was to build a web app which allowed us to perform the most common operations in bulk for our users. However, this didn’t work well because the types of requests we got varied wildly. Customers wanted things like tier merges or temporary upgrades across their entire user base, and we still found ourselves writing custom scripts for most cases.


## Giving agents the right primitives


We’ve tried automating our scripts with coding agents in the past, but it has never been effective. We tried to steer our agents to think like we do and give them an infinite playground of functions from our core codebase. It was never very effective though, as there were many mistakes being made and we often had to manually intervene.


The difference with our current tool feels like night and day though, and I believe the reason for that is that we invested time in the underlying infrastructure and abstraction which the agent is exposed to. With the scripting framework, we had deterministic yet flexible control (compared to a static UI) over the operations that the agent could perform, the interfaces it would need to learn, and the guard rails around it.


## /write-script


We've built an internal framework for scripting that looks something like Drizzle. The framework essentially built an abstraction layer over:


- Common operations we repeatedly performed in scripts — moving customers between plans, updating credit balances, adding new features, etc.
- Utilities for testing, dry runs, checkpointing, retries, and recovery
- The services these scripts interacted with, like Trigger.dev, Postgres, Stripe, and storage systems


To give you an idea of how this works, here is a basic script that would add a flag to a "Pro" plan, that gives access to "Premium Models"


```text
const   addPremiumModelsScript   =   await   script  ({
id:   "add-premium-models"  ,
org:   ORG  .id,
env: AppEnv.Live,
dryRun:   true  ,
params: {
concurrency:   5  ,
limit:   null   as   number   |   null  ,
},


run  :   async   ({   s  ,   ctx   })   =>   {
const   customers   =   await   s.  run  (loadProCustomers, {});


await   s.  batch  ({
id:   "add-premium-models"  ,
items: customers,
output:   "csv"  ,
checkpoint:   true  ,
concurrency: ctx.params.concurrency,
limit: ctx.params.limit,
fn  :   async   ({   item  :   customer  ,   ctx  ,   s   })   =>   {
const   result   =   await   s.  run  (addPremiumModels, { customer });
ctx.logger.  set  ({ status: result.status, featureId: result.featureId });
return   result;
},
});
},
});
```


We then compiled the knowledge of how to use this framework into a bunch of agent skills, and just like that, we found ourselves one-shotting these scripts with a single` /write-script` prompt to Claude Code. We made scripts both faster to write, and more robust too by building on the right abstractions. We now use this multiple times per week.


Here's our full /write-script skill to give you an idea of the primitives we created:


## /axiom-investigate


Another lifesaving usecase is for our logging infrastructure. Internally, we have a skill called` /axiom-investigate` , which we use to debug customer issues by asking an agent to query and search our logs in Axiom, our logging provider. For instance, when we get a slack message, like this:


> This user subscribed to a plan with the Stripe Link method, but now auto top-ups aren't working for him. Do you know why this is happening?


We can just do this:


```text
please   /axiom-investigate   this   thread   for   the   org   'X':
https://autumnpricing.slack.com/archives/C0ALUV7GGGZ/p1779295652238089
```


It’s hard to overstate how impactful this skill has been. The time it takes to investigate issues has probably dropped 10x. Even our CEO, who had never touched Axiom before, now uses this skill daily.


This skill has been so effective (aside from the fact that we don’t sift through 100s of log lines just for a single issue), is because the results from the agent are highly accurate.


I think the reason this works so well is that we’ve spent a lot of time and effort into ensuring our logs are well structured with the right signals needed to debug issues, making querying and debugging a very repeatable process. For reference, the strategy we adopt is very similar to the concepts mentioned[here](https://loggingsucks.com/) .


We never did this because we knew that we’d be able to use a coding agent over it — this was well before Opus 4.5 came out and long running tasks were the norm. It was simply because logical errors are more common as a billing company. However, because our logs are so structured, our agent has all the right primitives to deliver quick and accurate investigations.


Here's the full skill:


## Final thoughts


Incidentally, Autumn itself happens to fall into this category of tools: systems that create this abstraction layer, which I believe is best described as an interface which allows humans and agents to work together more effectively.


It’s an abstraction layer representing your application's pricing and billing state, and we’ve spent a lot of time designing it to be flexible, but intuitive. As a result, coding agents interact with billing systems much more effectively.


For instance, sales teams can update deals, grant entitlements, or manage customer billing schedules directly from Slack. GTM teams can make entire pricing changes and migrations without ever involving engineering.


This wasn’t something we thought about when we first started Autumn, but it’s definitely why we’re more bullish than ever on what we’re building.
