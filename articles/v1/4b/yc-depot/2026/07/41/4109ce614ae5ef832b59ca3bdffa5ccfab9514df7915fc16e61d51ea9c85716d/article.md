---
schema_version: "1.0.0"
document_id: "4109ce614ae5ef832b59ca3bdffa5ccfab9514df7915fc16e61d51ea9c85716d"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-depot-code-beta"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b2d4ee278a039423b1dda66822d8184457e032109f0bc95bbcc1f10ce8fd4fd4"
---

# Now available: Depot Code in private beta

Today, we're announcing that Depot Code, our own **source control hosting** , is now in private beta!


We're excited to get this into the hands of folks who are pushing the limits of what is possible today with git.


Fill out our two-question form to get Depot Code for your organization.


Request access


## Why use Depot Code?


Depot Code is a highly scalable and more performant source control host than the traditional model that we are all familiar with using over the past decade. It's entirely git under the hood, but with a git server built on top of blob storage for higher throughput, better reliability, and increased scale.


It's straightforward to try: create a git repository in Depot, push to it as your daily driver, and trigger Depot CI jobs. We'll also replicate your commits back to your upstream GitHub repository if you want.


Depot Code repositories in the Depot dashboard


In true Depot fashion, we're going to dive deep into the details of why we built source control hosting, the tech that powers it, and where we see this going over the next couple of months.


## Why build source control hosting?


Code isn't scarce anymore. It's more abundant than ever.


But the old SDLC model is broken. Our existing tools and assumptions can't keep up with the volume and velocity of code.


All the tools we have around code are built for human-centric processes, moving at human speed. They're designed for a throughput that no longer exists.


The primary event bus for that system: source control.


**We build primitives that make software delivery exponentially faster.** We have spent the past three years focusing on making software delivery infrastructure faster and more reliable for this new throughput of code. Our builds, runners, caching, sandboxes, and registry support high throughput with high performance.


We've rebuilt each step as an infrastructure primitive because the slower each step of the layer is, the slower you ship, the more frustrated you get, and the more velocity you lose as a result. Whether it's an engineering team of 5,000 or a 3-person team running 50 agents, the pain points are the same.


Our rationale is simple: the faster each primitive is, the faster the overall process is. The more you and your team, however it is composed, can ship code you trust to production.


We've spent three years accelerating the build, test, and CI loops around the delivery stack. But the bottlenecks have started to shift for us, and our customers, to the top of the loop: source control itself.


## How is Depot Code different?


Depot Code doesn't use the default git server. In fact, we wrote our own git server to be diskless. It stores all content back to S3 and is fully horizontally scalable. It requires no distributed consensus and is optimized for throughput, uptime, and the overall scale of software engineering today.


The default git server provided by the main human-centric processes today, like GitHub and GitLab, uses a hub & spoke model that requires a persistent file server with a persistent singleton. In both cases, they route every push through a proxy that streams file updates across three replicas that run a multi-phase commit. The git transaction is then played on each replica which are just a vanilla git server running with a local disk.


This is limiting, it welds storage and compute together. You have to add additional file servers that carry disk in order to scale up the system. You can't scale read/write capacity independently of storage capacity, so you have to scale for peak capacity. Furthermore, repos end up pinned to machines (granted they are replicated) which causes performance issues with monorepos or write-heavy repos because you can swamp a single write primary. To compound matters, this is a non-trivial stack to try to self-host inside of your own environment.


**Depot Code inverts the primitive.** Git packfiles and their indexes become objects on S3, refs and metadata live in a transactional store, and git servers become stateless horizontally scalable workers. It's not some exotic setup. So it's something that you self-host in your own environment without the massive complexity of the prior model.


## Get started


After you fill out the[request form](https://forms.gle/33jJc3uQFQqYhkKKA) , we'll reach to let you know we've turned it on for your organization.


For the beta, you create repositories from the Depot dashboard. Everything after that, cloning, pushing, and fetching, is regular` git` from the command line.


### Create a repository


1. In the Depot dashboard, under **Code** , click **Repositories** .
2. Click **Create repository** .


Create a standalone repository hosted on Depot Code


1. Choose whether to create a **Standalone** repository or a **Mirror** :


- **Standalone** : enter a repository name and a default branch name. Depot Code creates a fresh new repository for you to push your code.
- **Mirror** : choose an upstream GitHub repository to mirror. Depot Code keeps a synced copy of the upstream from which to serve clones and fetches, so your Depot CI workflows don't round-trip to GitHub.


From there, everything is plain` git` . Grab the repository URL from the dashboard and either clone it:


```text
git   clone   <  repository-ur  l  >
```


or add it as a remote to an existing local repository and push:


```text
git   remote   add   depot   <  repository-ur  l  >
git   push   -u   depot   main
```


## What's next


We'll make Depot Code available via an SDK so that you or an agent can fully drive it headless to build out any kind of workflows or collaboration patterns you want. You will be able to use it with our programmable CI engine, Depot CI, to design the perfect code collaboration pattern for your team.


The new world of software engineering we find ourselves in requires us to rethink our original assumptions, the tools we use, and the paradigms we've built around both.


We fundamentally believe that source control is no longer just a collaboration tool that can afford to move at human pace. Source control must be an infrastructure tool that supports high throughput operations that are performant and reliable at scale.


Depot Code is our first step in rebuilding that part of the loop.


## Related posts


- [Staying in control of your codebase in the AI era](https://depot.dev/blog/staying-in-control-of-your-codebase-in-the-ai-era)
- [Now available: Depot Sandbox SDK](https://depot.dev/blog/now-available-the-depot-sandbox-sdk)
- [Now available: Depot CI API and CLI](https://depot.dev/blog/now-available-depot-ci-api)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
