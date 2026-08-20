---
schema_version: "1.0.0"
document_id: "ffaddc23699f12c01c0bc1379332c70c3746a93a161a49fb3eee63c9c096aebf"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-news-import-57e353815c4b"
canonical_url: "https://docs.opensaas.sh/blog/2025-11-21-open-saas-public-roadmap/"
published_at: "2025-11-21T00:00:00+00:00"
first_seen_at: "2026-07-24T06:46:50.580409+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:c616f39fa8d361986fec6eb2d124a99319b2c41ebe3de600231e3b4d93e07ea0"
---

# The Public Development Roadmap for Open SaaS

# The Public Development Roadmap for Open SaaS


Nov 21, 2025


[Vince Dev Rel @ Wasp](https://wasp.sh/)


We created Open SaaS to help developers launch full-featured SaaS products faster.


But launching fast only works when you’re working with clear direction and a good foundation. As an open-source project powered by[Wasp](https://wasp.sh/) , and with our users in mind, we thought it was important to add two things to this project:


1. A solid roadmap so that development is focused.
2. Making that roadmap public, to give the community a clear view of what’s coming next and how they can get involved.


So we created just that.


Behold, the[Open SaaS public roadmap](https://github.com/orgs/wasp-lang/projects/6) !


---


## Why a roadmap?


Section titled “Why a roadmap?”


Open SaaS is becoming a beast of its own. At[13k GitHub stars](https://github.com/wasp-lang/open-saas) and growing, and with constant feature additions and improvements, we thought it was time to make future development more organized, transparent, and community-driven.


With tons of SaaS apps being deployed with Open SaaS, the needs of its users are also becoming more diverse. It would be impossible to address all of them, but we needed a way to track and prioritize the most important features and improvements.


Up until now, it was just me basically deciding which issues to work on and when. This was pretty inefficent to begin with, and then with more contributors joining the project, it became even more so.


The roadmap is thus a great way to make sense of all the different possible directions we can go in, help focus our efforts, and make development and contributions more meaningful.


## How it works


Section titled “How it works”


Open SaaS currently has about[100 open GitHub issues](https://github.com/wasp-lang/open-saas/issues) , so it would be crazy to organize and track them all in the roadmap.


So to make things overseeable, the roadmap is a GitHub project with a collection of “Epics”. An Epic is just a GitHub issue with a bunch of sub-issues. Each Epic has a status and a progress bar to show you how many of its sub-issues are completed.


No regular issues are allowed in it, only epics, of which we don’t want too many. This way, its easy to understand the current themes and priorities of the project, as well as what’s currently underway.


---


## Guiding themes


Section titled “Guiding themes”


When creating epics within the roadmap, there are a few categories we’re focusing on at the moment. These guiding themes inform the epics we create, and help us make sure we’re addressing the most important needs of our users.


### 1. Real-world launch readiness


Section titled “1. Real-world launch readiness”


Open SaaS should not just be a polished demo, or a flimsy template. It should be something you can deploy and scale. We want to make sure Open SaaS is a production-ready starter that can be used to build real-world SaaS products. This means having a solid architecture, a clean codebase, and a set of features that are robust and ready to be used in production.


### 2. Modularity & flexibility


Section titled “2. Modularity & flexibility”


Different[auth methods](https://docs.opensaas.sh/guides/authentication/) ,[billing providers](https://docs.opensaas.sh/guides/payment-integrations/) , file uploads, AI integrations, analytics, and more. We want to make sure Open SaaS is a flexible and modular platform that can be tailored to the needs of (almost) any SaaS product. This means having the most essential features you need to launch your SaaS quickly, and being able to easily remove the things you don’t.


### 3. AI-first developer experience


Section titled “3. AI-first developer experience”


Whether you use Cursor, VSCode + Copilot, Claude, or others, Open SaaS should have top-notch code quality and be able to “explain itself” well to AI-driven coding tools, making it enjoyable to develop with in 2026 and beyond.


We also want to provide demos and templates for AI-powered SaaS products, so you can get started building SaaS apps that leverage the newest technologies to create powerful and innovative products.


### 4. Community-powered evolution


Section titled “4. Community-powered evolution”


We want Open SaaS to be shaped by real feedback, real PRs, and real use cases. Roadmap ideas are only valuable when they reflect what people actually need. So your[feedback and contributions](https://github.com/wasp-lang/open-saas/issues) are important to us!


---


## What’s in the roadmap


Section titled “What’s in the roadmap”


We just got finished with a major UI redesign, which we dubbed “Open SaaS v2.0”. Take a look at the announcement below.


With the new UI, based on[Shadcn UI](https://ui.shadcn.com/) , finished and tucked away, we’ve turned back towards adding highly-requested features, and improving the codebase with smaller fixes.


### 1. Payment Provider Expansion


Section titled “1. Payment Provider Expansion”


Today, Open SaaS supports Stripe, Polar, and Lemon Squeezy as payment providers. It’s super[easy to get started](https://docs.opensaas.sh/guides/payment-integrations/) with one – just plug and play.


**Upcoming features:**


- Polar.sh integration (merged in[#461](https://github.com/wasp-lang/open-saas/pull/461) )
- Paddle, and additional gateways (especially regional-friendly options)
- Supporting up- or down-grading between different billing plans and models (proration)


---


### 2. AI-First Workflows & Templates


Section titled “2. AI-First Workflows & Templates”


Open SaaS already includes a simple OpenAI integration example, but we’re looking to expand and improve it.


**Coming soon:**


- Exploring using Vercel’s AI SDK within our demo app (issue[#567](https://github.com/wasp-lang/open-saas/issues/567) ).
- Cursor / Claude prompt presets for smoother onboarding (issue[#579](https://github.com/wasp-lang/open-saas/issues/579) )
- Creating better docs and guides for AI-powered SaaS development


---


### 3. Core Development Improvements


Section titled “3. Core Development Improvements”


We want to keep Open SaaS relevant and up-to-date with the latest trends, as well as with the latest Wasp (the framework it’s built on) features and improvements .


**Efforts underway:**


- Refactors, galore! E.g. removing` any` and` as` from the codebase, extracting` SVGs` , etc. (issue[#568](https://github.com/wasp-lang/open-saas/issues/568) )
- Improving OpenSaaS.sh development workflows (issue[#552](https://github.com/wasp-lang/open-saas/issues/552) )
- Improving e2e tests (issue[#558](https://github.com/wasp-lang/open-saas/issues/558) )


---


# How to Participate


Section titled “How to Participate”


So that’s just a quick overview of the current state of the roadmap. But we’re not done yet!


Open SaaS is community-driven — and we’d love your help.


### ⭐ Star the repo


Section titled “⭐ Star the repo”


This is super easy and helps a ton!


[https://github.com/wasp-lang/open-saas](https://github.com/wasp-lang/open-saas)


### 🧩 File issues or feature requests


Section titled “🧩 File issues or feature requests”


Especially around real-world SaaS requirements you’re running into.


[https://github.com/wasp-lang/open-saas/issues](https://github.com/wasp-lang/open-saas/issues)


### 🔧 Contribute code


Section titled “🔧 Contribute code”


If you’re interested in payments, dashboarding, AI workflows, or dev-experience improvements — we’ll happily support and guide.


[https://github.com/orgs/wasp-lang/projects/6](https://github.com/orgs/wasp-lang/projects/6)


### 🚀 Build with it


Section titled “🚀 Build with it”


The best feedback always comes from people deploying real products. Drop us a line on[Discord](https://discord.com/invite/aCamt5wCpS) or[Twitter](https://x.com/wasplang) and let us know what you’re building!


---


# Roadmap Updates


Section titled “Roadmap Updates”


We’ll keep updating this roadmap alongside major releases and aim for a quarterly revision cycle.


You can follow progress in the GitHub Project board:


👉 **[https://github.com/orgs/wasp-lang/projects/6](https://github.com/orgs/wasp-lang/projects/6)**


See you there!


**Tags:**


- [roadmap](https://docs.opensaas.sh/blog/tags/roadmap/)
- [wasp](https://docs.opensaas.sh/blog/tags/wasp/)
- [open-source](https://docs.opensaas.sh/blog/tags/open-source/)
- [development](https://docs.opensaas.sh/blog/tags/development/)


[What's the Best Way to Vibe Code a SaaS in 2026?](https://docs.opensaas.sh/blog/2026-03-16-best-way-to-vibe-code-saas-2026/)


[Building a SaaS with Gemini 3 and Open SaaS](https://docs.opensaas.sh/blog/2025-11-19-gemini-3-open-saas/)
