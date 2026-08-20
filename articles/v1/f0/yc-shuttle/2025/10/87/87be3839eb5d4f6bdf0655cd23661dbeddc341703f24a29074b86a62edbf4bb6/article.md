---
schema_version: "1.0.0"
document_id: "87be3839eb5d4f6bdf0655cd23661dbeddc341703f24a29074b86a62edbf4bb6"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2025/10/21/github-integration"
published_at: "2025-10-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:d32c74a2fc3d1ee0fad4506db6d51043716f75aa4f2d33f9c46346217aca66bb"
---

# From Push to Production: Deploy from GitHub with Shuttle

If you're building in Rust, chances are you spend more time crafting elegant code than thinking about deployments and that's exactly how it should be.


At Shuttle, we believe deploying your Rust application should feel as natural as writing it. That's why we've built a new **GitHub integration** feature that takes you from *push* to *production* automatically.


No manual redeploys. No juggling CLI commands. Just push your code to GitHub, and we'll handle the rest. 🚀


## Why It Matters #


Every developer knows the drill, push code, switch to your deploy tool, trigger a build, wait, fix a config, deploy again. It's not exactly the smoothest workflow.


With the new **Shuttle + GitHub** integration, that entire process collapses into one seamless step: **git push** .


When you connect your GitHub repository to Shuttle, your deployment pipeline syncs directly with your codebase. Push updates to your selected branch, and Shuttle automatically rebuilds and redeploys your app. Your project stays in sync with your latest code - no extra clicks required.


Fewer steps, fewer tools, and more time spent actually building.


## How It Works #


Connecting GitHub to Shuttle is simple - and it all happens inside the Shuttle console.


1. Head to your[Integrations](https://console.shuttle.dev/account/integrations) page
2. Click **"Connect to GitHub"** and authorize Shuttle.
3. Choose the repositories you want to deploy from.


Once connected, your GitHub account appears in the Shuttle dashboard. From there, you can:


- Select a repository and deploy directly from the dashboard.
- Choose which branch to deploy.
- Add environment secrets.
- Enable automatic deployments with one toggle.


Pro tip: Next time you push to that branch, your code is live - automatically.


## Key Features #


Here's what makes the GitHub integration more than just a connection:


1. **Automatic Deployments** - Every push triggers a fresh build and deployment. No manual redeploys.
2. **Deploy from Dashboard** - Launch new versions directly from the Shuttle console without touching the CLI.
3. **Pre-Built Templates** - Deploy example apps instantly from our GitHub templates — ideal for getting started fast.
4. **Link Existing Repos** - Bring your own repository and connect it to any Shuttle project, new or existing.
5. **Fully Hosted, Fully Rust** - All the performance and control of Rust, without worrying about infra.


## A Simpler Workflow for Rust Developers #


Let's say you're building a Rust API. Normally, you'd have to:


- Push your code to GitHub.
- Open your deployment tool or CI/CD pipeline.
- Configure build steps, secrets, and environments.
- Trigger a redeploy manually.


With Shuttle? You push to GitHub and... that's it.


Your app rebuilds and redeploys automatically — live, hosted, and ready before you've finished your coffee.


It's the clean, modern workflow that lets you focus on your code, not your config files.


---


## Deploy Templates in Seconds #


Don't have a repository yet?[Shuttle's template library](https://console.shuttle.dev/templates) make it easy to start.


Pick a template from our[Templates page](https://console.shuttle.dev/templates) , connect your GitHub account, and Shuttle automatically:


- Creates a new repository in your GitHub account
- Sets up a ready-to-run Shuttle project
- Deploys it instantly to production


It's the fastest way to go from zero to a live Rust app — no CLI installation, no setup pain.


## Managing Your Connection #


You can manage all your GitHub connections right inside Shuttle:


- View and change linked repositories per project.
- Disconnect or reconnect at any time.
- Enable or disable automatic deployments with a single switch.


All without affecting your existing deployments or GitHub repos.


#### Join the Shuttle Discord Community


Connect with other developers, learn, get help, and share your projects


[Join](https://discord.gg/shuttle)


## Start Deploying Smarter #


The GitHub integration is available to all Shuttle users.


Connect your repo, push your code, and let Shuttle handle the rest — from build to production.


👉[Connect your GitHub account and deploy now →](https://console.shuttle.dev/)
