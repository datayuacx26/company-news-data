---
schema_version: "1.0.0"
document_id: "4d581154704263c5197b3d731659f5b484b93122ae8319989288f2a668a3693f"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/new-ai-models-invite-links-community-deployments-and-more"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-08-10T06:05:12.278252+00:00"
fetched_at: "2026-08-10T06:05:15.916664+00:00"
content_hash: "sha256:b244abee2a7215919c3c3f62a8f9d5973bb369b744a3f1b9735d14b36cc8ad96"
---

# New AI Models, Invite Links, Community Deployments, and More

## New AI Models: GPT-5.2 Codex, Gemini 3.1 Pro, and Claude Sonnet 4.6


Three new flagship AI models are now available across the platform (Dashboard, API, CLI, and MCP):


- **GPT-5.2 Codex** (` gpt-5.2-codex` ) - OpenAI's most intelligent coding model, optimized for long-horizon agentic coding tasks. 400K context window, 128K max output tokens.
- **Gemini 3.1 Pro** (` gemini-3.1-pro-preview` ) - Google's most advanced model with improved reasoning, token efficiency, and agentic performance. 1M+ context window, 65K max output tokens.
- **Claude Sonnet 4.6** (` claude-sonnet-4-6` ) - Anthropic's flagship model with the best combination of speed and intelligence. Now the default model across the platform, featuring adaptive-thinking capability. 200K context window, 64K max output tokens.


All three models are available in AI Studio, Automate agents/workflows, and through the API/SDK. The backend includes improved fallback handling to gracefully recover when a model is unavailable.


## Invite Links for Team Management


A new **invite link** feature makes it easier to add team members to projects and workspaces:


- When adding a new member, an **InviteLinkModal** automatically appears with a shareable invite link.
- Links can be copied to clipboard or shared directly via email with a pre-filled mailto.
- In member lists, pending invitations now show a **link icon** that copies the invite link to clipboard with one click.
- Invite links are single-use, tied to the invited email address, and only work for pending invitations.
- Works for both **workspace** and **project** level invitations.


## Cosmic Community Deployments


A new **Cosmic Community** deployment option allows users to deploy projects without needing their own GitHub or Vercel accounts:


- The **Deploy** button now includes "Cosmic Community" as an organization option, which uses a shared GitHub organization and Vercel account.
- Repositories deployed via Cosmic Community are automatically set to public.
- Deployment logs for Community repositories are displayed directly in the Cosmic dashboard (instead of redirecting to Vercel).
- This lowers the barrier to entry for new users who want to deploy projects immediately.


## Enhanced Media Upload Experience


The media library now features a significantly improved upload experience:


- **Drag and drop** : Drop files directly onto the media library with visual feedback (border highlights during drag).
- **Paste from clipboard** : Global paste handler lets you paste images and files directly from your clipboard.
- **Upload progress** : Files being uploaded show preview thumbnails with upload progress indicators.
- **Improved empty state** : Clear instructions ("Drag and drop files here, paste from clipboard, or click to add") with supported file type info.


## Media Bandwidth Pricing


**Media bandwidth** is now tracked as a separate metric from API request bandwidth, with its own limits and overage rates:


Plan Media Bandwidth Included


Free 1 GB


Starter 100 GB


Pro 500 GB


- Overage rate: **$0.30 per GB** (vs $0.36/GB for API request bandwidth).
- The **pricing calculator** now includes a "Media Bandwidth (GB)" slider for estimating costs.


## Improved GitHub and Vercel Connection Management


- Repository settings now display connected GitHub and Vercel accounts with **username visibility** , showing which account is connected.
- Better handling of **disconnected states** : clear messages and actions when GitHub or Vercel accounts are disconnected, including user avatar and name display for disconnected users.


## Agent and Automation Improvements


- **Pending commit handling** : When agent operations are blocked by missing environment variables, the platform now stores pending commit data and automatically commits changes once the session is marked complete.
- **Event-triggered agent gating** : The agent creation form now displays an informational message that event-triggered agents require a Starter plan or higher, with a link to upgrade.
- **Computer-use agent prompt instructions** : Automate now includes detailed prompt instructions for browser automation agents.


## CLI v1.1.3


- Fixed workflow creation to handle nested API response structures.
- Updated documentation to reflect all new model versions.
- See[Cosmic CLI](https://www.npmjs.com/package/@cosmicjs/cli) .
