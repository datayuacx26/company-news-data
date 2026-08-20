---
schema_version: "1.0.0"
document_id: "5d6bf28eeea8bd3506a72fbf4f94f991acfcff28753c00bfda44a06d20f63b01"
company_key: "yc-draftbit"
company: "Draftbit"
source_id: "yc-draftbit-news-import-aa5bd69b40d1"
canonical_url: "https://draftbit.com/whats-new/projects-websites-billing-plan-changes/"
published_at: "2026-07-06T18:10:26.256+00:00"
first_seen_at: "2026-07-24T05:13:30.805139+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:555d23e6fd63361d6779578e8b7f2bb76b5d8505abf13fa8d1a8d1828d9e5be2"
---

# Projects, Websites, Billing Plan Changes, Revamped Dashboard, Version History, Docs Updates + more!

Hey builders! This release is a big one across the core Draftbit experience: Projects are becoming the center of your workspace, websites are now available to everyone, billing plans are clearer, the dashboard has a new shape, Version History is much easier to use, and docs have a new home.


> **TL;DR:** Projects now have dashboards, settings, activity, project-level assets, tasks, MCPs, and Supabase connections. Astro-powered website apps are open to all users. Billing plans now use clearer Base naming, BYOK availability is easier to understand, and free plans are moving away from recurring credit grants. The dashboard has a new collapsible left sidebar. Version History has been redesigned with paginated commit history. Docs now live at[docs.draftbit.com](https://docs.draftbit.com/) . Plus AI chat search, safer uploads, sandbox reliability fixes, model updates, and more.


## Projects Have Arrived


We've always had users building related apps in Draftbit: a customer-facing app and an admin app, a mobile app and a website, or multiple apps that share the same backend, credentials, assets, and integrations. Projects give all of that work a shared home.


Every app in Draftbit now belongs to a Project. From the project details page, you can view related apps, activity, and project details without needing to start a sandbox. Over time, Projects will become the main workspace for more of your app-building workflow.


**


A lot of app-related resources are now project-related instead. Shared Assets, Integrations, Tasks, MCPs, and Supabase Backends are organized at the Project level, meaning all apps in the project can access them without duplicating the same setup across each app. You can also create a new app directly from the project app switcher, move apps between projects, and manage project settings from inside the Builder.


This should make Draftbit feel much more natural once you're working on a real product with multiple apps, integrations, assets, tasks, and teammates involved.


## Astro-powered Websites


We've added a new type of app: Astro websites. You can now create website apps in Draftbit and use the same AI-powered building workflow to generate, edit, preview, and iterate on web experiences. That means your marketing site, landing pages, docs-style pages, and other web surfaces can live alongside your app.


**


Astro is a great fit for websites because it is built for fast, content-heavy pages with strong SEO defaults, while still letting you add interactive components where you need them. In practice, that makes it a strong foundation for landing pages, marketing sites, resource pages, and lightweight product websites.


## Billing Plan Changes


We've made some big changes to how our billing plans work, especially around AI usage.


Recently, we made it possible for all users, including Free plan users, to connect their existing Claude or OpenAI subscriptions to Draftbit. You can also connect API keys for Claude, OpenAI, and OpenRouter. This lets you use Draftbit with your own AI provider access instead of relying only on Draftbit credits.


That direction is important to us. Our goal is to make Draftbit an open building environment that works well with the AI services you already use. Over time, we expect to support more agents, more model providers, and more bring-your-own subscription options.


As part of that shift, we're moving away from recurring free credit grants. Newer models use more tokens, costs can vary a lot from one task to another, and recurring grants made it harder for users to understand what they had available and how long it would last.


Instead, we recommend that Free plan users bring their own AI subscription or API key while trying Draftbit. We've also expanded ways to earn Draftbit credits by helping us spread the word about Draftbit.


Draftbit Credits are not going away. Any credits you've purchased, earned, or been granted are still available to use, and credits will remain an option for users who prefer not to connect their own AI providers. We've also launched plan options without bundled credits, so you can choose the Draftbit plan that best matches how you want to pay for AI usage.


## A Revamped Dashboard


The dashboard has a new layout built around an inset, collapsible left sidebar. Navigation is more consistent, project and app cards are cleaner, and the workspace is easier to scan when you have more than one thing going on.


The goal is to make Draftbit feel less like a set of separate pages and more like a workspace. Projects, apps, billing, resources, docs, and account actions now have clearer places to live, and the new layout gives us more room to keep improving navigation without crowding the top of the screen. We'll be expanding this in the future with more capabilities for your projects and apps.


## Version History Got a Major Upgrade


Version History has been redesigned to be easier to read and more useful on long-running projects. The page now supports paginated commit history, so you're no longer capped at the old 50-commit limit when reviewing older work.


**


Commits triggered by users are also attributed to the person who started the action, which makes history easier to understand on teams and in projects where both humans and agents are making changes.


This should make Version History a more reliable place to answer questions like "what changed?", "who kicked this off?", and "how far back can I go?"


## Docs Updates


Draftbit docs now live primarily at[docs.draftbit.com](https://docs.draftbit.com/) . We've merged together our v1 docs and our new platform docs into a single resource, so you can always find what you need regardless of which version of Draftbit you're using.


## + More


#### Search your AI chats


AI chat history is now searchable, so you can find previous threads and messages without digging through a long thread list.


#### More reliable sandbox behavior


Sandbox startup, preview initialization, repo service handling, package installation, and legacy prewarming all got reliability improvements. Simulator pages now show logs, and the app loading view includes a transcript copy action.


#### Safer uploads and imports


File uploads are more reliable, font files are protected from editing, keystore uploads are capped at 10 KB, and V1 migration/import flows received several fixes.


#### Model and agent updates


We updated the OpenAI, Codex, Anthropic, and agent SDK libraries, added Claude Sonnet 5 support, added GLM 5.2 through OpenRouter, and disabled GPT-5.4 Nano for Codex.


#### Fixes


We've also fixed a few bugs here and there to make your experience faster and easier.


---


That's a wrap for this round. Thanks for building with us, and keep the feedback coming. Happy building!
