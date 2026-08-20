---
schema_version: "1.0.0"
document_id: "9fda2144fd894c7aa95ab3e60f7e0457d439cba90bbb395a347930998ba06ddd"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/autopilot-draft-agents-workflows-ai-chat-tabs"
published_at: "2026-03-05T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:8908e551fb53e693a853c50bb2ef94e1ffbf2c1adb563d124b69f4eab8085834"
---

# Autopilot, Draft Agents & Workflows, and AI Chat Tabs

## Autopilot: AI-Powered Project Creation


Introducing **Autopilot** , a new way to go from idea to deployed project with minimal effort. Describe your project and let Cosmic's AI handle the rest: content modeling, content generation, code scaffolding, deployment, and ongoing automation workflows.


- **New "Build with Autopilot" option** in the project creation flow, guiding users through goal selection, project type, and description input.
- **AI-generated project descriptions** based on your inputs for clearer project presentations.
- **Context management** with support for media references, brand guidelines, MCP servers, and reference links to give the AI richer context.
- **Model selection** so you can choose which AI model powers your Autopilot build.
- **Deployment monitoring** with retry logic and status checking to verify successful deploys.
- **Email notifications** when your Autopilot project is complete, with deployment status and automation plan details.
- **Automation plan** Autopilot sets up a plan for recommended ongoing automation operations. These may include automated workflows for research-based SEO content strategy, code application maintenance, and content distribution.


## Cosmic Runner Game


A new game is available while you wait for your Autopilot app to build, which may take 2 to 5 minutes. Navigate your spaceship through asteroids, collect power-ups, fight bosses, and share your high score.[Play it here](https://app.cosmicjs.com/play) .


## Draft Status for Agents and Workflows


Agents and workflows now start in **draft** status by default. Plan limits are only enforced when you activate them, giving you more freedom to set up and configure before going live.


- New agents and workflows are created as drafts with clear visual indicators across the dashboard.
- Activation flow with dedicated buttons to transition from draft to active.
- Draft entities are excluded from plan limit counts and usage metrics, so you can create freely and activate when ready.
- Agent limit checks and related warning UI have been removed from creation forms for a cleaner experience.


## Enhanced Conflict Resolution


Resolving merge conflicts in pull requests just got easier with both manual and AI-assisted options.


- **New Conflict Resolution Modal** for managing merge conflicts directly in the dashboard.
- **AI-assisted conflict resolution** with refined prompts and resolution strategies.
- **Manual resolution endpoint** allowing users to commit resolved files directly.
- Improved user feedback with toast notifications for conflict resolution status.


## Dashboard Enhancements


- **New AI Chat tabs** : The AI Chat now includes dedicated tabs for different workflows: **Content** (create content models and objects), **Build App** (scaffold and deploy applications), **Update Repo** (update connected repositories), and **Automate** (create and edit agents and workflows). Each tab maintains its own context and chat history with support for up to 8 parallel chat sessions.
- **Deployment monitoring** : Repository components now show loading states during pending deployments with polling for status updates.
- **Chat caching** : InlineAIChat now caches messages for faster retrieval and state management.
- **Environment variable optimization** : Concurrent fetching of environment variables and checks for improved performance.
- **Workflow activation** : Improved data consistency with refetch after activation instead of full page reload.
- **Improved link handling** : Links in AI generation modals now open in new tabs with proper security attributes.


## Bug Fixes


- Fixed workflow scheduling logic for accurate next-run calculations when user-specified times are in the past.
- Fixed bucket ID consistency during bucket creation and cloning operations.
- Corrected error messages and routing paths in the plan review page.
- Cloned projects now auto deploy to Vercel
