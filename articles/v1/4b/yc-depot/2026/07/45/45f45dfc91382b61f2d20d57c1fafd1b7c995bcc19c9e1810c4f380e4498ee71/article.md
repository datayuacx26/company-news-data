---
schema_version: "1.0.0"
document_id: "45f45dfc91382b61f2d20d57c1fafd1b7c995bcc19c9e1810c4f380e4498ee71"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/stripe-projects"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:365f506e662292a9ec5ec13f2322309fe02a5a0ff7d7a6b97f179d9b52cda648"
---

# Depot is now available in Stripe Projects

We're thrilled to join the[Stripe Projects](https://projects.dev/) developer preview. You and your agents can add Depot to a project and authenticate straight from the terminal using the Stripe CLI.


This is one of the coolest things we've gotten to build. It matches how we already think about Depot: fast, scalable software delivery, reachable by the agents doing the building.


Stripe Projects is a new workflow in the Stripe CLI that lets developers, and the agents working alongside them, provision real services, generate and store credentials, and manage usage from the command line. Instead of clicking through a dashboard to set up your stack, you describe what you need and get a working, production-like environment back in a few commands.


## What you get when you add Depot in Stripe Projects


Add Depot to a Stripe Project to create or link a Depot organization in your own name, activate a Depot plan, and automatically set up billing through Stripe. You'll receive a Depot API token straight to your environment. That token unlocks the Depot platform:[Depot CI](https://depot.dev/docs/ci/overview) , Depot[GitHub Actions runners](https://depot.dev/docs/github-actions/overview) , fast remote[container builds](https://depot.dev/docs/container-builds/overview) , and a shared, persistent build cache.


## Why we did this


Agents are building software, and they're driving more of the work with every release. But an agent that writes code still hits a wall the moment it needs real infrastructure. It can't bounce through a dashboard, generate a token, and wire it into a` .env` the way a person would. It needs a deterministic way to get real services and real credentials on its own.


Stripe Projects makes that possible, which is why we joined as a co-design and launch partner on the Agentic Provisioning Protocol (APP), the spec behind provisioning services and returning credentials safely from the CLI. Now an agent can provision a Depot organization itself and get straight to work with Depot for secure, fast, reliable CI.


## How to get started


To try the Stripe Projects developer preview with Depot:


1. Install the[Stripe CLI](https://docs.stripe.com/stripe-cli/install) .
2. Run` stripe projects init my-app` .
3. Run` stripe projects add depot` and choose a plan.


Stripe Projects creates or links a Depot organization in your name, activates your plan and billing, and returns a Depot API token to your environment. From there, you're ready to run your first container build, or connect to GitHub and run your workflows on Depot infrastructure.


## What's next


This is a developer preview, and we want to hear from teams building with agents or just trying this out. Come tell us what's working and what you'd like to see in our[Discord community](https://discord.gg/MMPqYSgDCg) .


## Related posts


- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)
- [Context isolation in coding agent loops](https://depot.dev/blog/context-isolation-in-coding-agent-loops)
- [Announcing Depot Metal](https://depot.dev/blog/announcing-depot-metal)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
